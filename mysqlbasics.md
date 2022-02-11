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
