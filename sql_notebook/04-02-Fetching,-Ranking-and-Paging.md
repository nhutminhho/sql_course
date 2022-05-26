## 01 Fetching
### Four functions
**Relative**
- `LAG(col,n)` - n rows before the current row
- `LEAD(col,n)` - n rows after the current row

**Absolute**
- `FIRST_VALUE(col)` first value in the table or `partition`
- `LAST_VALUE(col)`

### FIRST_VALUE and LAST_VALUE
- Be careful to use `RANGE BETWEEN ...` clause with `LAST_VALUE` to extends the window to the end of the table
    - No need to do so with the other three
```sql
SELECT
  Year,
  City,
  -- Get the last city in which the Olympic games were held
  LAST_VALUE(city) OVER (
   ORDER BY Year ASC
   RANGE BETWEEN
     UNBOUNDED PRECEDING AND
     UNBOUNDED FOLLOWING
  ) AS Last_City
FROM Hosts
ORDER BY Year ASC;
``` 
## 02 Ranking
### Three functions
- `ROW_NUMBER()` assigns unique numbers, even if tow rows' values are the same
- `RANK()` assigns the same number to rows with identical values, skipping the next numbers in such cases
- `DENSE_RANK()` assigns the same number to rows with identical values, but does **not** skip
```sql
SELECT
  Country,
  -- Rank athletes in each country by the medals they've won
  Athlete,
  DENSE_RANK() OVER (PARTITION BY country
                ORDER BY Medals DESC) AS Rank_N
FROM Athlete_Medals
ORDER BY Country ASC, RANK_N ASC;
```
```
country	athlete	rank_n
JPN	KITAJIMA Kosuke	1
JPN	UCHIMURA Kohei	2
JPN	TAKEDA Miho	3
JPN	TACHIBANA Miya	3
JPN	IRIE Ryosuke	4
JPN	TOMITA Hiroyuki	4
JPN	SUZUKI Satomi	4
JPN	TANI Ryoko	4
JPN	KASHIMA Takehiro	4
JPN	YOSHIDA Saori	4
JPN	ICHO Kaori	4
JPN	MATSUDA Takeshi	4
JPN	UTSUGI Reika	5
JPN	YONEDA Isao	5
JPN	SATO Rie	5
JPN	MUROFUSHI Koji	5
...
```
## 03 Paging
- Splitting data into (approximately) equal chunks
- **Uses**
    - Many APIs return data in "pages" to reduce data being sent
    - Separating data into quartiles or thirds(top middle 33%, and bottom thirds) to judge performance.
- `NTILE(n)` splits the data into `n` approximately equal pages.

```sql
WITH Athlete_Medals AS (
  SELECT Athlete, COUNT(*) AS Medals
  FROM Summer_Medals
  GROUP BY Athlete
  HAVING COUNT(*) > 1),
  
  Thirds AS (
  SELECT
    Athlete,
    Medals,
    NTILE(3) OVER (ORDER BY Medals DESC) AS Third
  FROM Athlete_Medals)
  
SELECT
  -- Get the average medals earned in each third
  third,
  AVG(medals) AS Avg_Medals
FROM Thirds
GROUP BY Third
ORDER BY Third ASC;
```