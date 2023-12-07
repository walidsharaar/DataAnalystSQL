## Summary The ins and outs of INNER JOIN | SQL
Understanding INNER JOIN in SQL involves connecting tables based on shared values in specific fields, producing meaningful results by combining data sets.

### Facts
- INNER JOIN, along with LEFT JOIN, is among the most common types of joins in SQL.
- Key fields in tables uniquely identify records, aiding in data visualization after joining.
- INNER JOIN matches records based on identical values in specified fields.
- Records matching the specified field are retained, while non-matching records fade out.
- The outcome typically includes the matching records from involved tables.
- A world leaders' database schema encompasses presidents, prime ministers, monarchs, states, and prime minister terms.
- Utilizing SQL queries, tables like presidents and prime ministers can be accessed and analyzed.
- SQL joins aid in identifying matching records across tables, such as countries with both presidents and prime ministers.
- Using aliases and shortcuts like AS and USING can streamline and simplify SQL queries for joins.
```
-- Select all columns from cities
select * 
from cities
-- Perform an inner join with the cities table on the left and the countries table on the right; do not alias tables here
SELECT * 
FROM cities 
-- Inner join to countries
inner join countries 
-- Match on country codes
on cities.country_code= countries.code

-- Select name fields (with alias) and region 
SELECT cities.name AS city, countries.name AS country, region
FROM cities
INNER JOIN countries
ON cities.country_code = countries.code;

-- Select fields with aliases
SELECT c.code AS country_code, name, year, inflation_rate
FROM countries AS c
-- Join to economies (alias e)
INNER JOIN economies AS e
-- Match on code field using table aliases
ON c.code = e.code;

```
#### USING in action | SQL
Using the USING clause simplifies joins when joining tables with common field names, enhancing query efficiency.
- USING simplifies joins by referencing a shared column in both tables.
- It's beneficial when joining tables with identical field names.
- Specifically helpful in simplifying the exploration of the languages table for official and unofficial languages.
```
-- Use the country code field to complete the INNER JOIN with USING; do not change any alias names.
SELECT c.name AS country, l.name AS language, official
FROM countries AS c
INNER JOIN languages AS l
-- Match using the code column
USING(code);

```

## Summary Defining relationships | SQL
Understanding SQL relationships involves recognizing one-to-many, one-to-one, and many-to-many connections between tables, each defined by distinct associations.

### Facts
- One-to-many relationships : One entity links to multiple entities, like artists and their songs, authors and books, etc.
- One-to-one relationships: Unique pairings between entities, such as one fingerprint per person at border control.
- Many-to-many relationships: Complex associations like languages and countries, where multiple languages can belong to multiple countries, like Dutch in the Netherlands and Belgium.

```
-- Select country and language names, aliased
select c.name as country , l.name as language
-- From countries (aliased)
from countries c
-- Join to languages (aliased)
inner join languages l
-- Use code as the joining field with the USING keyword
using(code);

-- Rearrange SELECT statement, keeping aliases
SELECT c.name AS country, l.name AS language
FROM countries AS c
INNER JOIN languages AS l
USING(code)
-- Order the results by language
order by language 


```

## Summary Multiple joins | SQL
Understanding the power of SQL joins gets a boost as we delve into multiple and chained joins, exploring syntax intricacies and applying them to real-world scenarios.

### Facts
- Multiple joins in a single query streamline complex data retrieval.
- Chained joins build on the result of previous joins, refining data selection.
- Applying joins to real-world scenarios like identifying countries with specific leadership structures enhances practical SQL skills.
- Chaining multiple INNER JOINs allows us to link several tables together.
- Utilizing additional fields in the ON clause refines result sets, narrowing down matches.
```
--Perform an inner join of countries AS c (left) with populations AS p (right), on code. Select name, year and fertility_rate.
select name,year,fertility_rate
from countries c
-- Inner join countries and populations, aliased, on code
inner join populations p
on c.code = p.country_code
-- Chain another inner join to your query with the economies table AS e, using code.Select name, and using table aliases, select year and unemployment_rate from economies.
-- Select fields
SELECT name, e.year, fertility_rate,e.unemployment_rate
FROM countries AS c
INNER JOIN populations AS p
ON c.code = p.country_code
-- Join to economies (as e)
inner join economies e
-- Match on country code
using(code);

SELECT name, e.year, fertility_rate, unemployment_rate
FROM countries AS c
INNER JOIN populations AS p
ON c.code = p.country_code
INNER JOIN economies AS e
ON c.code = e.code
-- Add an additional joining condition such that you are also joining on year
AND e.year = p.year;
```
