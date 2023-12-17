### Summary Database
Databases organize information into tables, allowing relationships between data; SQL, a powerful query language, accesses this data for insights.

Facts <br/>
- Databases store information like a library's patrons, books, and checkouts in tables. <br/>
- Patrons table holds data like card numbers, names, membership years, and fines.<br/>
- Relational databases define connections between tables, enabling data conclusions.<br/>
- Databases surpass spreadsheets in capacity and security, offering encrypted storage.<br/>
- Multiple users can simultaneously query databases for insights using SQL, a widely used language.<br/>

Understanding the organization of a database is an important first step when using SQL.Take a look at the database below.

![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/8944e2b6-b057-4c86-8db1-eca4794c550c)
*All credits goes to Datacamp learning platform_Database Diagram*


### Summary Table
Tables, the foundational elements of databases, consist of rows and columns holding specific data, requiring careful organization and naming conventions. Clear distinctions between tables and their fields aid in efficient database management.<br/>

### Facts <br/>
- Tables serve as the fundamental building blocks of databases, organized into rows (records) and columns (fields) for storing related data.<br/>
- Table names should be lowercase, devoid of spaces, and ideally denote a collective group or utilize plural names for clarity.<br/>
- Records represent individual observations stored within a table, each holding specific data about a subject or entity.<br/>
- Fields, columns within tables, hold singular pieces of information about all observations, necessitating clear and specific naming conventions.<br/>
- Unique identifiers or keys distinguish records within a table, typically using a unique value (like numbers) to avoid confusion.<br/>
- Maintaining separate tables for distinct subjects or entities is preferable to combining information into fewer tables, ensuring clarity and avoiding data duplication.<br/>

![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/f36da6fd-cd0c-42dc-be41-4b8f8f45d473)
*All credits goes to Datacamp learning platform_ Unique ID selection*

### Summary  Data 
This lesson covers the essence of databases, focusing on data types, storage, and schemas within a database.

### Facts <br/>
- Introduction to database data and storage. <br/>
- SQL data types for fields and their importance in storage and operations.<br/>
- Understanding strings, their types, and their efficient storage.<br/>
- Storing whole numbers (integers) and numbers with fractional parts (floats).<br/>
- Schemas as blueprints showing table designs and relationships, including data types.<br/>
- Explanation of how database information is physically stored on servers, detailing server roles and their capabilities.<br/>


### Summary Introducing queries
SQL queries are powerful tools for accessing and analyzing data in databases, especially when handling large and complex datasets.

### Facts
- SQL helps extract insights within and across relational database tables.  <br/>
- It's optimal for complex data relationships and extensive datasets like those in retail platforms. <br/>
- Keywords like SELECT and FROM guide query operations. <br/>
- Queries, written using keywords and syntax, generate result sets without altering the database. <br/>
- Multiple fields can be selected by listing their names separated by commas. <br/>
- An asterisk (*) selects all fields within a table for querying. <br/>
  ### Querying the table
```
-- Select title and author from the books table
SELECT title,author
FROM books;
```
### Summary Writing queries | SQL
SQL queries can be enhanced using keywords like aliasing, DISTINCT, and views to manipulate and manage data efficiently.

### Facts
- Aliasing: Renaming columns in result sets using the AS keyword allows for clarity and brevity without changing the original table.<br/>
- Selecting distinct records: The DISTINCT keyword helps retrieve unique values; it's useful when needing unique sets or eliminating duplicate values in queries.<br/>
- DISTINCT with multiple fields: When applied to multiple fields, DISTINCT shows unique combinations of those fields, aiding in examining distinct data sets.<br/>
- Views: SQL views create virtual tables from saved SELECT statements, updating automatically when underlying data changes. They're created using CREATE VIEW.<br/>
- Using views: Once created, views can be queried similar to regular tables, providing a way to access stored result sets.<br/>

```
-- Select unique authors from the books table
SELECT DISTINCT author
FROM 
books;
-- Select unique authors and genre combinations from the books table
SELECT DISTINCT author,genre
FROM books;
-- Alias author so that it becomes unique_author
SELECT DISTINCT author AS unique_author
FROM books;
-- Save the results of this query as a view called library_authors
CREATE VIEW library_authors AS 
SELECT DISTINCT author AS unique_author
FROM books;
-- Select all columns from library_authors
SELECT * FROM library_authors;
```
### Summary SQL flavors | SQL
SQL flavors encompass different versions that adhere to universal standards but may have additional features. Two prominent flavors are PostgreSQL and SQL Server, each with its unique traits but sharing similarities.

### Facts
- SQL flavors exist, varying in support and features, all conforming to universal standards.<br/>
- PostgreSQL: Open-source, DARPA-sponsored, and versatile.<br/>
- SQL Server: Microsoft's creation, compatible with Microsoft products, using T-SQL as its flavor.<br/>
- Comparing Flavors: Differences between flavors are akin to dialects; small variations exist, like LIMIT in PostgreSQL and TOP in SQL Server.<br/>
- Choosing a Flavor: The decision on which to learn might be influenced by employer preferences or remain inconsequential due to minor differences; mastering one allows for an easy transition to another.<br/>
```
-- Select the first 10 genres from books using PostgreSQL
SELECT genre
FROM books
LIMIT 10
```
### Conclusion
I've gained a solid grasp of SQL fundamentals.

### Facts
- I've learned the significance of databases and their application.<br/>
- Explored the structure and organization of relational databases.<br/>
- Developed the ability to craft SQL queries for database insights.<br/>
Where to go next
- Dive deeper into advanced SQL queries for more complex data manipulation and analysis.<br/>
- Consider specialized SQL flavor courses to explore nuances and expand your skills further.<br/>

![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/6b68e513-73e4-43c8-abd7-4bdf7fe29bf1)
*Statement of Accomplishment*
