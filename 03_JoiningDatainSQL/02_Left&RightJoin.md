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
