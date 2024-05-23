## Summary Grouping  | SQL

### Lesson Overview
- Introduction to using the GROUP BY clause in SQL queries to apply aggregations to table groups.
- GROUP BY clause used for analyzing groups such as customers by country or movies by genre, focusing on calculating average rental prices by genre.
- A subset table named 'movies_selected' includes movie name, genre, and rental price, used to determine the average rental price per genre using GROUP BY.
- Basic SQL GROUP BY usage to group data by genre and add aggregation functions like average, sum, minimum, or maximum to refine data analysis.
- Average rental price per genre calculated and displayed using an alias avg_price in the query, providing clarity on price distribution across genres.
- Illustration of how GROUP BY divides the movies_selected table into subgroups based on genre, highlighting the organizational aspect of this clause.
- Separate tables for each genre show specific groupings where aggregation functions like COUNT are applied to ascertain the number of movies per genre.
- Query enhanced to include the count of movies in each genre, combining COUNT and average price calculations for comprehensive data insights.
- Explanation of the HAVING clause for filtering GROUP BY results based on specified conditions, such as groups with more than two movies, demonstrating its utility in refining query results.

```
-- Create a table with a row for each country and columns for the country name and the date when the first customer account was created and use the alias first_account for the column with the dates then order by date in ascending order.

select country,
	min(date_account_start) AS first_account
from customers
group BY country
order by first_account;



```