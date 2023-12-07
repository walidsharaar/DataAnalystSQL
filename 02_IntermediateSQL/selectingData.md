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

## Summary Filtering text | SQL
SQL filtering involves using LIKE, NOT LIKE, and IN operators to search for text patterns or specific values in fields, allowing versatile queries.

### Facts
-  We shift focus from filtering numbers to filtering text data.
- WHERE clause is used to filter text; initially, specific text strings are targeted.
- LIKE, NOT LIKE, and IN keywords are introduced for pattern-based text filtering.
- LIKE operator with wildcards (%) matches various characters, while underscore (_) matches a single character.
- NOT LIKE operator filters records not matching the specified pattern (case-sensitive).
- Wildcards can be positioned anywhere and combined for diverse search criteria.
- WHERE clause with OR conditions can get complex; IN operator simplifies by specifying multiple values.
- IN operator quickly filters based on multiple conditions, as shown in filtering by release years.
- Text field filtering example: WHERE country is Germany or France.
  
```
-- Select the names that start with B
select name
from people
where name like 'B%'

-- Select the names of people whose names have 'r' as the second letter.
SELECT name
FROM people
-- Select the names that have r as the second letter
where name like '_r%'

--Select the names of people whose names don't start with 'A'.
SELECT name
FROM people
-- Select names that don't start with A
where name not like 'A%'

-- Find the title and release_year for all films over two hours in length released in 1990 and 2000
SELECT title, release_year
FROM films
WHERE release_year IN (1990, 2000) AND duration > 120;

-- Find the title and language of all films in English, Spanish, and French
SELECT title, language
FROM films WHERE language IN ('English', 'Spanish', 'French');

-- Find the title, certification, and language all films certified NC-17 or R that are in English, Italian, or Greek
SELECT title, certification, language
FROM films
WHERE certification IN ('NC-17', 'R') AND language IN ('English', 'Italian', 'Greek');

-- Count the unique titles
SELECT COUNT(DISTINCT title) AS nineties_english_films_for_teens
FROM films
-- Filter to release_years to between 1990 and 1999
WHERE release_year BETWEEN 1990 AND 1999
-- Filter to English-language films
AND language = 'English'
-- Narrow it down to G, PG, and PG-13 certifications
AND certification IN ('G', 'PG', 'PG-13');

```
## Summary NULL values | SQL
This lesson delves into handling NULL values in SQL, emphasizing their prevalence in real-world databases and the importance of filtering them for accurate analysis using operators like IS NULL and IS NOT NULL.

### Facts
- NULL in SQL represents missing or unknown values, often found in databases due to human error or unavailable information.
- Misinterpreting data due to NULL values, such as assuming available information when it's missing, can lead to inaccurate analysis.
- Using IS NULL in the WHERE clause helps identify missing data, while IS NOT NULL filters out NULL values for specific queries.
- COUNT() and COUNT() with IS NOT NULL yield the same results as they both count non-missing values.
- Knowing how to handle NULL values is crucial for accurate data analysis, as they're widespread in real-world databases. Operators like IS NULL and IS NOT NULL aid in selecting, excluding, or identifying missing values.
```
-- List all film titles with missing budgets
SELECT title AS no_budget_info
FROM films
WHERE budget IS NULL;

-- Count the number of films we have language data for
select count(title) as count_language_known
from films
where language  is not null;

```

## Summary Summarizing data | SQL
Aggregate functions in SQL summarize data using calculations like average, sum, minimum, and maximum for specific fields, enabling a comprehensive view of the dataset.

### Facts
- Aggregate functions in SQL condense multiple values into a single result, providing insights into datasets beyond individual records.
- Functions like COUNT(), AVG(), SUM(), MIN(), and MAX() operate on fields, not rows, showcasing aspects like average budget, total sum, lowest, and highest values within a dataset.
- While some functions like AVG() and SUM() work solely on numerical fields, COUNT(), MIN(), and MAX() function with both numerical and non-numerical data.
- MIN() and MAX() applied to non-numerical data showcase examples like finding the earliest country alphabetically or the last one in a database.
- Using aliases when summarizing data in SQL is considered a best practice, ensuring clarity and readability of query results.
```
-- Query the sum of film durations
select sum(duration) as total_duration
from films;
-- Calculate the average duration of all films
select avg(duration) as average_duration
from films;
-- Find the latest release_year
select max(release_year) as latest_year
from films;
-- Find the duration of the shortest film
select min(duration) as shortest_film
from films;

```

