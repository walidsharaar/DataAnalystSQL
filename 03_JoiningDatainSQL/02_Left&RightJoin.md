## Summary LEFT and RIGHT JOINs | SQL

This chapter delves into outer joins—LEFT and RIGHT JOINs—contrasting them with INNER JOINs. LEFT JOIN retains all records from the left table and matches those from the right table, while RIGHT JOIN retains all from the right table.

## Facts
- In INNER JOIN, only matching records from both tables are included in the result.
  ![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/00951eb3-62e2-4d78-8f1f-2389bdc08503)
*Inner Join*
- LEFT JOIN keeps all left_table records and matches those from the right_table. Unmatched values from right_table show as null.
- Diagrams illustrate the differences between INNER and LEFT JOIN outcomes.
- LEFT JOIN syntax mirrors INNER JOIN, but retrieves unmatched values from the right table.
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/8252b52a-7b88-49a0-85e2-39aaf6a3bc55)
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/22f6aae9-da3d-4c91-b5d6-7da9c7e374f3)
*left join*
- RIGHT JOIN retains all right_table records, showing null for unmatched left_table entries.
- RIGHT JOIN syntax mirrors LEFT JOIN, but focuses on retaining unmatched right_table values.
  ![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/731856c8-3418-47fe-a0b1-785c0b0e79e6)
*Right join*
- Concrete examples using world leaders showcase the differences between LEFT and RIGHT JOIN.
- RIGHT JOIN is less common due to its interchangeability with a rearranged LEFT JOIN, which feels more intuitive in query construction.
```
--Perform an inner join with cities AS c1 on the left and countries as c2 on the right.Use code as the field to merge your tables on.
SELECT 
    c1.name AS city,
    code,
    c2.name AS country,
    region,
    city_proper_pop
FROM cities AS c1
-- Perform an inner join with cities as c1 and countries as c2 on country code
inner join countries c2 on c1.country_code = c2.code
ORDER BY code DESC;

--Change the code to perform a LEFT JOIN instead of an INNER JOIN.
SELECT 
	c1.name AS city, 
    code, 
    c2.name AS country,
    region, 
    city_proper_pop
FROM cities AS c1
-- Join right table (with alias)
left join countries c2
ON c1.country_code = c2.code
ORDER BY code DESC;

--Complete the LEFT JOIN with the countries table on the left and the economies table on the right on the code field.
SELECT name, region, gdp_percapita
FROM countries AS c
LEFT JOIN economies AS e
-- Match on code fields
on e.code = c.code
-- Filter for the year 2010
where year=2010;

-- Select region, and average gdp_percapita as avg_gdp
select region,avg(gdp_percapita) as avg_gdp
FROM countries AS c
LEFT JOIN economies AS e
USING(code)
WHERE year = 2010
-- Group by region
GROUP BY region;
--Order the result set by the average GDP per capita from highest to lowest.Return only the first 10 records in your result.
SELECT region, AVG(gdp_percapita) AS avg_gdp
FROM countries AS c
LEFT JOIN economies AS e
USING(code)
WHERE year = 2010
GROUP BY region
-- Order by descending avg_gdp
order by avg_gdp desc
-- Return only first 10 records
limit 10
-- Modify this query to use RIGHT JOIN instead of LEFT JOIN
SELECT countries.name AS country, languages.name AS language, percent
FROM languages
RIGHT JOIN countries
USING(code)
ORDER BY language;
```
## Summary FULL JOINs | SQL
FULL JOIN combines LEFT JOIN and RIGHT JOIN, returning all IDs irrespective of matches, showing nulls for non-matching records.

### Facts
- FULL JOIN, the last of three outer join types, compared to LEFT JOIN, RIGHT JOIN, and INNER JOIN.
- FULL JOIN combines LEFT and RIGHT JOIN, retaining all IDs regardless of matches, displaying nulls for non-matching records.
- SQL code for FULL JOIN closely aligns with INNER, LEFT, and RIGHT JOIN syntax.\
- Example from leaders database illustrates FULL JOIN to retrieve countries and their leaders, showcasing nulls for non-matching positions.
  
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/5ffafd48-9a4d-45e9-829f-25c604a2cada)
*full join*

