## Summary Set theory for SQL Joins | SQL
Set theory introduces set operations like UNION, INTERSECT, and EXCEPT in SQL, enabling combining and comparing data from tables without explicit join conditions.

### Facts
- Set operations in SQL are akin to Venn diagrams, with UNION, INTERSECT, and EXCEPT mirroring their concepts.
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/f2666efb-4b42-4f95-b693-7d0c1c8c3fd6)
*Union Venn diagrams*
- UNION combines records from two tables, discarding duplicates, yielding a single set of distinct records.
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/0b59a991-2e6e-405a-9d60-7754f01e8490)
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/1fcbcc0c-2b5e-45d7-93ea-6f4ebec544fe)

*Union diagrams*

- UNION ALL functions similarly to UNION but retains duplicates, resulting in a combined set with all records, including duplicates.
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/15ba93e5-4f3b-4b18-b011-a830560ed9c9)

*Union all diagrams*

- Syntax for set operations involves SELECT statements for tables and the set operation (UNION/UNION ALL) between them, requiring identical column count and data types.
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/f996a83a-713b-4f12-acf4-10eda668755a)

*Union & Union all Syntax*
<br/>Understanding these set operations in SQL extends the toolkit for combining data without the need for explicit join conditions, focusing on set theory principles and data stacking rather than table merging.

```
--You have two tables, economies2015 and economies2019, available to you under the tabs in the console. You'll perform a set operation to stack all records in these two tables on top of each other, excluding duplicates
-- Select all fields from economies2015
select * from economies2015    
-- Set operation
union
-- Select all fields from economies2019
select * from economies2019
ORDER BY code, year;

-- Query that determines all pairs of code and year from economies and populations, without duplicates
SELECT code, year
FROM economies
UNION 
SELECT country_code, year
FROM populations
ORDER BY code, year;
```
## Summary INTERSECT | SQL
INTERSECT is a set operation that retrieves records common to two tables, akin to a Venn diagram intersection.

### Facts
- INTERSECT: Retrieves only records present in both input tables.
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/d0905b22-e53b-4caa-a1c9-8f05ff28b5b0)
*Intersect Venn diagrams*
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/ddc3628b-16e3-437f-8a6d-b7153c0260e6)
*Intersect Diagram*
- Syntax: Similar to UNION, involves SELECT statements with a set operator.
- INTERSECT vs. INNER JOIN: INTERSECT demands exact field matches and equal columns in both tables for common records; it omits duplicates unlike INNER JOIN.
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/74d58e74-3152-4302-b17f-912d583c9129)
*Inner vs Intersect Syntax*
- Application: Used to find countries with both prime ministers and presidents.
- Pitfalls: Matching specific fields might lead to unexpected or empty results based on data correlations.
