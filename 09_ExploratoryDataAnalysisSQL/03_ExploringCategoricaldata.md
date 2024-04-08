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
