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
```