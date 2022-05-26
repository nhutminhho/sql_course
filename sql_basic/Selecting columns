---
output:
  word_document: default
  html_document: default
---
# Selecting and summarizing columns
## Selecting Columns: SELECT, SELECT DISTINCT
- SELECT (single)
- SELECT (multiple)
- SELECT DISTINCT

#### EXERCISE: SINGLE COLUMN SELECT
Get the title of every film.
```sql
SELECT ___
FROM films;
```
```sql
SELECT title
FROM films;
```

Get all details for every film.
```sql
SELECT ___
FROM films;
```
```sql
SELECT *
FROM films;
```

Get the names of everyone involved in working on the films.
```sql
SELECT name
FROM people;
```

#### EXERCISE: MULTIPLE COLUMN SELECT
Get the title and release year of every film.
```sql
SELECT title, release_year
FROM films;
```

Get the title, release year and country for every film.
```sql
SELECT title, release_year, country
FROM films;
```

Get every person's name and their date of birth where possible.
```sql
SELECT name, birthdate
FROM people;
```

Get every person name and their date of death where possible.
```sql
SELECT name, deathdate
FROM people;
```

Get everyone's name, date of birth, and date of death (where possible).
```sql
SELECT name, birthdate, deathdate
FROM people;
```

### EXERCISE: SELECT DISTINCT
Get all the different countries.
```sql
SELECT ___ country
FROM films;
```
```sql
SELECT DISTINCT country
FROM films;
```

Get all the different film languages.
```sql
SELECT DISTINCT language
FROM films;
```

Get the different types of film roles.
```sql
SELECT DISTINCT role
FROM roles;
```

Get all the different certification categories.
```sql
SELECT DISTINCT certification
FROM films;
```

Get all the different IMDB scores - has any scored over 9.5?
```sql
SELECT DISTINCT imdb_score
FROM reviews;
```

## Aggregate Functions: COUNT, SUM, AVG, MIN, MAX
- COUNT
- COUNT DISTINCT
- COUNT(column_name) vs COUNT(\*)
- SUM (single)
- AVG (single)
- MIN (single)
- MAX (single)

#### EXERCISE: COUNT STAR, COUNT COLUMN AND COUNT DISTINCT
Count the number of rows in the people table.
```sql
SELECT COUNT(___)
FROM people;
```
```sql
SELECT COUNT(*)
FROM people;
```

Count the number of birthdate entries in the people table.
```sql
SELECT COUNT(birthdate)
FROM people;
```

Count the number of unique birthdate entries in the people table.
```sql
SELECT COUNT(DISTINCT birthdate)
FROM people;
```

Count the number of unique languages.
```sql
SELECT COUNT(DISTINCT language)
FROM films;
```

Count the number of unique countries.
```sql
SELECT COUNT(DISTINCT country)
FROM films;
```

Count the total number of people.
```sql
SELECT COUNT(*)
FROM people;
```

Count the number of people who have died.
```sql
SELECT COUNT(deathdate)
FROM people;
```

Count the number of years the dataset covers.
```sql
SELECT COUNT(DISTINCT release_year)
FROM films
```

### EXERCISE: SUM, AVG, MIN, MAX
Get the total duration of all films.
```sql
SELECT SUM(duration)
FROM films;
```

Get the average duration of all films.
```sql
SELECT AVG(duration)
FROM films;
```

Get the duration of the shortest film.
```sql
SELECT MIN(duration)
FROM films;
```

Get the amount made by the highest grossing film. **Note: even with currency stuff, it's Star Wars.**
```sql
SELECT MAX(gross)
FROM films;
```

Get the amount made by the lowest grossing film. **Note: http://www.imdb.com/title/tt1641841/?ref_=nv_sr_1 it's this, a movie from Thailand which cost $9,000,000 and made $162. USD Dollars.**
```sql
SELECT MIN(gross)
From films;
```

#### Aliasing and Basic Arithmetic: AS, +, -, \*, /, %
- AS
- Addition
- Subtraction
- Multiplication
- Division
- Modulo

### EXERCISE: SIMPLE ALIASING
Get the profit (or loss) for each movie, where possible.
```sql
SELECT title, gross - budget
___ profit_or_loss
FROM films;
```
```sql
SELECT title, gross - budget
AS profit_or_loss
FROM films;
```

Get the duration in hours for each film.
```sql
SELECT title, ___ AS duration_hours
FROM films;
```
```sql
SELECT title, duration / 60.0 AS duration_hours
FROM films;
```

Get the average film duration in hours.
```sql
SELECT AVG(duration) / 60.0
AS duration_hours  
FROM films;
```

Get the percentage of people who have died.
```sql
SELECT COUNT(___) * 100 / COUNT(*)
AS percentage_dead
FROM people;
```
```sql
SELECT COUNT(deathdate) * 100 / COUNT(*)
AS percentage_dead
FROM people;
```

Check if there's an even number of unique languages. (0 = yes, 1 = no)
```sql
SELECT COUNT(___ language) ___ 2
AS result
FROM films;
```
```sql
SELECT COUNT(DISTINCT language) % 2
AS result
FROM films;
```

Get the of years between the oldest film and newest film.
```sql
SELECT MAX(release_year) - MIN(release_year)
AS difference
FROM films;
```

Get the number of decades this dataset covers.
```sql
SELECT (MAX(release_year) - MIN(release_year)) / 10
AS number_of_decades
FROM films;
```

## Rounding Functions: ROUND, FLOOR, CEILING
- ROUND AVG
- FLOOR AVG
- CEILING AVG

### EXERCISE: ROUNDING
Get the average duration of all films, rounded to the nearest minute.
```sql
SELECT ROUND(AVG(duration))
AS rounded_avg_run_time
FROM films;
```

Get the average duration of all films, rounded down to nearest minute.
```sql
SELECT FLOOR(AVG(duration))
AS floored_avg_run_time
FROM films;
```

Get the average duration of all films, rounded up to the nearest minute.
```sql
SELECT CEILING(AVG(duration))
FROM films;
```
