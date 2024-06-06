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

  ```
  -- select all columns from the customer table where the number of movie rentals is smaller than 5.
  
         SELECT * FROM customers as c WHERE 5 > 
	(SELECT count(*) FROM renting as r WHERE r.customer_id=c.customer_id);
  
  --Calculate the minimum rating of customer with ID 7
  SELECT MIN(rating) FROM renting WHERE customer_id = 7;

  --Select all customers with a minimum rating smaller than 4.
  SELECT * FROM customers c WHERE 4> 
	(SELECT MIN(rating)
	FROM renting AS r
	WHERE r.customer_id = c.customer_id);
	--Select all movies with more than 5 ratings.
  SELECT *
  FROM movies AS m
  WHERE 5 < 
	(SELECT COUNT(rating)
	FROM renting AS r
	WHERE r.movie_id = m.movie_id);

  --Select all movies with an average rating higher than 8.
  SELECT * FROM movies AS m WHERE 8<
	(SELECT avg(rating)
	FROM renting AS r
	WHERE r.movie_id = m.movie_id)
  
  ```
## EXISTS vs NOT EXISTS Function
- The EXISTS function in SQL is a useful feature that checks if the result of a correlated nested query is empty or not.
- It returns a boolean value, either TRUE or FALSE, depending on whether the query has at least one row or an empty table.
- The function can be used to select movies with at least one rating from a table.
- For example, if the movie ID is 11, the query will return an empty table. If the movie ID is 1, it will be included in the result.
- If the function is not NULL, it returns TRUE, and the outer query will select the movie with no ratings.

  ```
  --Select all records of movie rentals from the customer with ID 115 and exclude records with null ratings.
  
  SELECT * FROM renting WHERE rating IS NOT NULL AND customer_id = 115;
  
  --Select all customers with at least one rating.
  SELECT * FROM customers AS c  WHERE EXISTS
	(SELECT *
	FROM renting AS r
	WHERE rating IS NOT NULL 
	AND r.customer_id = c.customer_id);
  --Create a list of all actors who play in a Comedy.
  SELECT * FROM actors AS a WHERE EXISTS
	(SELECT *
	 FROM actsin AS ai
	 LEFT JOIN movies AS m
	 ON m.movie_id = ai.movie_id
	 WHERE m.genre = 'Comedy'
	 AND ai.actor_id = a.actor_id);

  --Report the nationality and the number of actors for each nationality.

  SELECT a.nationality,   COUNT(*)  FROM actors AS a WHERE EXISTS
	(SELECT ai.actor_id
	 FROM actsin AS ai
	 LEFT JOIN movies AS m
	 ON m.movie_id = ai.movie_id
	 WHERE m.genre = 'Comedy'
	 AND ai.actor_id = a.actor_id) GROUP BY a.nationality;
  
  ```
### UNION and INTERSECT
- UNION is a set operator that joins two tables, ensuring no duplicates. An example of UNION involves two tables with elements A, B, C, and D, and two tables with elements D, E, and F.
- The UNION operator selects columns from both tables, resulting in a set of movies with either a renting price higher than 2.8 or being Action and Adventure movies.
- INTERSECT, on the other hand, is an INTERSECT of two tables, resulting in only the movie Astro Boy with a renting price higher than 2.8 and being an Action and Adventure movie.

  ```
  --Report the name, nationality and the year of birth of all actors who are not from the USA.
  SELECT name, nationality,  year_of_birth
  FROM actors
  where nationality !='USA';
  
  --Select all actors who are not from the USA and all actors who are born after 1990.
  SELECT name, 
       nationality, 
       year_of_birth FROM actors
  WHERE nationality <> 'USA'
  UNION
  SELECT name, 
       nationality, 
       year_of_birth
  FROM actors
  WHERE year_of_birth > 1990;
  
  --Select all actors who are not from the USA and who are also born after 1990.
  SELECT name, 
       nationality, 
       year_of_birth FROM actors WHERE nationality <> 'USA'
  Intersect
  SELECT name, 
       nationality, 
       year_of_birth
  FROM actors
  WHERE year_of_birth > 1990;
  
  --Select the IDs of all dramas with average rating higher than 9
  SELECT movie_id
  FROM movies
  WHERE genre = 'Drama'
  intersect
  SELECT movie_id
  FROM renting
  GROUP BY movie_id
  HAVING AVG(rating)>9;

  --Select all movies of in the drama genre with an average rating higher than 9.
  SELECT * FROM movies WHERE movie_id IN 
   (SELECT movie_id
    FROM movies
    WHERE genre = 'Drama'
    INTERSECT
    SELECT movie_id
    FROM renting
    GROUP BY movie_id
    HAVING AVG(rating)>9);
  
  
  ```
  
