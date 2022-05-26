## 01 Pivoting
- Enter CROSSTAB
```sql
CREATE EXTENSION IF NOT EXISTS tablefunc;

SELECT * FROM CROSSTAB($$
   source_sql TEXT
$$) AS ct (column_1 DATA_TYPE_1,
           ...,
           column_n DATA_TYPE_N);
```
- Pivoting with ranking
```sql
CREATE EXTENSION IF NOT EXISTS tablefunc;

SELECT * FROM CROSSTAB($$
  WITH Country_Awards AS (
    SELECT
      Country,
      Year,
      COUNT(*) AS Awards
    FROM Summer_Medals
    WHERE
      Country IN ('FRA', 'GBR', 'GER')
      AND Year IN (2004, 2008, 2012)
      AND Medal = 'Gold'
    GROUP BY Country, Year)

  SELECT
    Country,
    Year,
    RANK() OVER
      (PARTITION BY Year
       ORDER BY Awards DESC) :: INTEGER AS rank
  FROM Country_Awards
  ORDER BY Country ASC, Year ASC;
-- Fill in the correct column names for the pivoted table
$$) AS ct (Country VARCHAR,
           "2004" INTEGER,
           "2008" INTEGER,
           "2012" INTEGER)

Order by Country ASC;
```
```
country	2004	2008	2012
FRA	2	3	3
GBR	3	2	1
GER	1	1	2
```

## 02 ROLLUP and CUBE
- Group-level totals
### ROLLUP
- `ROLLUP` is a `GROUP BY` subclause that includes extra row for group-level aggregations
- `GROUP BY country, ROLLUP(medal)` will count all `country`- and `medal`-level totals, then count only `country`-level totals and fill in `medal` with `null`s for these rows.
- Hierarchical, de-aggregating from the leftmost provided column to the right-most
    - `ROLLUP(country, medal)` includes `Country`-level totals
    - `ROLLUP(medal, country)` includes `medal`-level totals
    - Both include grand totals
- Use with hierarchical data(e.g. date parts)

### CUBE
- non-hierarchical `ROLLUP`
- It generates all possible group-level aggregations
    - `CUBE(country, medal)` counts `country`-level, `medal`-level, and grand totals 

```sql
-- Count the medals per country and medal type
SELECT
  gender,
  medal,
  COUNT(*) AS Awards
FROM Summer_Medals
WHERE
  Year = 2012
  AND Country = 'RUS'
-- Get all possible group-level subtotals
GROUP BY CUBE(gender, medal)
ORDER BY Gender ASC, Medal ASC;
```
```
gender	medal	awards
Men	Bronze	34
Men	Gold	23
Men	Silver	7
Men	null	64
Women	Bronze	17
Women	Gold	24
Women	Silver	25
Women	null	66
null	Bronze	51
null	Gold	47
null	Silver	32
null	null	130
```

## 03 COALESCE and STRING_AGG
### COALESCE
- `COALESCE()` takes a list of values and returns the first non-`null` value, going from left to right
- `COALESCE(null, null, 1, null, 2)? 1`
- USEFUL when using SQL operations that return `null`
    - `ROLLUP` & `CUBE`
    - Pivoting
    - `LAG` & `LEAD`
```sql
SELECT
  -- Replace the nulls in the columns with meaningful text
  COALESCE(Country, 'All countries') AS Country,
  COALESCE(Gender, 'All genders') AS Gender,
  COUNT(*) AS Awards
FROM Summer_Medals
WHERE
  Year = 2004
  AND Medal = 'Gold'
  AND Country IN ('DEN', 'NOR', 'SWE')
GROUP BY ROLLUP(Country, Gender)
ORDER BY Country ASC, Gender ASC;
```
### STRING_AGG to compress data
- `STRING_AGG(col, separator)` takes all the values of a column and concatenates them, with `separator` in between each value
```sql
WITH Country_Medals AS (
  SELECT
    Country,
    COUNT(*) AS Medals
  FROM Summer_Medals
  WHERE Year = 2000
    AND Medal = 'Gold'
  GROUP BY Country),

  Country_Ranks AS (
  SELECT
    Country,
    RANK() OVER (ORDER BY Medals DESC) AS Rank
  FROM Country_Medals
  ORDER BY Rank ASC)

-- Compress the countries column
SELECT STRING_AGG(Country, ', ')
FROM Country_Ranks
-- Select only the top three ranks
WHERE Rank <= 3;
``` 