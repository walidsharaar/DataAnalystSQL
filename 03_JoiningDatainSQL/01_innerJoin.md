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


