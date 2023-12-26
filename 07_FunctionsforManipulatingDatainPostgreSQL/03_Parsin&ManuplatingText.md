## Reformatting string and character data | SQL
- Manipulating Date and Time Data
- Key skill in data science
  - Preparing for string and character data transformation
- Topics Covered
  - Functions & operators for reformatting string data
  - Parsing, length calculation, position determination
  - Truncating and padding strings
  - String Concatenation
    - Mergins trings using || operator
    - Real-world example: Creating 'full_name' from 'first_name' and 'last_name'
   
  ```
  -- Concatenate the first_name and last_name
  SELECT first_name || ' ' || last_name  || ' <' || email || '>' AS full_email
  FROM customer
  -- Concatenate the first_name and last_name and email
  SELECT CONCAT(first_name, ' ', last_name, ' <', email, '>') AS full_email
  FROM customer
  /*
  Do the following:
  Convert the film category name to uppercase.
  Convert the first letter of each word in the film's title to upper case.
  Concatenate the converted category name and film title separated by a colon.
  Convert the description column to lowercase.
  */
  
  SELECT 
  -- Concatenate the category name to coverted to uppercase
  -- to the film title converted to title case
  UPPER(c.name) || ': ' || INITCAP(f.title) AS film_category,
  -- Convert the description column to lowercase
  LOWER(f.description) AS description FROM 
  film AS f 
  INNER JOIN film_category AS fc 
  	ON f.film_id = fc.film_id 
  INNER JOIN category AS c 
  	ON fc.category_id = c.category_id;
  -- Replace whitespace in the film title with an underscore
  SELECT  REPLACE(title, ' ', '_') AS title FROM film; 
  ```

## Parsing string and character data | SQL
- Parsing string and character data
  - Learn string functions in PostgreSQL for parsing and manipulating text.
  - Combine and nest functions for enhanced capabilities.
- Determining string length
  - CHAR_LENGTH: Counts characters in a string; returns an integer.
  - LENGTH: Analogous to CHAR_LENGTH, interchangeable based on preference.
- Finding character position
  - POSITION: Locates a character's position from the string's start.
  - STRPOS: Similar to POSITION but with slightly different syntax.
- Parsing string data
  - LEFT: Extracts the first "n" characters from a string.
  - RIGHT: Extracts the last "n" characters from a string.
- Extracting substrings
  - SUBSTRING: Extracts a substring with source, start, and length parameters.
  - SUBSTR: Similar to SUBSTRING but with different syntax limitations.
- Advanced substring extraction
  - Combine SUBSTRING with other functions for diverse extractions.
  - Extracting text before/after the "@" sign in email addresses using POSITION and CHAR_LENGTH.

```
Select the title and description columns from the film table and find the number of characters in the description column with the alias desc_len.
SELECT 
  title,
  description,
  LENGTH(description) AS desc_len
FROM film;
-- Select the first 50 characters of the description column with the alias short_desc

SELECT 
  LEFT(description, 50) AS short_desc
FROM 
  film AS f;
-- Extract only the street address without the street number from the address column and use functions to determine the starting and ending position parameters.
SELECT SUBSTRING(address FROM POSITION(' ' IN address)+1 FOR LENGTH(address))
FROM address;

-- Extract the characters to the left of the @ of the email column in the customer table and alias it as username and then use SUBSTRING to extract the characters after the @ of the email column and alias the new derived field as domain.
SELECT
  -- Extract the characters to the left of the '@'
  LEFT(email, POSITION('@' IN email)-1) AS username,
  -- Extract the characters to the right of the '@'
  SUBSTRING(email FROM POSITION('@' IN email)+1 FOR LENGTH(email)) AS domain
FROM customer;
```

## Truncating and padding string data | SQL
- Truncating and Overwriting Strings : Demonstrates methods to truncate and replace characters in a string from.
- TRIM Function
  - Removes specified characters from the start, end, or both of a string.
  - Accepts optional parameters for removal specifications.
  - Example illustrates removing whitespace from the beginning and end of a string.
- LTRIM and RTRIM Functions
  - Analogous to TRIM but remove characters from either the beginning or end, not both.
  - Default usage truncates whitespace.
  - LTRIM removes spaces at the string's beginning.
  - RTRIM demonstrates removing characters from the end.
- Padding with Characters
  - LPAD function adds characters to a string to match a specified length.
  - Useful for uniform field length. Example pads 'padded' with hash characters to reach a length of ten.
- Padding with Whitespace
  - LPAD without the third parameter pads strings with spaces by default.
  - Truncation occurs if the specified length is less than the original string length.
  - RPAD function pads characters to the right.
