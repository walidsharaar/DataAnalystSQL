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
