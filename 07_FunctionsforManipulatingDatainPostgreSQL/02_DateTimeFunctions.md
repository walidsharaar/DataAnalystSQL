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

-- Select the current timestamp
SELECT now();
-- Select the current date
SELECT current_date;
--
SELECT 
	-- Select the current date
	current_date,
    -- CAST the result of the NOW() function to a date
    cast( now() AS date )

--Select the current timestamp without timezone
SELECT CURRENT_TIMESTAMP::timestamp AS right_now;

--Now select a timestamp five days from now and alias it as five_days_from_now.
SELECT
	CURRENT_TIMESTAMP::timestamp AS right_now,
    interval '5 days' + CURRENT_TIMESTAMP AS five_days_from_now;

--Finally, let's use a second-level precision with no fractional digits for both the right_now and five_days_from_now fields.

SELECT
	CURRENT_TIMESTAMP(0)::timestamp AS right_now,
    interval '5 days' + CURRENT_TIMESTAMP(0) AS five_days_from_now;

```

## Extracting and transforming date/ time data | SQL
- Extracting and transforming date/time data:
	-- Learned about AGE function for calculating timestamp differences.
  	-- Exploring built-in functions for transforming timestamp and interval data types for data preparation.
- Manipulating timestamp data:
	-- Using EXTRACT, DATE_PART, and DATE_TRUNC functions to extract sub-fields, manipulate precision, and create new columns.
  	-- Helpful when precision in timestamps isn't necessary for analysis, enabling extraction of specific parts like year or month.
- Utilizing EXTRACT and DATE_PART functions:
	-- Parameters: field identifier and source (timestamp, time, or interval data types).
  	-- Field identifiers: year, month, quarter, day of week, etc.
  	-- Functions produce similar results, interchangeable with slight variations in parameter passing.
- Extracting sub-fields from timestamp data:
	-- DVD Rentals database example: payment_date column holds transaction timestamps.
  	-- While crucial for detailed records, aggregation for analysis or trend spotting is necessary at times.
- Aggregation using timestamp data:
	--  Identifying highest revenue by quarter: aggregate amount column using EXTRACT function for quarter and year.
	--  GROUP BY clause used for field specification in SELECT clause, grouping by quarter and year.
- Truncating timestamps with DATE_TRUNC():
	-- Truncates timestamp/interval data to specified precision.
  	-- Precision values: subset of field identifiers from EXTRACT() and DATE_PART() functions.
  	-- Returns interval or timestamp based on the precision specified.
```
SELECT 
  -- Extract day of week from rental_date
  EXTRACT(dow FROM rental_date) AS dayofweek 
FROM rental 
LIMIT 100;
--Count the total number of rentals by day of the week.
-- Extract day of week from rental_date
SELECT 
  EXTRACT(dow FROM rental_date) AS dayofweek, 
  -- Count the number of rentals
  COUNT(rental_id) as rentals 
FROM rental 
GROUP BY 1;
--count the total number of rentals by rental_day and alias it as rentals
SELECT 
  DATE_TRUNC('day', rental_date) AS rental_day,
  -- Count total number of rentals 
  COUNT(rental_id) as rentals 
FROM rental
GROUP BY 1;

/* Extract the day of the week from the rental_date column using the alias dayofweek.
Use an INTERVAL in the WHERE clause to select records for the 90 day period starting on 5/1/2005.
*/
SELECT 
  -- Extract the day of week date part from the rental_date
  EXTRACT(dow FROM rental_date) AS dayofweek,
  AGE(return_date, rental_date) AS rental_days
FROM rental AS r 
WHERE 
  -- Use an INTERVAL for the upper bound of the rental_date 
  rental_date BETWEEN CAST('2005-05-01' AS DATE)
   AND CAST('2005-05-01' AS DATE) + INTERVAL '90 day';

-- Use a CASE statement and DATE_TRUNC() to create a new column called past_due which will be TRUE if the rental_days is greater than the rental_duration otherwise, it will be FALSE.
SELECT 
  c.first_name || ' ' || c.last_name AS customer_name,
  f.title,
  r.rental_date,
  -- Extract the day of week date part from the rental_date
  EXTRACT(dow FROM r.rental_date) AS dayofweek,
  AGE(r.return_date, r.rental_date) AS rental_days,
  -- Use DATE_TRUNC to get days from the AGE function
  CASE WHEN DATE_TRUNC('day', AGE(r.return_date, r.rental_date)) > 
    f.rental_duration * INTERVAL '1' day 
  THEN TRUE 
  ELSE FALSE END AS past_due 
FROM 
  film AS f 
  INNER JOIN inventory AS i 
  	ON f.film_id = i.film_id 
  INNER JOIN rental AS r 
  	ON i.inventory_id = r.inventory_id 
  INNER JOIN customer AS c 
  	ON c.customer_id = r.customer_id 
WHERE 
  -- Use an INTERVAL for the upper bound of the rental_date 
  r.rental_date BETWEEN CAST('2005-05-01' AS DATE) 
  AND CAST('2005-05-01' AS DATE) + INTERVAL '90 day';

```
