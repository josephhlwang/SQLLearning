# mySQL Notes

This is the notes for: https://www.youtube.com/watch?v=7S_tz1z_5bA&t=18s

These notes cover the basic concepts of querying data and working with databases.

- [mySQL Notes](#mysql-notes)
    - [Getting Started](#getting-started)
    - [Retrieving Data](#retrieving-data)
      - [USE](#use)
      - [SELECT](#select)
      - [WHERE](#where)
      - [ORDER BY](#order-by)
      - [LIMIT](#limit)
    - [Joining Tables](#joining-tables)
      - [Inner Joins](#inner-joins)
      - [Outer Joins](#outer-joins)
      - [Cross Joins](#cross-joins)
      - [UNION](#union)
    - [Inserting Data](#inserting-data)
      - [Single Row](#single-row)
      - [Multiple Rows](#multiple-rows)
      - [Copying A Table](#copying-a-table)
    - [Updating Data](#updating-data)
    - [Deleting Data](#deleting-data)
    - [Summarization](#summarization)
      - [Aggregate Functions](#aggregate-functions)
      - [GROUP BY](#group-by)
        - [HAVING](#having)
      - [ROLLUP](#rollup)
    - [Complex Queries](#complex-queries)
      - [Subquries](#subquries)
        - [ALL/ANY](#allany)
      - [Related Subqueries](#related-subqueries)
      - [EXISTS](#exists)
    - [Built-in Functions](#built-in-functions)
      - [Numeric Functions](#numeric-functions)
      - [String functions](#string-functions)
      - [Date/Time functions](#datetime-functions)
      - [Formatting Date/Time](#formatting-datetime)
      - [IFNULL/COALESCE](#ifnullcoalesce)
      - [IF](#if)
      - [CASE](#case)
    - [Views](#views)
      - [Creating a View](#creating-a-view)
      - [Deleting a View](#deleting-a-view)
    - [Stored Procedures](#stored-procedures)
      - [Creating a Stored Procedure](#creating-a-stored-procedure)
      - [Procedures with Default Values](#procedures-with-default-values)
      - [Deleting a Stored Procedure](#deleting-a-stored-procedure)
    - [Functions](#functions)
      - [Creating a Function](#creating-a-function)

### Getting Started

SQL is not case sensitive but is best practice to capitalize SQL keywords. Terminate each statement with _;_. Line breaks, spaces and tabs are ignored.

### Retrieving Data

Put each clause on a new line for style.

#### USE

To select database to work with use, `USE <db_name>`.

#### SELECT

To specify which columns to get use, `SELECT <columns>`.

To specify which table to retrieve columns from use, `FROM <table_name>`.

`SELECT *` for all columns or `SELECT first_name, list_name` for only the _fist_name_ and _last_name_ columns.
The order of the list is kept in the result.

To return a new column in the query based on existing columns use, `SELECT (points + 10) * 100 AS discount`.
Using the `AS` keyword, we can return a calculation of the _points_ column as _discount_. The order of operation is the same as math.

To select distinct rows of a column use `SELECT DISTINCT state`

#### WHERE

To filter columns given condition use, `WHERE <condition>`.

`<condition>` can be replaced by a boolean expression to filter out rows. We can use _>, >=, <, <=, =, !=_.

Ex. `WHERE state != 'VA'`

To evaluate multiple conditions use the `AND`, `OR` and `NOT` operations.
The `AND` operator is always evaluated first but the order can be changed with parentheses.

Ex. `WHERE birth_date > '1990-01-01' OR points > 1000 AND state = 'VA'`

To evaluate mutiple values of the same column, use the `IN` operator.

Ex. `WHERE state NOT IN ('VA', 'FL', 'GA')`, selects rows with state of ('VA', 'FL', 'GA').

To evaluate values between a threshold, use the `BETWEEN` operator.

Ex. `WHERE points BETWEEN 1000 AND 3000`

To evaluate strings, use the `REGEXP` operator.

Ex. `WHERE name REGEXP 'regexp'`, selects rows with name containing _field_.

`^ex` : matches the beginning of a string.
`ex$` : matches the end of a string.
`ex1|ex2` : matches either regex.
`[abc]ex` : matches _aex_, _bex_ and _cex_.
`[a-c]ex` : matches _aex_, _bex_ and _cex_.

To check for _null_ values, use the `IS NULL` operator.

Ex. `WHERE phone IS NULL`, selects rows with _null_ phone numbers.

#### ORDER BY

By default, the table is ordered by the primary key of each table by ascending order.

To order results by columns use, `ORDER BY <columns>`.

If more than one column is given, the other columns are used if there are ties in the first column.

To sort by descending order, use the `DESC` operator.

Ex. `ORDER BY name DESC`.

#### LIMIT

To limit the number of rows returned use, `LIMIT <int>`.

To use the `LIMIT` operation for pagination, `LIMIT <skip_int>,<return_int>`.
The first _<skip_int>_ rows will be skipped and the next <return_int> rows will be returned.

### Joining Tables

#### Inner Joins

To inner join tables use, `FROM <table1> JOIN <table2> ON <table1>.column = <table2>.column`.

Note: You can join the same table with itself or join multiple tables with each other.

For compound inner join(joining from multiple columns), use `FROM <table1> JOIN <table2> ON <table1>.column1 = <table2>.column1 AND <table1>.column2 = <table2>.column2`.

#### Outer Joins

While inner joins only select a row if the join condition is met, outer joins will select all rows regardless of the condition. When the condition is not met for a particular, the unjoined columns are null.

To outer join tables use, `FROM <table1> <LEFT/RIGHT> JOIN <table2> ON <table1>.column = <table2>.column`.

#### Cross Joins

Cross joins joins every column of one table with every column of another.

To cross join tables use, `FROM <table1> CROSS JOIN <table2>` or `FROM <table1>, <table2>`.

#### UNION

To take the union of two selections use the `UNION` keyword.

Ex. `<selection1> UNION <selection2>`.

### Inserting Data

#### Single Row

To insert a single new row use, `INSERT INTO <table_name> VALUES(<c1_value>, .., <cn_value>)`.
Inside `VALUES()`, you can use `NULL` for null values and `DEFAULT` for default values.

Another way to insert a single new row but only entering values for select columns _c1, ..., cn_ and using default/null values for others use, `INSERT INTO <table_name> (c1,...,cn) VALUES(<c1_value>, .., <cn_value>)`.

#### Multiple Rows

To insert multiple new rows use, `INSERT INTO <table_name> VALUES(<c1_value1>, .., <cn_value1>), ...,(<c1_valuen>, .., <cn_valuen>)`.

To get the ID of the last insert, use `LAST_INSERT_ID()`.

You also also insert rows from one table into another table using `SELECT`.

#### Copying A Table

To copy the contents of a table without copying its table attributes, use `CREATE TABLE <copied_table_name> AS SELECT * FROM <table_name>`.

### Updating Data

To update a table use, `UPDATE <table_name> SET <column1> = <value1>, <column2> = <value2> WHERE <condition>`.

You can also update a column with the value of another column.
Ex. `UPDATE payments SET price = price * discount, payment = DEFAULT WHERE order_id = 1`

### Deleting Data

To delete a table use, `DELETE FROM <table_name> WHERE <condition>`.

### Summarization

#### Aggregate Functions

Default functions include: `MAX()`, `MIN()`, `AVG()`, `SUM()`, `COUNT()`.

Ex. `SELECT MAX(invoice_total) AS highest_invoice FROM invoices`

You can get the number of rows with `COUNT(*)`.
You can use expressions within functions. Ex. `SUM(invoice_total + 1.1)`.
You can filter the rows with the `WHERE` keyword.
You can choose unique rows. Ex. `COUNT(DISTINCT client_id)`

#### GROUP BY

To group rows that have same values commonly used with aggregate functions use the `GROUP BY <column>` keyword.

Ex. `SELECT SUM(invoice_total) AS total_invoices FROM invoices GROUP BY client_id`
Here for each unique client, we are finding their total invoices.

You can specify multiple columns in `<column>` to group by multiple columns.
Ex. `SELECT SUM(invoice_total) AS total_invoices FROM invoices GROUP BY state, city`
Here for each unique state and city combination, we are finding their total invoices.

##### HAVING

Since you can only use the `WHERE` keyword before the `GROUP BY` meaning that you can only filter rows before the group, use the `HAVING` keyword to filter rows after the group.

Ex.

```sql
SELECT SUM(invoice_total) AS total_invoices
FROM invoices
WHERE date > '2019-01-01'
GROUP BY client_id
HAVING total_invoices > 500
```

Here, we are finding the clients with total invoices greater than 500 after 2019.

You can do similar conditioning in `HAVING` as in `WHERE` but you can only reference the selected columns.

#### ROLLUP

The `WITH ROLLUP` keyword provides a total sum as the last row of a summarization query.

Ex. `SELECT SUM(invoice_total) AS total_invoices FROM invoices GROUP BY client_id WITH ROLLUP`
Will return the total invoice for all clients as the last row.

### Complex Queries

#### Subquries

`SELECT` subqueries can be used in parallel with other select statements. You can use all techniques used before in regular queries in subqueries. You can use subqueries within `SELECT`, `FROM`, `WHERE`, etc...
Ex.

```sql
SELECT first_name, last_name
FROM employees
WHERE salary > (
  SELECT AVG(salary)
  FROM employees
)
```

Selects all the employees with higher than average salary. It finds the average salary using a subquery.

At times both join and subqueries can be used to solve the same problem, chose the most efficient and readable solution.

##### ALL/ANY

Sometimes you want to condition a `WHERE` condition based on a list of values. You can use the `ALL` or `ANY` keyword to check a list of values.

Ex.

```sql
SELECT *
FROM invoices
WHERE invoice_total > ALL(
  SELECT invoice_total
  FROM invoices
  WHERE client_id = 3
)
```

Ex.

```sql
SELECT *
FROM invoices
WHERE invoice_total > ANY(
  SELECT invoice_total
  FROM invoices
  WHERE client_id = 3
)
```

These two queries will select all the rows with invoice totals greater than ALL/ANY of the invoices of client 3.

#### Related Subqueries

When the table of the subquery is the same table of the main query, you can use the alias of the main table in the subquery.

Ex.

```sql
SELECT *
FROM invoices i
WHERE invoice_total > (
	SELECT AVG(invoice_total)
    FROM invoices
    WHERE client_id = i.client_id
)
```

So for each invoice, the invoice is only selected by the `WHERE` keyword if the invoice total is greater than the average invoice total for its client. The client is matched with the alias of the main table.

#### EXISTS

If you don't care about the number, only if some rows exists, you can use the `EXISTS` keyword.

```sql
SELECT *
FROM clients c
WHERE EXISTS (
	SELECT client_id
    FROM invoices
    WHERE client_id = c.client_id
)
```

Since the `IN` keyword checks all the values of the subquery, when that subquery produces a large list, the main query will take a long time. The `EXISTS` keyword is a more efficient as it checks where the list has any items or not.

### Built-in Functions

#### Numeric Functions

Use `ROUND(<num>, <decimal_digits>)` to round a number to _n_ decimal digits.

Use `TRUNCATE(<num>, <decimal_digits>)` to truncate a number to _n_ decimal digits.

Use `CEILING()` and `FLOOR()` to ceil/floor a number.

Use `ABS()` for an absolute value of a number.

Use `RAND()` for a random number between 0 and 1.

#### String functions

Use `LENGTH()` to find the length of a string.

Use `UPPER()` to convert a string into upper case.

Use `LOWER()` to convert a string into upper case.

Use `LTRIM()` and `RTRIM()` to remove spaces on each side of a string.

Use `SUBSTRING(<str>, <start_pos>, <length>)` to get substring of _str_ starting at _start_pos_ of _length_. Note strings start at 1 in SQL.

Use `SUBSTRING(<search_str>, <str>)` to get the index of the first occurence of _search_str_ in _str_. Otherwise 0 is returned.

Use `SUBSTRING(<str>, <search_str>, <replace_str>)` to replace _search_str_ with _replace_str_ in _str_.

Use `CONCAT(<str1>, ..., <strn>)` to combine _n_ strings.

#### Date/Time functions

Use `NOW()` to get the current date and time.

Use `CURDATE()` to get the current date.

Use `CURDATE()` to get the current time.

Use `YEAR(<date>)`, `MONTH(<date>)`, `YEAR(<date>)` to get the _year_, _month_ and _day_
components of a date.

Use `HOUR(<time>)`, `MINUTE(<time>)`, `SECOND(<time>)` to get the _hour_, _minute_ and _second_ components of a time.

Use `DAYNAME(<date>)`, `MONTHNAME(<date>)` to get the _month_ and _day_ names of a date.

#### Formatting Date/Time

Use the `DATE_FORMAT(<date>, <format>)` and `TIME_FORMAT(<time>, <format>)` to format the SQL dates into a more user friendly format.

Click [here](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date-format) on documentation for dates.

Click [here](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_time-format) on documentation for time.

#### IFNULL/COALESCE

Use `IFNULL(<column>, <column1/str>)` to return another column if first _column_ is null.

Use `COALESCE(<column>, <column1/str>, ..., <columnn/str>)` to create a hierarchical list of returns.
If first column is null, return second. If second column is null return third, etc.

#### IF

Use `IF(<condition>, <value1>, <value2>)` to return _value1_ if condition is true and _value2_ if false.

Ex.

```sql
SELECT
  order_id,
  IF(YEAR(order_date) = YEAR(NOW()), "Active", "Archived")
FROM orders
```

#### CASE

This creates a new column that marks orders as _"Active"_ if the order is in the current year and _"Archived"_ if order is not.

To test more than one condition use `CASE` with `WHEN`, `THEN` and optionally `ELSE`.
For each case, it is paired with a condition using `WHEN` and the returned result using `THEN`. When no conditions are met, the `ELSE` value is returned.

Ex.

```sql
SELECT
  order_id,
  CASE
    WHEN YEAR(order_date) = YEAR(NOW()) THEN "Active"
    WHEN YEAR(order_date) = YEAR(NOW())-1 THEN "Last Year"
    WHEN YEAR(order_date) < YEAR(NOW()) THEN "Archived"
    ELSE 'Future'
  END AS category
FROM orders
```

### Views

#### Creating a View

To simplify coding queries, a view object can be saved in the database to provide an easy way to execute repeated queries.

A view is basically saved SQL query code.

To create a view, use `CREATE VIEW <view_name>`.

Ex.

```sql
-- view name arbitrary
CREATE VIEW new_view AS
-- below is code for query
SELECT ...
```

This creates a view called _new_view_ that performs the query below it.

Views are commonly stored in a file with the same name to be shared in source control.

#### Deleting a View

To delete a view use, `DROP VIEW <view_name>` or a safer, better way `DROP VIEW IF EXISTS <view_name>`.

To replace a view use, `CREATE OR REPLACE VIEW <view_name> AS`. This is very usefull if you want to overwrite the old view; you don't have to drop the view first.

### Stored Procedures

Stored procedures contain blocks of SQL code, in an application, the SQL code is stored in the DB.

#### Creating a Stored Procedure

To create a stored procedure in a query, do

```sql
DELIMITER $$
CREATE PROCEDURE get_clients()
BEGIN
  SELECT * FROM clients;
END$$
DELIMITER ;
```

You can also create a store procedure using the GUI.

To create a stored procedure with parameters, do

```sql
-- delimiter must be changed
DELIMITER $$
-- procedure name and params arbitrary
CREATE PROCEDURE get_clients_by_state(state CHAR(2))
-- query begins in BEGIN and ends in END
BEGIN
  SELECT * FROM clients c WHERE c.state = state;
END$$
DELIMITER ;
```

An alias is used in the select statement when the parameter and the column have the same name.

#### Procedures with Default Values

When you want to return a default value when the procedure is called with a null value, you can use the `IFNULL()` function.

Ex.

```sql
DELIMITER $$
CREATE PROCEDURE get_payments(client_id INT, payment_method TINYINT)
BEGIN
  SELECT *
  FROM payments p
  WHERE p.client_id = IFNULL(client_id, p.client_id)
  AND p.payment_method = IFNULL(payment_method, p.payment_method);
END$$
DELIMITER ;
```

If the _client_id_ or the _payment_method_ param is null, the `IFNULL()` function will make the query return all possible rows.

#### Deleting a Stored Procedure

To delete a procedure use, `DROP PROCEDURE <procedure_name>` or a safer, better way `DROP PROCEDURE IF EXISTS <procedure_name>`.

### Functions

Functions and procedures are very similar except functions can only return a single value.

#### Creating a Function

To create a function, do

```sql
-- function name, params arbitrary
CREATE FUNCTION get_risk_factor(client_id INT)
-- return type
RETURNS INTEGER
-- attributes
READS SQL DATA
-- functions must start with BEGIN and end with END
BEGIN
  RETURN(
    SELECT SUM(invoice_total)/COUNT(*)*5
    FROM invoices i
    WHERE i.client_id = client_id
  );
END
```

This function has a integer param client*id that returns an integer.
`READS SQL DATA` is one attribute of many, [click](https://dev.mysql.com/doc/refman/8.0/en/create-procedure.html) for more.
It finally returns the calculation of \_risk_factor* for client.
