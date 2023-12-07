### Summary Database
Databases organize information into tables, allowing relationships between data; SQL, a powerful query language, accesses this data for insights.

Facts <br/>
💾 Databases store information like a library's patrons, books, and checkouts in tables. <br/>
📚 Patrons table holds data like card numbers, names, membership years, and fines.<br/>
🔄 Relational databases define connections between tables, enabling data conclusions.<br/>
🌟 Databases surpass spreadsheets in capacity and security, offering encrypted storage.<br/>
🤝 Multiple users can simultaneously query databases for insights using SQL, a widely used language.<br/>

Understanding the organization of a database is an important first step when using SQL.Take a look at the database below.

![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/8944e2b6-b057-4c86-8db1-eca4794c550c)
*All credits goes to Datacamp learning platform_Database Diagram*


### Summary Table
Tables, the foundational elements of databases, consist of rows and columns holding specific data, requiring careful organization and naming conventions. Clear distinctions between tables and their fields aid in efficient database management.<br/>

### Facts <br/>
- 📊 Tables serve as the fundamental building blocks of databases, organized into rows (records) and columns (fields) for storing related data.<br/>
- 🏷️ Table names should be lowercase, devoid of spaces, and ideally denote a collective group or utilize plural names for clarity.<br/>
- 🧾 Records represent individual observations stored within a table, each holding specific data about a subject or entity.<br/>
- 📝 Fields, columns within tables, hold singular pieces of information about all observations, necessitating clear and specific naming conventions.<br/>
- 🔑 Unique identifiers or keys distinguish records within a table, typically using a unique value (like numbers) to avoid confusion.<br/>
- 🔄 Maintaining separate tables for distinct subjects or entities is preferable to combining information into fewer tables, ensuring clarity and avoiding data duplication.<br/>

![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/f36da6fd-cd0c-42dc-be41-4b8f8f45d473)
*All credits goes to Datacamp learning platform_ Unique ID selection*

### Summary  Data 
📊 This lesson covers the essence of databases, focusing on data types, storage, and schemas within a database.

Facts <br/>
⏰ Introduction to database data and storage. <br/>
💾  SQL data types for fields and their importance in storage and operations.<br/>
🔤 Understanding strings, their types, and their efficient storage.<br/>
🔢 Storing whole numbers (integers) and numbers with fractional parts (floats).<br/>
📐 Schemas as blueprints showing table designs and relationships, including data types.<br/>
💽 Explanation of how database information is physically stored on servers, detailing server roles and their capabilities.<br/>


### Summary Introducing queries
🔍 SQL queries are powerful tools for accessing and analyzing data in databases, especially when handling large and complex datasets.

### Facts
- 🔍 SQL helps extract insights within and across relational database tables.  <br/>
- 🔍 It's optimal for complex data relationships and extensive datasets like those in retail platforms. <br/>
- 🔍 Keywords like SELECT and FROM guide query operations. <br/>
- 🔍 Queries, written using keywords and syntax, generate result sets without altering the database. <br/>
- 🔍 Multiple fields can be selected by listing their names separated by commas. <br/>
- 🔍 An asterisk (*) selects all fields within a table for querying. <br/>
  ### Querying the table
-- Select title and author from the books table
```
SELECT title,author
FROM books;
```
