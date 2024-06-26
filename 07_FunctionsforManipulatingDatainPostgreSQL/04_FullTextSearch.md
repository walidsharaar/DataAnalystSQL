## Introduction to Full-Text Search: Explores PostgreSQL's built-in functions for data transformation in SQL.

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
-- Verify that the new data type has been created by looking in the pg_type system table.
-- Create an enumerated data type, compass_position
CREATE TYPE compass_position AS ENUM (
  	-- Use the four cardinal directions
  	'North', 
  	'South',
  	'East', 
  	'West'
);
-- Confirm the new data type is in the pg_type system table
SELECT *
FROM pg_type
WHERE typname='compass_position';

/*
Select the column_name, data_type, udt_name.
Filter for the rating column in the film table.
*/
-- Select the column name, data type and udt name columns
SELECT column_name, data_type, udt_name
FROM INFORMATION_SCHEMA.COLUMNS 
-- Filter by the rating column in the film table
WHERE table_name ='film' AND column_name='rating';

-- Select all columns from the pg_type table where the type name is equal to mpaa_rating.
SELECT *
FROM pg_type 
WHERE typname='mpaa_rating'
```

## Full Text Search Enhancements:
- Extensions Overview:** PostgreSQL extensions like fuzzystrmatch and pg_trgm enhance full text search capabilities.
- Common Extensions:** PostGIS, PostPic, fuzzystrmatch, and pg_trgm cater to specific functionalities within PostgreSQL.
- Querying Extensions:
  - Discovering Extensions: Use `pg_available_extensions` to find available extensions and `pg_extension` to see enabled ones.
  - Listing Enabled Extensions: Example showcases the presence of 'plpgsql' as an enabled extension.

- Loading Extensions:
  - Enabling Extensions: Use `CREATE EXTENSION` to load and enable extensions, ensuring they're not duplicated with `IF NOT EXISTS`.
  - Confirmation: After enabling, verify the addition in `pg_extension` to see the updated list.

- Fuzzy Search Features:
  - fuzzystrmatch: Utilize functions like 'levenshtein' to gauge string similarity by calculating edit distances.
  - Example Usage: Shows the distance between 'GUMBO' and 'GAMBOL' as 2, indicating the required changes.

- String Comparison with pg_trgm:
  - Trigram Matching: pg_trgm uses trigrams to determine string similarity through the 'similarity' function.
  - Result Interpretation: 'GUMBO' and 'GAMBOL' have a similarity value of 0.181818, showcasing their level of likeness.

```
-- Enable the pg_trgm extension
CREATE EXTENSION IF NOT EXISTS pg_trgm;

-- Select all rows extensions
SELECT * 
FROM pg_extension;

/*
Select the film title and description and calculate the similarity between the title and description.
*/
-- Select the title and description columns
SELECT 
  title, 
  description, 
  -- Calculate the similarity
  similarity(title, description)
FROM 
  film

/*
Select the film title and film description and calculate the levenshtein distance for the film title with the string JET NEIGHBOR.
*/
-- Select the title and description columns
SELECT  
  title, 
  description, 
  -- Calculate the levenshtein distance
  levenshtein(title, 'JET NEIGHBOR') AS distance
FROM 
  film
ORDER BY 3

/*
Select the title and description for all DVDs from the film table and perform a full-text search by
converting the description to a tsvector and match it to the phrase 'Astounding & Drama' using a tsquery in the WHERE clause.
*/
-- Select the title and description columns
SELECT  
  title, 
  description 
FROM 
  film 
WHERE 
  -- Match "Astounding Drama" in the description
  to_tsvector(description) @@ 
  to_tsquery('Astounding & Drama');
/*
Add a new column that calculates the similarity of the description with the phrase 'Astounding Drama' and sort the results by the new similarity column in descending order.

*/
SELECT 
  title, 
  description, 
  -- Calculate the similarity
  similarity(description, 'Astounding Drama') 
FROM 
  film 
WHERE 
  to_tsvector(description) @@ 
  to_tsquery('Astounding & Drama') 
ORDER BY 
	similarity(description, 'Astounding Drama') DESC;

```

![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/c935851a-6d8f-4b9d-b911-65c81f334bf9)
