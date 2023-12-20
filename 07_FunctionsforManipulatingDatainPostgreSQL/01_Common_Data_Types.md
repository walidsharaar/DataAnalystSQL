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

