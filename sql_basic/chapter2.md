# Filtering Rows
## Filtering: WHERE, =, <>, <, <=, >, >=, AND, OR
- WHERE =
- WHERE <>
- WHERE <=
- WHERE >=
- WHERE = x OR = y  
- WHERE > x AND < y

### EXERCISE: SIMPLE FILTERING WITH WHERE (STRINGS)
Get all French language films.
```sql
SELECT ___
FROM films
___ language = ___;
```
```sql
SELECT *
FROM films
WHERE language = 'French';
```

Get the name of the person born on November 11th, 1974.
```sql
SELECT name, birthdate
FROM people
___ birthdate = ___;
```
```sql
SELECT name, birthdate
FROM people
WHERE birthdate = '1974-11-11';
```

Get the number of Hindi movies.
```sql
SELECT COUNT(___)
FROM films
___ language = ___;
```
```sql
SELECT COUNT(*)
FROM films
WHERE language = 'Hindi';
```

Get all movies with an R certification.
```sql
SELECT *
FROM films
WHERE certification = 'R';
```

Get all films released in 2016.
```sql
SELECT *
FROM films
WHERE release_year = 2016;
```

Get the number of films released before 2000.
```sql
SELECT COUNT(*)
FROM films
WHERE release_year < 2000;
```

Get the title and release year of films released since 2000.
```sql
SELECT title, release_year
FROM films
WHERE release_year > 2000;
```

### EXERCISE: WHERE AND
Get all Spanish films released before 2000.
```sql
SELECT title, release_year
FROM films
___ release_year < ___
___ language = ___;
```
```sql
SELECT title, release_year
FROM films
WHERE release_year < 2000
AND language = 'Spanish';
```

Get the all Spanish films released since 2000.
```sql
SELECT *
FROM films
WHERE release_year > 2000
AND language = 'Spanish';
```

Get average duration for films released in France in 1992.
```sql
SELECT ___(duration)
FROM films
___ release_year = 1992
___ country = 'France';
```
```sql
SELECT AVG(duration)
FROM films
WHERE release_year = 1992
AND country = 'France';
```

### EXERCISE: WHERE AND, OR
Get films released in 1990 or released in 2000 in English or Spanish.
```sql
SELECT title, release_year
FROM films
WHERE ___ = 1990 OR release_year = 2000
___ ___ = 'French' ___ language = ___;
```
```sql
SELECT title, release_year
FROM films
WHERE release_year = 1990 OR release_year = 2000
AND language = 'French' OR language = 'Spanish';
```

Get films released since 2000 that are in French or Spanish, and made more than $20m.
```sql
SELECT ___
FROM films
WHERE release_year > 2000
___ language = 'French' ___ language = 'Spanish'
AND ___ > 20000000;
```
```sql
SELECT *
FROM films
WHERE release_year > 2000
AND language = 'French' OR language = 'Spanish'
AND gross > 20000000;
```

Get films released in the 90s.
```sql
SELECT title, release_year
FROM films
___ release_year >= 1990 __ release_year <= 2000;
```
```sql
SELECT title, release_year
FROM films
WHERE release_year >= 1990 AND release_year <= 2000;
```

Get average duration for films released in the UK or which were released in 2012.
```sql
SELECT AVG(duration)
FROM films
AS average_duration
WHERE release_year = 2012
OR COUNTRY = 'UK';
```
```sql
SELECT ___(duration)
FROM films
___ average_duration
___ release_year ___ 2012
___ COUNTRY = 'UK';
```

## Advanced Filtering: BETWEEN, IN, IS NULL, IS NOT NULL, LIKE, NOT LIKE
- BETWEEN x AND y
- WHERE x IS NULL
- WHERE x IS NOT NULL
- LIKE
- NOT LIKE

### EXERCISE: BETWEEN
Get films released in the 90s.
```sql
SELECT title, release_year
FROM films
WHERE release_year ___ 1990 ___ 2000;
```
```sql
SELECT title, release_year
FROM films
WHERE release_year BETWEEN 1990 AND 2000;
```

Get the number of films released in the 90s.
```sql
SELECT COUNT(___)
FROM films
WHERE ___ BETWEEN 1990 AND 2000;
```
```sql
SELECT COUNT(*)
FROM films
WHERE release_year BETWEEN 1990 AND 2000;
```


Get the number of films made between 2000 and 2015 with budgets over $100 million.
```sql
SELECT title, budget
___ films
WHERE release_year
___ 2000 ___ 2015
AND ___ > ___;
```
```sql
SELECT title, budget
FROM films
WHERE release_year
BETWEEN 2000 AND 2015
AND budget > 100000000;
```

### EXERCISE: IN
Get films released in  in 1990 or released in 2000 that were longer than two hours.
```sql
SELECT title, release_year
FROM films
WHERE release_year IN (___, ___)
___ duration > 120;
```
```sql
SELECT title, release_year
FROM films
WHERE release_year IN (1990, 2000)
AND duration > 120;
```

### EXERCISE: BETWEEN AND AND
Get the number of films released between 2000 and 2015 that were longer than two hours.
```sql
SELECT COUNT(*)
FROM films
WHERE release_year BETWEEN 2000 AND 2015
AND duration > 120;
```

### EXERCISE: IS NULL
Get the names of people who are still alive.
```sql
SELECT name
FROM people
___ deathdate IS ___;
```
```sql
SELECT name
FROM people
WHERE deathdate IS NULL;
```

### EXERCISE: LIKE AND NOT LIKE
Get people whose names begin with 'B'.
```sql
SELECT name
FROM people
___ ___ ___ 'B%';
```
```sql
SELECT name
FROM people
WHERE name LIKE 'B%';
```

Get people whose names begin with 'Br'.
```sql
SELECT name
FROM people
WHERE name LIKE 'Br%';
```

Get people whose names have 'r' as the second letter.
```sql
SELECT name
FROM people
WHERE name LIKE '_r%';
```

Get people whose names don't start with A.
```sql
SELECT name
FROM people
WHERE name NOT LIKE 'A%';
```
