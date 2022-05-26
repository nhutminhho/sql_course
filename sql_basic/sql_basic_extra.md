### LEFTOVER EXERCISES
Get both the lowest and highest grossing films, for comparision.
```sql
SELECT MIN(gross), MAX(gross)
FROM films;
```

Get the highest number of Facebook likes for any film.
```sql
SELECT MAX(facebook_likes)
FROM reviews;
```

Get the bottom ten lowest box office take per country.
```sql
SELECT country, MIN(gross)
FROM films
GROUP BY country
ORDER BY min DESC;
```

Get the average amount made by each country.
```sql
SELECT country, AVG(gross)
FROM films
GROUP BY country;
```

Get the total amount made by the bottom ten countries.
```sql
SELECT country, SUM(gross)
FROM films
GROUP BY country
ORDER BY sum
LIMIT 10;
```

Get the total amount spent by the bottom ten countries.
```sql
SELECT country, SUM(budget)
FROM films
GROUP BY country
ORDER BY sum
LIMIT 10;
```

Get rounded average box office earnings per year.
```sql
SELECT release_year, ROUND(AVG(gross))
FROM films
GROUP BY release_year
ORDER BY release_year;
```

Get lowest and highest box office earnings per year.
```sql
SELECT release_year, MIN(gross), MAX(gross)
FROM films
GROUP BY release_year
ORDER BY release_year DESC;
```

**Note for the following two, we will need to filter for non NULL, which might be more complex than it seems.**
Get the highest box office take per country.
```sql
SELECT country, MAX(gross)
FROM films
GROUP BY country;
```

###### Some Extra Exercises
Count of movies not rated.
```sql
SELECT COUNT(*)
FROM films
WHERE certification = 'Not Rated' OR certification IS NULL;
```

Count of movies not in English.
```sql
SELECT COUNT(*)
FROM films
WHERE language <> 'English';

```

Number of movies in black and white.
```sql
SELECT COUNT(*)
FROM films
WHERE color = 'Black and White';
```

Highest grossing per certification.
```sql
SELECT certification, MAX(gross)
FROM films
GROUP BY certification
ORDER BY max DESC;
```

Count of films in each certification bracket.
```sql
SELECT certification, COUNT(title)
FROM films
GROUP BY certification
ORDER BY count DESC;
```

Country with most R-Rated films.
```sql
SELECT country, COUNT(certification)
FROM films
WHERE certification = 'R'
GROUP BY country
ORDER BY count DESC;
```

Longest duration per year.
```sql
SELECT release_year, MAX(duration) AS max_duration
FROM films
GROUP BY release_year
ORDER BY max_duration DESC;
```

Count of films made per country.
```sql
SELECT country, COUNT(title)
FROM films
GROUP BY country
ORDER BY count DESC;
```

Count of user reviews vs critic reviews.
```sql
SELECT COUNT(num_user) AS count_users, COUNT(num_critic) AS count_critics
FROM reviews;
```

Count of actors.
```sql
SELECT COUNT(*)
FROM roles
WHERE role = 'actor';
```

Count of directors.
```sql
SELECT COUNT(*)
FROM roles
WHERE role = 'director';
```

## Subqueries
- Subqueries in one table
Get the title, duration and release year of the shortest film(s).
```sql
SELECT title, duration, release_year
FROM films
WHERE duration = (
  SELECT MIN(duration) FROM films
);
```

Get the title, duration and release year of the longest film(s).
```sql
SELECT title, duration, release_year
FROM films
WHERE duration = (
  SELECT MAX(duration)
  FROM films
);
```

Get the title, release_year and box office take for the highest grossing film.
```sql
SELECT title, release_year, gross
FROM films
WHERE gross = (
  SELECT MAX(gross)
  FROM films
);
```

Get the title, release_year and box office take for the lowest grossing film.
```sql
SELECT title, release_year, gross
FROM films
WHERE gross = (
  SELECT MIN(gross)
  FROM films
);
```

Get the duration of the longest movie made in the USA.
```sql
SELECT title, duration
FROM films
WHERE duration = (
  SELECT MAX(duration)
  FROM films
  WHERE country = 'USA'
);
```

Get details for the film with the lowest box office earnings per year.
```sql
SELECT release_year, title, gross
FROM films
WHERE release_year IN (
  SELECT release_year
  FROM films
  WHERE gross IN (
    SELECT MIN(gross)
    FROM films
    GROUP BY release_year
  )
);
```

Get details for the film with the highest box office earnings per year.
```sql
SELECT release_year, title, gross
FROM films
WHERE release_year IN (
  SELECT release_year
  FROM films
  WHERE gross IN (
    SELECT MAX(gross)
    FROM films
    GROUP BY release_year
  )
);
```

### IDEAS
Film with least reviews.
```sql
```

Film with most votes.
```sql
```

Film with least votes.
```sql
```

Film with most facebook likes.
```sql
```

Number of days worth of film time.
```sql
```

The cost of a film per minute per film.
```sql
```

The average cost of a film per minute.
```sql
```
