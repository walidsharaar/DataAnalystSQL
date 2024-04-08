## Summary Character data types and common issues | SQL
This part introduces character data types in PostgreSQL, distinguishing between categorical variables and unstructured text, and emphasizes the importance of organizing and scrutinizing text data to identify common issues 
such as inconsistencies in case, white space, and punctuation.
### Facts
- PostgreSQL offers three character column types: char, varchar, and text, differing in string length accommodation.
- Distinguishes between categorical variables and unstructured text for data analysis purposes.
- Utilizes GROUP BY and count to examine the distribution of categorical variables.
- Ordering by count reveals the frequency of categories and helps spot errors in rarely occurring values.
- Ordering by category value aids in identifying potential duplicates and other mistakes.
- Character data types are sorted alphabetically, considering spaces and letter cases.
- Common issues include case differences, white space variations, punctuation discrepancies, and the distinctions between an empty string, strings of all spaces, and null.

```
--How many rows does each priority level have?
SELECT priority, count(*)
from evanston311
  group by
 priority;

-- Find values of zip that appear in at least 100 rows
-- Also get the count of each value
SELECT zip, count(*)
  FROM evanston311
 GROUP BY zip
HAVING count(*)>100;

-- Find values of source that appear in at least 100 rows
-- Also get the count of each value
SELECT distinct(source), count(*)
  FROM evanston311
 group by source
having count(*)<100;
```
