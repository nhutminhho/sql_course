## 01 Correlated subquery
- Use values from the *outer* query to generate a result
- Re-run for every row generated in the final dataset
- Used for advanced joining, filtering, and evaluating data
- Useful for matching data across multiple columns.
- **Significantly slows down query runtime**

```sql
SELECT
    m.date,
    (SELECT team_long_name
     FROM team AS t
     WHERE t.team_api_id = m.hometeam_id) AS hometeam,
    -- Connect the team to the match table
    (SELECT team_long_name
     FROM team AS t
     WHERE t.team_api_id = m.awayteam_id) AS awayteam,
    -- Select home and away goals
     home_goal,
     away_goal
FROM match AS m;
```

## 02 Nested subqueries
- Subquery inside another subquery
- Perform multiple layers of transformation
- Can be correlated or uncorrelated
```sql
SELECT
	-- Select the season and max goals scored in a match
	season,
    MAX(home_goal + away_goal) AS max_goals,
    -- Select the overall max goals scored in a match
   (SELECT MAX(home_goal + away_goal) FROM match) AS overall_max_goals,
   -- Select the max number of goals scored in any match in July
   (SELECT MAX(home_goal + away_goal) 
    FROM match
    WHERE id IN (
          SELECT id FROM match WHERE EXTRACT(MONTH FROM date) = 07)) AS july_max_goals
FROM match
GROUP BY season;
```

### 03 Common Table Expressions
- Table declared before the main query
- Named and referenced later in `FROM` statement

```sql
WITH cte AS (
    SELECT col1, col2
    FROM table)
SELECT
    AVG(col1) AS avg_col
FROM cte;
```

```sql
WITH home AS (
  SELECT m.id, m.date, 
  		 t.team_long_name AS hometeam, m.home_goal
  FROM match AS m
  LEFT JOIN team AS t 
  ON m.hometeam_id = t.team_api_id),
-- Declare and set up the away CTE
away AS (
  SELECT m.id, m.date, 
  		 t.team_long_name AS awayteam, m.away_goal
  FROM match AS m
  LEFT JOIN team AS t 
  ON m.awayteam_id = t.team_api_id)
-- Select date, home_goal, and away_goal
SELECT 
	home.date,
    home.hometeam,
    away.awayteam,
    home.home_goal,
    away.away_goal
-- Join away and home on the id column
FROM home
INNER JOIN away
ON home.id = away.id;
```

## 04 Deciding on techniques to use
### Joins
- Combine 2+ tables
    - Simple operations/ aggregations

### Correlated subqueries
- Match subqueries & table
    - Avoid limits of joins: join two separate columns in one table, to a single column in another at a time. 
    - High processing time
### Multiple/Nested Subqueries
- Multi-step transformations
    - Improve accuracy and reproducibility

### Common Tabel Expressions
- Organize subqueries sequentially
- Can reference other CTEs
