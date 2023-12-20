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
