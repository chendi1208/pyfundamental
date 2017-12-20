# Modifying data

## 1: Modifying Data In Tables

There are many times when we don't just want to query data from SQL tables, and instead we want to modify the existing data or add new data. Modifying data in SQL tables is possible through 3 statements:

- `INSERT` -- adds new data.
- `UPDATE` -- changes the values of some columns in existing data.
- `DELETE` -- removes existing data.

In this mission, we'll cover these 3 statements. As we do so, we'll be working with `factbook.db`, a SQLite database that contains information about each country in the world. We'll be working with a table in the file called `facts`. Each row in `facts` represents a single country, and contains several columns, including:

- `name` -- the name of the country.
- `area` -- the total land and sea area of the country.
- `population` -- the population of the country.
- `birth_rate` -- the birth rate of the country.
- `created_at` -- the date the record was created.
- `updated_at` -- the date the record was updated.

Here are the first few rows of `facts`:

| id   | code | name        | area    | area_land | area_water | population | population_growth | birth_rate | death_rate | migration_rate | created_at                 | updated_at                 |
| ---- | ---- | ----------- | ------- | --------- | ---------- | ---------- | ----------------- | ---------- | ---------- | -------------- | -------------------------- | -------------------------- |
| 1    | af   | Afghanistan | 652230  | 652230    | 0          | 32564342   | 2.32              | 38.57      | 13.89      | 1.51           | 2015-11-01 13:19:49.461734 | 2015-11-01 13:19:49.461734 |
| 2    | al   | Albania     | 28748   | 27398     | 1350       | 3029278    | 0.3               | 12.92      | 6.58       | 3.3            | 2015-11-01 13:19:54.431082 | 2015-11-01 13:19:54.431082 |
| 3    | ag   | Algeria     | 2381741 | 2381741   | 0          | 39542166   | 1.84              | 23.67      | 4.31       | 0.92           | 2015-11-01 13:19:59.961286 | 2015-11-01 13:19:59.961286 |

## 2: Working With Dates In SQL

Dates are an extremely important part of querying and analyzing data. Some common use cases are segmenting records by date, figuring out how many events occurred on each date, and finding all the dates on which a particular event occurred.

Because of how common it is to use dates when querying data, SQL has built-in support for handling dates. It makes it easy to query based on date and time ranges. We can query a date range from the `facts` table using the following syntax:

```

```

```
SELECT * FROM facts WHERE created_at < "2015-11-01" AND created_at > "2015-10-30";
```

Here's what's happening in this query:

- SQL is comparing the `created_at`column in `facts` to `2015-11-01`, and selecting any rows where `created_at`is less.
- SQL is comparing the `created_at`column in `facts` to `2015-10-30`, and selecting any rows where `created_at`is greater.

You may recognize that this looks exactly like some of the `WHERE` statements you used in previous missions. The only caveat is to put the date you want to compare in quotes so SQL parses the date properly. Dates have a natural ordering, and SQL automatically uses this ordering. For example, `2015-11-01` is greater than `2015-10-30`, because it comes afterwards.

We can also query based on times using the following syntax:

```

```

```
SELECT [columnA, columnB, ...]
```

```
FROM tableName
```

```
WHERE dateColumn < "date1"
```

```
AND dateColumn > "date2";
```

Here's a concrete example:

```

```

```
SELECT * FROM facts 
```

```
WHERE created_at < "2015-11-01 14:00"
```

```
AND created_at > "2015-10-30 12:00";
```

The above SQL query selects any rows in `facts` that were created between `12pm`on `October 30th, 2015`, and `2pm` on `November 1st, 2015`. If you're not familiar with the time notation, it's military time, a 24-hour clock which starts at `0:00`(midnight), and ends at `24:00`. `9:00`military time is `9am`, and `17:00` military time is `5pm`. The primary advantage of military time is that we don't need to specify am or pm.

### Instructions

- Select any rows from `facts` where `updated_at` is greater than `October 30th, 2015` at `4pm`, and `updated_at` is less than `November 2nd, 2015` at `3pm`.

```

SELECT * from facts where updated_at < "2015-11-2 15:00" and created_at > "2015-10-30 16:00";
```

## 3: Data Types

In Python, variables can have data types, such as *string*, *float*, or *integer*. Whereas these data types don't have to be specified upfront in Python, each column in a SQL table has to have a data type specified when the table is created. This helps SQL store and search the data efficiently. Every SQL database engine has slightly different names for data types. Some of the common data types for SQLite are:

- `INTEGER` -- similar to the *integer* type in Python.
- `REAL` -- similar to the *float* type in Python.
- `FLOAT` -- similar to the *float* type in Python.
- `TEXT` -- similar to the *string* type in Python.
- `VARCHAR(255)` -- similar to the *string* type in Python.

The reason why SQLite has so many names for similar data types is to provide compatibility with other databases, some of which will only allow one or the other (`REAL` or `FLOAT`, for example).

To see the data types of each column in a table, you can use the `PRAGMA` statement:

```

```

```
PRAGMA table_info(tableName);
```

Here's a concrete example:

```

```

```
PRAGMA table_info(table);
```

We'll cover the `PRAGMA` statement in more depth later on, but for now it's enough to know that it shows information about a SQL database.

### Instructions

- Write a SQL query that returns the data type of each column in `facts`.

```
PRAGMA table_info(facts);
```

## 4: Primary Keys

Every SQL table has a primary key. A primary key is a column or combination of columns that are unique for each row in the table. The primary key is how SQL uniquely identifies each row. Most tables have an integer column called `id` by default, which is the primary key.

If you look back on the output of the `PRAGMA` statement in the last screen, you'll see something like this:

| cid  | name | type         | notnull | dflt_value | pk   |
| ---- | ---- | ------------ | ------- | ---------- | ---- |
| 0    | id   | INTEGER      | 1       | None       | 1    |
| 1    | code | varchar(255) | 1       | None       | 0    |
| 2    | name | varchar(255) | 1       | None       | 0    |

There's a column called `pk`, which is an integer. It's set to `1` (or `True`) for the first row, and `0` (or `False`) for the others. So the first column in the `facts`table, `id`, is the primary key.

The most common name for the primary key column in a SQL table is `id`, although it doesn't have to be.

### Instructions

- Write a SQL query that uses the `ORDER BY` and `LIMIT`statements to select the entire row that has the highest `id`value.

```
SELECT * FROM facts ORDER BY id DESC limit 1;
```

## 5: Inserting Data Into A Table

Sometimes, we'll receive new rows that we want to add to a SQL table. We can insert a row into a table using the `INSERT`SQL statement. Here's an example `INSERT` statement:

```

```

```
INSERT INTO tableName
```

```
VALUES (value1, value2, ...);
```

Here's a concrete example:

```

```

```
INSERT INTO facts
```

```
VALUES (262, "dq", "DataquestLand", 60000, 40000, 20000, 500000, 100, 50, 10, 20, "2016-02-25 12:00:00", "2016-02-25 12:00:00");
```

There's quite a bit happening, so let's unpack each part:

- `INSERT INTO` -- this indicates that we're adding a row to a particular table.
- `facts` -- the table we're inserting the row into.
- `VALUES` -- this indicates that we're specifying which values to insert.`262` -- the primary key -- this is one higher than the current maximum primary key.The other values we're inserting are comma separated, and match the column order in `facts`.Date and text fields are in quotes, but *integer* and *float* fields aren't.Dates must be in the `yyyy-mm-dd HH:MM:SS` format.

After the above query runs, we'll have a new row in `facts` that we can work with like any other row.

### Instructions

- Write a SQL query that inserts a row into `facts` with the following values:

```
262, "dq", "DataquestLand", 60000, 40000, 20000, 500000, 100, 50, 10, 20, "2016-02-25 12:00:00", "2016-02-25 12:00:00"

```

```

INSERT INTO facts
VALUES (262, "dq", "DataquestLand", 60000, 40000, 20000, 500000, 100, 50, 10, 20, "2016-02-25 12:00:00", "2016-02-25 12:00:00");
```

## 6: Missing Values

We've worked with data sets that have missing values before. Because it's so common for data to be missing, SQL has explicit support for handling missing, or `NULL`, values. You can retrieve any rows where a specific column is `NULL` by using the following syntax:

```

```

```
SELECT * from tableName
```

```
WHERE columnName IS NULL;
```

Here's a concrete example:

```

```

```
SELECT * FROM facts 
```

```
WHERE area IS NULL;
```

If a value in a row is missing when you add it, you can also use `NULL` with the `INSERT INTO` statement:

```

```

```
INSERT INTO facts
```

```
VALUES (262, "dq", "DataquestLand", NULL, 40000, 20000, 500000, 100, 50, 10, 20, "2016-02-25 12:00:00", "2016-02-25 12:00:00");
```

Some columns may specify a `NOT NULL`constraint, which specifies that they can't contain any missing values. If you try to insert a `NULL` value into one of these columns, you'll get an error.

### Instructions

- Write a SQL query that inserts a row into `facts` with the following values:

```
263, "dq", "DataquestLand", NULL, NULL, 20000, 500000, 100, 50, 10, 20, "2016-02-25 12:00:00", "2016-02-25 12:00:00"
```

```

INSERT INTO facts
VALUES (263, "dq", "DataquestLand", NULL, NULL, 20000, 500000, 100, 50, 10, 20, "2016-02-25 12:00:00", "2016-02-25 12:00:00");
```

## 7: Updating Rows

Let's say we wanted to simulate a takeover of Australia by New Zealand and rename Australia. You can use the `UPDATE`statement for this:

```

```

```
UPDATE tableName
```

```
SET column1=value1, column2=value2, ...
```

```
WHERE column1=value3, column2=value4, ...
```

Here's a concrete example:

```

```

```
UPDATE facts
```

```
SET name="New Zealand", code="nz"
```

```
WHERE name="Australia"
```

The above will rename any rows where `name` equals `Australia` to `New Zealand`. Let's break down what's happening:

- `UPDATE` -- indicates that we're updating rows in the table.
- `facts` -- the table we're updating.
- `SET` -- indicates which columns we're going to update.
- `name="New Zealand"` -- set the values in the `name` column to the value `New Zealand`.
- `code="nz"` -- set the values in the `code` column to the value `nz`.
- `WHERE` -- specifies which rows to update.
- `name="Australia"` -- only update rows where the `name` column equals `Australia`.

The above query updates two columns, `name`, and `code`, but we could easily update one column or more columns. We can also use `AND` and `OR` in the `WHERE`statement to provide complex logic around which rows to update.

It's very important to specify a `WHERE`clause when running an `UPDATE`statement -- if you don't, all rows will be updated.

### Instructions

- Write a SQL query that uses the `UPDATE` statement to update any rows where `name` is `United States` to `DataquestLand`.

```

UPDATE facts
SET name="DataquestLand"
WHERE name="United States";
```

## 8: Deleting Rows

Let's say we wanted to remove any rows associated with the United States. We'd have to use the `DELETE` statement like this:

```

```

```
DELETE FROM tableName
```

```
WHERE column1=value1, column2=value2, ...;
```

Here's a concrete example:

```

```

```
DELETE FROM facts
```

```
WHERE name="United States";
```

The above query will delete any rows where `name` equals `United States`. Here's a breakdown of what's happening:

- `DELETE FROM` -- indicates that we're deleting rows.
- `facts` -- the table we're deleting rows from.
- `WHERE` -- specifies which rows to delete.
- `name="United States"` -- only delete rows where `name` equals `United States`.

It's very important to specify a `WHERE`clause for the `DELETE` statement -- if you don't, all rows will be deleted.

### Instructions

- Write a SQL query that removes all the rows in `facts`where `name` equals `Canada`.

```

DELETE FROM facts
WHERE name="Canada";
```

## 9: Conclusion

In this mission, we covered how to modify data in SQL tables, using the `INSERT`, `UPDATE`, and `DELETE`statements. In the next mission, we'll dive into how to setup and modify SQL tables, including changing which columns are in the table.

# Table schemas

## 1: Table Schema

In the last mission, we looked at a database called `factbook.db`, which contained information on each country in the world.

We looked at the `facts` table inside `factbook.db`. Here are the first few rows of `facts`:

| id   | code | name        | area    | area_land | area_water | population | population_growth | birth_rate | death_rate | migration_rate | created_at                 | updated_at                 |
| ---- | ---- | ----------- | ------- | --------- | ---------- | ---------- | ----------------- | ---------- | ---------- | -------------- | -------------------------- | -------------------------- |
| 1    | af   | Afghanistan | 652230  | 652230    | 0          | 32564342   | 2.32              | 38.57      | 13.89      | 1.51           | 2015-11-01 13:19:49.461734 | 2015-11-01 13:19:49.461734 |
| 2    | al   | Albania     | 28748   | 27398     | 1350       | 3029278    | 0.3               | 12.92      | 6.58       | 3.3            | 2015-11-01 13:19:54.431082 | 2015-11-01 13:19:54.431082 |
| 3    | ag   | Algeria     | 2381741 | 2381741   | 0          | 39542166   | 1.84              | 23.67      | 4.31       | 0.92           | 2015-11-01 13:19:59.961286 | 2015-11-01 13:19:59.961286 |

