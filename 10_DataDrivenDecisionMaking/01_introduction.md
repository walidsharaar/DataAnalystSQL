# Summary Introduction to data driven decision making | SQL

### Facts
- Irene Ortner, alongside Professors Bart Baesens and Tim Verdonck, introduces the data-driven decision-making course.
- Refresh basic SQL and introduce advanced SQL techniques for business intelligence, including OLAP extensions like CUBE and ROLLUP.
- The course features a fictional movie rental company, MovieNow, which offers online movie streaming and retains comprehensive data on movies and customer interactions.
- Detailed exploration of 'customers' table in MovieNow's database, including customer ID, personal details, and account creation date.
- Insight into the 'movies' table, covering movie ID, title, genre, runtime, release year, and rental cost.
- Description of the 'renting' table, documenting each rental with identifiers, customer and movie details, ratings, and rental date.
- Examination of the 'actors' table, which lists actor IDs, names, birth years, nationalities, and genders.
- Overview of the 'actsin' table, noting the connections between actors and movies through unique identifiers.
- Use of data for making both short-term operational and long-term strategic decisions, such as movie purchases based on actor popularity or regional expansion based on customer growth analytics.
- Definition and monitoring of key performance indicators like revenue, customer satisfaction, and engagement to inform and refine decision-making processes.
- Encouragement for students to start exploring the database independently, applying what they've learned to real scenarios.

```
 - select only those columns from renting which are needed to calculate the average rating per movie.
SELECT movie_id,
       rating
FROM renting;
```

### Summary Filtering and ordering | SQL
- Filtering and Ordering: Filtering data to find relevant records and ordering them correctly is key for effective data analysis.
- WHERE Clause: A WHERE clause in SQL filters rows based on specific conditions, like selecting customers from Italy.
- Operators in WHERE Clause: Various operators such as comparison, BETWEEN, IN, and NULL can be used to refine selections in SQL queries.
- Comparison Operators Example: SQL can exclude genres or select movies with a minimum renting price using comparison operators.
- BETWEEN Operator Example: The BETWEEN operator helps select records within a specific date range, requiring precise syntax with dates in single quotes.
- IN Operator Example: The IN operator allows for checking if column values match any values in a given list, such as actor nationalities.
- NULL Operator Example: The NULL operator is used to handle missing or unknown data values, allowing selection based on the presence or absence of data.
- Boolean Operators AND: The AND operator combines multiple conditions to refine SQL queries further, such as selecting Italian customers with accounts created within a specific timeframe.
- Boolean Operators OR: The OR operator connects conditions alternatively, broadening the query to include more rows that meet any listed criteria.
- ORDER BY: SQL queries can be ordered by specific column values in ascending order by default using the ORDER BY statement.
- ORDER BY ... DESC: To prioritize higher values first, DESC is used in the ORDER BY clause to sort data in descending order, showcasing the highest ratings.

```

```
