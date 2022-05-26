## 01 INNER JOIN
- KEYWORD : `INNER JOIN` & `ON`
- JOIN two or more tables, ON one or more conditions(`AND`)
- Use alias to save time
```sql
-- 6. Select fields
SELECT c.code, name, region, e.year, fertility_rate, unemployment_rate
  -- 1. From countries (alias as c)
  FROM countries AS c
  -- 2. Join to populations (as p)
  INNER JOIN populations AS p
    -- 3. Match on country code
    ON c.code = p.country_code
  -- 4. Join to economies (as e)
  INNER JOIN economies AS e
    -- 5. Match on country code and year
    ON c.code = e.code AND p.year = e.year;
```
### INNER JOIN via USING
- When the key fields in the two tables share the same name, we can use `USING ()` instead on `ON`.
```sql
-- 4. Select fields
SELECT c.name AS country,
      continent, l.name as language, official
  -- 1. From countries (alias as c)
  FROM countries AS c
  -- 2. Join to languages (as l)
  INNER JOIN languages AS l
    -- 3. Match using code
    USING (code);
```
### Self-join
- Compare values in a field to other values of the same field from within the same table.
- In the following exercise, compare population in 2010 with that in 2015 (in the same table).
```sql
SELECT p1.country_code,
       p1.size AS size2010, 
       p2.size AS size2015,
       -- 1. calculate growth_perc
       ((p2.size - p1.size)/p1.size * 100.0) AS growth_perc
-- 2. From populations (alias as p1)
FROM populations AS p1
  -- 3. Join to itself (alias as p2)
  INNER JOIN populations AS p2
    -- 4. Match on country code
    ON p1.country_code = p2.country_code
        -- 5. and year (with calculation)
        AND p1.year = p2.year - 5;
```

### CASE WHEN AND THEN
- multiple if-then-else statement
- Often it's useful to look at a numerical field not as raw data, but instead as being in different categories or groups.
- `INTO` to create a new table
```python3
SELECT country_code, size,
  CASE WHEN size > 50000000
            THEN 'large'
       WHEN size > 1000000
            THEN 'medium'
       ELSE 'small' END
       AS popsize_group
INTO pop_plus       
FROM populations
WHERE year = 2015;
```

## 02 OUTER JOINs and CROSS JOINs
- LEFT & RIGHT JOIN
- FULL JOIN
- CROSS JOIN
    - All possible combination of the two tables.
    - A CROSS JOIN clause allows you to produce a Cartesian Product of rows in two or more tables.
    - Do not use `ON` or `USING`
```sql
SELECT select_list
FROM T1
CROSS JOIN T2;
```

## 03 Set theory clauses
- UNION
    - To combine the result sets of two queries using the UNION operator, the queries must conform to the following rules:
    1. The number and the order of the columns in the select list of both queries must be the same.
    2. The data types must be compatible.
    - The UNION operator removes all duplicate rows from the combined data set. To retain the duplicate rows, you use the the UNION ALL instead.
```sql
-- Select field
SELECT country_code
  -- From cities
  FROM cities
	-- Set theory clause
	UNION
-- Select field
SELECT code
  -- From currencies
  FROM currencies
-- Order by country_code
ORDER BY country_code;
```
- UNION ALL
- INTERSECT
    -  looking at the records in common for country code and year for the economies and populations tables.
```sql
-- Select fields
SELECT code, year
  -- From economies
  FROM economies
	-- Set theory clause
	INTERSECT
-- Select fields
SELECT country_code, year
  -- From populations
  FROM populations
-- Order by code and year
ORDER BY code,year;
```
- EXCEPT
     - Determine the names of capital cities that are not listed in the cities table.
```sql
-- Select field
SELECT capital
  -- From countries
  FROM countries
	-- Set theory clause
	EXCEPT
-- Select field
SELECT name
  -- From cities
  FROM cities
-- Order by ascending capital
ORDER BY capital;
```
### Semi-joins & anti-joins
- Use a right table to determine which records to keep in the left table.
```sql
-- Select distinct fields
SELECT DISTINCT(name)
  -- From languages
  FROM languages
-- Where in statement
WHERE code IN
  -- Subquery
  (SELECT code
   FROM countries
   WHERE region = 'Middle East')
-- Order by name
ORDER BY name;
```
- anti : NOT IN
    - Anti-joins are particularly useful in diagnosing problems with other joins in terms of getting fewer or more records than you expected. 
## 04 Subqueries
- in `WHERE` (most common) , `SELECT`, `FROM`
### inside `WHERE`
- get the urban area population for only capital cities.
```sql
-- 2. Select fields
SELECT name, country_code, urbanarea_pop
  -- 3. From cities
  FROM cities
-- 4. Where city name in the field of capital cities
WHERE name IN
  -- 1. Subquery
  (SELECT capital
   FROM countries)
ORDER BY urbanarea_pop DESC;
```
### inside `SELECT`
> In this exercise, you'll see how some queries can be written using either a join or a subquery.
> The code given in query.sql selects the top nine countries in terms of number of cities appearing in the cities table. 
```sql
SELECT countries.name AS country, COUNT(*) AS cities_num
  FROM cities
    INNER JOIN countries
    ON countries.code = cities.country_code
GROUP BY country
ORDER BY cities_num DESC, country
LIMIT 9;
```
```sql
SELECT countries.name AS country,
  (SELECT COUNT(*)
   FROM cities
   WHERE countries.code = cities.country_code) AS cities_num
FROM countries
ORDER BY cities_num DESC, country
LIMIT 9;
```
### inside `FROM`
```sql
-- Select fields
SELECT name, continent, inflation_rate
  -- From countries
  FROM countries
	-- Join to economies
	INNER JOIN economies
	-- Match on code
	USING(code)
  -- Where year is 2015
  WHERE year = 2015
    -- And inflation rate in subquery (alias as subquery)
    AND inflation_rate IN (
        SELECT MAX(inflation_rate) AS max_inf
        FROM (
             SELECT name, continent, inflation_rate
             FROM countries
             INNER JOIN economies
             USING(code)
             WHERE year = 2015) AS subquery
      -- Group by continent
        GROUP BY continent);
```