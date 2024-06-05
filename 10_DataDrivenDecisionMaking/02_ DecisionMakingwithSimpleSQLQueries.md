## Summary Grouping  | SQL

### Lesson Overview
- Introduction to using the GROUP BY clause in SQL queries to apply aggregations to table groups.
- GROUP BY clause used for analyzing groups such as customers by country or movies by genre, focusing on calculating average rental prices by genre.
- A subset table named 'movies_selected' includes movie name, genre, and rental price, used to determine the average rental price per genre using GROUP BY.
- Basic SQL GROUP BY usage to group data by genre and add aggregation functions like average, sum, minimum, or maximum to refine data analysis.
- Average rental price per genre calculated and displayed using an alias avg_price in the query, providing clarity on price distribution across genres.
- Illustration of how GROUP BY divides the movies_selected table into subgroups based on genre, highlighting the organizational aspect of this clause.
- Separate tables for each genre show specific groupings where aggregation functions like COUNT are applied to ascertain the number of movies per genre.
- Query enhanced to include the count of movies in each genre, combining COUNT and average price calculations for comprehensive data insights.
- Explanation of the HAVING clause for filtering GROUP BY results based on specified conditions, such as groups with more than two movies, demonstrating its utility in refining query results.

```
-- Create a table with a row for each country and columns for the country name and the date when the first customer account was created and use the alias first_account for the column with the dates then order by date in ascending order.

select country,
	min(date_account_start) AS first_account
from customers
group BY country
order by first_account;

--Group the data in the table renting by movie_id and report the ID and the average rating.
select movie_id, 
       avg(rating)    -- Calculate average rating per movie
from renting
group by  movie_id;

--Add two columns for the number of ratings and the number of movie rentals to the results table and use alias names avg_rating, number_rating and number_renting for the corresponding columns.
SELECT movie_id, 
       avg(rating) AS avg_rating, 
       count(rating) as number_rating,                
       count(renting_id) as number_renting 
FROM renting
GROUP BY movie_id;

--Group the data in the table renting by customer_id and report the customer_id, the average rating, the number of ratings and the number of movie rentals.Select only customers 
--with more than 7 movie rentals and order the resulting table by the average rating in ascending order.
SELECT customer_id,
      avg(rating),  
      count(rating),  
      count(renting_id)  
FROM renting
GROUP BY customer_id
having count(*) >7 
ORDER BY avg(rating);

```

## Joins

1. **Joining Tables in SQL**:
   - Join queries allow combining data from multiple tables.
   - LEFT JOIN is an OUTER JOIN that augments one table (left) with information from another table (right).
   - All rows from the left table are retained, and only matching rows from the right table are used.

2. **Table Aliases**:
   - Use aliases (names) for tables to improve readability.
   - Example: Alias the "customers" table as "c" (c-dot column name).

3. **LEFT JOIN Example**:
   - Select all columns from the "renting_selected" table (alias "r").
   - LEFT JOIN with the "customers_selected" table (alias "c") on the condition that customer_id matches.
   - Result includes columns from both

```
--Augment the table renting with all columns from the table customers with a LEFT JOIN. Use as alias' for the tables r and c respectively.
SELECT * 
FROM renting r 
LEFT JOIN customers c
ON r.customer_id=c.customer_id;

---Select only records from customers coming from Belgium.
SELECT *
FROM renting AS r
LEFT JOIN customers AS c
ON r.customer_id = c.customer_id
where country='Belgium'; 
-- Average rating
SELECT avg(rating) -- Average ratings of customers from Belgium
FROM renting AS r
LEFT JOIN customers AS c
ON r.customer_id = c.customer_id
WHERE c.country='Belgium';

--First, you need to join movies on renting to include the renting_price from the movies table for each renting record and use as alias' for the tables m and r respectively.
SELECT *
FROM renting AS r
left join movies AS m -- Choose the correct join statment
ON r.movie_id=m.movie_id;

--Calculate the revenue coming from movie rentals, the number of movie rentals and the number of customers who rented a movie.
SELECT 
	SUM(m.renting_price), 
	COUNT(*), 
	COUNT(DISTINCT r.customer_id)
FROM renting AS r
LEFT JOIN movies AS m
ON r.movie_id = m.movie_id;

-- Calculate the revenue in 2018, the number of movie rentals and the number of active customers in 2018. An active customer is a customer who rented at least one movie in 2018.

SELECT 
	SUM(m.renting_price), 
	COUNT(*), 
	COUNT(DISTINCT r.customer_id)
FROM renting AS r
LEFT JOIN movies AS m
ON r.movie_id = m.movie_id
WHERE date_renting between '2018-01-01' and '2018-12-31';

--Create a list of actor names and movie titles in which they act. Make sure that each combination of actor and movie appears only once and use as an alias for the table actsin the two letters ai.
select a.name, 
       m.title
from actsin AS ai
LEFT JOIN movies AS m
on m.movie_id = ai.movie_id
LEFT JOIN actors AS a
ON a.actor_id = ai.actor_id;

--Report the total income for each movie and order the result by decreasing income.
select rm.title,
       SUM(rm.renting_price) AS income_movie
FROM
       (SELECT m.title, 
               m.renting_price
       FROM renting AS r
       LEFT JOIN movies AS m
       ON r.movie_id=m.movie_id) AS rm
GROUP BY rm.title
ORDER BY income_movie DESC; 

-- Give age of American actors and actresses. Report the date of birth of the oldest and youngest US actor and actress.

SELECT a.gender, 
       MIN(a.year_of_birth), 
       MAX(a.year_of_birth)
FROM
    (SELECT * 
    FROM actors
    WHERE nationality = 'USA') AS a GROUP BY a.gender;

```



