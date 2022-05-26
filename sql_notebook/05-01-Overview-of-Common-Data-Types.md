## 01 Common data types
- Text data types
     - `CHAR`, `CARCHAR` and `TEXT`
- Numeric data types
     - `INT` and `DECIMAL`
- Date / time data types
     - `DATE`, `TIME`, `TIMESTAMP`, `INTERVAL`
- Arrays

### Getting information about your database
- PostgreSQL has a system database called `INFORMATION_SCHEMA` that allows us to extract information about objects, including tables, in our database.
- `INFORMATION_SCHEMA.TABLES` `INFORMATION_SCHEMA.COLUMNS`

### Determining data types from existing tables
```sql
-- Get the column name and data type
SELECT
 	column_name, 
    data_type
-- From the system database information schema
FROM INFORMATION_SCHEMA.COLUMNS 
-- For the customer table
WHERE table_name = 'customer';
```

## 02 Date and time types
#### TIMESTAMP  data types
- ISO 8601 format: yyyy-mm-dd  
- microsecond precision
### DATE and TIME data types
- `TIME` data types can be stored with a timezone but they are stored without a timezone by default.
### INTERVAL data types
```sql
SELECT
 	-- Select the rental and return dates
	rental_date,
	return_date,
 	-- Calculate the expected_return_date
	rental_date + INTERVAL '3 days' AS expected_return_date
FROM rental;
```
## 03 Working with ARRAYs
### CREATE TABLE & INSERT
```sql
CREATE TABLE my_first_table (
     first_column text,
     second_column integer
);

INSERT INTO my_first_table
     (first_column, second_column) VALUES ('text value' , 12) ;

### CREATE TABLE & INSERT with ARRAY
```sql
CREATE TABLE grades (
    student_id int,
    email text[][],
    test_scores int[]
);
```
```sql
INSERT INTO grades
    VALUES (1,
    '{{"work", "work1@datacamp.com"}, {"other", "other1@datacamp.com"}}',
    '{92,85,96,88}' );
```
### Accessing ARRAYS
- *PostgreSQL array indexes start with 1*
```sql
SELECT
    email[1][1] AS type,
    email[1][2] AS address,
    test_scores[1],
FROM grades;
```

### ARRAY functions and operators
- The `ANY` function allows you to search for a value in any index position of an ARRAY. 
```
WHERE 'search text' = ANY(array_name)
```
```sql
SELECT
  title, 
  special_features 
FROM film 
-- Modify the query to use the ANY function 
WHERE 'Trailers' = ANY (special_features);
```
- The contains operator `@>` operator is alternative syntax to the `ANY` function and matches data in an ARRAY using the following syntax.
```
WHERE array_name @> ARRAY['search text'] :: type[]  
```
```sql
SELECT 
  title, 
  special_features
FROM film 
-- Filter where special_features contains 'Deleted Scenes'
WHERE special_features @> ARRAY['Deleted Scenes'];
```