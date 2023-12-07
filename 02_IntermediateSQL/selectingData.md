## Summary Querying a database | SQL

This course on SQL focuses on querying databases, covering techniques like COUNT(), DISTINCT, and using PostgreSQL to extract insights from data.

## Facts
- The course covers foundational SQL knowledge and moves onto querying databases for actionable insights. <br/>
- PostgreSQL is the primary platform used throughout the course.<br/>
- The database used in exercises comprises four tables: films, reviews, people, and roles.<br/>
- COUNT() function allows counting records in a specified field.<br/>
- COUNT() can count multiple fields by using the function multiple times.<br/>
- Using * with COUNT() counts the total number of records in a table.<br/>
-  DISTINCT removes duplicates, helpful in extracting unique values from fields.<br/>
-  COUNT() with DISTINCT counts unique values in a field, excluding duplicates.<br/>
```
-- Count the total number of records in the people table, aliasing the result as count_records
SELECT COUNT(*) AS count_records
FROM people;

-- Count the number of birthdates in the people table
SELECT COUNT(birthdate) as count_birthdate
FROM people;
-- Count the records for languages and countries represented in the films table
SELECT COUNT(language) as count_languages,COUNT(country) as count_countries
FROM films;

-- Return the unique countries from the films table
SELECT DISTINCT country
FROM films

-- Count the distinct countries from the films table
select count(distinct country) as count_distinct_countries
from  films
```

## Summary Query execution | SQL
Understanding the execution order in SQL is crucial. Processing involves stages like selecting tables, executing selections, and refining results. Debugging errors, including comma and keyword mistakes, is a vital skill in SQL.

### Facts
- SQL processing isn't linear; it starts with defining the table, then selecting data, and finally refining results using LIMIT.<br/>
- Debugging SQL involves interpreting error messages, which can pinpoint issues like misspelled fields or keywords.<br/>
- Common errors like missing commas or misspelled keywords trigger specific error messages, aiding in locating the issue.<br/>
- Mastering debugging in SQL involves making mistakes, learning from them, and understanding various error types.<br/>
```
-- Debug the code
SELECT certification
FROM films
LIMIT 5;

-- Debug this code
SELECT film_id, imdb_score, num_votes
FROM reviews;

-- Debug this code
SELECT COUNT(birthdate) AS count_birthdays
FROM people;
```
## Summary SQL style | SQL
Understanding SQL involves more than just knowing the language—formatting, style guides, and conventions significantly impact code readability and collaboration among peers.

### Facts
- SQL allows flexible formatting without strict rules on new lines, capitalization, or indentation, although adhering to formatting standards enhances readability.
- Established SQL style standards improve code legibility, including capitalized keywords and structured new lines.
- SQL style guides, like Holywell's, provide best practices for formatting, capitalization, and naming conventions.
- Varied formatting preferences exist, but clarity and readability remain the primary goal.
- Using semicolons at the end of SQL queries, though not mandatory in all flavors, is considered a best practice for portability and clarity.
- Handling non-standard field names, like those with spaces, requires enclosing them in double-quotes for proper querying.
- Adhering to SQL style guides fosters smoother collaboration and enhances readability for better understanding and debugging.

## Summary Filtering numbers | SQL

Understanding the WHERE clause in SQL for filtering data based on specific criteria, including numbers and strings.

### Facts
- The WHERE clause allows filtering of data in SQL queries, similar to selecting coats from a closet based on specific attributes.
- It uses comparison operators (greater than, less than, equal to, etc.) for numeric values to filter data, such as movies released after a certain year.
- Various comparison operators help filter data, including 'not equal to' for excluding specific values.
- These operators can also be applied to strings, using single quotation marks for text-based filtering.
- The order of execution for SQL statements involving WHERE follows a specific sequence, impacting how the data is retrieved.
```
-- Select film_ids and imdb_score with an imdb_score over 7.0
SELECT film_id, imdb_score
from reviews
where imdb_score>7.0

-- Select film_ids and facebook_likes for ten records with less than 1000 likes 
select film_id,facebook_likes
from reviews
where facebook_likes<1000
limit 10 ;

-- Count the records with at least 100,000 votes
select count(*) as films_over_100K_votes
from 
reviews
where num_votes >= 100000

-- Count the Spanish-language films
select count(language) as count_spanish
from films
where language='Spanish'

```

## Summary Multiple criteria | SQL
SQL learning progresses from filtering single criteria to incorporating multiple criteria using keywords like OR, AND, and BETWEEN, enhancing query precision.

### Facts
- Understanding SQL filters growth in skills rapidly, moving from single criteria to multiple criteria.
- Filtering criteria can involve various attributes, such as color and length, e.g., narrowing coat choices to yellow and shorter ones.
- Learning additional keywords (OR, AND, BETWEEN) expands filtering capabilities in WHERE clauses for more nuanced queries.
- OR operator allows selection based on meeting at least one condition, like choosing green or purple coat options.
- Combining OR with WHERE enables filtering films by specified years, showing films released in 1994 or 2000.
- Using AND in filters requires meeting all specified conditions, exemplified in queries for films released between 1994 and 2000.
- Combining AND and OR allows complex queries, like filtering films by release years and certification criteria.
- BETWEEN simplifies filtering within ranges, like querying films released between 1994 and 2000 inclusively.
- Utilizing BETWEEN, AND, OR collectively enhances query complexity, enabling filtering films released in a specific range and country.

```
-- Select the title and release_year for all German-language films released before 2000
SELECT title, release_year
FROM films
WHERE release_year < 2000
AND language = 'German';

-- Update the query to see all German-language films released after 2000
SELECT title, release_year
FROM films
WHERE release_year > 2000
AND language = 'German';

-- Select all records for German-language films released after 2000 and before 2010
SELECT *
FROM films
WHERE release_year > 2000
AND release_year < 2010
AND language = 'German';

-- Find the title and year of films from the 1990 or 1999
select title,release_year
from films
where release_year = 1990 or release_year=1999;

-- Filter the records to only include English or Spanish-language films.
SELECT title, release_year
FROM films
WHERE (release_year = 1990 OR release_year = 1999)
-- Add a filter to see only English or Spanish-language films
and (language='Spanish' or language='English') ;

--Finally, restrict the query to only return films worth more than $2,000,000 gross.
SELECT title, release_year
FROM films
WHERE (release_year = 1990 OR release_year = 1999)
AND (language = 'English' OR language = 'Spanish')
-- Filter films with more than $2,000,000 gross
	and gross>2000000;

-- Select the title and release_year for films released between 1990 and 2000
select title, release_year
from films
where release_year between 1990 and 2000;

-- Build on your previous query to select only films with a budget over $100 million.
SELECT title, release_year
FROM films
WHERE release_year BETWEEN 1990 AND 2000
-- Narrow down your query to films with budgets > $100 million
and budget>100000000;

--Now, restrict the query to only return Spanish-language films.
SELECT title, release_year
FROM films
WHERE release_year BETWEEN 1990 AND 2000
	AND budget > 100000000
-- Restrict the query to only Spanish-language films
	and language='Spanish';

--Finally, amend the query to include all Spanish-language or French-language films with the same criteria.
SELECT title, release_year
FROM films
WHERE release_year BETWEEN 1990 AND 2000
	AND budget > 100000000
-- Amend the query to include Spanish or French-language films
	AND language in('Spanish','French') ;
```
