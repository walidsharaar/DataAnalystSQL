## Overview of basic arithmetic operators | SQL

- Overview: Manipulating PostgreSQL data types for data cleansing & transformation
- Topics: Built-in date/time functions, operators, & their applications
- Adding/subtracting date/time: Different return values based on data types used
- Date/time arithmetic precision: Integer addition to dates implies days; timestamp operations yield INTERVAL
- Calculating time periods with AGE: Determines the difference between timestamps
- Sakila database & AGE(): Highlighting dataset age through functions
- INTERVALs in arithmetic: Crucial for relative date/time calculations
- Multiplication & division with INTERVALs: Handling date/time operations with precision
```
--Subtract the rental_date from the return_date to calculate the number of days_rented
SELECT f.title, f.rental_duration,
       -- Calculate the number of days rented
       r.return_date - r.rental_date AS days_rented
FROM film AS f
     INNER JOIN inventory AS i ON f.film_id = i.film_id
     INNER JOIN rental AS r ON i.inventory_id = r.inventory_id
ORDER BY f.title;
/*
Convert rental_duration by multiplying it with a 1 day INTERVAL
Subtract the rental_date from the return_date to calculate the number of days_rented.
Exclude rentals with a NULL value for return_date.
*/

SELECT
    f.title,
 	-- Convert the rental_duration to an interval
    INTERVAL '1' day * f.rental_duration,
 	-- Calculate the days rented as we did previously
    r.return_date - r.rental_date AS days_rented
FROM film AS f
    INNER JOIN inventory AS i ON f.film_id = i.film_id
    INNER JOIN rental AS r ON i.inventory_id = r.inventory_id
-- Filter the query to exclude outstanding rentals
WHERE r.return_date IS NOT NULL
ORDER BY f.title;

/*
Convert rental_duration by multiplying it with a 1-day INTERVAL.
Add it to the rental date.
*/

SELECT
    f.title,
	r.rental_date,
    f.rental_duration,
    -- Add the rental duration to the rental date
    INTERVAL '1' day * f.rental_duration + r.rental_date AS expected_return_date,
    r.return_date
FROM film AS f
    INNER JOIN inventory AS i ON f.film_id = i.film_id
    INNER JOIN rental AS r ON i.inventory_id = r.inventory_id
ORDER BY f.title;
```
### Functions for retrieving current date/time | SQL
- Functions for retrieving current date/time: Learn to retrieve and utilize current date/time values at varying precision levels.
- Retrieving current timestamp:
	-- NOW() function fetches timestamp with microseconds and timezone.
 	-- Casting for timestamp without timezone is achievable through explicit conversion.
- Casting for data conversion:
	-- Casting allows conversion between data types.
  	-- PostgreSQL supports double colon syntax and CAST() function for type conversion.
- Alternative methods in PostgreSQL:
	-- CURRENT_TIMESTAMP mirrors NOW() function.
  	-- Either function can be used interchangeably in queries.
- Precision and function differences:
	-- CURRENT_TIMESTAMP permits specifying precision for seconds.
  	-- NOW() and CURRENT_TIMESTAMP vary in precision options.
- Current date and time functions:
	-- CURRENT_DATE returns date without time
  	-- CURRENT_TIME retrieves time with timezone but excludes date.
  These points cover the techniques and nuances in PostgreSQL for fetching current date/time values with varying levels of precision and type conversions.

```
-- Return current date and time as timestamp with timezone

SELECT CURRENT_TIMESTAMP;


```