## Summary Summarizing subsets | SQL

Learn how to merge filtering and summarizing skills in SQL queries by combining WHERE clauses with aggregate functions. Use ROUND() to refine numerical outputs, specifying decimal places or rounding to whole numbers.

### Facts
- Combining WHERE with aggregate functions executes filtering before summarizing.
-  Using SUM, MIN, MAX, and COUNT functions provides insights into specific data subsets.
- Examples showcase finding total, smallest, largest budgets, and counting non-missing values.
- ROUND() cleans decimals; its parameters determine precision or rounding to whole numbers.
- Optional parameters in ROUND() allow rounding to whole numbers (default) or using negative values to round to the left of the decimal.

```
-- Calculate the sum of gross from the year 2000 or later
select sum(gross) as total_gross
from films
where release_year>=2000

-- Calculate the average gross of films that start with A
select avg(gross) as avg_gross_A
from films
where title like 'A%'

-- Calculate the lowest gross film in 1994
SELECT MIN(gross) AS lowest_gross
FROM films
WHERE release_year = 1994;

-- Calculate the highest gross film released between 2000-2012
SELECT MAX(gross) AS highest_gross
FROM films
WHERE release_year BETWEEN 2000 AND 2012;

-- Round the average number of facebook_likes to one decimal place
select round(avg(facebook_likes),1) as avg_facebook_likes
from reviews

-- Calculate the average budget rounded to the thousands
select round(avg(budget),-3) as avg_budget_thousands
from films

```

## Summary Aliasing and arithmetic | SQL
SQL lessons cover arithmetic operations, highlighting how to use parentheses for clarity, manage division precision, and the importance of aliasing in queries.

### Facts
- Basic arithmetic operations in SQL involve symbols for addition, subtraction, multiplication, and division, using parentheses for clarity.
- SQL defaults to integers when dividing integers, requiring decimal points for precision.
- Aggregate functions operate vertically on fields, while arithmetic performs calculations horizontally on records.
- Aliasing becomes essential when using arithmetic operations or aggregate functions, as they generate unnamed fields in queries.
- Clear field naming, especially with multiple function uses in a query, demands proper aliasing to avoid confusion.
- SQL execution order dictates that an alias defined in the SELECT clause can't be used in the WHERE clause due to processing sequence.

```
-- Calculate the title and duration_hours from films
SELECT title, duration / 60.0 AS duration_hours
FROM films;

-- Calculate the percentage of people who are no longer alive
SELECT COUNT(deathdate) * 100.0 / COUNT(*) AS percentage_dead
FROM people;

-- Find the number of decades in the films table
SELECT (MAX(release_year) - MIN(release_year)) / 10.0 AS number_of_decades
FROM films;

-- Round duration_hours to two decimal places
SELECT title, round((duration / 60.0),2) AS duration_hours
FROM films;

```

## Summary Sorting results | SQL
Learned about sorting data in SQL using ORDER BY. It organizes results in ascending or descending order based on specified fields, enabling clearer data interpretation.

### Facts
- Sorting results aids in organizing data for easier comprehension, similar to arranging a messy closet by specific attributes for quick access.
- ORDER BY in SQL arranges data either in ascending order (default) or descending order when specified, applied after the FROM statement.
- Adding ASC clarifies sorting in ascending order, while DESC sorts data in descending order.
- ORDER BY can sort by single or multiple fields, using commas to separate and prioritize sorting criteria.
- Different sorting orders can be applied to each field, granting flexibility in sorting multiple attributes.
- ORDER BY executes towards the end in the sequence of SQL commands, after SELECT and before LIMIT.

