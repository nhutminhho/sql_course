## 01 Aggregate window functions
- To calculate running totals etc.
- `SUM(col) OVER(...)`
   - `MAX()`
- Example: Running totals of athlete medals
```sql
SELECT
  -- Calculate the running total of athlete medals
  Athlete,
  Medals,
  SUM(Medals) OVER (ORDER BY Athlete ASC) AS Max_Medals
FROM Athlete_Medals
ORDER BY Athlete ASC;
```

## 02 Frames
- By default, a frame starts at the beginning of a table or partition and ends at the current row
### ROWS BETWEEN
- `ROWS BETWEEN [START] AND [FINISH]`
    - `n PRECEDING`
    - `CURRENT ROW`
    - `n FOLLOWING`
### ROWS vs RANGE
- `RANGE BETWEEN [START] AND [FINISH]`
    - Functions much the same as `ROWS BETWEEN`
    - `RANGE` treats duplicates in `OVER`'s  `ORDER BY` subclauses as a single entity
- `ROWS BETWEEN` is almost always used over `RANGE BETWEEN`
- Example: Moving maximum of Chinese athletes' medals
```sql
SELECT
  -- Select the athletes and the medals they've earned
  Athlete,
  medals,
  -- Get the max of the last two and current rows' medals 
  MAX(medals) OVER (ORDER BY Athlete ASC
            ROWS BETWEEN 2 PRECEDING
            AND CURRENT ROW) AS Max_Medals
FROM Chinese_Medals
ORDER BY Athlete ASC;
```

## 03 Moving averages and totals
### Moving average(MA): average of last `n` periods
- Used to indicate momentum/trends
- Also useful in eliminating seasonality
### Moving total: Sum of last `n` periods
- Used to indicate performance; if the sum is going down, overall performance is going down

- Example: 3-year Moving average of Russian medals
```sql
SELECT
  Year, Medals,
  --- Calculate the 3-year moving average of medals earned
  AVG(Medals) OVER
    (ORDER BY Year ASC
     ROWS BETWEEN
     2 PRECEDING AND CURRENT ROW) AS Medals_MA
FROM Russian_Medals
ORDER BY Year ASC;
```
