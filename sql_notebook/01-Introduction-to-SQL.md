## 01 Selecting columns
> In SQL, keywords are not case-sensitive. BUT it's good practice to make SQL keywords uppercase to distinguish them from other parts of your query, like column and table names.
- `SELECT * FROM table_names`
- SELECT DISTINCT : select **unique** values in a certain column
```sql
SELECT DISTINCT country
FROM films;
```
- COUNT : return the number of rows in one or more cols
     - `COUNT(*)` tells you how many rows are in a table. However, if you want to count the number of non-missing values in a particular column, you can call `COUNT` on just that column.
```sql
SELECT COUNT(DISTINCT(country))
FROM films;
```

## 02 Filtering rows
- WHERE
- WHERE AND , WHERE OR
    - Be sure to enclose individual clauses in parentheses when combining 'AND' & 'OR'
```sql
SELECT title, release_year
FROM films
WHERE (release_year >= 1990 AND release_year < 2000)
AND (language = 'French' OR language = 'Spanish') 
AND gross > 2000000;
```
- BETWEEN 
```sql
SELECT title, release_year
FROM films
WHERE release_year BETWEEN 1990 AND 2000;
```
- WHERE IN (inclusive)
```sql
SELECT title, release_year
FROM films
WHERE release_year IN (1990, 2000)
AND duration > 120;
```
- IS NULL, IS NOT NULL
- LIKE, NOT LIKE
    - `LIKE 'Data%'` % matches 0/1/many characters
    - `LIKE 'DataC_mp'` _ matches a single character

## 03 Aggregate Functions
- AVG, SUM, MAX, MIN
- A note on arithmetic
     - SQL assumes that if you divide an integer by an integer, you want to get an integer back. So be careful when dividing!
     - To get more precision, add decimal places to your number
```sql
SELECT (4.0/3.0) AS results;
```
- AS: to add aliases

## 04 Sorting and grouping
- ORDER BY
    - DESC
- GROUP BY
- HAVING
    - Use `HAVING` clause when you need to filter based on the result of an aggregate function.
    - Aggregate functions can **NOT** be used in `WHERE` clauses.
```sql
SELECT release_year
FROM films
GROUP BY release_year
HAVING COUNT(title) > 10;
```
- LIMIT : to limit the number of rows returned