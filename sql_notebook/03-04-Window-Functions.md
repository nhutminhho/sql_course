## 01 OVER
### Working with aggregate values
- Requires you to use `GROUP BY` with **all** non-aggregate columns

### Window functions
- Perform calculations on an already generated result set (a window)
- Aggregate calculations
   - Similar to subqueries in `SELECT`
   - Running totals, rankings, moving averages
- Processed after every part of query except `ORDER BY`
- Window function will apply the filter included in `WHERE` clause
```sql
SELECT 
	-- Select the id, country name, season, home, and away goals
	m.id, 
    c.name AS country, 
    m.season,
	m.home_goal,
	m.away_goal,
    -- Use a window to include the aggregate average in each row
	AVG(m.home_goal + m.away_goal) OVER() AS overall_avg. # or you need to use a subqury
FROM match AS m
LEFT JOIN country AS c ON m.country_id = c.id;
```
### Generate a RANK
- `RANK() OVER(ORDER BY xx ) AS rank`
```python3
SELECT 
	-- Select the league name and average goals scored
	l.name AS league,
    AVG(m.home_goal + m.away_goal) AS avg_goals,
    -- Rank leagues in descending order by average goals
    RANK() OVER(ORDER BY AVG(m.home_goal + m.away_goal) DESC) AS league_rank
FROM league AS l
LEFT JOIN match AS m 
ON l.id = m.country_id
WHERE m.season = '2011/2012'
GROUP BY l.name
-- Order the query by the rank you created
-- ORDER BY league_rank;
```
## 02 PARTITION BY
- Calculate separate values for different categories
- Calculate different calculations in the same column
- Can partition data by 1 or more columns
- Can partition aggregate calculations, ranks, etc
```python3
SELECT 
	date,
	season,
	home_goal,
	away_goal,
	CASE WHEN hometeam_id = 8673 THEN 'home' 
         ELSE 'away' END AS warsaw_location,
	-- Calculate average goals partitioned by season and month
    AVG(home_goal) OVER(PARTITION BY season, 
         	EXTRACT(month FROM date)) AS season_mo_home,
    AVG(away_goal) OVER(PARTITION BY season, 
            EXTRACT(month FROM date)) AS season_mo_away
FROM match
WHERE 
	hometeam_id = 8673 
    OR awayteam_id = 8673
ORDER BY (home_goal + away_goal) DESC;
```

## 03 Sliding windows
- Running totals, sums, averages, etc
- Can be partitioned by one or more columns
```sql
 ROWS BETWEEN <start> AND <finish>
```
```
PRECEDING   # to specify the number of rows before, or after, the current row that you want to include in a calculation
FOLLOWING
UNBOUNDED PRECEDING # include every row since the beginning, or the end, of the data set in your calculations. 
UNBOUNDED FOLLOWING
CURRENT ROW
```
```sql
SELECT 
	-- Select the date, home goal, and away goals
	date,
    home_goal,
    away_goal,
    -- Create a running total and running average of home goals
    SUM(home_goal) OVER(ORDER BY date DESC
         ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS running_total,
    AVG(home_goal) OVER(ORDER BY date DESC
         ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS running_avg
FROM match
WHERE 
	awayteam_id = 9908 
    AND season = '2011/2012';
```