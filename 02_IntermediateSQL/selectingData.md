## Summary Querying a database | SQL

This course on SQL focuses on querying databases, covering techniques like COUNT(), DISTINCT, and using PostgreSQL to extract insights from data.

## Facts
- The course covers foundational SQL knowledge and moves onto querying databases for actionable insights. <br/>
- PostgreSQL is the primary platform used throughout the course.<br/>
- The database used in exercises comprises four tables: films, reviews, people, and roles.<br/>
- COUNT() function allows counting records in a specified field.<br/>
- COUNT() can count multiple fields by using the function multiple times.<br/>
- Using * with COUNT() counts the total number of records in a table.<br/>
-  DISTINCT removes duplicates, helpful in extracting unique values from fields.<br/>
-  COUNT() with DISTINCT counts unique values in a field, excluding duplicates.<br/>
```
-- Count the total number of records in the people table, aliasing the result as count_records
SELECT COUNT(*) AS count_records
FROM people;

-- Count the number of birthdates in the people table
SELECT COUNT(birthdate) as count_birthdate
FROM people;
-- Count the records for languages and countries represented in the films table
SELECT COUNT(language) as count_languages,COUNT(country) as count_countries
FROM films;

-- Return the unique countries from the films table
SELECT DISTINCT country
FROM films

-- Count the distinct countries from the films table
select count(distinct country) as count_distinct_countries
from  films
```

## Summary Query execution | SQL
Understanding the execution order in SQL is crucial. Processing involves stages like selecting tables, executing selections, and refining results. Debugging errors, including comma and keyword mistakes, is a vital skill in SQL.

### Facts
- SQL processing isn't linear; it starts with defining the table, then selecting data, and finally refining results using LIMIT.<br/>
- Debugging SQL involves interpreting error messages, which can pinpoint issues like misspelled fields or keywords.<br/>
- Common errors like missing commas or misspelled keywords trigger specific error messages, aiding in locating the issue.<br/>
- Mastering debugging in SQL involves making mistakes, learning from them, and understanding various error types.<br/>
```
-- Debug the code
SELECT certification
FROM films
LIMIT 5;

-- Debug this code
SELECT film_id, imdb_score, num_votes
FROM reviews;

-- Debug this code
SELECT COUNT(birthdate) AS count_birthdays
FROM people;
```
## Summary SQL style | SQL
Understanding SQL involves more than just knowing the language—formatting, style guides, and conventions significantly impact code readability and collaboration among peers.

### Facts
- SQL allows flexible formatting without strict rules on new lines, capitalization, or indentation, although adhering to formatting standards enhances readability.
- Established SQL style standards improve code legibility, including capitalized keywords and structured new lines.
- SQL style guides, like Holywell's, provide best practices for formatting, capitalization, and naming conventions.
- Varied formatting preferences exist, but clarity and readability remain the primary goal.
- Using semicolons at the end of SQL queries, though not mandatory in all flavors, is considered a best practice for portability and clarity.
- Handling non-standard field names, like those with spaces, requires enclosing them in double-quotes for proper querying.
- Adhering to SQL style guides fosters smoother collaboration and enhances readability for better understanding and debugging.
