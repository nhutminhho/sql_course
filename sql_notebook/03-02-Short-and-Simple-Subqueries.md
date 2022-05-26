### What is a subquery?
- A query nested inside another query
- Useful for intermediary transformations
- Can be in any part of a query
    - `SELECT`,`FROM`,`WHERE`,`GROUP BY`
- Can return a variety of information
    - Scalar quantities
    - A list
    - A table
### Why subqueries?
- Comparing groups to summarized values
- Reshaping data
    - *What is the highest monthly average of goals scored in the Bundesliga?*
- Combining data that cannot be joined
    - *How do you get both the home and away team names into a table of match results*

## 01 WHERE
```sql
SELECT 
	-- Select the date, home goals, and away goals scored
    date,
	home_goal,
	away_goal
FROM  matches_2013_2014
-- Filter for matches where total goals exceeds 3x the average
WHERE ( home_goal + away_goal) > 
       (SELECT 3 * AVG(home_goal + away_goal)
        FROM matches_2013_2014); 
```
```sql
SELECT 
	-- Select the team long and short names
	team_long_name,
	team_short_name
FROM team
-- Exclude all values from the subquery
WHERE team_api_id  NOT IN
     (SELECT DISTINCT hometeam_id  FROM match);
```

## 02 FROM
- Restructure and transform your data
    - Transforming data from long to wide before selecting
    - Prefiltering data
- Calculating aggregates of aggregates

- information about matches with 10 or more goals in total!
```sql
SELECT
	-- Select country name and the count match IDs
    c.name AS country_name,
    COUNT(sub.id) AS matches
FROM country AS c
-- Inner join the subquery onto country
-- Select the country id and match id columns
INNER JOIN (SELECT country_id, id 
           FROM match
           -- Filter the subquery by matches with 10+ goals
           WHERE (home_goal + away_goal) >= 10) AS sub
ON c.id = sub.country_id
GROUP BY country_name;
```

## 03 SELECT
- Returns a **single** value
    - Include aggregate values to compare to individual values
- Used in mathematical calculations
    - Deviation from the average
- Make sure have all filters in the right places
    - Properly filter both the main and the subquery
```sql
SELECT
	-- Select the league name and average goals scored
	l.name AS league,
	ROUND(AVG(m.home_goal + m.away_goal),2) AS avg_goals,
    -- Subtract the overall average from the league average
	ROUND(AVG(m.home_goal + m.away_goal) -
		(SELECT AVG(home_goal + away_goal)
		 FROM match 
         WHERE season = '2013/2014'),2) AS diff
FROM league AS l
LEFT JOIN match AS m
ON l.country_id = m.country_id
-- Only include 2013/2014 results
WHERE season = '2013/2014'
GROUP BY l.name;
```

## 04 Best practices
- Format queries
   - Line up `SELECT` ,`FROM`, `WHERE`, and `GROUP BY`
- Annotate queries
```sql
/* This query filters for col1 = 2
and only selects data from table1 */
SELECT
  col1,
  col2
FROM table1  -- this table has 10,000 rows
WHERE col1 = 2;
```
- Indent queries (subqueries!)
- Keep only necessary subqueries.
     - Computation intensive
- Properly filter each subquery.

