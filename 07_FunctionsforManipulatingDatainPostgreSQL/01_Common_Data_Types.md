## Course Introduction:
- Brian Piccolo, Sr. Director of Digital Strategy, leads the course on PostgreSQL functions and operators.
- Focus on expanding SQL skills through functions and operators for PostgreSQL databases.
  
### Sakila Database:
- Utilizes the Sakila Database, modeling a DVD rental store.
- Highly normalized database, ideal for practice queries and exploring PostgreSQL data types/functions.
### Course Topics:
- Review of covered topics:
- Understanding data types and their traits.
- Working with built-in functions and operators for data manipulation.
- Manipulating date, time, and text data.
- Introduction to full-text search via PostgreSQL extensions.
### Common Data Types:
- PostgreSQL's diverse native data types:
    - Text types like CHAR, VARCHAR, TEXT.
    - Numeric types such as INT, DECIMAL.
    - Date/time types like DATE, TIME, TIMESTAMP, INTERVAL, and ARRAYs.
### Text Data Types:
- CHAR, VARCHAR for fixed/varying characters; TEXT for unlimited length. Examples: categorical data (e.g., film titles), manipulation techniques (substring extraction).
### Numeric Data Types:
- Storage for integers (e.g., payment_id) and floating point numbers (e.g., payment amount). Exploration of PostgreSQL date/time and array data types in subsequent lessons.
### Determining Data Types:
- Crucial skill: determining column data types in existing databases.
- Leveraging INFORMATION_SCHEMA in PostgreSQL for querying data type information.

```

 -- Select all columns from the TABLES system database
 SELECT * 
 FROM INFORMATION_SCHEMA.TABLES
 -- Filter by schema
 WHERE table_schema = 'public';

 -- Select all columns from the COLUMNS system database
 SELECT * 
 FROM INFORMATION_SCHEMA.COLUMNS 
 -- Limit to the customer table
 WHERE table_name = 'actor';

-- Get the column name and data type
SELECT
 	column_name, 
    data_type
-- From the system database information schema
FROM INFORMATION_SCHEMA.COLUMNS 
-- For the customer table
WHERE table_name ='customer';

```

## Summary Date and time data types | SQL
- Timestamp Data Types
  - Crucial for data preparation in machine learning and data science.
  - Includes precision timestamps, intervals, date, and time types.
  - Contains date and time values with microsecond precision.
  - Often used in SQL for precise time records like payment or updates.
  - PostgreSQL follows the ISO 8601 format: YYYY-MM-DD HH:MM:SS.microseconds.
- DATE and TIME Data Types
  - DATE: Stores only the date without a time.
  - TIME: Stores only the time without a date.
  - Useful for storing partial TIMESTAMP values.
- INTERVAL Data Types
  - Stores time as a period (years, months, days, hours, seconds).
  - Helpful for performing arithmetic operations on date and time columns.
  - PostgreSQL allows storing TIMESTAMP and TIME data with or without a timezone.
 
```
-- Select the rental date and return date from the rental table. Add an INTERVAL of 3 days to the rental_date to calculate the expected return date`.
SELECT
 	-- Select the rental and return dates
	rental_date,
	return_date,
 	-- Calculate the expected_return_date
	rental_date + INTERVAL '3 days' AS expected_return_date
FROM rental;

```
## Working with ARRAYs | SQL
- Working with ARRAYs
	- PostgreSQL arrays resemble arrays in most programming languages.
 	- Arrays can be multi-dimensional and of varying lengths for any native data type in PostgreSQL.
- Before Getting Started
	- Data science often involves extracting data using SQL queries.
 	- To work with data, databases need tables with columns and inserted records. Example: CREATE TABLE for a table called my_first_table with text and integer columns.
- ARRAY as a Special Type
	- Creating ARRAY types involves adding square brackets to a data type.
 	- Illustration: Creating a table with nested email arrays and integer test scores arrays.
- INSERT Statements with ARRAYS
	- INSERT STATEMENT adds records to the created table using array representations in SQL.
- Accessing ARRAYs
	- Accessing arrays in SELECT statements involves array notation, starting indexing at 1 in PostgreSQL.
- Searching ARRAYs
	- Array notation in WHERE clause acts as a filter, searching for specific values in arrays.
- ARRAY Functions and Operators (ANY)
	- The ANY function searches arrays for values and returns matching records.
 	- ARRAY Functions and Operators (Contains Operator)
  ```
  -- Select the title and special features from the film table and compare the results between the two columns.
  SELECT 
  title, 
  special_features FROM film;
  -- Select all films that have a special feature Trailers by filtering on the first index of the special_features ARRAY.
	SELECT 
  title, 
  special_features 
	FROM film
	-- Use the array index of the special_features column
	WHERE special_features[1] = 'Trailers';
  -- Now let's select all films that have Deleted Scenes in the second index of the special_features ARRAY.
  SELECT 
  title, 
  special_features
  FROM film
  -- Use the array index of the special_features column
  WHERE special_features[2] = 'Deleted Scenes';
  --Match 'Trailers' in any index of the special_features ARRAY regardless of position.
  SELECT 
  title, 
  special_features
  FROM film
  -- Modify the query to use the ANY function
  WHERE 'Trailers' = ANY (special_features);
  --Use the contains operator to match the text Deleted Scenes in the special_features column.
    title, 
  special_features  FROM film
  -- Filter where special_features contains 'Deleted Scenes'
  WHERE special_features @> ARRAY[ 'Deleted Scenes' ];
 ```
