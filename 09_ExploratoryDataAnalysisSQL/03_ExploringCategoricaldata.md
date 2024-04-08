## Summary Character data types and common issues | SQL
This part introduces character data types in PostgreSQL, distinguishing between categorical variables and unstructured text, and emphasizes the importance of organizing and scrutinizing text data to identify common issues 
such as inconsistencies in case, white space, and punctuation.
### Facts
- PostgreSQL offers three character column types: char, varchar, and text, differing in string length accommodation.
- Distinguishes between categorical variables and unstructured text for data analysis purposes.
- Utilizes GROUP BY and count to examine the distribution of categorical variables.
- Ordering by count reveals the frequency of categories and helps spot errors in rarely occurring values.
- Ordering by category value aids in identifying potential duplicates and other mistakes.
- Character data types are sorted alphabetically, considering spaces and letter cases.
- Common issues include case differences, white space variations, punctuation discrepancies, and the distinctions between an empty string, strings of all spaces, and null.

```
--How many rows does each priority level have?
SELECT priority, count(*)
from evanston311
  group by
 priority;

-- Find values of zip that appear in at least 100 rows
-- Also get the count of each value
SELECT zip, count(*)
  FROM evanston311
 GROUP BY zip
HAVING count(*)>100;

-- Find values of source that appear in at least 100 rows
-- Also get the count of each value
SELECT distinct(source), count(*)
  FROM evanston311
 group by source
having count(*)<100;

-- Find the 5 most common values of street and the count of each
SELECT street, count(*) 
  FROM evanston311
 GROUP BY street
 ORDER BY count(*) DESC
 LIMIT 5;

```


## Summary Cases and spaces | SQL
Functions like upper, lower, and trim, along with the LIKE and ILIKE operators, help handle text data inconsistencies related to character case and spaces, ensuring accurate querying and data manipulation.
### Facts
- Differences in character case and spaces are common inconsistencies in text data that can be addressed with specific functions and operators.
- The upper and lower functions convert character data to a uniform case, affecting letters but not punctuation or numbers.
- Using the lower or upper function allows for case insensitive comparisons, useful for matching specific entries like 'apple' in various case formats.
- The LIKE operator, enhanced with ILIKE for case insensitivity, matches patterns including extra spaces or characters, effectively capturing variations of a word like 'apple'.
- Caution is advised with LIKE searches as they might match unintended strings, such as 'pineapple' when searching for 'apple'.
- Trim functions (trim, ltrim, rtrim) are used to remove spaces or specified characters from the ends of a string, with the default being the space character.
- Other characters can be specified for removal with the trim functions, which are case sensitive, necessitating the inclusion of both upper and lower case versions of a character if needed.
- Combining functions, like using lower before trim, can streamline the process of cleaning data by first standardizing the case and then removing unwanted characters.

  ```
  --Trim digits 0-9, #, /, ., and spaces from the beginning and end of street. Select distinct original street value and the corrected street value and order the results by the original street value.
  SELECT distinct street,
       -- Trim off unwanted characters from street
       trim(street, '0123456789 #/.') AS cleaned_street
  FROM evanston311
  ORDER BY street;
  --Use ILIKE to count rows in evanston311 where the description contains 'trash' or 'garbage' regardless of case.
  -- Count rows
  SELECT count(*)
  FROM evanston311
  WHERE description ILIKE '%trash%' 
    OR description ILIKE '%garbage%';
  
  --Count rows where the description includes 'trash' or 'garbage' but the category does not.
 
  SELECT count(*)
  FROM evanston311
  WHERE (description ILIKE '%trash%'
    OR description ILIKE '%garbage%') 
   AND category NOT LIKE '%Trash%'
   AND category NOT LIKE '%Garbage%';
  ```
## Summary Splitting and concatenating text | SQL
Working with text in programming often involves splitting strings into parts, extracting substrings, and concatenating text elements using specific functions and operations.
### Facts
- Splitting and concatenating text operations are essential for handling and manipulating string values.
- The left and right functions extract characters from the beginning or end of a string, respectively.
- The substring function extracts characters from the middle of a string based on a starting position and length.
- Delimiters are characters or strings used to split text into parts.
- The split_part function divides a string into parts based on a delimiter and returns a specified part.
- Concatenation combines multiple text elements into a single string, with the concat function and double pipe || operator being key methods.

