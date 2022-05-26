# SQL Notes
###### TERMINOLOGY
rows = records
cols = attributes
NULL = unknown value
###### STATEMENTS
In SQL, statements like `SELECT` are not case sensitive. This means that SQL will understand if you write `select`, for example. However, SQL won't understand if you don't end your statements with `;`, so don't forget!

###### SELECTING: SELECT, FROM, SELECT DISTINCT
The `SELECT` statement is used to retrieve data from a database. For example, `SELECT title FROM films;` will give you a table (called the _result table_, containing all the values in the `name` column from the `films` table. Here, the `FROM` keyword allows you to specify which _table_ you want to get data from. Using the `DISTINCT` keyword allows us to filter out duplicate information from a column. For example, `SELECT DISTINCT name FROM people;` will give us a table containing all the _unique_ names in the `people` table. You can get data from multiple columns if you need to, by separating the column names with a comma. Writing `SELECT *`, SQL will fetch data from _all_ columns of a table. However, you may not want *all* of the data. Thankfully, the `LIMIT` keyword allows you to specify how many records of the result set you want to see. For example, `SELECT * FROM films LIMIT 10;` will give you only the first ten results.

###### COUNT
The `COUNT` statements gives you the number of records in a column (a.k.a the number of records an attribute has). For example, `SELECT COUNT(DISTINCT name) FROM people` gives you the number of unique names in the `people` table. If you write `SELECT COUNT(*) FROM people` you will get the number of records in the entire `people` table.

###### AGGREGATE FUNCTIONS: AVG, SUM, MIN, MAX
Often you will want to perform some calculation on the data in a database. SQL provides a few functions to help you out with this. For example, `SELECT AVG(budget) FROM films;` would give you a result set where the only row contains the average value from the `budget` column of the `films` table. In the same fashion, the `MAX()` function would return the highest budget. The `SUM()` function returns the result of adding up the numeric values in a column. Can you guess what the `MIN()` function does?

###### ROUNDING FUNCTIONS: ROUND, FLOOR, CEILING
Similar to aggregate functions, SQL provides some functions to round numerical data. The `ROUND()` function will round a numeric value to the nearest integer. The `FLOOR()` function will return the _largest_ integer not _greater than_ the argument. For example, `FLOOR(-42.8)` will give you `-43`. Similarly, the `CEILING()` function will return the _smallest_ integer not _less than_ the argument.

###### ALIASING: AS
In SQL, the `AS` keyword allows us to specify an '_alias_' (temporary name) for a column in the result set. For example, `SELECT COUNT(title) AS title_count FROM films;` will give you a result set with a single column named `title_count`. Aliases are helpful for making results more readable.

###### BASIC FILTERING: WHERE
In SQL, the `WHERE` keyword allows you to filter records based on conditions. For example, `SELECT title FROM films WHERE release_year > 2000;` will give you the names of all the films released since the year 2000. You can filter using `WHERE` with both numeric and text values. You can build up your query using the `AND` and `OR` keywords. For example, `SELECT title FROM films WHERE release_year > 1994 AND release_year < 2000;`, which will give you all the films released between 1994 and 2000. Similarly, `SELECT title FROM films WHERE release_year = 1994 OR release_year = 2000;` will give you the names of all the films released in _either_ 1994 or 2000.

###### ADVANCED FILTERING: BETWEEN, IN, IS NULL, IS NOT NULL
In SQL, the `BETWEEN` keyword allows you filter values within a specified range. For example, `SELECT title FROM films WHERE release_year BETWEEN 1994 AND 2000;` will give you the names of all the films released between 1994 and 2000. Remember: the `BETWEEN` operator is _inclusive_; the beginning and end values are included in the results. In SQL, the `IS NULL` operator allows us to filter out records which have unknown values. Often, you will want to filter out so we only get results which are not `NULL`. To do this, you can use the `IS NOT NULL` operator. For example, `SELECT name FROM people WHERE birthdate IS NOT NULL;` will give you the names of all the people whose birthdate we know.

###### SORTING: ORDER BY
In SQL, the `ORDER BY` keyword is used to sort the result set in ascending or descending order. By default `ORDER BY` will sort in ascending order. If you want to sort the results in descending order, you can use the `DESC` keyword. For example, `SELECT title FROM films ORDER BY release_year;` will give you the titles of films sorted by release year, from newest to oldest. You can use `ORDER BY` to sort by multiple columns too if you need to.

###### SORTING: GROUP BY
In SQL, `GROUP BY` allows you to group a result set by one or more columns. `GROUP BY` is used with aggregate functions like `COUNT()` or `MAX`. For example, `SELECT title, COUNT(title) FROM films GROUP BY release_year;` will give you the number of films released in each year.

###### SUPER MEGA ADVANCED FILTERING: HAVING
In SQL, `WHERE` cannot be used with aggregate functions. To deal with this, you can use the `HAVING` keyword. For example, `SELECT release_year FROM films GROUP BY release_year HAVING COUNT(title) > 10;` will show only those years in which more than 10 films were released.

###### VIDEO EXERCISES:
Introducing SQL
- Motivating example: preview of Citi Bike case study. - Why not just use R / Python?
  - Memory
  - Speed
  - Centralization
  - Access control
  - DBMS tools
  - Scalability
- Vocabulary: comparing to spreadsheet
  - Database: like a spreadsheet
  - Table: like a worksheet
  - Columns / Rows: same concept
  - Fields: cells
  - Entities:
  - Attributes:
- Basic SQL syntax
  - We're using PostgreSQL
  - Not case sensitive
  - Statements end with semi-colon
  - Brief intro to order of operations
- Introducing SELECT, SELECT * and DISTINCT - Brief note on LIMIT

Aggregate Functions
- Introducing COUNT
- COUNT DISTINCT
- Difference between COUNT(\*), COUNT(col) and COUNT(DISTINCT)
- SUM, AVG, MIN, MAX
- Introduction to Aliasing using AS - ROUND, FLOOR, CEILING

Arithmetic in SQL## Arithmetic in SQL
- Some stuff about arithmetic in SQL
- Introduction to alisaing using AS

WHERE Oh WHERE
- Introduction to WHERE
- Filtering numeric values
- Filtering text values
- Basic comparison operators
- Multiple WHERE conditions: AND, OR

More Advanced Filtering
- Introduction to BETWEEN, IN
- IS NULL, IS NOT NULL

Sorting with ORDER BY
- Introduction to ORDER BY
- Single columns, multiple columns
- Sorting in reverse order with DESC

HAVING A Great Time
- Introduction to HAVING
- Comparison with WHERE
- The HAVING clause was added to SQL because the WHERE keyword could not be used with aggregate functions.

Introduction to the CitiBike Data
- Trips, Stations, Weather
- Imagine you're part of an urban planning team trying to understand biking habits in NYC to improve bike routes
- Etc.

Summary
- Summarize the case study
- Summarize the course
- Give suggestions for further projects using what you've learned
