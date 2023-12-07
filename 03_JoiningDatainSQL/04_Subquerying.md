## Summary Subquerying with semi joins and anti joins | SQL
Subquerying and anti-joins introduce advanced SQL techniques beyond traditional JOIN operations. Semi joins filter data using the WHERE clause based on a condition, while anti joins select records where no match is found between columns.

### Facts
- The six familiar joins in SQL are "additive," adding columns to the original left table via INNER JOIN operations.
- INNER JOIN syntax adds fields with different names and duplicates fields with identical names, solvable through aliasing.
- Semi joins differ from traditional joins by leveraging the WHERE clause to filter records without using explicit JOIN keywords.
- A semi join selects values in the first table based on a condition met in the second table.
- Leveraging WHERE clauses and subqueries is fundamental in performing semi joins, allowing for nuanced data filtering.
- Anti joins, in contrast, choose records in the first table where no matching condition is found in the second table.
- Illustrative examples demonstrate how to modify semi joins into anti joins for refined data filtering, such as selecting countries in the Americas founded after 1800.

![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/d7c3e15a-0e0b-4fd1-b02c-b27582ad76c2)
*Addative Join*
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/411b31cf-1904-4f99-9613-1e7dac7efa7c)
*Semi Join*

![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/5eeec05d-c665-4fa9-b021-a11d69050092)

*Anti Join*

```
-- Select country code for countries in the Middle East
SELECT code
from countries
where region = 'Middle East'

--Write a second query to SELECT the name of each unique language appearing in the languages table; do not use column aliases here.
-- Select unique language names
select distinct name
FROM languages
-- Order by the name of the language
order by name;

-- Create a semi join out of the two queries you've written, which filters unique languages returned in the first query for only those languages spoken in the 'Middle East'.
SELECT DISTINCT name
FROM languages
-- Add syntax to use bracketed subquery below as a filter
where code in
    (SELECT code
    FROM countries
    WHERE region = 'Middle East')
ORDER BY name;

-- build on your query to complete your anti join, by adding an additional filter to return every country code that is not included in the currencies table.
SELECT code, name
FROM countries
WHERE continent = 'Oceania'
-- Filter for countries not included in the bracketed subquery
 and code not in
    (SELECT code
    FROM currencies);
```

## Summary Subqueries inside WHERE and SELECT | SQL
This lesson delves into the use of subqueries within WHERE and SELECT statements in SQL, demonstrating their syntax, nuances, and application in filtering and counting data.

### Facts
- Subqueries in WHERE clauses are common for filtering data; the lesson revisits syntax and nuances, including using the WHERE IN clause.
- IN operator can accept subqueries, provided the result matches the data type of the field being filtered.
- Subqueries in WHERE clauses can involve fields from the same or different tables, impacting their compatibility.
- Subqueries in SELECT clauses help count and filter data across different tables, requiring careful setup with aliases and WHERE conditions.
- Using a subquery within a SELECT statement allows aggregation and filtering across distinct elements, facilitating complex data manipulation and counting.

```
-- Select average life_expectancy from the populations table
select avg(life_expectancy)
-- Filter for the year 2015
from populations
where year=2015

SELECT *
FROM populations
-- Filter for only those populations where life expectancy is 1.15 times higher than average
WHERE life_expectancy > 1.15 *
  (SELECT AVG(life_expectancy)
   FROM populations
   WHERE year = 2015) 
    AND year = 2015;

-- Return the name, country_code and urbanarea_pop for all capital cities (not aliased).
-- Select relevant fields from cities table
SELECT name, country_code, urbanarea_pop
FROM cities
-- Filter using a subquery on the countries table
WHERE name IN
  (SELECT capital
   FROM countries)
ORDER BY urbanarea_pop DESC;

-- Find top nine countries with the most cities
SELECT countries.name AS country, COUNT(*) AS cities_num
FROM countries
LEFT JOIN cities
ON countries.code = cities.country_code
GROUP BY country
-- Order by count of cities as cities_num
ORDER BY cities_num DESC, country
LIMIT 9;
SELECT countries.name AS country,
-- Subquery that provides the count of cities   
  (SELECT COUNT(*)
   FROM cities
   WHERE cities.country_code = countries.code) AS cities_num
FROM countries
ORDER BY cities_num DESC, country
LIMIT 9;
```

## Summary Subqueries inside FROM | SQL
Subqueries within a FROM clause allow for complex filtering and data manipulation in SQL queries, enabling the creation of temporary tables to extract specific information.

### Facts
- Subqueries within FROM: Utilized to filter continents with monarchs and extract the most recent independence year for each continent.
- Combining tables: Multiple tables can be included using commas in the FROM clause, addressing duplicates with the DISTINCT command.
- Nested subqueries: Integration of initial subqueries within a FROM statement, using aliases and WHERE clauses for matching records.
- Finalizing the query: Employing DISTINCT to remove duplicate results and ordering the output by continent to display continents with monarchs and their recent independence years.
```
-- Select code, and language count as lang_num
SELECT code, COUNT(*) AS lang_num
FROM languages
GROUP BY code;

-- Select local_name and lang_num from appropriate tables
SELECT local_name, sub.lang_num
FROM countries,
    (SELECT code, COUNT(*) AS lang_num
     FROM languages
     GROUP BY code) AS sub
-- Where codes match    
WHERE countries.code = sub.code
ORDER BY lang_num DESC;
--Select country code, inflation_rate, and unemployment_rate from economies. Filter code for the set of countries which do not contain the words "Republic" or "Monarchy" in their gov_form.
-- Select relevant fields
SELECT code, inflation_rate, unemployment_rate
FROM economies
WHERE year = 2015 
  AND code NOT IN
-- Subquery returning country codes filtered on gov_form
    (SELECT code
     FROM countries
     WHERE (gov_form LIKE '%Monarchy%' OR gov_form LIKE '%Republic%'))
ORDER BY inflation_rate;


```
## Summary The finish line | SQL
Congratulations on completing the course! Let's review the various types of joins, set operations, basic subqueries, and next steps in SQL.

### Facts
- SQL joins combine columns from tables via a lookup process: INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL JOIN, CROSS JOIN, semi joins, anti joins, and self joins were covered.
- Set operations in SQL offer an alternative way of joining data.
- Subquerying within SELECT, WHERE, and FROM clauses was explored as part of basic subqueries.
- To advance in SQL, consider further courses or practice joining data through projects, exercises, and competitions. Build a portfolio in Workspace for hands-on experience!

![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/5ab3d992-0a7b-4575-8965-cfa4aa220ecf)
*Accomplishment Statement*
