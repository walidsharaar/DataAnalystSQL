
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
```
