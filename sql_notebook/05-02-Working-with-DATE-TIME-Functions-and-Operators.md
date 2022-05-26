## 01 Date / time arithmetics
### Adding and subtracting date / time data
- date -> integer
```sql
SELECT date '2005-09-11' - date '2005-09-10';
```
```
integer
1
```
```sql
SELECT date '2005-09-11' + integer '3';
```
```
date
2005-09-14
```
- timestamp -> interval
```sql
SELECT date '2005-09-11 00:00:00' - date '2005-09-09 12:00:00';
```
```
interval
1 day 12:00:00
```
### Calculate time period with `AGE()`
```sql
SELECT AGE(timestamp '2005-09-11 00:00:00', timestamp '2005-09-09 12:00:00');
```
```
interval
1 day 12:00:00
```
### Date / time arithmetic using INTERVALs
```sql
SELECT timestamp '2019-05-01' + INTERVAL '1 day' * 21;
```
```
timestamp without timezone
2019-05-22 00:00:00
```
## 02 Functions for retrieving current date/time
### Retriving the current timestamp
- `SELECT NOW();` : current timestamp with timezone by default
    - to retrieve timestamp without timezone:
    - `SELECT NOW()::timestamp:` PostgreSQL specific casting
    - `SELECT CAST(NOW() as timestamp);` CAST() function
- `SELECT CURRENT_TIMESTAMP;`
    - `CURRENT_TIMESTAMP(2)`
### Current date and time
- `SELECT CURRENT_DATE;`
- `SELECT CURRENT_TIME;`
## 03 Extracting and transforming date/time data
### Extracting sub-fields from timestamp data
- return integers
- EXTRACT（field *FROM* source)
```sql
SELECT EXTRACT(quarter FROM timestamp '2015-01-24 00:00:00`) AS quarter;
```
- DATE_PART('field', source)
```sql
SELECT DATE_PART('quarter', timestamp '2015-01-24 00:00:00`) AS quarter;
```
```
quarter
1
```
- Using a numeric reference in GROUP BY clause comes in handy when we use functions to derive new columns.
```sql
-- Extract day of week from rental_date
SELECT 
  EXTRACT(dow FROM rental_date) AS dayofweek, 
  -- Count the number of rentals
  COUNT(*) as rentals 
FROM rental 
GROUP BY 1;
```

### Truncating timestamps using DATE_TRUNC()
The `DATE_TRUNC()` function will truncate timestamp or interval data types
- Return timestamp or interval.
- Truncate timestamp '2005-05-21 15:30:00' by year
```sql
SELECT DATE_TRUNC('year', TIMESTAMP '2005-05-21 15:30:00');
```
```
Result: 2005-01-01 00:00:00
```

### field
- `century`
- `day`
- `decade`
    - The year field divided by 10
- `dow`
    - The day of the week as Sunday(0) to Saturday(6)
- `doy`
    - The day of the year (1-365/366)
- `epoch`
    - For date and timestamp values, the number of seconds since 1970-01-01 00:00:00 UTC (can be negative); for interval values, the total number of seconds in the interval
- `hour`
- `isodow`
    - Monday(1) to Sunday(7)
- `quarter`
[...](https://www.postgresql.org/docs/9.1/functions-datetime.html)