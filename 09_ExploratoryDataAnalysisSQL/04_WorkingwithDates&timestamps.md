## Summary Date/time types and formats | SQL
This segment covers the fundamentals of handling date/time data in databases, highlighting types, formats, and operations like addition and subtraction.
### Facts
- Date/time data encompasses columns storing dates or times, with main types being date, timestamp, and intervals.
- Two primary types are dates (year, month, day) and timestamps (date plus time down to microseconds).
- Intervals represent durations and are often results of subtracting one date/time from another, defaulting to days and time display.
- Multiple formats for recording dates and times exist, leading to potential ambiguity in data representation.
- ISO 8601 standardizes date/time recording, ordering units from largest to smallest and using fixed digit counts.
- Timezones complicate datetime data, with Postgres using UTC and allowing for timezone offsets in timestamps.
- Dates and timestamps can be compared using conventional operators, and the current timestamp is obtainable via the now function.
- Subtraction between dates results in an interval type.
- Dates can have days added directly, while more complex intervals require specifying durations in terms of years, days, and minutes, among others.

```
-- Count requests created on January 31, 2017 and count the number of Evanston 311 requests created on January 31, 2017 by casting date_created to a date.
SELECT count(*) 
  FROM evanston311
 WHERE date_created::date = '2017-01-31'
-- Count requests created on February 29, 2016
SELECT count(*)
  FROM evanston311 
 WHERE date_created >= '2016-02-29' 
   AND date_created < '2016-03-01';

-- Count requests created on March 13, 2017 and upper bound by adding 1 to the lower bound.
SELECT count(*)
  FROM evanston311
 WHERE date_created >= '2017-03-13'
   AND date_created < '2017-03-13'::date + 1;

-- Subtract the min date_created from the max
SELECT max(date_created) - min(date_created)
  FROM evanston311;

--Compute the average difference between the completion timestamp and the creation timestamp by category and order the results with the largest average time to complete the request first..
SELECT category, 
       avg(date_completed - date_created) AS completion_time
  FROM evanston311
 GROUP BY category
 ORDER BY completion_time DESC;
```