```
--Perform a full join with countries (left) and currencies (right). Filter for the North America region or NULL country names.
SELECT name AS country, code, region, basic_unit
FROM countries
-- Join to currencies
FULL JOIN currencies
USING (code)
-- Where region is North America or null
WHERE region = 'North America'
	OR name IS NULL
ORDER BY region;
-- Repeat the same query as before, turning your full join into a left join with the currencies table. Have a look at what has changed in the output by comparing it to the full join result.
SELECT name AS country, code, region, basic_unit
FROM countries
-- Join to currencies
left Join currencies 
USING (code)
WHERE region = 'North America' 
	OR name IS NULL
ORDER BY region;
-- Repeat the same query again, this time performing an inner join of countries with currencies. Have a look at what has changed in the output by comparing it to the full join and left join results!
SELECT name AS country, code, region, basic_unit
FROM countries
-- Join to currencies
INNER JOIN currencies
USING (code)
WHERE region = 'North America' 
	OR name IS NULL
ORDER BY region;
--Complete the FULL JOIN with countries as c1 on the left and languages as l on the right, using code to perform this join.
SELECT 
	c1.name AS country, 
    region, 
    l.name AS language,
	basic_unit, 
    frac_unit
FROM countries as c1 
-- Full join with languages (alias as l)
FULL JOIN languages as l 
USING(code)
-- Full join with currencies (alias as c2)
FULL JOIN currencies AS c2
USING(code)
WHERE region LIKE 'M%esia';
--chain this join with another FULL JOIN, placing currencies on the right, joining on code again
SELECT 
	c1.name AS country, 
    region, 
    l.name AS language,
	basic_unit, 
    frac_unit
FROM countries as c1 
-- Full join with languages (alias as l)
FULL JOIN languages as l 
USING(code)
-- Full join with currencies (alias as c2)
FULL JOIN currencies AS c2
USING(code)
WHERE region LIKE 'M%esia';
```
## Summary Crossing into CROSS JOIN | SQL

Exploring the concept of CROSS JOIN in SQL, which generates all possible combinations between two tables without conditions.

### Facts
- CROSS JOIN Diagram: Illustrates how CROSS JOIN combines all combinations from two tables, showcasing the result of nine combinations between table1 and table2.
- CROSS JOIN Syntax: Requires minimal syntax without specifying conditions like ON or USING.
- Pairing Prime Ministers with Presidents: Applying CROSS JOIN to a scenario involving global leaders from Asia and South America databases, generating all possible pairings for journalist coverage.
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/07469286-638b-4e1b-94e5-b8f0376f71a3)
*cross join*
```
--Complete the code to perform an INNER JOIN of countries AS c with languages AS l using the code field to obtain the languages currently spoken in the two countries.
SELECT c.name AS country, l.name AS language
-- Inner join countries as c with languages as l on code
FROM countries AS c        
INNER JOIN languages AS l
USING(code)
WHERE c.code IN ('PAK','IND')
	AND l.code in ('PAK','IND');
-- Change your INNER JOIN to a different kind of join to look at possible combinations of languages that could have been spoken in the two countries given their history. Observe the differences in output for both joins.
SELECT c.name AS country, l.name AS language
FROM countries AS c        
-- Perform a cross join to languages (alias as l)
CROSS JOIN languages AS l
WHERE c.code in ('PAK','IND')
	AND l.code in ('PAK','IND');
```

- Use joins, filtering, sorting, and limiting to identify the five countries and their regions with the lowest life expectancy in 2010.
```
SELECT 
	c.name AS country,
    region,
    life_expectancy AS life_exp
FROM countries AS c
-- Join to populations (alias as p) using an appropriate join
left join populations p 
ON c.code = p.country_code
-- Filter for only results in the year 2010
where year=2010
-- Sort by life_exp
order by life_expectancy asc
-- Limit to five records
limit 5;

```
## Summary Self joins | SQL
Self joins involve joining a table to itself, typically used to compare values within the same table. They're handy for tasks like pairing countries from the same continent.

### Facts
- Self joins link a table with itself to compare values internally.
- They're helpful for creating pairs like countries on the same continent.
- No dedicated syntax exists; aliasing is necessary for a self join in SQL.
- Setting joining fields is crucial to match the table to itself.
- Avoiding pairing records with themselves requires additional conditions.
- The final self-joined table displays country combinations within continents, excluding self-pairings.

```
--Select the country_code from p1 and the size field from both p1 and p2, aliasing p1.size as size2010 and p2.size as size2015 (in that order).
-- Select aliased fields from populations as p1
SELECT 
	p1.country_code, 
    p1.size AS size2010, 
    p2.size AS size2015
-- Join populations as p1 to itself, alias as p2, on country code
FROM populations AS p1
INNER JOIN populations AS p2
USING(country_code);

-- Since you want to compare records from 2010 and 2015, eliminate unwanted records by extending the WHERE statement to include only records where the p1.year matches p2.year - 5.
SELECT 
	p1.country_code, 
    p1.size AS size2010, 
    p2.size AS size2015
FROM populations AS p1
INNER JOIN populations AS p2
USING(country_code)
WHERE p1.year = 2010
-- Filter such that p1.year is always five years before p2.year
    AND p1.year = p2.year - 5;


```
