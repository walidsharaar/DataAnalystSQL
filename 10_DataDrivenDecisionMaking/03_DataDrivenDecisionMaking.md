1. **Nested Queries**:
   - A nested query is a query where a complete SELECT block appears in the WHERE or HAVING clause of another query.
   - The inner block (subquery) is executed first by the query optimizer and can have single or multiple values.
   - These values are used by the outer query.

2. **Example 1: Disappointed Customers**:
   - Inner Query: Select distinct customer IDs who rated movies with a score of 3 or lower.
   - Outer Query: Retrieve names of customers whose IDs are in the result of the inner query.

3. **Example 2: Earliest Account Creation Date**:
   - Inner Query: Find the earliest account creation date for a customer from Austria (single value).
   - Outer Query: Group customer data by country, listing country names and earliest account creation dates. Use HAVING to report countries where the first account date is earlier than Austria's.

4. **Example 3: Actors in the Movie 'Ray'**:
   - Nested WHERE Clauses: Select movie ID for 'Ray' (single value).
   - Next Level: Select actor IDs for actors in the movie with the ID from the previous step.
   - Outer Query: Report actor names whose IDs are in the result of the inner query.

```
-- Select all movie IDs which have more than 5 views.
select movie_id 
from renting
group by movie_id
having count(*) > 5

--Select all information about movies with more than 5 views.
SELECT *
FROM movies
where  movie_id in
	(SELECT movie_id
	FROM renting
	GROUP BY movie_id
	HAVING COUNT(*) > 5)
--List all customer information for customers who rented more than 10 movies.
SELECT *
FROM customers
WHERE customer_id IN
	(SELECT customer_id
	FROM renting
	GROUP BY customer_id
	HAVING COUNT(*) > 10);

--Select movie IDs and calculate the average rating of movies with rating above average.

SELECT movie_id,  
       AVG(rating)
FROM renting
GROUP BY movie_id
HAVING AVG(rating) >   
	(SELECT AVG(rating)
	FROM renting);


--The advertising team only wants a list of movie titles. Report the movie titles of all movies with average rating higher than the total average

SELECT title 
FROM movies
WHERE movie_id IN
	(SELECT movie_id
	 FROM renting
     GROUP BY movie_id
     HAVING AVG(rating) > 
		(SELECT AVG(rating)
		 FROM renting));

```

## Correlated nested queries 
- are a type of query where the inner block's nested subquery is evaluated before processing the outer select block. 
- In a correlated query, the condition in the WHERE clause references a column in the outer query. 
- The nested query is then evaluated once for each row in the outer query. For example, to determine which movies were rented more than 5 times, the inner query evaluates for'm-dot-movie_id' equal to one, resulting in 8 movie rentals.
- The outer query checks if 5 is smaller than the inner query's value, resulting in the movie 'One Night at McCools's'. Correlated queries implement a looping mechanism, looping through the subquery for each row in the table.
  
