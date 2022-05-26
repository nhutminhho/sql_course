## 01 Reformatting String and Character Data
### Concatenating Strings 
- String concatenation operation `||`
```sql
SELECT
  first_name || ' ' || last_name AS full_name
FROM customer;
```
- `CONCAT()` function
```sql
SELECT
   CONCAT(first_name, ' ', last_name) AS full_name
FROM customer;
```
### Change the case of string
- `UPPER()`
- `LOWER()`
- `INITCAP()`
### Replacing characters in a string
- ` REPLACE(description, 'A Astounding', 'An Astounding') AS description`

### Manipulating string data with REVERSE
-`REVERSE()`
## 02 Parsing String and Character Data
### Determining the length of a string
- `CHAR_LENGTH()`
- `LENGTH()`
### Finding the position of a character in a string
- `POSITION('@' IN email)`
- `STRPOS(email, '@')`
### Parsing string data
- `LEFT(description, 50)`
- `RIGHT(description, 50)`
- `SUBSTRING(description, 10, 50)`
   - `SUBSTRING(email FROM 0 FOR POSITION('@' IN email))`
- `SUBSTR(description, 10, 50)`
   - Not allow for the alternative syntax with FROM and FOR keywords
## 03 Truncating and Padding String data
### Remove whitespace from strings
- `TRIM([leading | trailing | both] [characters] from string)`
     - First parameter: default value is 'both'
     - Second parameter: default value is 'blank space'
     - `TRIM(' padded  ');` -> 'padded'
- `LTRIM(' padded  ');` -> 'padded   '
- `RTRIM(' padded  ');` -> ' padded'

### Padding strings with character data
- `LPAD()`
    - `LPAD('padded', 10, '#')` -> '####padded'
- `RPAD()`

## Putting it all together
> In this exercise, we are going to use the film and category tables to create a new field called film_category by concatenating the category name with the film's title. You will also practice how to truncate text fields like the film table's description column without cutting off a word.
```sql
SELECT 
  UPPER(c.name) || ': ' || f.title AS film_category, 
  -- Truncate the description without cutting off a word
  LEFT(description, 50 - 
    -- Subtract the position of the first whitespace character
    POSITION(
      ' ' IN REVERSE(LEFT(description, 50))
    )
  ) 
FROM 
  film AS f 
  INNER JOIN film_category AS fc 
  	ON f.film_id = fc.film_id 
  INNER JOIN category AS c 
  	ON fc.category_id = c.category_id;
```