In the last mission, we also looked at the columns for `facts`, and the corresponding datatypes. Here's an excerpt:

| cid  | name | type         | notnull | dflt_value | pk   |
| ---- | ---- | ------------ | ------- | ---------- | ---- |
| 0    | id   | INTEGER      | 1       | None       | 1    |
| 1    | code | varchar(255) | 1       | None       | 0    |
| 2    | name | varchar(255) | 1       | None       | 0    |

This definition, which contains information like column names, data types, and which column is the primary key, is called a table schema.

Table schemas let us create new tables to store data. What we store may change over time, so SQL also allows us to modify the schema of a table over time.

## 2: Adding Columns

Let's say we want to add a column called `awesomeness` to track how awesome all the countries in the `facts` table are. We would need to add a column to `facts` to track this.

We can add a column with the `ALTER TABLE` statement:

```

```

```
ALTER TABLE tableName
```

```
ADD columnName dataType;
```

Here's a concrete example:

```

```

```
ALTER TABLE facts
```

```
ADD awesomeness integer;
```

The above code will add a column called `awesomeness` to the `facts` table. Let's break down what's happening:

- `ALTER TABLE` -- indicates that we're making a change to the table schema.
- `facts` -- the table we're altering.
- `ADD` -- indicates that we're adding a column.
- `awesomeness` -- the name of the column we're adding.
- `integer` -- the data type of the column we're adding. We talked about data types in the last mission, and [here's](https://www.sqlite.org/datatype3.html) a full list.

When you add a column, all the values associated with it will be `NULL` to begin with.

### Instructions

- Write a SQL query that adds a column called `leader` to the `facts` table, with the data type `text`.

```

ALTER TABLE facts
ADD leader text;
```

## 3: Removing Columns

You can also use the `ALTER TABLE` statement to remove columns that are no longer needed:

```

```

```
ALTER TABLE tableName
```

```
DROP COLUMN columnName;
```

Here's a concrete example:

```

```

```
ALTER TABLE facts
DROP COLUMN awesomeness;
```

The above query would remove the column `awesomeness` from the table `facts`. This command is only possible in certain SQL versions, and isn't possible with the SQLite database engine, so we won't practice it right now.

## 4: Creating Tables

We can create a new table in `factbook.db` using a new table schema. Let's say that we want to create a table that keeps track of leaders of countries. We would use the `CREATE TABLE` statement to do this:

```

```

```
CREATE TABLE dbName.tableName(
```

```
   column1 dataType1 PRIMARY KEY,
```

```
   column2 dataType2,
```

```
   column3 dataType3,
```

```
   ...
```

```
);
```

Here's a concrete example:

```

```

```
CREATE TABLE factbook.leaders(
```

```
   id integer PRIMARY KEY,
```

```
   name text,
```

```
   country text
```

```
);
```

Let's break down each piece of this example:

- `CREATE TABLE` -- indicates that we want to create a table.
- `factbook.leaders` -- we're creating a table called `leaders` in the `factbook` database. The `.` separates the two.
- `(` -- indicates that we're specifying the columns and data types inside.
- `id integer PRIMARY KEY` -- we're creating a column called `id` that has the data type `integer`, and is the primary key for the table. As we mentioned in the last mission, every table needs a primary key, and it's typical to make an `integer` column called `id` the primary key.
- `name text` -- we're creating a second column called `name` with the data type `text`.
- `country text` -- we're creating a third column called `country` with the data type `text`.
- `)` -- indicates that we're done specifying columns.

When we run the above query, we'll end up with a new table called `leaders` that has three columns, `id`, `name`, and `country`. The columns will be in the order they are specified in the query.

## 5: Relations Between Tables

In the past screen, we created a table called `leaders`. Instead of directly referring to the row in the `facts`table that contained the country the leader was from, we had to create a `country` column in `leaders` that contains the full name of the country. Here's how the `leaders` table might look:

| id   | name         | country       | worth     |
| ---- | ------------ | ------------- | --------- |
| 1    | Barack Obama | United States | 7000000.0 |

As you can see, the `country` is just a *string*. Querying any `leaders` associated with a country requires us to specify the country name as a *string*:

```

```

```
SELECT * from leaders
```

```
WHERE country="United States";
```

It also makes querying the country associated with a particular leader difficult. We first need to extract the `country` column as a *string* from the `leaders` table, then we need to query the `facts` table with the *string*:

```

```

```
SELECT * from facts
```

```
WHERE name="United States";
```

There's no way to combine both sets of records with this table schema, or query them more efficiently.

Fortunately, SQL databases are commonly known as relational databases because they support relations between tables. These relations enable us to efficiently query across multiple tables.

The most common type of relation is known as a foreign key. A SQL foreign key points from a record in one table to a record in another table. Here's an example of creating a table that contains a foreign key:

```

```

```
CREATE TABLE factbook.leaders(
```

```
   id integer PRIMARY KEY,
```

```
   name text,
```

```
   country integer,
```

```
   worth float,
```

```
   FOREIGN KEY(country) REFERENCES facts(id)
```

```
);
```

We've seen the `CREATE TABLE` statement before, but here are the main changes:

- `country` is now an integer column, because it "points" to the `id` column of the `facts` table.
- `FOREIGN KEY` -- specifies that we're setting one column in our table schema to be a foreign key to another table.
- `(country)` -- specifies the column that is a foreign key.
- `REFERENCES` -- indicates the table and column we're referencing with the foreign key.
- `facts` -- the table that we're referencing with the foreign key.
- `(id)` -- the column in `facts` that we're referencing with our foreign key.

After setting up this table, the `country` column of `leaders` can only be assigned integer values that exist in the `id` column of `facts`. This will point to a row in the `facts` table, and indicate that a particular leader represents a particular country.

Here's a diagram that may help you visualize what's happening:

factsleadersidcodenameidnamecountry1afAfgh1Ashr1anisaftanGhani

The above diagram shows how the integer value in the `country` column of `leaders` "points" to the corresponding value in the `id` column of `facts`, and thus associates a row in `leaders` with a row in `facts`.

## 6: Creating A Table With Relations

Now that we understand foreign keys and relations, we can create a table that contains a foreign key.

### Instructions

Write a SQL query that creates a table called `states` in the `factbook` database that will contain information on each state in a country. It should have the following columns:

- `id` -- `integer` data type, should be a primary key.
- `name` -- `text` data type.
- `area` -- `integer` data type.
- `country` -- `integer` foreign key to the `id` column of the `facts` table.

```

CREATE TABLE factbook.states(
    id integer PRIMARY KEY,
    name text,
    area integer,
    country integer,
    FOREIGN KEY(country) REFERENCES facts(id)
);
```

## 7: Querying Across Foreign Keys

We can use the `INNER JOIN` statement to make querying across foreign key relationships easier:

```

```

```
SELECT [column1, column2, ...] from tableName1
```

```
INNER JOIN tableName2
```

```
ON tableName1.column3 == tableName2.column4;
```

Here's a concrete example:

```

```

```
SELECT * from states
```

```
INNER JOIN facts
```

```
ON states.country == facts.id;
```

We've seen the basic `SELECT` statement before, so here's what's new:

- `INNER JOIN` -- indicates that we're combining data from two tables into one query.
- `facts` -- specifies the table we're joining with `states`.
- `ON` -- indicate how SQL matches a record in `states` with a record in `facts`.
- `facts.id` -- specifies that the `id`column in the `facts` table will be used to join.
- `states.country` -- specifies that the `country` column in the `states` table will be used to join.

You may recall from a previous screen that the `country` column of `states` "points" to the `id` column of `facts`. Therefore, we're saying that we're finding the associated row in `facts` for each row in `states`.

We'll get back each row of `states`, but with information from `facts` added in. This enables us to get all the information we need in one place, without having to resort to multiple queries.

### Instructions

Write a SQL query that:

- Uses the `SELECT` statement to query all the columns of the `landmarks` table.
- Uses `INNER JOIN` to combine data from the `landmarks`table with data from the `facts` table.Uses the `id` column from `facts` and the `country`column of `landmarks` to perform the join.

```

SELECT * from landmarks
INNER JOIN facts
ON landmarks.country == facts.id;
```

## 8: Types Of Joins

There are a few different types of joins. Before diving into them, it's helpful to know that each table in a `JOIN`statement has a *side*:

```

```

```
SELECT * from landmarks
```

```
INNER JOIN facts
```

```
ON landmarks.country == facts.id;
```

In the above `JOIN` statement, `landmarks` is the left side table, and `facts` is the right side table. The table that is specified first is the left table, and the second is the right side table. Values come from the left and right sides when the `JOIN` is performed:

| id   | name                | country | id_1 | code | name_1        | area    | area_land | area_water | population | population_growth | birth_rate | death_rate | migration_rate | created_at                 | updated_at                 |
| ---- | ------------------- | ------- | ---- | ---- | ------------- | ------- | --------- | ---------- | ---------- | ----------------- | ---------- | ---------- | -------------- | -------------------------- | -------------------------- |
| 1    | Statue of Liberty   | 186     | 186  | us   | United States | 9826675 | 9161966   | 664709     | 321368864  | 0.78              | 12.49      | 8.15       | 3.86           | 2015-11-01 13:35:14.898271 | 2015-11-01 13:35:14.898271 |
| 2    | Golden Gate Bridge  | 186     | 186  | us   | United States | 9826675 | 9161966   | 664709     | 321368864  | 0.78              | 12.49      | 8.15       | 3.86           | 2015-11-01 13:35:14.898271 | 2015-11-01 13:35:14.898271 |
| 3    | Washington Monument | 186     | 186  | us   | United States | 9826675 | 9161966   | 664709     | 321368864  | 0.78              | 12.49      | 8.15       | 3.86           | 2015-11-01 13:35:14.898271 | 2015-11-01 13:35:14.898271 |

In the above table, `id`, `name`, and `country` come from `landmarks`. `id_1`, `code`, `name_1`, `area`, and the rest of the columns come from `facts`. The `_1` suffixes are because some columns from the tables have the same names, so the suffixes are to disambiguate. The records are combined via the `JOIN` when `country` from the `landmarks` table matches `id_1` from the `facts` table.

Given that, here are the types of joins:

- `INNER JOIN` -- only displays rows where there is a match for the `ON` condition in both tables. Any rows that aren't matched are excluded.
- `LEFT OUTER JOIN` -- if there is no match for a row from the table on the left side of the `ON` condition, it is shown with `NULL` values for all the right side values.
- `RIGHT OUTER JOIN` -- if there is no match for a row from the table on the right side of the `ON` condition, it is shown with `NULL` values for all the left side values.

`INNER JOIN` is the most common type of join, but `LEFT OUTER JOIN` is also occassionally used. It's very uncommon to use `RIGHT OUTER JOIN`, and SQLite doesn't support it.

From a syntax points of view, using the statements is the exact same, you just swap out `INNER JOIN` for `LEFT OUTER JOIN`.

## 9: Types Of Joins

### Instructions

Write a SQL query that:

- Uses the `SELECT` statement to query all the columns of the `landmarks` table.
- Uses `LEFT OUTER JOIN` to combine data from the `landmarks` table with data from the `facts` table.Uses the `id` column from `facts` and the `country`column of `landmarks` to perform the join.

```

SELECT * from landmarks
LEFT OUTER JOIN facts
ON landmarks.country == facts.id;
```

## 10: Next Steps

In this mission, we covered the basics of modifying table schema, creating tables, and defining relations. Relations allow us to harness the full power of SQL, and query more efficiently. In the next few missions, we'll cover relations in more depth, and talk about optimizing table performance.

# Database Normalization and Relations

## 1: Introduction

In the previous mission, we learned how to create a foreign key to reference a table in another record and how to use joins to query across tables using the foreign key. In this mission, we'll dive more deeply into relations between tables, learn about data normalization, and how we can take advantage of them to perform more complex joins.

In this mission, we'll work with data on Academy Award nominations from 2001 to 2010 for just the lead and supporting acting roles. The Academy Awards, also known as the Oscars, is an annual awards ceremony hosted to recognize the achievements in the film industry. There are many different awards categories and the members of the academy vote every year to decide which artist or film should get the award. The full dataset, containing data on all award categories from years 1927 to 2010, can be found [here](https://www.aggdata.com/awards/oscar). We've cleaned and transformed the data and created `academy_awards.db`.

The database file `academy_awards.db` contains 2 tables:

- `nominations`, where each row describes an individual actor's nomination.
- `ceremonies`, where each row describes an individual Academy Awards ceremony.

The `nominations` table has the following schema:

- **id** - *integer* field, primary key for uniquely identifying rows.
- **ceremony_id** - *integer* field, foreign key reference to the **id** column from the `ceremonies` table.
- **category**: *text* field, award category. Can only be one of the following 4 values:`Actor -- Leading Role`.`Actor -- Supporting Role`.`Actress -- Leading Role`.`Actress -- Supporting Role`.
- **nominee**: *text* field, name of the actor or actress.
- **movie**: *text* field, name of the movie the actor or actresses was nominated for.
- **character**: *text* field, name of the character this actor or actress played.
- **won**: *Boolean* field, if the actor or actress won the award (either `0` or `1`).

The **won** column is specified as Boolean in the schema but since SQLite doesn't have a Boolean type, SQLite uses the *integer* data type instead. The *integer* `0` represents `False` while the *integer* `1` represents `True`.

Here's what the first 10 rows in the `nominations` table look like:

| id   | ceremony_id | category                 | nominee         | movie                  | character       | won  |
| ---- | ----------- | ------------------------ | --------------- | ---------------------- | --------------- | ---- |
| 1    | 10          | Actor -- Leading Role    | Javier Bardem   | Biutiful               | Uxbal           | 0    |
| 2    | 10          | Actor -- Leading Role    | Jeff Bridges    | True Grit              | Rooster Cogburn | 0    |
| 3    | 10          | Actor -- Leading Role    | Jesse Eisenberg | The Social Network     | Mark Zuckerberg | 0    |
| 4    | 10          | Actor -- Leading Role    | Colin Firth     | The King's Speech      | King George VI  | 1    |
| 5    | 10          | Actor -- Leading Role    | James Franco    | 127 Hours              | Aron Ralston    | 0    |
| 6    | 10          | Actor -- Supporting Role | Christian Bale  | The Fighter            | Dicky Eklund    | 1    |
| 7    | 10          | Actor -- Supporting Role | John Hawkes     | Winter's Bone          | Teardrop        | 0    |
| 8    | 10          | Actor -- Supporting Role | Jeremy Renner   | The Town               | James Coughlin  | 0    |
| 9    | 10          | Actor -- Supporting Role | Mark Ruffalo    | The Kids Are All Right | Paul            | 0    |
| 10   | 10          | Actor -- Supporting Role | Geoffrey Rush   | The King's Speech      | Lionel Logue    | 0    |

Since awards are only given to one winner, 4 nominees for each award lose and 1 nominee wins. You'll notice that among the nominees for each award, there was 1 nominee with a value of **1** for **won** and 4 nominees with a value of **0** for **won**. You may have also noticed that the movie `The King's Speech` shows up twice. This is because separate actors were nominated for different roles in the same movie.

The `ceremonies` table has the following schema:

- **id** - *integer field*, primary key for uniquely identify rows.
- **year** - *integer field*, the year of the ceremony.
- **host** - *text field*, the host for that ceremony.

Here's what the entire `ceremonies` table, which only contains 11 rows, looks like:

| id   | year | host            |
| ---- | ---- | --------------- |
| 0    | 2000 | Billy Crystal   |
| 1    | 2001 | Steve Martin    |
| 2    | 2002 | Whoopi Goldberg |
| 3    | 2003 | Steve Martin    |
| 4    | 2004 | Billy Crystal   |
| 5    | 2005 | Chris Rock      |
| 6    | 2006 | Jon Stewart     |
| 7    | 2007 | Ellen DeGeneres |
| 8    | 2008 | Jon Stewart     |
| 9    | 2009 | Hugh Jackman    |
| 10   | 2010 | Steve Martin    |

Let's now explore the data to become more familiar with it.

## 2: Database Normalization

The `ceremonies` table contains just the information on the actual awards ceremony while the `nominations`table contains just the information on individual nominations. If we had instead stored the `year` and `host`values as columns within the `nominations` table and avoided using a `ceremonies` table altogether, our `nominations` table would look like this:

| id   | year | host         | category                 | nominee         | movie                  | character       | won  |
| ---- | ---- | ------------ | ------------------------ | --------------- | ---------------------- | --------------- | ---- |
| 1    | 2010 | Steve Martin | Actor -- Leading Role    | Javier Bardem   | Biutiful               | Uxbal           | 0    |
| 2    | 2010 | Steve Martin | Actor -- Leading Role    | Jeff Bridges    | True Grit              | Rooster Cogburn | 0    |
| 3    | 2010 | Steve Martin | Actor -- Leading Role    | Jesse Eisenberg | The Social Network     | Mark Zuckerberg | 0    |
| 4    | 2010 | Steve Martin | Actor -- Leading Role    | Colin Firth     | The King's Speech      | King George VI  | 1    |
| 5    | 2010 | Steve Martin | Actor -- Leading Role    | James Franco    | 127 Hours              | Aron Ralston    | 0    |
| 6    | 2010 | Steve Martin | Actor -- Supporting Role | Christian Bale  | The Fighter            | Dicky Eklund    | 1    |
| 7    | 2010 | Steve Martin | Actor -- Supporting Role | John Hawkes     | Winter's Bone          | Teardrop        | 0    |
| 8    | 2010 | Steve Martin | Actor -- Supporting Role | Jeremy Renner   | The Town               | James Coughlin  | 0    |
| 9    | 2010 | Steve Martin | Actor -- Supporting Role | Mark Ruffalo    | The Kids Are All Right | Paul            | 0    |
| 10   | 2010 | Steve Martin | Actor -- Supporting Role | Geoffrey Rush   | The King's Speech      | Lionel Logue    | 0    |

While this representation is easier to query, since you don't have to do a join each time you want to get the `year` or `host` information, it has a few problems:

- it contains a *lot* of redundant data, which means the database will take up more disk space and cost more to store.
- if we want to update or remove a value in the `year` or `host` columns, we need to update every row that contains that same value. This is cumbersome to remember and can cause human error.
- updating or removing many rows can be slow for larger databases. As your data grows bigger, which is often the case with databases used in production, the update and removal speeds become significantly worse.

We instead chose to **normalize** the data, which involves separating data into smaller tables with less redundant information and creating relations between the appropriate tables. By having the `year` and `host` columns in a separate `ceremonies` table, we get the following benefits:

- much less data redundancy since the actual values for `year` and `host` are only stored in 1 row in `ceremonies`, instead of replicated for each relevant row in `nominations`.
- separation of concerns and ease of updating.

You can read more about the benefits of database normalization [here](https://en.wikipedia.org/wiki/Database_normalization#Objectives).

## 3: Types Of Relations

There are many types of relations you can create between tables to represent the links between columns. In this mission, we'll focus on the 2 most common relations:

- one-to-many.
- many-to-many.

A **one-to-many** relation exists whenever many rows in one table need to relate to one row in the other table. The relation between `ceremonies` and `nominations` is a one-to-many relation since many rows in the `nominations` table can be linked to an individual row in the `ceremonies` table. A row in the `ceremonies` table contains no reference to the `nominations` table. However, many rows in the `nominations` table contain a reference to the `ceremonies` table using the `ceremony_id` foreign key.

Below is a diagram that demonstrates how multiple rows in the `nominations` table, that share the same `ceremony_id` of `10`, relate to the row in the `ceremonies` table whose id is `10`:

nominationsceremoniesceremony_ididcategorynomineeyearhostid101Actor--LeadingRoleJavierBardem2010Steve10102Actor--LeadingRoleJeffBridgesMartin103Actor--LeadingRoleJesseEisenberg

An important thing to remember in a one-to-many relation is that the reference is one-sided. The `nominations` table contains a foreign key reference to the `id` column in `ceremonies` but the `ceremonies`table contains no references to values in the `nominations` table.

Here are some other examples of one-to-many relations:

- a car insurance policy can have multiple people on it, but each person can only belong to one policy.
- a mother can have many children, but each child can only have one birth mother.
- a reporter can have many articles but each article can only have one associated reporter.

## 4: Querying A Normalized Database

As with many things in software development, there is a tradeoff to database normalization. We need to write longer queries sometimes and use joins more often to grab information from multiple tables. Many companies have databases with hundreds or thousands of tables with many relations in between, so this can get complicated quickly! As you become more familiar with querying normalized databases, you'll be able to overcome the added complexity much more easily.

To write a query that involves 2 tables that are in a one-to-many relation, you need to join on the foreign key column that the "many" side uses to reference the "one" side. When using the `WHERE` statement to express filtering criteria, we can use columns from both tables. For example, to return all of the movies that won an award in 2010, we'd need to write the following query:

```

```

```
SELECT movie FROM nominations 
```

```
INNER JOIN ceremonies
```

```
ON nominations.ceremony_id == ceremonies.id
```

```
WHERE ceremonies.year == 2010 AND nominations.won == 1;
```

In the `WHERE` statement, we expressed that we were only interested in rows where the `year` value was `2010` from the `ceremonies` table and where the `won`value was `1` from the `nominations` table.

When joining 2 tables, you can be more explicit about which columns you want returned from which tables using dot notation -- e.g. `nominations.movie`. In the following query, we modified the earlier query to select the `year` and `host` columns from `ceremonies` and the `movie` and `nominee` columns from `nominations`:

```

```

```
SELECT ceremonies.year, ceremonies.host, nominations.movie, nominations.nominee FROM nominations 
```

```
INNER JOIN ceremonies
```

```
ON nominations.ceremony_id == ceremonies.id
```

```
WHERE ceremonies.year == 2010;
```

In the denormalized schema, which had the `year` and `host` columns in `nominations` itself, we'd only need to write the following query to accomplish the same result:

```

```

```
SELECT movie FROM nominations
```

```
WHERE nominations.year == 2010;
```

Let's practice using joins further to query tables in a one-to-many relation.

### Instructions

- We've imported the `sqlite3` library for you already and connected to the `academy_awards.db` database. The Connection instance is named `conn`.
- Write a query that returns all of the years that the actress `Natalie Portman` was nominated for an award.Only return the `year` column from `ceremonies` and the `movie` column from `nominations`.Run the query and assign the full results *list* to the variable `portman_movies`.Then display `portman_movies` using the `print`function.

```

portman_query = '''select ceremonies.year, nominations.movie from nominations
inner join ceremonies 
on nominations.ceremony_id == ceremonies.id 
where nominations.nominee == "Natalie Portman";
'''
portman_movies = conn.execute(portman_query).fetchall()
for p in portman_movies:
    print(p)
```

## 5: Many-To-Many Relation

If we wanted to extend our analysis to study how Academy Awards affect a nominee's career, we'd need to first add more data on which movies each actor starred in. We need a way to represent the relation between actors and movies. Our first instinct might be to use a `movies` table, an `actors` table, and specify a one-to-many relationship between them. The `movies` table could contain an `actor_id` field that acts as a foreign key reference to the `id` column from the `actors` table.

We immediately run into a road block. Each movie contains many actors and since the `actor_id` column would be an *integer* field, we have no way to reference multiple rows from the `actors` table. We could have a separate row in `movies` where each row contains a different value for `actor_id` and cover all the actors in the movie that way. This unfortunately means a large amount of data duplication, since the rest of the columns describing that movie probably won't be different.

What if we had a *list* data type where we could store multiple values:

| id   | movie       | actors                         |
| ---- | ----------- | ------------------------------ |
| 1    | The Fighter | Christian Bale, Amy Adams, ... |
| 2    | Doubt       | Meryl Streep, Amy Adams, ...   |

SQLite unfortunately doesn't contain a *list* data type, so we can't simply store the *list* of actor names. While some other databases do contain a *list* data type, this is still a poor design for our data. While searching for a movie by name would be a simple `SELECT` query, searching by actors would be incredibly cumbersome and slow.

You may have noticed that the actress **Amy Adams** stars in all 3 of the movies above. If we wanted to write a query that searched every element in the `actors` list for every row in `movies`, the query would take a long time to return as our table starts to hold more than a few thousand movies. We can't use a one-to-many relation since SQLite, and many databases, don't contain a *list* data type and it would be inefficient to query.

The right way to model actors and movies is to use a **many-to-many** relation. A many-to-many relation allows us to flexibly represent both:

- the actors in a movie and
- the movies an actor has starred in.

To represent a many-to-many relation, we need to use an intermediate table called a **join table**, which we'll learn more about in the next screen.

## 6: Join Table

To model a many-to-many relationship, we need to create a separate table that contains the foreign keys to each of the tables that we're creating a many-to-many relationship between. This table is called a **join table**, but is often referenced by [many other names](https://en.wikipedia.org/wiki/Associative_entity). The rows in the join table contain the foreign keys to the 2 other tables. Here's what a join table representing the many-to-many relationship between movies and actors would look:

movies_actorsactorsmoviesmovie_ididactor_idactorididmovie111AmyAdams11TheFighter122ChristianBale22Doubt231MerylStreep33Junebug243EmbethDavidtz4351364

In a many-to-many relation, we separate the data contained *within* the rows with the actual relation *between* the rows. This means we can, for example, edit a movie's name without touching the many actor records that are related to that movie. Each table above has it's own `id` column:

- the `movies.id` column is used as a foreign key reference by the `movies_actors.movie_id` column.
- the `actors.id` column is used as a foreign key reference by the `movies_actors.actor_id` column.
- the `movies_actors.id` column is used just to uniquely identify each row in `movies_actors`.

The `movies_actors` table is no different than any other table in our database and it's role as a join table between `movies` and `actors` is a design pattern. For example, we can add more columns to the `movies_actors` table just like with any other table. We could take advantage of this to add attributes that are specific to that movie-actor combination (e.g. `Salary` or `Awards Nominated`).

Creating a join table is similar to creating a regular table except that there need to be 2 foreign columns that reference the 2 tables in the many-to-many relationship:

```

```

```
CREATE table movies_actors (
```

```
id INTEGER PRIMARY KEY,
```

```
movie_id INTEGER REFERENCES movies(id),
```

```
actor_id INTEGER REFERENCES actors(id) 
);
```

Let's explore the data in these tables we just discussed a bit further. We've added information about all of the actors and movies from the `nominations` table to the `movies`, `actors`, and `movies_actors` tables. This will enable us to practice using many-to-many relations

## 7: Join Table

### Instructions

- Write a query that returns the first 5 rows in `movies_actors` and assign the results to `five_join_table`.
- Write a query that returns the first 5 rows in `actors` and assign the results to `five_actors`.
- Write a query that returns the first 5 rows in `movies` and assign the results to `five_movies`.
- Then use the `print` function to display `five_join_table`, `five_actors`, and `five_movies`.

```

five_movies = conn.execute("select * from movies limit 5;").fetchall()
five_actors = conn.execute("select * from actors limit 5;").fetchall()
five_join_table = conn.execute("select * from movies_actors limit 5;").fetchall()

print(five_movies)
print(five_actors)
print(five_join_table)
```

## 8: Querying A Many-To-Many Relation

Recall that the values in our join table, `movies_actors`, are all just *integer* id values that refer to different rows in the `movies` and `actors` tables. If we wanted to know the actors who starred in `The Fighter` that were nominated for an Academy Award between 2001 and 2010, we'd have to use multiple joins in our query across all 3 tables.

Let's first join the `movies` table with the `movies_actors` table:

```

```

```
SELECT * FROM movies
```

```
INNER JOIN movies_actors ON movies.id == movies_actors.movie_id
```

Here's a preview of the results of that query. Note that in the column names, we use dot notation to connect the table name with the column name (e.g. `movies.id` refers to the `id` column from the `movies` table):

| movies.id | movies.movie | movies_actors.id | movies_actors.movie_id | movies_actors.actor_id |
| --------- | ------------ | ---------------- | ---------------------- | ---------------------- |
| 1         | Biutiful     | 1                | 1                      | 1                      |
| 2         | True Grit    | 2                | 2                      | 2                      |

You may have noticed that the `movies_actors.id` column skips from `5` to `10`. We wanted to demonstrate that there's not just one row in the result for each movie in `movies` since the movie, `The King's Speech`shows up twice in the sample results. The results of the query so far are really just the cross join of all the rows in the `movies` table with all the rows in the `movies_actors` table.

We then need to join these results with the `actors` columns. To do this, add another `JOIN` statement in our query:

```

```

```
SELECT * FROM movies
```

```
INNER JOIN movies_actors ON movies.id == movies_actors.movie_id
```

```
INNER JOIN actors ON movies_actors.actor_id == actors.id
```

Here's a preview of the results of this query:

| movies.id | movies.movie       | movies_actors.id | movies_actors.movie_id | movies_actors.actor_id | actors.id | actors.actor    |
| --------- | ------------------ | ---------------- | ---------------------- | ---------------------- | --------- | --------------- |
| 1         | Biutiful           | 1                | 1                      | 1                      | 1         | Javier Bardem   |
| 2         | True Grit          | 2                | 2                      | 2                      | 2         | Jeff Bridges    |
| 3         | The Social Network | 3                | 3                      | 3                      | 3         | Jesse Eisenberg |
| 4         | The King's Speech  | 4                | 4                      | 4                      | 4         | Colin Firth     |
| 5         | 127 Hours          | 5                | 5                      | 5                      | 5         | James Franco    |
| 4         | The King's Speech  | 10               | 4                      | 10                     | 10        | Geoffrey Rush   |

We now have a row for each actor in `actors` that played in each movie in `movies`. However, if you go back to the original problem, you'll notice that we were only interested in actors that starred in `The Fighter`. To accomplish this, we can modify the columns returned in the `SELECT` statement and add filtering criteria using the `WHERE` statement:

```

```

```
SELECT actors.actor FROM movies
```

```
INNER JOIN movies_actors ON movies.id == movies_actors.movie_id
```

```
INNER JOIN actors ON movies_actors.actor_id == actors.id
```

```
WHERE movies.movie == "The Fighter";
```

We'll get back just the 3 actors in our database that starred in `The Fighter`:

| actors.actor   |
| -------------- |
| Christian Bale |
| Amy Adams      |
| Melissa Leo    |

You may have noticed that we used dot notation to specify the column name in the query above:

- `movies.movie` in the `WHERE` statement.
- `actors.actor` in the `SELECT`statement.

While this dot notation is required in the `JOIN` statement, it's optional in the `SELECT` and `WHERE` statements. It's a good habit, however, to write out the full name (instead of just `movie` or `actor`) of the column using dot notation. Besides the `id` column, you'll often work with multiple tables that contain the same column names and using dot notation helps us and the database system know what exactly we're referring to.

In the query above,

- we started with the `movies` table (in our `select`),
- joined with the `movies_actors` table,
- and then finally joined with the `actors` table.

We could have actually accomplished the same thing by:

- starting with the `actors` table,
- joining with the `movies_actors` table, and then joining with the `movies` table

since the filtering criteria is still the same (`WHERE movies.movie == "The Fighter"`).

Here's a good summary of the steps you need to do when querying tables that are in a many-to-many relation:

- first, state the question you want answered:
  - we want all of the actors that starred in `The Fighter`. Information on `The Fighter` is in the `movies`table and there's a join table we'll need to use to get the related information from the `actors` table.
- then, understand what joins you need and the filtering criteria you need:
  - we need to join the `movies` table with the `movies_actors` table, then join the results with the `actors`table.
  - our filtering criteria is that we only want the row from `movies` corresponding to `The Fighter`.
- finally, write the query you need based on the joins, columns, and filtering criteria you need.

Writing multiple joins to query tables in a many-to-many relation can be overwhelming at first but it's nothing that practice can't help you overcome. As you practice more, you'll find yourself skipping right to writing the query itself as this kind of querying becomes second nature to you.

## 9: Querying A Many-To-Many Relation

### Instructions

- Modify the query we wrote earlier to instead return all the actors that starred in `The King's Speech`.
  - We're interested in both the actor name as well as the movie name this time (in that order).
- Run the query and assign the results *list* to `kings_actors`.
- Then, use the `print` function to display `kings_actors`.

```

q = '''
SELECT actors.actor,movies.movie FROM movies
INNER JOIN movies_actors ON movies.id == movies_actors.movie_id
INNER JOIN actors ON movies_actors.actor_id == actors.id
WHERE movies.movie == "The King's Speech";
'''
kings_actors = conn.execute(q).fetchall()
print(kings_actors)
```

## 10: Practice: Querying A Many-To-Many Relation

In this step, you'll synthesize the concepts learned in this mission by writing a query from scratch that pulls information from 3 tables.

### Instructions

- Write a query that returns all of the movies that `"Natalie Portman"` played in.We want to return only the `movie` name (from the `movies` table) and the `actor` name (from the `actors` table).You need to first join the `movies` table with the `movies_actors` table.Then, you need to join the `movies_actors` table with the `actors` table.Finally, you need to add a `where` statement that limits the results to just where `actors.actor` is equal to `Natalie Portman`.
- Run the query and assign the full results *list* to `portman_joins`.
- Use the `print` function to display `portman_joins`.

```

q = '''
select movies.movie, actors.actor from movies
inner join movies_actors on movies.id == movies_actors.movie_id
inner join actors on actors.id == movies_actors.actor_id
where actors.actor == "Natalie Portman";
'''
portman_joins = conn.execute(q).fetchall()
print(portman_joins)
```

## 11: Caveats

While normalization helps reduce data redundancy and allows us to decouple related columns into separate tables, too much normalization can do more harm than good. A highly normalized database means that even some basic queries can involve joining multiple tables together.

You may have wondered why we didn't try to separate out the actors (`nominee` column) and the movie names (`movie` column) in the `nominations` table. We could have replaced these columns with foreign key references to the `actors` and `movies` tables instead. This is because we probably wouldn't have realized the gains of normalization by replacing the actual values with foreign key references.

If we think that we'll almost always be accessing the movies and actors names when we're querying the `nominations` table, then forcing the user to do multiple joins to get the relevant information is quite cumbersome. In addition, we know that once an awards ceremony has finished, the movies and nominees are not going to change. This means that another benefit of normalization, easy updating and editing of related values, probably won't be realized.

When we represented the `year` and `host` columns in a separate table from the nominations table, we made the assumption that we don't always need to access both columns every single time when querying the `nominations` table. We preferred having less data redundancy and writing a join when we needed to.

Lastly, it's important to remember that the schema isn't set in stone. In many cases, it's best to start out with a denormalized representation of your data with one, or a few, giant tables. As your data grows and your use cases change, you can rethink your schema and restructure your data accordingly. When structuring your data and writing a schema, it's important to remember the tradeoffs that come with normalization.

## 12: Next Steps

In this mission, we learned about 2 different kinds of relations, how to query tables in a database with these kinds of relations, and the paradigm of database normalization. Next up is a guided project where we'll explore how to create a schema and and the relations we learned about in this mission.

# Guided Project: Preparing data for SQLite

## 1: Introduction To The Data

So far, we've learned how to write SQL queries to interact with existing databases. In this guided project, you'll learn how to clean a CSV dataset and add it to a SQLite database. If you're new to either our guided projects or Jupyter notebook in general, you can learn more [here](https://www.dataquest.io/mission/162/guided-project-using-jupyter-notebook). You can find the solutions to this guided project [here](https://github.com/dataquestio/solutions/blob/master/Mission215Solutions.ipynb).

We'll work with data on Academy Award nominations, which can be downloaded [here](https://www.aggdata.com/awards/oscar). The Academy Awards, also known as the Oscars, is an annual awards ceremony hosted to recognize the achievements in the film industry. There are many different awards categories and the members of the academy vote every year to decide which artist or film should get the award. The awards categories have changed over the years, and you can learn more about when categories were added on [Wikipedia](http://bit.ly/1Rig7Gs).

Here are the columns in the dataset, `academy_awards.csv`:

- `Year` - the year of the awards ceremony.
- `Category` - the category of award the nominee was nominated for.
- `Nominee` - the person nominated for the award.
- `Additional Info` - this column contains additional info like:the movie the nominee participated in.the character the nominee played (for acting awards).
- `Won?` - this column contains either `YES` or `NO` depending on if the nominee won the award.

Read in the dataset into a Dataframe and explore it to become more familiar with the data. Once you've cleaned the dataset, you'll use a Pandas helper method to export the data into a SQLite database.

### Instructions

- Import `pandas` and read the CSV file `academy_awards.csv`into a Dataframe using the `read_csv` method.When reading the CSV, make sure to set the encoding to `ISO-8859-1` so it can be parsed properly.
- Start exploring the data in Pandas and look for data quality issues.
  - Use the `head` method to explore the first few rows in the Dataframe.
  - There are 6 unnamed columns at the end. Use the `value_counts` method to explore if any of them have valid values that we need.
  - You'll notice that the `Additional Info` column contains a few different formatting styles. Start brainstorming ways to clean this column up.

## 2: Filtering The Data

The dataset is incredibly messy and you may have noticed many inconsistencies that make it hard to work with. Most columns don't have consistent formatting, which is incredibly important when we use SQL to query the data later on. Other columns vary in the information they convey based on the type of awards category that row corresponds to.

In the **SQL and Databases: Intermediate** course, we worked with a subset of the same dataset. This subset contained only the nominations from years 2001 to 2010 and only the following awards categories:

- `Actor -- Leading Role`
- `Actor -- Supporting Role`
- `Actress -- Leading Role`
- `Actress -- Supporting Role`

Let's filter our Dataframe to the same subset so it's more manageable.

### Instructions

- Before we filter the data, let's clean up the `Year` column by selecting just the first 4 digits in each value in the column, therefore excluding the value in parentheses:
  - Use [Pandas vectorized string methods](http://pandas.pydata.org/pandas-docs/stable/text.html#text-string-methods) to select just the first 4 elements in each *string*.E.g. `df["Year"].str[0:2]` returns a Series containing just the first 2 characters for each element in the `Year` column.
  - Assign this new Series to the `Year` column to overwrite the original column.
  - Convert the `Year` column to the `int64` data type using [`astype`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.astype.html). Make sure to reassign the integer Series object back to the `Year` column in the Dataframe or the changes won't be reflected.
- Use conditional filtering to select only the rows from the Dataframe where the `Year` column is larger than `2000`. Assign the new filtered Dataframe to `later_than_2000`.
- Use conditional filtering to select only the rows from `later_than_2000` where the `Category` matches one of the 4 awards we're interested in.
  - Create a *list* of *strings* named `award_categories`with the following *strings*:`Actor -- Leading Role``Actor -- Supporting Role``Actress -- Leading Role``Actress -- Supporting Role`
  - Use the [`isin` method](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.isin.html) in the conditional filter to return all rows in a column that match any of the values in a *list* of *strings*.Pass in `award_categories` to the `isin` method to return all rows : `later_than_2000[later_than_2000["Category"].isin(award_categories)]`Assign the resulting Dataframe to `nominations`.

## 3: Cleaning Up The Won? And Unnamed Columns

Since SQLite uses the *integers* `0` and `1`to represent Boolean values, convert the `Won?` column to reflect this. Also rename the `Won?` column to `Won` so that it's consistent with the other column names. Finally, get rid of the 6 extra, unnamed columns, since they contain only null values in our filtered Dataframe `nominations`.

### Instructions

- Use the [Series method `map`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.map.html) to replace all `NO` values with `0`and all `YES` values with `1`.
  - Select the `Won?` column from `nominations`.
  - Then create a *dictionary* where each key is a value we want to replace and each value is the corresponding replacement value.The following dictionary `replace_dict = { True: 1, False: 0 }` would replace all `True`values with `1` and all `False` values with `0`.
  - Call the `map` function on the Series object and pass in the *dictionary* you created.
  - Finally, reassign the new Series object to the `Won?`column in `nominations`.
- Create a new column `Won` that contains the values from the `Won?` column.
  - Select the `Won?` column and assign it to the `Won`column. Both columns should be in the Dataframe still.
- Use the `drop` method to remove the extraneous columns.
  - As the required parameter, pass in a *list* of *strings* containing the following values:`Won?``Unnamed: 5``Unnamed: 6``Unnamed: 7``Unnamed: 8``Unnamed: 9``Unnamed: 10`
  - Set the `axis` parameter to `1` when calling the `drop`method.
  - Assign the resulting Dataframe to `final_nominations`.

## 4: Cleaning Up The Additional Info Column

Now clean up the `Additional Info`column, whose values are formatted like so:

`MOVIE {'CHARACTER'}`

Here are some examples:

- `Biutiful {'Uxbal'}` - `Biutiful` is the movie and `Uxbal` is the character this nominee played.
- `True Grit {'Rooster Cogburn'}` - `True Grit` is the movie and `Rooster Cogburn` is the character this nominee played.
- `The Social Network {'Mark Zuckerberg'}` - `The Social Network` is the movie and `Mark Zuckerberg` is the character this nominee played.

The values in this column contain the movie name and the character the nominee played. Instead of keeping these values in 1 column, split them up into 2 different columns for easier querying.

### Instructions

- Use [vectorized string methods](http://pandas.pydata.org/pandas-docs/stable/basics.html#vectorized-string-methods) to clean up the `Additional Info` column:
  - Select the `Additional Info` column and strip the single quote and closing brace (`"'}"`) using the [`rstrip` method](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.str.rstrip.html). Assign the resulting Series object to `additional_info_one`.
  - Split `additional_info_one` on the *string*, `" {'`, using the `split` method and assign to `additional_info_two`. Each value in this Series object should be a *list* containing the movie name first then the character name.
  - Access the first element from each *list* in `additional_info_two` using vectorized string methods and assign to `movie_names`. Here's what the code looks like: `additional_info_two.str[0]`
  - Access the second element from each *list* in `additional_info_two` using vectorized string methods and assign to `characters`.
- Assign the Series `movie_names` to the `Movie` column in the `final_nominations` Dataframe.
- Assign the Series `characters` to the `Character` column in the `final_nominations` Dataframe.
- Use the `head` method to preview the first few rows to make sure the values in the `Character` and `Movie` columns resemble the `Additional Info` column.
- Drop the `Additional Info` column using the `drop`method.

Your Dataframe should look like:

![Imgur](https://dq-content.s3.amazonaws.com/bknJGXc.png)

## 5: Exporting To SQLite

Now that our Dataframe is cleaned up, let's write these records to a SQL database. We can use the Pandas Dataframe method [`to_sql`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.to_sql.html) to create a new table in a database we specify. This method has 2 required parameters:

- `name` - *string* corresponding to the name of the table we want created. The rows from our Dataframe will be added to this table after it's created.
- `conn`: the Connection instance representing the database we want to add to.

Behind the scenes, Pandas creates a table and uses the first parameter to name it. Pandas uses the data types of each of the columns in the Dataframe to create a SQLite schema for this table. Since SQLite uses *integer* values to represent Booleans, it was important to convert the `Won`column to the *integer* values `0` and `1`. We also converted the `Year` column to the *integer* data type, so that this column will have the appropriate type in our table. Here's the mapping for our columns from the Pandas data type to the SQLite data type:

| column    | Pandas data type | SQLite data type |
| --------- | ---------------- | ---------------- |
| Year      | int64            | integer          |
| Won       | int64            | integer          |
| Category  | object           | text             |
| Nominee   | object           | text             |
| Movie     | object           | text             |
| Character | object           | text             |

After creating the table, Pandas creates a large `INSERT` query and runs it to insert the values into the table. We can customize the behavior of the `to_sql`method using its parameters. For example, if we wanted to append rows to an existing SQLite table instead of creating a new one, we can set the `if_exists`parameter to `"append"`. By default, `if_exists` is set to `"fail"` and no rows will be inserted if we specify a table name that already exists. If we're inserting a large number of records into SQLite and we want to break up the inserting of records into chunks, we can use the `chunksize` parameter to set the number of rows we want inserted each time.

Since we're creating a database from scratch, we need to create a database file first so we can connect to it and export our data. To create a new database file, we use the `sqlite3` library to connect to a file path that doesn't exist yet. If Python can't find the file we specified, it will create it for us and treat it as a SQLite database file.

SQLite doesn't have a [special file format](http://stackoverflow.com/questions/808499/what-is-the-best-extension-name-sqlite-database-files) and you can use any file extension you'd like when creating a SQLite database. We generally use the `.db` extension, which isn't a file extension that's generally used for other applications.

### Instructions

- Create the SQLite database `nominations.db` and connect to it.
  - Import `sqlite3` into the environment.
  - Use the `sqlite3` method `connect` to connect to the database file `nominations.db`.Since it doesn't exist in our current directory, it will be automatically created.Assign the returned Connection instance to `conn`.
- Use the [Dataframe method `to_sql`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.to_sql.html) to export `final_nominations` to `nominations.db`.
  - For the first parameter, set the table name to `"nominations"`.
  - For the second parameter, pass in the Connection instance.
  - Set the `index` parameter to `False`.

## 6: Verifying In SQL

Let's now query the database to make sure everything worked as expected.

### Instructions

- Import `sqlite3` into the environment.
- Create a Connection instance using the `sqlite3` method `connect` to connect to your database file.
- Explore the database to make sure the `nominations` table matches our Dataframe.Return and print the schema using [`pragma table_info()`](https://www.sqlite.org/pragma.html#pragma_table_info). The following schema should be returned:`Year`: `Integer`.`Category`: `Text`.`Nominee`: `Text`.`Won`: `Text`.`Movie`: `Text`.`Character`: `Text`.Return and print the first 10 rows using the `SELECT`and `LIMIT` statements.
- Once you're done, use the Connection method `close` to close the connection to the database.

## 7: Next Steps

In this guided project, you used Pandas to clean a CSV dataset and export it to a SQLite database. As a data scientist, it's important to learn many tools and how to use them together to accomplish what you need to. As you do more guided projects, you'll become more familiar with the strengths and weaknesses of each tool. For example, you probably have noticed that data cleaning is much easier in Pandas than in SQL.

- For next steps, explore the rest of our original dataset `academy_awards.csv`and brainstorm how to fix the rest of the dataset:The awards categories in older ceremonies were different than the ones we have today. What relevant information should we keep from older ceremonies?What are all the different formatting styles that the `Additional Info` column contains. Can we use tools like regular expressions to capture these patterns and clean them up?The nominations for the `Art Direction` category have lengthy values for `Additional Info`. What information is useful and how do we extract it?Many values in `Additional Info` don't contain the character name the actor or actress played. Should we toss out character name altogether as we expand our data? What tradeoffs do we make by doing so?What's the best way to handle awards ceremonies that included movies from 2 years?E.g. see `1927/28 (1st)` in the `Year` column.

Next up is a guided project where we'll continue down the path we started and explore how to normalize our data into multiple tables using relations.

# Guided Project: Creating relations in SQLite

## 1: Introduction To The Data

So far in the **SQL and Databases: Intermediate** course, we explored how to modify existing data in a table and create our own schemas. In the [Database Normalization and Relations mission](https://www.dataquest.io/mission/181/database-normalization-and-relations), we then learned about the benefits of normalization and how to query an existing database on Academy Award nominations. In the previous guided project, we walked through how to clean and prepare the original CSV dataset on Academy Award nominations and export the data into a SQLite database as a single, denormalized table. In this guided project, we will walk through how to normalize our single table into multiple tables and how to create relations between them.

If you're new to either our guided projects or Jupyter notebook in general, you can learn more [here](https://www.dataquest.io/mission/162/guided-project-using-jupyter-notebook). You can find the solutions to this guided project [here](https://github.com/dataquestio/solutions/blob/master/Mission216Solutions.ipynb).

As a quick refresher, the Academy Awards, also known as the Oscars, is an annual awards ceremony hosted to recognize the achievements in the film industry. There are many different awards categories and the members of the academy vote every year to decide which artist or film should get the award. Each row in our data represents a nomination for an award. Recall that our database file, `nominations.db`, contains just the `nominations` table. This table has the following schema:

- `Year` - the year of the awards ceremony, *integer* type.
- `Category` - the category of award the nominee was nominated for, *text* type.
- `Nominee` - the person nominated for the award, *text* type.
- `Movie` - the movie the nominee participated in, *text* type.
- `Character` - the name of the character the nominee played, *text* type.
- `Won` - if this nominee won the award, *integer* type.

Let's now set up our enviroment and spend some time getting familiar with the data before we start normalizing it.

### Instructions

- Let's first get everything you need setup.
  - Import `sqlite3` into the environment.
  - Connect to `nominations.db` and assign the Connection instance to `conn`.
- Let's now write and run some queries to explore the data.
  - Return the schema using [`pragma table_info()`](https://www.sqlite.org/pragma.html#pragma_table_info) and assign to `schema`.
  - Return the first 10 rows using the `SELECT` and `LIMIT`statements and assign to `first_ten`.
  - Since both `schema` and `first_ten` are *lists*, use a `for` loop to iterate over each element and print each element. This makes our output easier to understand.

## 2: Creating The Ceremonies Table

Let's now add information on the host for each awards ceremony. Instead of adding a `Host` column to the `nominations` table and having lots of redundant data, we'll create a separate table called `ceremonies`which contains data specific to the ceremony itself.

Let's create a `ceremonies` table that contains the `Year` and `Host` for each ceremony and then set up a one-to-many relationship between `ceremonies` and `nominations`. In this screen, we'll focus on creating the `ceremonies` table and inserting the data we need and in the next guided step, we'll focus on setting up the one-to-many relationship.

The `ceremonies` table will contain 3 fields:

- `id` - unique identifier for each row, *integer* type.
- `Year` - the year of the awards ceremony, *integer* type.
- `Host` - the host of the awards ceremony, *text* type.

Before we can create and insert into the `ceremonies` table, we need to look up the host for each ceremony from 2000 to 2010. While we could represent each row as a *tuple* and write a SQL query with an `INSERT` statement to add each row to the `ceremonies` table, this is incredibly cumbersome.

The Python sqlite3 library comes with an [`executemany` method](https://docs.python.org/3/library/sqlite3.html#sqlite3.Cursor.executemany) that let's us easily mass insert records into a table. The `executemany` method requires the records we want to insert to be represented as a *list* of *tuples*. We then just need to write a single `INSERT` query with placeholder elements and specify that we want the *list* of *tuples* to be dropped into the query.

Let's first create the *list* of *tuples* representing the data we want inserted and then we'll walk through the placeholder query we need to write. We'll skip over creating the `ceremonies` table for now since we've explored how to create a table earlier in the course.

[Wikipedia](https://en.wikipedia.org/wiki/List_of_Academy_Awards_ceremonies#Ceremonies) lists the host(s) for each awards ceremony. Even though the 2010 ceremony had 2 hosts, we selected just the first host, `Steve Martin`, to keep things simple. Here's what the *list* of *tuples* looks like:

```

```

```
years_hosts = [(2010, "Steve Martin"),
```

```
               (2009, "Hugh Jackman"),
```

```
               (2008, "Jon Stewart"),
```

```
               (2007, "Ellen DeGeneres"),
```

```
               (2006, "Jon Stewart"),
```

```
               (2005, "Chris Rock"),
```

```
               (2004, "Billy Crystal"),
```

```
               (2003, "Steve Martin"),
```

```
               (2002, "Whoopi Goldberg"),
```

```
               (2001, "Steve Martin"),
```

```
               (2000, "Billy Crystal"),
```

```
            ]
```

We then need to write the `INSERT` query with placeholder values. Instead of having specific values in the query string, we use a question mark (`?`) to act as a placeholder in the `values` section of the query:

```

```

```
insert_query = "INSERT INTO ceremonies (Year, Host) VALUES (?,?);"
```

Since the placeholder elements (`?`) will be replaced by the values in `years_hosts`, you need to make sure the number of question marks matches the length of each *tuple* in `years_hosts`. Since each *tuple* has 2 elements, we need to have 2 question marks as the placeholder elements. We don't need to specify values for the `id` column since it's a primary key column. When inserting values, recall that SQLite automatically creates a unique primary key for each row.

We then call the `executemany` method and pass in `insert_query` as the first parameter and `years_hosts` as the second parameter:

```

```

```
conn.executemany(insert_query, years_hosts)
```

Python will iterate through `years_hosts`and replace the placeholder elements with the values in `years_hosts` to generate and execute the following query:

```

```

```
INSERT INTO ceremonies (Year, Host) VALUES
```

```
(2010, "Steve Martin"),
```

```
(2009, "Hugh Jackman"),
```

```
(2008, "Jon Stewart"),
```

```
(2007, "Ellen DeGeneres"),
```

```
(2006, "Jon Stewart"),
```

```
(2005, "Chris Rock"),
```

```
(2004, "Billy Crystal"),
```

```
(2003, "Steve Martin"),
```

```
(2002, "Whoopi Goldberg"),
```

```
(2001, "Steve Martin"),
```

```
(2000, "Billy Crystal")
```

```
;
```

Let's now create the `ceremonies` table and populate the table with the data on ceremonies.

### Instructions

- Create the `ceremonies` table with the following schema:
  - `id`: *integer*, primary key.
  - `Year`: *integer*.
  - `Host`: *text*.
- Create the *list* of *tuples*, `years_hosts`, that contains the values for the rows we want to insert into the `ceremonies`table.
- Use the Connection method `executemany` to insert the values into the `ceremonies` table.
- To verify that the `ceremonies` table was created and populated correctly, run the following queries:
  - Return the first 10 rows using the `SELECT` and `LIMIT`statements.
  - Return the schema using [`pragma table_info()`](https://www.sqlite.org/pragma.html#pragma_table_info).

## 3: Foreign Key Constraints

Since we'll be creating relations using foreign keys, we need to turn on **foreign key constraints**. By default, if you insert a row into a table that contains one or multiple foreign key columns, the record will be successfully inserted even if the foreign key reference is incorrect.

For example, since the `ceremonies` table only contains the `id` values `1` to `10`, inserting a row into `nominations` while specifying that the `ceremony_id` value be `11` will work and no error will be returned. This is problematic because if we try to actually join that row with the `ceremonies` table, the results set will be empty since the `id` value `11` doesn't map to any row in the `ceremonies` table. To prevent us from inserting rows with nonexisting foreign key values, we need to turn on foreign key constraints by running the following query:

```

```

```
PRAGMA foreign_keys = ON;
```

**The above query needs to be run every time you connect to a database where you'll be inserting foreign keys.**Whenever you try inserting a row into a table containing foreign key(s), SQLite will query the linked table to make sure that foreign key value exists. If it does, the transaction will continue as expected. If it doesn't, then an error will be returned and the transaction won't go through.

### Instructions

- Turn on foreign key constraints by writing and running the following query:
  - `PRAGMA foreign_keys = ON;`

## 4: Setting Up One-To-Many

The next step is to remove the `Year`column from `nominations` and add a new column, `ceremony_id`, that contains the foreign key reference to the `id` column in the `ceremonies` table. Unfortunately, we can't remove columns from an existing table in SQLite or change its schema. [The goal of SQLite](https://en.wikipedia.org/wiki/SQLite#History) is to create an incredibly lightweight, open source database that contains a common, but reduced, set of features. While this has allowed SQLite to [become the most popular database in the world](https://www.sqlite.org/mostdeployed.html), SQLite doesn't have the ability to heavily modify an existing table to keep the code base lightweight.

The only alterations we can make to an existing table are renaming it or adding a new column. This means that we can't just remove the `Year` column from `nominations` and add the `ceremony_id`column. We need to instead:

- create a new table `nominations_two`with the schema we want,
- populate `nominations_two` with the records we want,
- delete the original `nominations` table,
- rename `nominations_two` to `nominations`.

For `nominations_two`, we want the following schema:

- `id`: primary key, *integer*,
- `category`: *text*,
- `nominee`: *text*,
- `movie`: *text*,
- `character`: *text*,
- `won`: *integer*,
- `ceremony_id`: foreign key reference to `id` column from `ceremonies`.

First, we need to select all the records from the original `nominations` table with the columns we want and use an `INNER JOIN` to add the `id` field from `ceremonies` for each row:

```

```

```
SELECT nominations.category, nominations.nominee, nominations.movie, nominations.character, nominations.won, ceremonies.id
```

```
FROM nominations
```

```
INNER JOIN ceremonies ON
```

```
nominations.year == ceremonies.year
```

```
;
```

Then we can write the placeholder insert query we need to insert these records into `nominations_two`. Let's create and populate the `nominations_two` table in this screen and we'll work through the rest in the next screen.

### Instructions

- Write and run the query to create the `nominations_two`table with the schema specified above.
- Write and run the query we discussed above that returns the records from the `nominations` table and assign the results set to `joined_nominations`.
- Write a placeholder insert query that can insert values into `nominations_two` and assign this query to `insert_nominations_two`. Make sure the ordering of the columns matches the ordering from `joined_nominations`.
- Use the Connection method `executemany` to insert the records in `joined_nominations` using the placeholder insert query `insert_nominations_two`.
- Verify your work by returning the first 5 rows from `nominations_two`.

## 5: Deleting And Renaming Tables

We now need to delete the `nominations`table since we'll be using the `nominations_two` table moving forward. We can use the [`DROP TABLE`](https://sqlite.org/lang_droptable.html) statement to drop the original `nominations` table.

Once we drop this table, we can use the `ALTER TABLE` statement to rename `nominations_two` to `nominations`. Here's what the syntax looks like for that statement:

```

```

```
ALTER TABLE [current_table_name]
```

```
RENAME TO [future_table_name]
```

### Instructions

- Write and run the query that deletes the `nominations`table from the database.
- Write and run the query that renames `nominations_two` to `nominations`.

## 6: Creating A Join Table

In the [**Database Normalization and Relations** mission](https://www.dataquest.io/mission/181/database-normalization-and-relations), we learned how to query the tables involved in the many-to-many relation between movies and actors.

Here's a preview of what those tables look like:

movies_actorsactorsmoviesmovie_ididactor_idactorididmovie111AmyAdams11TheFighter122ChristianBale22Doubt231MerylStreep33Junebug243EmbethDavidtz4351364

Creating a join table is no different than creating a regular one. To create the `movies_actors` join table we need to declare both of the foreign key references when specifying the schema:

```

```

```
CREATE TABLE movies_actors (
```

```
id INTEGER PRIMARY KEY,
```

```
movie_id INTEGER REFERENCES movies(id), 
```

```
actor_id INTEGER REFERENCES actors(id)
```

```
);
```

In this screen, you'll create the 3 tables we need.

### Instructions

- Create the 3 tables we need to model the relationship between movies and actors. You need to create the `movies`and `actors` tables before creating the `movies_actors`table for the foreign key references to work.
- Create the `movies` table using the following schema:
  - `id`: primary key, *integer* type.
  - `movie`: movie name, *text* type.
- Create the `actors` table using the following schema:
  - `id`: primary key, *integer* type.
  - `actor`: actor's full name, *text* type.
- Create the `movies_actors` join table using the following schema:
  - `id`: primary key, *integer* type.
  - `movie_id`: foreign key reference to `movies.id`column.
  - `actor_id`: foreign key reference to `actors.id`column.

## 7: Next Steps

In this guided project, you explored how to create relations and set up a join table. That's it for the guided steps but we encourage you to keep exploring! Here are some ideas:

- What other datasets can we add to the database?
- Based on what you know, brainstorm how you would populate the join table and the linked tables using data from `nominations`.

# Using PostgreSQL

## 1: SQLite Vs PostgreSQL

So far, we've been using a database engine called [SQLite](https://www.sqlite.org/). SQLite is one of the most common database engines, and has many advantages:

- The database is stored in a single file, making it portable.
- You can use a SQLite database directly from Python, and don't need a separate program running.
- It implements most SQL commands, enabling you to use most of the statements you're familiar with.

However, particularly when developing larger applications, SQLite has a few downsides that make other database engines more attractive:

- Only one process at a time can write to the database. When you have a complex web application, you may have multiple processes updating information in the database at the same time. For example, on [Facebook](https://www.facebook.com/), one process might handle updating user information, and another might handle generating the news feed.
- You can't take advantage of performance features, such as caching. Because a SQLite database is a single file, and it doesn't require a special program to run, it can't have performance optimizations like caching. When running a site like [Facebook](https://www.facebook.com/) that has a ton of traffic, it's important to be able to lookup data quickly.
- SQLite doesn't have any built-in security. With a production website, it's common to want some people to be able to modify tables in a database (write), and others to only be able to make `SELECT` queries to tables in the database (read). This is because giving someone write access to the database can be a security risk, in that they can update or overwrite data. SQLite doesn't allow for restricting access to a database in this way.

In general, SQLite is good in cases where having a small and simple database engine is important. SQLite is used extensively in embedded applications, such as Android and iOS applications.

In cases where there will be multiple users or performance is important, [PostgreSQL](http://www.postgresql.org/) is the most commonly used database engine. [PostgreSQL](http://www.postgresql.org/) is open source, and is free to download and use.

In this mission, we'll look at the basics of [PostgreSQL](http://www.postgresql.org/), then dive into creating a database, querying data, and some advanced features.

## 2: PostgreSQL Overview

[PostgreSQL](http://www.postgresql.org/), also known as Postgres, is an extremely powerful database engine. At a high level, [PostgreSQL](http://www.postgresql.org/) consists of two pieces, a server and clients. The server is a program that manages databases and handles queries. Clients communicate back and forth to the server. Only the server ever directly accesses the databases -- the clients can only make requests to the server. If you've gone through the [APIs and Web Scraping course](https://www.dataquest.io/section/apis-and-scraping), the communication process is very similar to making requests to a remote API.

One of the advantages of this model is that multiple clients can communicate with the server at the same time. This allows multiple processes to write to a database at the same time.

It's possible to run a [PostgreSQL](http://www.postgresql.org/) server either remotely or locally. If it's remote, you connect to it via the internet. If it's local, you connect to it on your own machine. In both cases, you'll be connecting to PostgreSQL via a system port.

One way to think of ports is to think of receiving mail at an apartment building. Let's say `5` people live in an apartment building, but they only have a single address. All incoming mail will come to the address, then have to be sorted out and given to each person:

ApartmentnumberofApartmentmailrecipientnumber112Sort2SendingincomingReceivingmailmailmail334455

All incoming mail is merged into a single pile, because the whole apartment building only has one address. Each apartment occupant then has to sort through the pile to find their mail. Not only is this inefficient, it also results in some apartments getting mail that isn't theirs by accident.

We can make life easier for everyone by giving each apartment its own address:

ApartmentnumberofApartmentmailrecipientnumber1122SendingReceivingmailmail334455

Now, nobody has to sort mail, and it's unlikely that someone will accidentally get a message that isn't theirs.

Every computer runs dozens to hundreds of programs. Many of these programs can accept incoming connections from the internet. For instance, web servers, such as the servers that run Dataquest, run on ordinary computers and accept connections from people all over the world. Once the connections are created, data is sent along the connections.

If every program received data in the same stream, you'd have a similar situation to all of the apartments only having one address. Each program would be responsible for figuring out which messages were for it, and many messages would be sent to the wrong program. It would be impossible to know which program you were communicating with when you connected to the computer.

One way to avoid this is for each program to have its own address. A system port is similar to an apartment number in that a port on a computer can only be used by one server at a time. For example, web servers run on port `80`. Any incoming messages on this computer port are automatically sent to the program.

By default, PostgreSQL uses port `5432` to communicate with the outside world. If you start a PostgreSQL server, it will listen for incoming connections on port `5432`. Clients will be able to connect to the server using this port. If you start a client, you'll have to specify which server to connect to, along with the port to connect to.

## 3: Psycopg2

There are many clients for [PostgreSQL](http://www.postgresql.org/), including [graphical clients](https://wiki.postgresql.org/wiki/Community_Guide_to_PostgreSQL_GUI_Tools). The most common Python client for PostgreSQL is called [psycopg2](http://initd.org/psycopg/). Connecting to a PostgreSQL database using [psycopg2](http://initd.org/psycopg/) is similar to connecting to a SQLite database using the [sqlite3](https://docs.python.org/3.5/library/sqlite3.html) libary. [psycopg2](http://initd.org/psycopg/) also uses [Connection](http://initd.org/psycopg/docs/connection.html) and [Cursor](http://initd.org/psycopg/docs/cursor.html) objects.

We'd connect to a database using [psycopg2](http://initd.org/psycopg/) like this:

```

```

```
import psycopg2
```

```
conn = psycopg2.connect("dbname=postgres user=postgres")
```

```
cur = conn.cursor()
```

You may notice that we have to specify both a database name and a user name. A [PostgreSQL](http://www.postgresql.org/) server can have multiple databases and multiple users, so we need to specify which user we're connecting as, and which database we're connecting to.

When [PostgreSQL](http://www.postgresql.org/) is first installed, the default user account is called `postgres`, with an associated database called `postgres`.

You may also notice that we didn't specify a server to connect to, or a port to connect using. [psycopg2](http://initd.org/psycopg/) will default to connecting to port `5432` on the current computer.

When you're done with a [Connection](http://initd.org/psycopg/docs/connection.html) object, you should close it to avoid issues where one connection prevents another from executing a query. You can close a connection like this:

```

```

```
conn.close()
```

Closing a connection will terminate the client's connection with the [PostgreSQL](http://www.postgresql.org/) server. It's a good idea to close a connection whenever you're done executing your queries.

We've automatically started a [PostgreSQL](http://www.postgresql.org/) server, and created a database called `dq`, with an associated user called `dq`.

### Instructions

- Import the [psycopg2](http://initd.org/psycopg/) library.
- Connect to the `dq` database with the user `dq`.
- Initialize a [Cursor](http://initd.org/psycopg/docs/cursor.html) object from the connection.
- Use the [print](https://docs.python.org/3/library/functions.html#print) function to display the [Cursor](http://initd.org/psycopg/docs/cursor.html) object.
- Close the [Connection](http://initd.org/psycopg/docs/connection.html) using the [close](http://initd.org/psycopg/docs/connection.html#connection.close) method.

```

import psycopg2
conn = psycopg2.connect("dbname=dq user=dq")
cur = conn.cursor()
print(cur)
conn.close()
```

## 4: Creating A Table

Once we've connected to a database, we can create a table inside that database. You may recall the `CREATE TABLE`statement from an earlier mission:

```

```

```
CREATE TABLE tableName(
```

```
   column1 dataType1 PRIMARY KEY,
```

```
   column2 dataType2,
```

```
   column3 dataType3,
```

```
   ...
```

```
);
```

We can use the same syntax to create a table in the `dq` database. In order to execute the query, we can use the [execute](http://initd.org/psycopg/docs/cursor.html#cursor.execute) method of the [Cursor](http://initd.org/psycopg/docs/cursor.html) object:

```

```

```
conn = psycopg2.connect("dbname=dq user=dq")
```

```
cur = conn.cursor()
```

```
cur.execute("SELECT * FROM notes;")
```

The above code will connect to the database `dq`, then execute a query. The syntax above should look familiar to you from using the [sqlite3](https://docs.python.org/3.5/library/sqlite3.html) library, as all the methods are the same.

### Instructions

- Connect to the `dq` database as the user `dq`
- Write a SQL query that creates a table called `notes` in the `dq` database, with the following columns and data types:`id` -- `integer` data type, and is a primary key.`body` -- `text` data type.`title` -- `text` data type.
- Execute the query using the [execute](http://initd.org/psycopg/docs/cursor.html#cursor.execute) method.
- Close the [Connection](http://initd.org/psycopg/docs/connection.html) using the [close](http://initd.org/psycopg/docs/connection.html#connection.close) method.

```

conn = psycopg2.connect("dbname=dq user=dq")
cur = conn.cursor()
cur.execute("CREATE TABLE notes(id integer PRIMARY KEY, body text, title text)")
conn.close()
```

## 5: SQL Transactions

If you checked the database `dq` now, you would notice that there actually isn't a `notes` table inside it. This isn't a bug -- it's because of a concept called SQL transations. With SQLite, every query we made that modified the data was immediately executed, and immediately changed the database.

With PostgreSQL, we're dealing with multiple users who could be changing the database at the same time. Let's imagine a simple scenario where we're keeping track of accounts for different customers of a bank. We could write a simple query to create a table for this:

```

```

```
CREATE TABLE accounts(
```

```
   id integer PRIMARY KEY,
```

```
   name text,
```

```
   balance float
```

```
);
```

Let's say we have the following two rows in the table:

```

```

```
id    name    balance
```

```
1     Jim     100
```

```
2     Sue     200
```

Let's say `Sue` gives `100` dollars to `Jim`. We could model this with two queries:

```

```

```
UPDATE accounts SET balance=200 WHERE name="Jim";
```

```

```

```
UPDATE accounts SET balance=100 WHERE name="Sue";
```

In the above example, we remove `100`dollars from `Sue`, and add `100` dollars to `Jim`. Let's say either the second `UPDATE`statement has an error in it, the database fails, or another user has a conflicting query. The first query would run properly, but the second would fail. That would result in the following:

`Jim` would be credited `100` dollars, but `100` dollars would not be removed from `Sue`. This would cause the bank to lose money.

Transactions prevent this type of behavior by ensuring that all the queries in a transaction block are executed at the same time. If any of the transactions fail, the whole group fails, and no changes are made to the database at all.

Whenever we open a [Connection](http://initd.org/psycopg/docs/connection.html) in [psycopg2](http://initd.org/psycopg/), a new transaction will automatically be created. All queries run up until the [commit](http://initd.org/psycopg/docs/connection.html#connection.commit) method is called will be placed into the same transaction block. When [commit](http://initd.org/psycopg/docs/connection.html#connection.commit) is called, the PostgreSQL engine will run all the queries at once.

If we don't want to apply the changes in the transaction block, we can call the [rollback](http://initd.org/psycopg/docs/connection.html#connection.rollback) method to remove the transaction. Not calling either [commit](http://initd.org/psycopg/docs/connection.html#connection.commit) or [rollback](http://initd.org/psycopg/docs/connection.html#connection.rollback) will cause the transaction to stay in a pending state, and will result in the changes not being applied to the database.

### Instructions

- Connect to the `dq` database as the user `dq`.
- Write a SQL query that creates a table called `notes` in the `dq` database, with the following columns and data types:`id` -- `integer` data type, and is a primary key.`body` -- `text` data type.`title` -- `text` data type.
- Execute the query using the [execute](http://initd.org/psycopg/docs/cursor.html#cursor.execute) method.
- Use the [commit](http://initd.org/psycopg/docs/connection.html#connection.commit) method on the [Connection](http://initd.org/psycopg/docs/connection.html) object to apply the changes in the transaction to the database.
- Close the [Connection](http://initd.org/psycopg/docs/connection.html).

```

conn = psycopg2.connect("dbname=dq user=dq")
cur = conn.cursor()
cur.execute("CREATE TABLE notes(id integer PRIMARY KEY, body text, title text)")
conn.commit()
conn.close()
```

## 6: Autocommitting

There are cases when you won't want to manage a transaction, and you'll instead want changes right away. This is most common when you're making changes to the database that you want to be guaranteed to happen immediately.

Some changes also have such widespread effects that they can't be wrapped inside of a transaction. One example of this is creating a database. When creating a database, we'll need to activate autocommit mode first.

To activate autocommit mode, we'll need to set the [autocommit](http://initd.org/psycopg/docs/connection.html#connection.autocommit) property of the [Connection](http://initd.org/psycopg/docs/connection.html) object to `True`.

```

```

```
conn = psycopg2.connect("dbname=dq user=dq")
```

```
conn.autocommit = True
```

```
cur = conn.cursor()
```

```
cur.execute("CREATE TABLE notes(id integer PRIMARY KEY, body text, title text)")
```

The above command will create a table called `notes` without having to explicitly commit the transaction. We'll then be able to use the `notes` table right away.

### Instructions

- Connect to the `dq` database as the user `dq`.
- Set the [autocommit](http://initd.org/psycopg/docs/connection.html#connection.autocommit) property of the [Connection](http://initd.org/psycopg/docs/connection.html) object to `True`.
- Write a SQL query that creates a table called `facts` in the `dq` database, with the following columns and data types:`id` -- `integer` data type, and is a primary key.`country` -- `text` data type.`value` -- `integer` data type.
- Execute the query using the [execute](http://initd.org/psycopg/docs/cursor.html#cursor.execute) method.
- Close the [Connection](http://initd.org/psycopg/docs/connection.html).

```

conn = psycopg2.connect("dbname=dq user=dq")
conn.autocommit = True
cur = conn.cursor()
cur.execute("CREATE TABLE facts(id integer PRIMARY KEY, country text, value text)")
conn.close()
```

## 7: Executing Queries

We can issue `SELECT` queries against a database using the [execute](http://initd.org/psycopg/docs/cursor.html#cursor.execute) method, along with the [fetchall](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall) and [fetchone](http://initd.org/psycopg/docs/cursor.html#cursor.fetchone) methods to retrieve results:

```

```

```
cur.execute("SELECT * FROM notes;")
```

```
rows = cur.fetchall()
```

```
print(rows)
```

The above code will select all of the rows in the `notes` table, then print them all out.

Of course, we don't have any rows in our table yet. You may recall how to insert rows from a previous mission:

```

```

```
INSERT INTO tableName
```

```
VALUES (value1, value2, ...);
```

The below query will insert a row into the `notes` table:

```

```

```
INSERT INTO notes
```

```
VALUES (1, 'This is my note text.', 'Test note');
```

### Instructions

- Connect to the `dq` database as the user `dq`.
- Execute a SQL query that inserts a row into the `notes` table with the following values:`id` -- `1``body` -- `'Do more missions on Dataquest.'``title` -- `'Dataquest reminder'`.
- Execute a SQL query that selects all of the rows from the `notes` table.
- Fetch all of the results and print them out.
- Close the [Connection](http://initd.org/psycopg/docs/connection.html).

```

conn = psycopg2.connect("dbname=dq user=dq")
cur = conn.cursor()
cur.execute("INSERT INTO notes VALUES (1, 'Do more missions on Dataquest.', 'Dataquest reminder');")
cur.execute("SELECT * from notes;")
rows = cur.fetchall()
print(rows)
conn.close()
```

## 8: Creating A Database

One of the most powerful aspects of [PostgreSQL](http://www.postgresql.org/) is that it enables you to create multiple databases. Different databases are generally used to hold information about different applications. For instance, if you have the following three datasets and applications:

- An application that enables you to add and remove friends in your neighborhood.
- A dataset on household income worldwide.
- An application that allows you to store and share notes.

You could in theory make different tables for each of these in an existing database. But eventually, you'll reach a point where each application has multiple tables, due to foreign keys and joins. It will get messy to manage all the tables for each application separately. By storing data for a single application in a single database, we encapsulate that application, and make it easier to manage and alter the data for it.

We can create a database using the `CREATE DATABASE` SQL statement:

```

```

```
CREATE DATABASE dbName;
```

Here's a concrete example:

```

```

```
CREATE DATABASE notes;
```

The above SQL command will create a database called `notes`. We can specify the user who will own the database when we create it as well, using the `OWNER`statement:

```

```

```
CREATE DATABASE notes OWNER postgres;
```

The above statement will create a database called `notes` with the default `postgres`user as the owner. The owner of a database is the only one that can access and modify a database, unless they give permission to other users. An exception is superusers, who we'll cover in a later mission, who can perform any action on any database without being given permission.

### Instructions

- Connect to the `dq` database with the user `dq`.
- Set the connection to autocommit mode.
- Create a database called `income` where the owner is the user `dq`.
- Close the [Connection](http://initd.org/psycopg/docs/connection.html).

```

conn = psycopg2.connect("dbname=dq user=dq")
conn.autocommit = True
cur = conn.cursor()
cur.execute("CREATE DATABASE income OWNER dq;")
conn.close()
```

## 9: Deleting A Database

We can delete a database using the `DROP DATABASE` statement. The `DROP DATABASE`statement will immediately remove a database, provided the user executing the query has the right permissions. It should be used with caution when working with real data.

```

```

```
DROP DATABASE dbName;
```

Here's a more concrete example:

```

```

```
DROP DATABASE income;
```

The above statement will remove the database called `income`, along with any tables it contains.

### Instructions

- Connect to the `dq` database with the user `dq`.
- Set the connection to autocommit mode.
- Drop the `income` database.
- Close the [Connection](http://initd.org/psycopg/docs/connection.html).

```

conn = psycopg2.connect("dbname=dq user=dq")
conn.autocommit = True
cur = conn.cursor()
cur.execute("DROP DATABASE income;")
conn.close()
```

## 10: Next Steps

In this mission, we covered the basics of [PostgreSQL](http://www.postgresql.org/), along with transactions and working with databases. In the next mission, we'll look at managing databases, users, and permissions in [PostgreSQL](http://www.postgresql.org/).

# Command line PostgreSQL

## 1: The Psql Tool

In the last mission, we worked with [PostgreSQL](http://www.postgresql.org/), or Postgres, databases and tables. In this mission, we'll learn how to work with the PostgreSQL command line tool, called [psql](http://www.postgresql.org/docs/9.4/static/app-psql.html).

[psql](http://www.postgresql.org/docs/9.4/static/app-psql.html) is similar to the [sqlite3](https://www.sqlite.org/cli.html) command line tool in that it allows you to connect to and manage databases. [psql](http://www.postgresql.org/docs/9.4/static/app-psql.html) connects to a running PostgreSQL server process, then enables you to:

- Run queries.
- Manage users and permissions.
- Manage databases.
- See PostgreSQL system information.

By default, psql will connect to a PostgreSQL server running on the current computer, using port `5432`. If you don't specify a user and database to connect to, it will use the defaults. By default, the name of the currently logged in system user will be used as both the PostgreSQL user name and database name.

If you're logged in to a computer as the system user `dq`, then type `psql`, you will connect to the `dq` database as a PostgreSQL user called `dq`. We'll learn later on how to connect to different databases using different PostgreSQL users.

After you're finished working with [psql](http://www.postgresql.org/docs/9.4/static/app-psql.html), you can exit using the `\q` command.

### Instructions

- Start the PostgreSQL command line tool by typing `psql`.
- Exit psql by typing `\q`.

```
~$ psql <<EOF
~$ EOF
```

## 2: Running SQL Queries

After starting the psql command line tool, we can run SQL queries. Any valid SQL query will be executed. Because the psql shell is about giving instant feedback, transactions don't apply, and each command we type is immediately executed. This allows us to quickly test out queries and get results.

Since creating a database is one SQL query, we can do it via psql. You may recall that the syntax to create a database is like the following:

```
CREATE DATABASE dbName;
```

Queries in psql must end with a semicolon (`;`), or they won't be performed.

### Instructions

- Start the psql command line tool.
- Create a database called `bank_accounts`
- Exit the psql command line tool.

```
~$ psql <<EOF
~$ CREATE DATABASE bank_accounts;
~$ EOF
```

## 3: Special PostgreSQL Commands

We can run several special commands using psql. These commands start with a backslash (`\`), and can perform a variety of functions, including:

- Listing databases
- Listing tables
- Managing users

You can see a full list of all of the special functions by running `\?` after starting psql. You'll need to type `q` to exit the resulting help interface. You can also find the full list [here](http://www.postgresql.org/docs/9.4/static/app-psql.html).

Two common functions to run are:

- `\l` -- list all available databases.
- `\dt` -- list all tables in the current database.
- `\du` -- list users.

### Instructions

- Start psql.
- List all available databases.
- Exit psql.

```
~$ psql <<EOF
~$ \l
~$ EOF
```

## 4: Switching Databases

When we're connected to a specific SQL database, we can only create tables within that database, and run queries on tables in that database. In the past few screens, we've been connected to the `dq` database. This prevents us from manipulating tables in the `bank_accounts` database.

You can connect to a different database using the `-d` option of psql. If you wanted to connect to a database called `dataquest`, you could use the following command:

```

```

```
psql -d dataquest
```

psql will start connected to the specified database, and you'll be able to create tables in the database.

### Instructions

- Start psql and connect to the `bank_accounts` database.
- Create a table called `deposits` in `bank_accounts` with the following columns:`id`, integer, primary key`name`, text`amount`, float
- Use the `\dt` command to list all of the tables in `bank_accounts`.
- Exit psql.

```
~$ psql -d bank_accounts <<EOF
~$ CREATE TABLE deposits (
~$     id integer PRIMARY KEY,
~$     name text,
~$     amount float
~$ );
~$ EOF
```

## 5: Creating Users

In order to manage access to different databases, you can also create users. Users will be able to log into a PostgreSQL database and run queries. You can create a user with the [CREATE ROLE](http://www.postgresql.org/docs/9.4/static/sql-createrole.html) statement. Here's how the statement looks:

```

```

```
CREATE ROLE userName;
```

By default, the user isn't allowed to login to PostgreSQL and run queries. You can fix this by adding the `WITH` and `LOGIN`statements:

```

```

```
CREATE ROLE userName WITH LOGIN;
```

If you run the pseudo-code above with a real username, you may be unable to login as that user. Depending on the configuration of your PostgreSQL instance, you may either be unable to login entirely, or will only be able to login when your system user name is the same as the PostgreSQL user name you want to login as. You can get around this by creating a password -- you'll then be able to login using the password. We'll cover PostgreSQL authentication and login methods in more depth in a later mission.

You can create a password using the `WITH PASSWORD` statement like this:

```

```

```
CREATE ROLE userName WITH LOGIN PASSWORD `password`;
```

If the user needs to be able to create databases, you can add that ability in with the `CREATEDB` statement:

```

```

```
CREATE ROLE userName WITH CREATEDB LOGIN PASSWORD 'password';
```

As you may be able to tell from above, we can keep modifying how the user is created by adding statements after the `WITH` statement. Some other statements we can add are:

- `CREATEROLE` -- allows the user to create other users.
- `SUPERUSER` -- makes the user a superuser. We'll cover what a superuser is later on.

For a full list of statements that can be added, see [here](http://www.postgresql.org/docs/9.4/static/sql-createrole.html).

### Instructions

- Start psql.
- Create a user called `sec` with the following modifying statements:`LOGIN``PASSWORD 'test'``CREATEDB`
- List all the users using `\du`.
- Exit psql.

```
~$ psql <<EOF
~$ CREATE ROLE sec WITH LOGIN CREATEDB PASSWORD 'test';
~$ EOF
```

## 6: Adding Permissions

When users are created, they don't have any ability, or permissions, to access tables in existing databases. This is done for security reasons, so that all permissions are issued explicitly instead of being unexpected. You can issue permissions to a user using the [GRANT](http://www.postgresql.org/docs/9.4/static/sql-grant.html) statement. The [GRANT](http://www.postgresql.org/docs/9.4/static/sql-grant.html) statement will issue permissions to access certain tables in a database to a certain user. You can allow a user to perform `SELECT` queries on a given table like this:

```

```

```
GRANT SELECT ON tableName TO userName;
```

If you want to grant different types of permissions, you can separate them with commas. The below query will allow a given user to query data from a table, update rows in the table, insert rows into the table, and delete rows from the table:

```

```

```
GRANT SELECT, INSERT, UPDATE, DELETE ON tableName TO userName;
```

A shortcut for this is to use the `ALL PRIVILEGES` statement:

```

```

```
GRANT ALL PRIVILEGES ON tableName TO userName;
```

You can use the psql `\dp` command to find out what privileges have been granted to users for a specific table:

```

```

```
\dp tableName
```

### Instructions

- Start psql and connect to the `bank_accounts` database.
- Grant all privileges on the table `deposits` to the user `sec`.
- List all the privileges for `deposits` using `\dp`.
- Exit psql.

```
~$ psql -d bank_accounts <<EOF
~$ GRANT ALL PRIVILEGES ON deposits TO sec;
~$ EOF
```

## 7: Removing Permissions

There are times when you'll want to remove permissions that you granted to a user previously. Permissions can be removed using the [REVOKE](http://www.postgresql.org/docs/9.4/static/sql-revoke.html) statement. The [REVOKE](http://www.postgresql.org/docs/9.4/static/sql-revoke.html) statement enables you to take back any permissions given via the `GRANT` statement. You can revoke the ability for a user to run queries:

```

```

```
REVOKE SELECT ON tableName FROM userName;
```

If you want to revoke different types of permissions, you can separate them with commas. The below query will revoke permissions for a given user to query data from a table, update rows in the table, insert rows into the table, and delete rows from the table:

```

```

```
REVOKE SELECT, INSERT, UPDATE, DELETE ON tableName FROM userName;
```

A shortcut for this is to use the `ALL PRIVILEGES` statement:

```

```

```
REVOKE ALL PRIVILEGES ON tableName FROM userName;
```

The above syntax likely looks very similar to the `GRANT` syntax from the last screen. This is by design, and both are as similar as possible to make adding and removing permissions straightforward.

### Instructions

- Start psql and connect to the `bank_accounts` database.
- Revoke all privileges on the table `deposits` from the user `sec`.
- List all the privileges for `deposits` using `\dp`.
- Exit psql.

```
~$ psql -d bank_accounts <<EOF
~$ REVOKE ALL PRIVILEGES ON deposits FROM sec;
~$ EOF
```

## 8: Superusers

A superuser is a special type of user that overrides all access restrictions. Superusers can perform any function in a database, and a user should only be made a superuser in special cases. Adding the `SUPERUSER` statement to a `CREATE ROLE`statement will make a user a superuser:

```

```

```
CREATE ROLE userName WITH SUPERUSER;
```

You can also setup login and a password for the superuser:

```

```

```
CREATE ROLE userName WITH LOGIN PASSWORD 'password' SUPERUSER;
```

### Instructions

- Start psql.
- Create a user called `aig` with the following modifying statements:`LOGIN``PASSWORD 'test'``SUPERUSER`
- List all the users using `\du`.
- Exit psql.

```
~$ psql <<EOF
~$ CREATE ROLE aig WITH LOGIN PASSWORD 'test' SUPERUSER;
~$ EOF
```

# Project: PostgreSQL Installation

## 1: Introduction

So far, we've explored many database concepts using SQLite and PostgreSQL. In this project, we'll walk through how to install the PostgreSQL database system and the psycopg2 Python library for Windows, Mac, and Linux. We'll be focusing on installing and running PostgreSQL locally on your own machine instead of on a remote server.

## 2: Installing PostgreSQL

First things first, let's install PostgreSQL. During the setup process, you'll be asked to specify a default username and password. Select a username and password combination you'll remember since you're only installing PostgreSQL locally and don't need a highly secure combination.

Also during installation, you may be asked to specify a port number. Even though PostgreSQL will be running on the same machine, other applications communciate with it through the port as if it were on a remote machine. Port number **5432** is the default for PostgreSQL.

Here are the installation instructions for each operating system:

Mac:

- Download Postgres.app [here](http://postgresapp.com/), move to the **Applications** folder, and double click to launch. This applications runs in the background and you'll need it to be running to connect to it from Python. By default, PostgreSQL will run on port **5432**.
- Add the following line to the end of `~/.bash_profile`:`export PATH=$PATH:/Applications/Postgres.app/Contents/Versions/latest/bin`

Windows:

- Download the latest Windows installer [here](http://www.bigsql.org/postgresql/installers.jsp), double click the installer, and go through the installation wizard.
- When asked for a port number, use **5432**.

Linux:

- Read the installation directions for your specific flavor of Linux from the [official documentation](https://www.postgresql.org/download/linux/).
- If asked for a port number, use **5432**.

To test your installation, open your command line application, type `psql`, and you should be in the PostgreSQL shell.

## 3: Psycopg2

Now let's install the psycopg2 Python library:

Mac & Linux:

- Install using Anaconda:
  - `conda install psycopg2`.

Windows:

- Run the code from [here](https://github.com/nwcell/psycopg2-windows#installation-scripts) for the corresponding version of Python on your machine. Run `python -version` to return the version of Python if you're unsure.

## 4: Connecting To PostgreSQL From Psycopg2

Launch your Python shell and import the psycopg2 library. Then, run the following code to connect to PostgreSQL and test that everything works as expected:

```

```

```
import psycopg2
```

```
conn = psycopg2.connect(dbname="postgres", user="postgres")
```

```
cursor = conn.cursor()
```

```
cursor.execute("CREATE TABLE notes(id integer PRIMARY KEY, body text, title text)")
conn.close()
```

If no errors were returned, then you're setup is good to go! If you run into any issues, use Google, StackOverflow, and the Dataquest member-only Slack community to get help.