1. **Objective**:
   - Explore the favorite actors for specific customer groups using complex SQL queries.

2. **Combining SQL Statements**:
   - Combine LEFT JOIN, WHERE, GROUP BY, HAVING, and ORDER BY in a single query.

3. **Data Preparation**:
   - Use a LEFT JOIN to augment the "renting" table with actor and customer information.
   - Join "renting" with "actsin" (via "movie_id") and then with "actors" (via "actor_id").

4. **Male Customers**:
   - Select male customers from the joined table.
   - Group actors' names using "GROUP BY a-dot-name."
   - Count how often each actor's movies are watched by male customers.

5. **Favorite Actor Criteria**:
   - Determine what makes an actor the favorite.
   - Consider either frequent viewership or high movie ratings.

6. **HAVING and ORDER BY**:
   - Exclude actors with NULL average ratings using the HAVING clause.
   - Order results by average rating (highest first) and views (highest for the same rating).

7. **Result**:
   - In the fictional movie database, Ray Romano is the favorite actor among male customers, with a perfect rating of 10 but only 3 views.

```
-- Select only those records of customers born in the 70s.
SELECT *
FROM renting AS r
LEFT JOIN customers AS c
ON c.customer_id = r.customer_id
LEFT JOIN movies AS m
ON m.movie_id = r.movie_id
WHERE c.date_of_birth BETWEEN '1970-01-01' AND '1979-12-31';

--For each movie, report the number of times it was rented, as well as the average rating. Limit your results to customers born in the 1970s.

SELECT m.title, 
COUNT(*), 
AVG(r.rating)
FROM renting AS r
LEFT JOIN customers AS c
ON c.customer_id = r.customer_id
LEFT JOIN movies AS m
ON m.movie_id = r.movie_id
WHERE c.date_of_birth BETWEEN '1970-01-01' AND '1979-12-31'
GROUP BY m.title;

--Remove those movies from the table with only one rental and order the result table such that movies with highest rating come first.
SELECT m.title, 
COUNT(*),
AVG(r.rating) 
FROM renting AS r
LEFT JOIN customers AS c
ON c.customer_id = r.customer_id
LEFT JOIN movies AS m
ON m.movie_id = r.movie_id
WHERE c.date_of_birth BETWEEN '1970-01-01' AND '1979-12-31'
GROUP BY m.title
HAVING COUNT(*) > 1 
ORDER BY AVG(r.rating) DESC; 

--report the favorite actors only for customers from Spain.
SELECT a.name,  c.gender,
       COUNT(*) AS number_views, 
       AVG(r.rating) AS avg_rating
FROM renting as r
LEFT JOIN customers AS c
ON r.customer_id = c.customer_id
LEFT JOIN actsin as ai
ON r.movie_id = ai.movie_id
LEFT JOIN actors as a
ON ai.actor_id = a.actor_id
WHERE c.country = 'Spain'
GROUP BY a.name, c.gender
HAVING AVG(r.rating) IS NOT NULL 
  AND COUNT(*) > 5 
ORDER BY avg_rating DESC, number_views DESC;

```
