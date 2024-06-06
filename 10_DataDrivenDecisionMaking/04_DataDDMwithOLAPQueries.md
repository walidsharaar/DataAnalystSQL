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