```
-- Concatenate house_num, a space, and street and trim spaces from the start of the result
  SELECT ltrim(concat(house_num, ' ', street)) AS address
  FROM evanston311;
-- Use split_part() to select the first word in street; alias the result as street_name and also select the count of each value of street_name.
-- Select the first word of the street value
SELECT split_part(street, ' ', 1) AS street_name, count(*)
  FROM evanston311
 GROUP BY street_name
 ORDER BY count DESC
 LIMIT 20;

-- Select the first 50 characters of description with '...' concatenated on the end where the length() of the description is greater than 50 characters. Otherwise just select the description as is. Like select only descriptions that begin with the word 'I' and not the letter 'I'. For example, you would want to select "I like using SQL!", but would not want to select "In this course we use SQL!".
SELECT CASE WHEN length(description) > 50
            THEN left(description, 50) || '...'
       ELSE description
       END
  FROM evanston311
 WHERE description LIKE 'I %'
 ORDER BY description;
```

## Summary Strategies for multiple transformations | SQL
Learned to manage data that requires multiple transformations through techniques like CASE WHEN statements and the creation of temporary recoding tables.
### Facts
- Multiple transformations can be applied to data with varied delimiters using the split_part function.
- The CASE WHEN statement allows for applying specific transformations based on row characteristics, aiding in data categorization.
- Using a temporary table to map messy values to standardized ones enables cleaner data analysis and aggregation.
- Creating a temporary table involves defining original and standardized columns, initially filled with the original values.
- Updating the temporary table's values involves cleaning and standardizing the data for accurate analysis.
- Joining the original and recoded tables refines the dataset, allowing for more meaningful grouping and summarization.

```
-- Create recode with a standardized column; use split_part() and then rtrim() to remove any remaining whitespace on the result of split_part(). Fill in the command below with the name of the temp table

DROP TABLE IF EXISTS recode;
CREATE TEMP TABLE recode AS
  SELECT DISTINCT category, 
         rtrim(split_part(category, '-', 1)) AS standardized
    FROM evanston311;
       
SELECT DISTINCT standardized 
  FROM recode
 WHERE standardized LIKE 'Trash%Cart'
    OR standardized LIKE 'Snow%Removal%';

-- UPDATE standardized values LIKE 'Trash%Cart' to 'Trash Cart' and udpate standardized values of 'Snow Removal/Concerns' and 'Snow/Ice/Hazard Removal' to 'Snow Removal'.

DROP TABLE IF EXISTS recode;

CREATE TEMP TABLE recode AS
  SELECT DISTINCT category, 
         rtrim(split_part(category, '-', 1)) AS standardized
    FROM evanston311;
  
UPDATE recode 
   SET standardized='Trash Cart' 
 WHERE standardized LIKE 'Trash%Cart';

UPDATE recode
   SET standardized='Snow Removal' 
 WHERE standardized LIKE 'Snow%Removal%';
 
-- check updates
SELECT DISTINCT standardized 
  FROM recode
 WHERE standardized LIKE 'Trash%Cart'
    OR standardized LIKE 'Snow%Removal%';

-- Create a temp table indicators from evanston311 with three columns: id, email, and phone. Use LIKE comparisons to detect the email and phone patterns that are in the description, and cast the result as an integer with CAST(). Your phone indicator should use a combination of underscores _ and dashes - to represent a standard 10-digit phone number format. Remember to start and end your patterns with % so that you can locate the pattern within other text!

DROP TABLE IF EXISTS indicators;
CREATE TEMP TABLE indicators AS
  SELECT id, 
         CAST (description LIKE '%@%' AS integer) AS email,
         CAST (description LIKE '%___-___-____%' AS integer) AS phone 
    FROM evanston311;

-- Inspect the contents
SELECT *
  FROM indicators;


-- Join the indicators table to evanston311, selecting the proportion of reports including an email or phone grouped by priority.Include adjustments to account for issues arising from integer division.


DROP TABLE IF EXISTS indicators;
CREATE TEMP TABLE indicators AS
  SELECT id, 
         CAST (description LIKE '%@%' AS integer) AS email,
         CAST (description LIKE '%___-___-____%' AS integer) AS phone 
    FROM evanston311;
SELECT priority, 
       sum(email)/count(*)::numeric AS email_prop, 
       sum(phone)/count(*)::numeric AS phone_prop 
  FROM evanston311
       LEFT JOIN indicators
       ON evanston311.id=indicators.id
 GROUP BY priority;
```
