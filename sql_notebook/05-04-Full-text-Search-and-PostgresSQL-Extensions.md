## 01 Full-text Search
**Full text search provides a means for performing natural language queries of text data in your database.** 
- Stemming
- Spelling mistakes
- Ranking
```sql
SELECT title, description
FROM film
WHERE to_tsvector(title) @@ to_tsquery('elf')
```
## 02 Extending PostgreSQL
### User-defined data types
**Enumerated data types**
```sql
CREATE TYPE dayofweek AS ENUM (
    'Monday',
    'Tuesday',
    'Wednesday',
    'Thursday',
    'Friday',
    'Saturday',
    'Sunday'
);
```
### Getting information about user-defined data types
- from `pg_type`  
```sql
SELECT typname, typcategory
FROM pg_type
WHERE typname = 'dayofweek'
```
```
typname     typcategory
dayofweek   E
```
- from `INFORMATION_SCHEMA.COLUMNS`
```sql
SELECT column_name, data_type, udt_name
FROM INFORMATION_SCHEMA.COLUMNS
WHERE table_name='film'
```
```
column_name  data_type     udt_name
rating       USER-DEFINED  mpaa_rating
```
### User-defined functions
```sql
CREATE FUNCTION squared(i integer) RETURNS integer AS $$
    BEGIN
        RETURN i * i;
    END;
$$ LANGUAGE plpgsql;
```
```sql
SELECT squared(10);
```
## 03 Improving full text search with extensions
### Commonly used extensions
- PostGIS
- PostPic
- **fuzzystrmatch**
- **pg_trgm**
### Querying extension meta data
- Available Extensions
```sql
SELECT name
FROM pg_available_extensions;
```
- Installed Extensions
```sql
SELECT extname
FROM pg_extensions;
```
### Enable extensions
```sql
CREATE EXTENSION IF NOT EXISTS fuzzystrmatch;
```
### Using fuzzystrmatch or fuzzy searching
- The levenshtein function calculates the levenshtein distance between two strings which is the number of edits required for the strings to be a perfect match.
```sql
SELECT levenshitein('GUMBO', 'GAMBOL');
```
```
levenshitein
2
```
### Compare two strings with pg_trgm
-  Trigrams are groups of 3 consecutive characters in a string and based on the number of matching trigrams in two strings will provide a measurement of how similar they are
```sql
SELECT similarity('GUMBO', 'GAMBOL');
```
