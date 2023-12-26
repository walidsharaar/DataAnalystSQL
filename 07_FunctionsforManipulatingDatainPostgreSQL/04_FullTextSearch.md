## Introduction to Full-Text Search: Explores PostgreSQL's built-in functions for data transformation in SQL.
- Topics:
  - Advanced PostgreSQL features: Custom code extensions, full-text search capabilities, improving search with extensions, and advanced combined capabilities.
- The LIKE Operator:
  - Uses wildcards in a WHERE clause for pattern matching.
  - Underscore matches one character, percent matches zero or more characters.
  - Example: Using percent wildcard to find titles starting with "ELF."
  - Alters percent wildcard position for different search results.
  - Matches specific characters in a string but is case sensitive.
- LIKE vs. Full-Text Search:
  - Demonstrates full-text search using to_tsvector and to_tsquery functions, which accommodate variations and are case insensitive.
- What is Full-Text Search?:
  - Provides natural language queries, stemming, fuzzy string matching, and result ranking by similarity.
- Full-Text Search Syntax Explained:
  - Utilizes to_tsvector and to_tsquery functions to convert text to tsvector data, enabling powerful basic search queries.

  ```
  -- Select only records that contain the word 'GOLD'
  SELECT * FROM film WHERE title LIKE '%GOLD%';

  -- Select the film description as a tsvector
  SELECT to_tsvector(description)
  FROM film;
  --Select the title and description columns from the film table and erform a full-text search on the title column for the word elf.
  -- Select the title and description
  SELECT title, description FROM film
  -- Convert the title to a tsvector and match it against the tsquery
  WHERE to_tsvector(title) @@ to_tsquery('elf');
  ```
## Extending PostgreSQL | SQL
- PostgreSQL Extension:
  - Explores built-in functions & extending capabilities.
  - Custom data types, functions, and operators enhance database functionality.
- User-defined Data Types:
  - Created via CREATE TYPE command, registering in system tables.
  - Enumerated types (enums) define fixed value sets (e.g., days of the week).
- Retrieving Data Type Info:
  - Query pg_type for data type details (typname, typcategory).
  - INFORMATION_SCHEMA.COLUMNS also provides data type insights.
- User-defined Functions:
  - Created using CREATE FUNCTION, bundling SQL queries together.
  - Example: Function 'squared' calculates the square of an integer input.
- Functions in Sakila DB:
  - Sakila DB demonstrates PostgreSQL's extensibility.
  - Sample functions: 'get_customer_balance', 'inventory_held_by_customer', 'inventory_in_stock'.
  - These functions perform various tasks like calculating balances and inventory statuses.
```
-- Create an enumerated data type, compass_position
CREATE TYPE compass_position AS ENUM (
  	-- Use the four cardinal directions
  	'North', 
  	'South',
  	'East', 
  	'West');
```
