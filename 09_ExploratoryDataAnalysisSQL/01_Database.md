
## Summary
This course focuses on exploring a PostgreSQL database, covering data summary, handling messy data, and leveraging SQL skills for exploratory data analysis.

### Facts
- PostgreSQL is the database system used in this course, although functions might differ in other SQL systems.
-  A database client is crucial for accessing and working with a database, offering commands to extract information on tables, columns, and relationships.
- An entity-relationship diagram aids in understanding the database structure, showcasing tables, columns, and table relationships.
-  Different tables like evanston311, fortune500, stackoverflow, and supporting tables hold diverse datasets in this course.
- Selecting rows from tables gives an initial glimpse of the table's content, often done by using 'SELECT *' and 'LIMIT' commands.
-  While exploring tables, remembering that NULL represents missing data is essential. Techniques like 'is NULL'/'is not NULL' and count functions help handle NULL values.
-  Counting rows or distinct values in columns provides insights into data, with distinctions in NULL value handling between count functions and direct selection.

```
-- Select the count of the number of rows
SELECT count(1)
  FROM fortune500;
-- Select the count of ticker, 
-- subtract from the total number of rows, 
-- and alias as missing
SELECT count(*) - count(ticker) AS missing
  FROM fortune500;
-- Select the count of profits_change, 
-- subtract from total number of rows, and alias as missing
SELECT count(*) - count(profits_change) AS missing
  FROM fortune500;
-- Select the count of industry, 
-- subtract from total number of rows, and alias as missing
SELECT count(*) - count(industry) AS missing
  FROM fortune500;
/*Look at the contents of the company and fortune500 tables. Find a column that they have in common where the values for each company are the same in both tables.
Join the company and fortune500 tables with an INNER JOIN.
Select only company.name for companies that appear in both tables.*/
SELECT company.name 
-- Table(s) to select from
  FROM company 
       INNER JOIN fortune500 
       ON company.ticker=fortune500.ticker;
```

## Summary The keys to the database
Understanding database relationships involves recognizing foreign keys, ER diagrams, primary keys, and functions like coalesce.

### Facts
- Foreign keys link tables via specific column references, enabling relationships between rows.
- ER diagrams visually represent relationships; arrows denote foreign key connections between tables.
- Self-referencing occurs when a foreign key in a table points to the same table, like parent_id in the company table.
- Lack of a foreign key doesn’t hinder table joins; comparable columns suffice, as seen in the fortune500 and company tables.
- Primary keys uniquely identify table rows; they're indicated in ER diagrams with a bordered format.
- Coalesce function merges non-NULL values row-wise, offering flexibility in selecting or combining column data, providing default or backup values.

```
/*First, using the tag_type table, count the number of tags with each type.
Order the results to find the most common tag type.
*/
-- Count the number of tags with each type
SELECT type, count(*) AS count
  FROM tag_type
 GROUP BY type
 ORDER BY count DESC;

/*Join the tag_company, company, and tag_type tables, keeping only mutually occurring records.
Select company.name, tag_type.tag, and tag_type.type for tags with the most common type from the previous step.*/

-- Select the 3 columns desired
SELECT name, tag_type.tag, tag_type.type
  FROM company
       INNER JOIN tag_company 
       ON company.id = tag_company.company_id
       INNER JOIN tag_type
       ON tag_company.tag = tag_type.tag
  WHERE type='cloud';

/*Use coalesce() to select the first non-NULL value from industry, sector, or 'Unknown' as a fallback value.
Alias the result of the call to coalesce() as industry2.
Count the number of rows with each industry2 value.
Find the most common value of industry2.
*/

SELECT coalesce(industry, sector, 'Unknown') AS industry2,
       count(*) 
  FROM fortune500 
 GROUP BY industry2
 ORDER BY count DESC
 LIMIT 1;

/* Join company to itself to add information about a company's parent to the original company's information.
Use coalesce to get the parent company ticker if available and the original company ticker otherwise.
INNER JOIN to fortune500 using the ticker.
Select original company name, fortune500 title and rank.
*/
SELECT company_original.name, title, rank
  FROM company AS company_original
	   LEFT JOIN company AS company_parent
       ON company_original.parent_id = company_parent.id 
       INNER JOIN fortune500 
       ON coalesce(company_parent.ticker, 
                   company_original.ticker) = 
             fortune500.ticker
 ORDER BY rank; 
```

## Summary Column types and constraints | SQL

Understanding column types and constraints in databases involves recognizing various data types and limitations on values within columns. This includes constraints like foreign keys, primary keys, unique, and not null, as well as different data types such as numeric, character, date/time, and special types like boolean, monetary, geometric, and structured data types.

### Facts
- Column Constraints
Foreign keys and primary keys limit values; columns can have unique, not null, or check constraints.
- Data Types
Numeric, character, date/time, and boolean are common; special types include monetary, geometric, and structured data.
- Numeric Types
Diverse numeric types store varied value ranges and require different memory allocations.
- Types in Diagrams
Entity relationship diagrams specify column types; even without diagrams, column types are fundamental in documentation.
- Casting with CAST()
Casting converts values temporarily; use the cast function to convert values or entire columns.
- Casting with ::
An alternate notation, double colon, performs casting similarly to the cast function, offering a more concise method.

```
--  Select profits_change and profits_change cast as integer from fortune500 and look at how the values were converted.
-- Select the original value
SELECT profits_change, 
	   -- Cast profits_change
       CAST(profits_change AS integer) AS profits_change_int
  FROM fortune500;
-- Compare the results of casting of dividing the integer value 10 by 3 to the result of dividing the numeric value 10 by 3.
SELECT 10/3,
       10::numeric/3;
-- Now cast numbers that appear as text as numeric. Note: 1e3 is scientific notation.
SELECT '3.2'::numeric,
       '-123'::numeric,
       '1e3'::numeric,
       '1e-3'::numeric,
       '02314'::numeric,
       '0002'::numeric;

--Use GROUP BY and count() to examine the values of revenues_change and order the results by revenues_change to see the distribution.

SELECT revenues_change, count(*) 
  FROM fortune500
 GROUP BY revenues_change 
 ORDER BY revenues_change;

--Repeat step 1, but this time, cast revenues_change as an integer to reduce the number of different values.

SELECT revenues_change::integer, count(*) 
  FROM fortune500
 GROUP BY revenues_change::integer 
 ORDER BY revenues_change;

-- How many of the Fortune 500 companies had revenues increase in 2017 compared to 2016? To find out, count the rows of fortune500 where revenues_change indicates an increase.
-- Count rows 
SELECT count(*)
  FROM fortune500
 -- Where...
 WHERE revenues_change > 0;
```
