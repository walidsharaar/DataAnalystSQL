1. **Nested Queries**:
   - A nested query is a query where a complete SELECT block appears in the WHERE or HAVING clause of another query.
   - The inner block (subquery) is executed first by the query optimizer and can have single or multiple values.
   - These values are used by the outer query.

2. **Example 1: Disappointed Customers**:
   - Inner Query: Select distinct customer IDs who rated movies with a score of 3 or lower.
   - Outer Query: Retrieve names of customers whose IDs are in the result of the inner query.

3. **Example 2: Earliest Account Creation Date**:
   - Inner Query: Find the earliest account creation date for a customer from Austria (single value).
   - Outer Query: Group customer data by country, listing country names and earliest account creation dates. Use HAVING to report countries where the first account date is earlier than Austria's.

4. **Example 3: Actors in the Movie 'Ray'**:
   - Nested WHERE Clauses: Select movie ID for 'Ray' (single value).
   - Next Level: Select actor IDs for actors in the movie with the ID from the previous step.
   - Outer Query: Report actor names whose IDs are in the result of the inner query.

```
```
