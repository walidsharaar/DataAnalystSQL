# Summary Introduction to data driven decision making | SQL

### Facts
- Irene Ortner, alongside Professors Bart Baesens and Tim Verdonck, introduces the data-driven decision-making course.
- Refresh basic SQL and introduce advanced SQL techniques for business intelligence, including OLAP extensions like CUBE and ROLLUP.
- The course features a fictional movie rental company, MovieNow, which offers online movie streaming and retains comprehensive data on movies and customer interactions.
- Detailed exploration of 'customers' table in MovieNow's database, including customer ID, personal details, and account creation date.
- Insight into the 'movies' table, covering movie ID, title, genre, runtime, release year, and rental cost.
- Description of the 'renting' table, documenting each rental with identifiers, customer and movie details, ratings, and rental date.
- Examination of the 'actors' table, which lists actor IDs, names, birth years, nationalities, and genders.
- Overview of the 'actsin' table, noting the connections between actors and movies through unique identifiers.
- Use of data for making both short-term operational and long-term strategic decisions, such as movie purchases based on actor popularity or regional expansion based on customer growth analytics.
- Definition and monitoring of key performance indicators like revenue, customer satisfaction, and engagement to inform and refine decision-making processes.
- Encouragement for students to start exploring the database independently, applying what they've learned to real scenarios.

```
 - select only those columns from renting which are needed to calculate the average rating per movie.
SELECT movie_id,
       rating
FROM renting;
```

### Summary Filtering and ordering | SQL
- Filtering and Ordering: Filtering data to find relevant records and ordering them correctly is key for effective data analysis.
- WHERE Clause: A WHERE clause in SQL filters rows based on specific conditions, like selecting customers from Italy.
- Operators in WHERE Clause: Various operators such as comparison, BETWEEN, IN, and NULL can be used to refine selections in SQL queries.
- Comparison Operators Example: SQL can exclude genres or select movies with a minimum renting price using comparison operators.
- BETWEEN Operator Example: The BETWEEN operator helps select records within a specific date range, requiring precise syntax with dates in single quotes.
- IN Operator Example: The IN operator allows for checking if column values match any values in a given list, such as actor nationalities.
- NULL Operator Example: The NULL operator is used to handle missing or unknown data values, allowing selection based on the presence or absence of data.
- Boolean Operators AND: The AND operator combines multiple conditions to refine SQL queries further, such as selecting Italian customers with accounts created within a specific timeframe.
- Boolean Operators OR: The OR operator connects conditions alternatively, broadening the query to include more rows that meet any listed criteria.
- ORDER BY: SQL queries can be ordered by specific column values in ascending order by default using the ORDER BY statement.
- ORDER BY ... DESC: To prioritize higher values first, DESC is used in the ORDER BY clause to sort data in descending order, showcasing the highest ratings.

```
--Select all movies rented on October 9th, 2018.
SELECT *
FROM renting
where date_renting ='2018-10-09'; 

--Select all records of movie rentals between beginning of April 2018 till end of August 2018.
SELECT *
FROM renting
where date_renting between '2018-04-01' and '2018-08-31';

---Put the most recent records of movie rentals on top of the resulting table and order them in decreasing order.
SELECT *
FROM renting
WHERE date_renting BETWEEN '2018-04-01' AND '2018-08-31'
ORDER BY date_renting DESC; -- Order by recency in decreasing order


--Select all movies which are not dramas.
SELECT *
FROM movies
where genre != 'Drama'; 

-- Select the movies 'Showtime', 'Love Actually' and 'The Fighter'
SELECT *
FROM movies
where title in ('Showtime', 'Love Actually' , 'The Fighter');

--Select from table renting all movie rentals from 2018 and filter only those records which have a movie rating.

SELECT *
FROM renting
WHERE date_renting BETWEEN '2018-01-01' and '2018-12-31' 
AND rating is not null; 

```

## Summary Aggregations - summarizing data | SQL

- Aggregations: For effective decision making, it's more efficient to review summary data (like averages or counts) rather than individual records, such as the average ratings or total views of movies.
- Overview of Basic Aggregations: The average (AVG) renting price of movies is calculated to understand general rental costs.
- Common Aggregate Functions: Functions like SUM, COUNT, MIN, and MAX are introduced for data summarization.
- Handling NULLs in Aggregations: Aggregate functions ignore NULL values; for instance, COUNT reflects only non-NULL entries, affecting results when NULLs are present in data like 'year_of_birth'.
- Use of DISTINCT: The DISTINCT keyword helps in eliminating duplicate entries and focusing on unique data, such as listing all unique customer countries.
- DISTINCT with NULL Values: In aggregations, DISTINCT includes NULL as a distinct value, and during ordering, NULL is treated as the largest value.
- Alias in Column Names: Using the AS keyword to give aliases to columns in SELECT queries enhances clarity and readability of the output, like renaming columns to 'average price' and 'number of genres'.

```
-- Count the number of customers born in the 80s.
SELECT count(customer_id) -- Count the total number of customers
FROM customers
WHERE date_of_birth between '1980-01-01' and '1989-12-31'; 

-- Count the number of customers from Germany.
SELECT count(customer_id)  
FROM customers
where country ='Germany';

--Count the number of countries where MovieNow has customers.
SELECT COUNT(DISTINCT country) 
FROM customers;

-- Select all movie rentals of the movie with movie_id 25 from the table renting. For those records, calculate the minimum, maximum and average rating and count the number of ratings for this movie.

SELECT min(rating) min_rating, -- Calculate the minimum rating and use alias min_rating
	   max(rating) max_rating, -- Calculate the maximum rating and use alias max_rating
	   avg(rating) avg_rating, -- Calculate the average rating and use alias avg_rating
	   count(rating) number_ratings -- Count the number of ratings and use alias number_ratings
FROM renting
where movie_id='25'; -- Select all records of the movie with ID 25

--First, select all records of movie rentals since January 1st 2019.

SELECT * 
FROM renting
WHERE date_renting >= '2019-01-01'; 


--count the number of movie rentals and calculate the average rating since the beginning of 2019.

SELECT 
	COUNT(*), 
	AVG(rating) 
FROM renting
WHERE date_renting >= '2019-01-01';

--Use as alias column names number_renting and average_rating respectively.
SELECT 
	COUNT(*) number_renting, 
	AVG(rating) average_rating  
FROM renting
WHERE date_renting >= '2019-01-01';

--count how many ratings exist since 2019-01-01.
SELECT 
	COUNT(*) AS number_renting,
	AVG(rating) AS average_rating, 
    count(rating) AS number_ratings 
FROM renting
WHERE date_renting >= '2019-01-01';




```
