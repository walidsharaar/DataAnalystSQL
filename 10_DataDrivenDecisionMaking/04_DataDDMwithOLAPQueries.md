### OLAP Cube Operator
- on-line analytical processing (OLAP) as a key technique for gaining insight into business data.
- OLAP allows for interactive analysis and visualization of data, often aggregating data for better understanding.
- The chapter discusses the CUBE, ROLLUP, and GROUPING SETS operators, which are used to facilitate data aggregation.
- Pivot tables are a popular tool for representing different levels of aggregation for business reports.
- A pivot table is used to demonstrate the aggregation levels of data, with a specific table for rentings_extended.
- The CUBE operator is used to obtain all these aggregation levels with a simple query.

```
--Create a table with the total number of customers, of all female and male customers, of the number of customers for each country and the number of men and women from each country.
SELECT gender,   country, COUNT(*)
FROM customers
GROUP BY CUBE (gender, country)
ORDER BY country;

--List the number of movies for different genres and the year of release on all aggregation levels by using the CUBE operator.
SELECT year_of_release,
       genre,
	   COUNT(*)
FROM movies
GROUP BY CUBE (year_of_release, genre)
ORDER BY year_of_release;

--Augment the records of movie rentals with information about movies and customers
SELECT *
FROM renting AS r
LEFT JOIN movies AS m
ON m.movie_id = r.movie_id
LEFT JOIN customers AS c
ON r.customer_id = c.customer_id;

--Calculate the average rating for each country.
SELECT 
	c.country, 
	AVG(r.rating)
FROM renting AS r
LEFT JOIN movies AS m
ON m.movie_id = r.movie_id
LEFT JOIN customers AS c
ON r.customer_id = c.customer_id
GROUP BY c.country;

--Calculate the average rating for all aggregation levels of country and genre.
SELECT 
	c.country, 
	m.genre, 
	AVG(r.rating) AS avg_rating 
FROM renting AS r
LEFT JOIN movies AS m
ON m.movie_id = r.movie_id
LEFT JOIN customers AS c
ON r.customer_id = c.customer_id
GROUP BY CUBE (c.country, m.genre);


```

### The ROLLUP operator
- is a tool used to execute OLAP queries, allowing for different levels of aggregation.
- It is used in conjunction with a GROUP BY statement, resulting in a table with different levels of aggregation.
- The order of column names between parentheses is crucial, with the first column aggregated on two levels and the second on only one level.
- The ROLLUP operator is used to present data on different levels of detail, dropping a level of detail at each step.
- The ROLLUP clause can include multiple aggregations, such as movie rentals and ratings, to display the corresponding numbers in columns n_rentals and n_ratings.
```
--Generate a table with the total number of customers, the number of customers for each country, and the number of female and male customers for each country and order the result by country and gender

SELECT country,
       gender,
	   COUNT(*)
FROM customers
GROUP BY ROLLUP (country, gender)
ORDER BY country, gender;

--Calculate the average ratings and the number of ratings for each country and each genre. Include the columns country and genre in the SELECT clause.
SELECT 
	c.country, 
	m.genre, 
    AVG(r.rating), 
	COUNT(*)
FROM renting AS r
LEFT JOIN movies AS m
ON m.movie_id = r.movie_id
LEFT JOIN customers AS c
ON r.customer_id = c.customer_id
GROUP BY c.country, m.genre 
ORDER BY c.country, m.genre;

```

### Grouping sets
- The GROUPING SETS operator is a flexible addition to OLAP, allowing for the extraction of different levels of aggregation for the number of movie rentals.
- The GROUPING SETS operator can be seen as a UNION over GROUP BY statements, where each group represents one GROUP BY statement.
- The GROUPING SETS operator can be used to count the number of movie rentals for each combination of country and genre, group by country, group by genre, and total aggregation.
- The resulting table is a combination of all four queries, equivalent to a GROUP BY CUBE query of country and genre.
- The results of the GROUPING SETS query are presented in a pivot table, with the first row representing the total aggregation corresponding to the empty parentheses.

```
--Count the number of actors in the table actors from each country, the number of male and female actors and the total number of actors.

SELECT 
	nationality,
    gender, 
    COUNT(*) 
FROM actors
GROUP BY GROUPING SETS ((nationality), (gender), ());

-- Select records of movies with at least 4 ratings, starting from 2018-04-01.
SELECT *
FROM renting AS r
LEFT JOIN movies AS m
ON m.movie_id = r.movie_id
WHERE r.movie_id IN ( SELECT movie_id FROM renting GROUP BY movie_id HAVING COUNT(rating) >= 4) AND r.date_renting >= '2018-04-01'; 

--For each combination of the actors' nationality and gender, calculate the average rating, the number of ratings, the number of movie rentals, and the number of actors.
SELECT a.nationality,
       a.gender,
	   AVG(r.rating) AS avg_rating, 
	   COUNT(r.rating) AS n_rating, 
	   COUNT(*) AS n_rentals, 
	   COUNT(DISTINCT a.actor_id) AS n_actors 
FROM renting AS r
LEFT JOIN actsin AS ai
ON ai.movie_id = r.movie_id
LEFT JOIN actors AS a
ON ai.actor_id = a.actor_id
WHERE r.movie_id IN ( 
	SELECT movie_id
	FROM renting
	GROUP BY movie_id
	HAVING COUNT(rating) >=4 )
AND r.date_renting >= '2018-04-01'
GROUP BY a.nationality, a.gender;

--Provide results for all aggregation levels represented in a pivot table.

SELECT a.nationality,
       a.gender,
	   AVG(r.rating) AS avg_rating,
	   COUNT(r.rating) AS n_rating,
	   COUNT(*) AS n_rentals,
	   COUNT(DISTINCT a.actor_id) AS n_actors
FROM renting AS r
LEFT JOIN actsin AS ai
ON ai.movie_id = r.movie_id
LEFT JOIN actors AS a
ON ai.actor_id = a.actor_id
WHERE r.movie_id IN ( 
	SELECT movie_id
	FROM renting
	GROUP BY movie_id
	HAVING COUNT(rating) >= 4)
AND r.date_renting >= '2018-04-01'
GROUP BY CUBE (a.nationality, a.gender); 
```
