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
```
