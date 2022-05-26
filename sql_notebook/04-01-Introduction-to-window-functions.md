## 01 Introduction
- Perform an operation across a set of rows that are somehow related to the current row
- Similar to `GROUP BY` aggregate function, but all rows remain in the output.
**Uses**
- Fetch values from preceding or following rows
   - Determining reigning champion status
   - Calculating growth over time
- Assigning ordinal ranks to rows based on their values' positions in a sorted list
- Running totals, moving averages

### Enter ROW_NUMBER
```sql
SELECT
  Year,
  -- Assign numbers to each year
  ROW_NUMBER() OVER() AS Row_N
FROM (
  SELECT DISTINCT Year
  FROM Summer_Medals
  ORDER BY Year ASC
) AS Years
ORDER BY Year ASC;
```
### Anatomy of a window function
- `FUNCTION_NAME() OVER(...)`
   - `ORDER BY`
   - `PARTITION BY`
   - `ROWS/RANGE PRECEDING/FOLLOWING/UNBOUNDED`
- `OVER()` indicates it is a window function, what's inside `OVER()`is called a subclause

## 02 ORDER BY
- `ORDER BY` in `OVER` orders the rows related to the current row
```sql
WITH Athlete_Medals AS (
  SELECT
    -- Count the number of medals each athlete has earned
    Athlete,
    COUNT(*) AS Medals
  FROM Summer_Medals
  GROUP BY Athlete)

SELECT
  -- Number each athlete by how many medals they've earned
  athlete,
  ROW_NUMBER() OVER (ORDER BY medals DESC) AS Row_N
FROM Athlete_Medals
ORDER BY Medals DESC;
```
### Ordering in- and outside OVER
- `ORDER BY` inside `OVER` takes effect before `ORDER BY` outside `OVER`

### LAG
- `LAG(col, n) OVER(...) returns `col`'s value at the row `n` rows before the current row
```sql
SELECT
  Year, Champion,
  -- Fetch the previous year's champion
  LAG(Champion) OVER
    (ORDER BY year ASC) AS Last_Champion
FROM Weightlifting_Gold
ORDER BY Year ASC;
```
## 03 PARTITION BY
- `PARTITION BY` splits the table into partitions based on a column's unique values
    - The results aren't rolled into one column
- Operated on separately by the window function
    - ROW_NUMBER will reset for each partition
    - `LAG` will only fetch a previous value if its previous row is in the same partition

```sql
SELECT
  Gender, Year, Event,
  Country AS Champion,
  -- Fetch the previous year's champion by gender and event
  LAG(Country) OVER (PARTITION BY Gender, Event
            ORDER BY Year ASC) AS Last_Champion
FROM Athletics_Gold
ORDER BY Event ASC, Gender ASC, Year ASC;
```