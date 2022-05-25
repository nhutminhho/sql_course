# Sorting and Grouping
## Sorting: ORDER BY
- (single)
- ORDER BY (multiple)
- ORDER BY DESC (single)
- ORDER BY DESC (multiple)

### EXERCISE: ORDER BY SINGLE COLUMN
Get people, sort by name.
```sql
SELECT name
FROM people ___ name;
```
```sql
SELECT name
FROM people ORDER BY name;
```

Get people, in order of when they were born.
```sql
SELECT birthdate, name
FROM people
___ ___ ___ ;
```
```sql
SELECT birthdate, name
FROM people
ORDER BY birthdate;
```

Get films released in 2000 or 2015, in the order they were released.
```sql
SELECT title, release_year
FROM films
WHERE release_year in (2000, 2015)
ORDER BY release_year;
```

Get all films except those released in 2015, order them so we can see results.
```sql
SELECT ___
FROM films
WHERE release_year ___ 2015
___ ___  release_year;
```
```sql
SELECT *
FROM films
WHERE release_year <> 2015
ORDER BY release_year;
```

### EXERCISE: ORDER BY SINGLE COLUMN DESC
Get the score and film id for every film, from highest to lowest.
```sql
SELECT imdb_score, film_id
FROM reviews
___ ___ imdb_score ___;
```
```sql
SELECT imdb_score, film_id
FROM reviews
ORDER BY imdb_score DESC;
```

Get the titles of films in reverse order.
```sql
SELECT *
FROM films
ORDER BY title DESC;
```

### EXERCISE: ORDER BY MULTIPLE COLUMNS
Get people, in order of when they were born, and alphabetical order.
```sql
SELECT birthdate, name
FROM people
___ ___ birthdate, ___;
```
```sql
SELECT birthdate, name
FROM people
ORDER BY birthdate, name;
```

Get films from in 2000 or 2015, sorted in the order they were released, and how long they were.
```sql
SELECT release_year, duration, title
FROM films
___ release_year ___ (2000, 2015)
___ ___ release_year, ___;
```
```sql
SELECT release_year, duration, title
FROM films
WHERE release_year IN (2000, 2015)
ORDER BY release_year, duration;
```

Get films between 2000 and 2015, sorted by certification and the year they were released.
```sql
SELECT certification, release_year, title
FROM films
WHERE release_year IN (2000, 2015)
ORDER BY certification, release_year;
```

Get people whose names start with A, B or C, (redundantly) ordered.
```sql
SELECT name, birthdate
FROM people
WHERE name ___ 'A%' OR name ___ 'B%' ___ name LIKE 'C%'
ORDER BY birthdate;
```
```sql
SELECT name, birthdate
FROM people
WHERE name LIKE 'A%' OR name LIKE 'B%' OR name LIKE 'C%'
ORDER BY birthdate;
```

## Grouping: GROUP BY, HAVING
- GROUP BY (single)
- GROUP BY (multiple)
- GROUP BY then ORDER BY
- GROUP BY with COUNT
- GROUP BY with SUM
- GROUP BY with AVG
- GROUP BY with MIN
- GROUP BY with MAX
- HAVING

### EXERCISE: GROUP BY SINGLE COLUMN
Get count of films made in each year.
```sql
SELECT release_year, ___(release_year)
FROM films
___ ___ release_year;
```
```sql
SELECT release_year, COUNT(release_year)
FROM films
GROUP BY release_year;
```

Get count of films, group by release year then order by release year.
```sql
SELECT release_year, COUNT(title) as films_released
FROM films
___ ___ release_year
___ ___ release_year;
```
```sql
SELECT release_year, COUNT(title) as films_released
FROM films
GROUP BY release_year
ORDER BY release_year;
```

Get count of films released in each year, ordered by count, lowest to highest.
```sql
SELECT release_year, COUNT(title) AS films_released
FROM films
___ ___ release_year
ORDER BY ___;
```
```sql
SELECT release_year, COUNT(title) AS films_released
FROM films
GROUP BY release_year
ORDER BY films_released;
```

Get count of films released in each year, ordered by count highest to lowest.
```sql
SELECT release_year, COUNT(title) AS films_released
FROM films
___ ___ release_year
___ ___ films_released ___;
```
```sql
SELECT release_year, COUNT(title) AS films_released
FROM films
GROUP BY release_year
ORDER BY films_released DESC;
```

Get lowest box office earnings per year.
```sql
SELECT release_year, ___(___)
___ films
GROUP BY release_year
___ BY release_year;
```
```sql
SELECT release_year, MIN(gross)
FROM films
GROUP BY release_year
ORDER BY release_year;
```

Get the total amount made in each language.
```sql
SELECT language, SUM(gross)
FROM films
___ ___ ___;
```
```sql
SELECT language, SUM(gross)
FROM films
GROUP BY language;
```

Get the total amount spent by each country.
```sql
SELECT country, SUM(gross)
FROM films
GROUP BY country;
```
```sql
SELECT country, SUM(gross)
FROM films
___ ___ ___;
```



### EXERCISE: GROUP BY MULTIPLE COLUMNS
Get the most spent making a film for each year, for each country.
```sql
SELECT release_year, country, MAX(budget)
FROM films
GROUP BY ___, ___
ORDER BY release_year, country;
```
```sql
SELECT release_year, country, MAX(budget)
FROM films
GROUP BY release_year, country
ORDER BY release_year, country;
```

Get the lowest box office made by each country in each year.
```sql
SELECT release_year, country, MIN(gross)
FROM films
___ ___  release_year, country
___ ___ release_year, country;
```
```sql
SELECT release_year, country, MIN(gross)
FROM films
GROUP BY release_year, country
ORDER BY release_year, country;
```

### EXERCISE: HAVING
Get the rounded average budget and average box office earnings for movies since 1990, but only if the average budget was greater than $60m in that year (Double check this).
```sql
SELECT release_year, ___(___(budget)) AS avg_budget, ROUND(AVG(gross)) AS avg_box_office
FROM films
___ release_year > 1990
___ BY release_year
HAVING AVG(budget) > 20000000
___ BY release_year DESC;
```
```sql
SELECT release_year, ROUND(AVG(budget)) AS avg_budget, ROUND(AVG(gross)) AS avg_box_office
FROM films
WHERE release_year > 1990
GROUP BY release_year
HAVING AVG(budget) > 20000000
ORDER BY release_year DESC;
```

Get the name, average budget, average box office take of countries who have made more than 10 films. Order by name, and get the top five.
```sql
SELECT country, ROUND(AVG(budget)) AS avg_budget, ROUND(AVG(gross)) AS avg_box_office
FROM films
___ BY country
HAVING COUNT(title) ___ 10
___ BY country
LIMIT 5;
```
```sql
SELECT country, ROUND(AVG(budget)) AS avg_budget, ROUND(AVG(gross)) AS avg_box_office
FROM films
GROUP BY country
HAVING COUNT(title) > 10
ORDER BY country
LIMIT 5;
```

##### SIMPLE HAVING EXERCISE