```
-- Select name from people and sort alphabetically
select name 
from people
order by name asc

-- Select the title and duration from longest to shortest film
select title, duration 
from films 
order by duration desc

-- Select the release year, duration, and title sorted by release year and duration
select release_year, duration,title
from films
order by release_year

-- Select the certification, release year, and title sorted by certification and release year
select release_year,title, certification
from films
order by release_year asc

```

## Summary Grouping data | SQL

Learned about sorting data and moved on to grouping results based on different fields using GROUP BY in SQL queries. Error handling when selecting fields not in the GROUP BY clause was highlighted, and the order of execution for GROUP BY in queries was explained.

### Facts
- Learned to sort data before diving into grouping results.
- Grouping data in SQL involves summarizing data for specific groups.
- GROUP BY clause in SQL is used with aggregate functions for summarizing statistics.
- Selecting a field not in the GROUP BY clause results in an error; it can be resolved by adding an aggregate function.
- GROUP BY can be applied to multiple fields, affecting how data is grouped.
- Combining GROUP BY with ORDER BY allows for grouping, calculation, and result ordering.
- Order of execution for GROUP BY in queries: after FROM and before other clauses.

```
-- Find the release_year and film_count of each year
select release_year, count(title) as film_count
from films
group by release_year

-- Find the release_year and average duration of films for each year
select release_year, avg(duration) as avg_duration
from films
group by release_year

-- Find the release_year, country, and max_budget, then group and order by release_year and country
select release_year,country, max(budget) as max_budget
from films
group by release_year, country

```

## Summary Filtering grouped data | SQL
Understanding SQL's HAVING clause involves the distinction between WHERE and HAVING in filtering grouped data, determined by the order of execution and the type of filtering needed for aggregate functions.

### Facts
- In SQL, HAVING filters grouped records, whereas WHERE filters individual records.
- The order of execution in SQL queries is: FROM, WHERE, GROUP BY, HAVING, SELECT, ORDER BY, and LIMIT.
- WHERE is executed before GROUP BY, impacting the use of aliases and aggregate functions.
- HAVING is crucial when filtering based on aggregated data, such as average film duration per year.
- Understanding business questions helps determine whether to use WHERE or HAVING in SQL queries, depending on the need for grouping and aggregation.

```
-- Select the country and distinct count of certification as certification_count
SELECT country, COUNT(DISTINCT certification) AS certification_count
FROM films
-- Group by country
GROUP BY country
-- Filter results to countries with more than 10 different certifications
HAVING COUNT(DISTINCT certification) > 10;

-- Select the country and average_budget from films
select country , round(avg(budget),2) as average_budget
from films
-- Group by country
group by country
-- Filter to countries with an average_budget of more than one billion
having (avg(budget))>1000000000
-- Order by descending order of the aggregated budget
order by (avg(budget)) desc

-- Select the release_year for films released after 1990 grouped by year
SELECT release_year
FROM films
WHERE release_year > 1990
GROUP BY release_year;

SELECT release_year, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
FROM films
WHERE release_year > 1990
GROUP BY release_year
-- Modify the query to see only years with an avg_budget of more than 60 million
having avg(budget) > 60000000;

--Finally, order the results from the highest average gross and limit to one.
SELECT release_year, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
FROM films
WHERE release_year > 1990
GROUP BY release_year
HAVING AVG(budget) > 60000000
-- Order the results from highest to lowest average gross and limit to one
order by avg_gross desc
limit 1;
```

## Summary Congratulations! | SQL
Congratulations on completing the course! I've learned various SQL keywords and techniques, becoming proficient in querying, filtering, sorting, and summarizing data.

### Facts
- Expanded SQL vocabulary with COUNT, LIMIT, IS NULL, IS NOT NULL
- Covered filtering, comparison operators, and arithmetic
- Explored ROUND, aggregate functions, ORDER BY, DESC, GROUP BY, HAVING
- Acquired skills in error handling, debugging, and writing clean SQL code
Skills
- Proficient in selecting, querying, filtering, summarizing, sorting, and grouping data
What's next?
- Ready for the joining course or explore practice options on DataCamp

![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/58287e83-1d0b-457d-9945-df8f3977d782)
*Statement of Accomplishment*
