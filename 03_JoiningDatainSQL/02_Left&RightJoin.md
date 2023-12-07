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
