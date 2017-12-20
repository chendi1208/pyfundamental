# Logical Operators and Ordering

## 1: Introduction To Logical Operators

In the previous mission, we covered the basics of databases, SQL, and the `SELECT` statement. In this mission, we'll explore how to express more complex filtering criteria. We'll continue to work with the data set, [`recent-grads.csv`](https://github.com/fivethirtyeight/data/tree/master/college-majors), which we've loaded into the table `recent_grads`. Here's a preview:

| Rank | Major_code | Major                                    | Major_category | Total | Sample_size | Men   | Women | ShareWomen | Employed |
| ---- | ---------- | ---------------------------------------- | -------------- | ----- | ----------- | ----- | ----- | ---------- | -------- |
| 1    | 2419       | PETROLEUM ENGINEERING                    | Engineering    | 2339  | 36          | 2057  | 282   | 0.120564   | 1976     |
| 2    | 2416       | MINING AND MINERAL ENGINEERING           | Engineering    | 756   | 7           | 679   | 77    | 0.101852   | 640      |
| 3    | 2415       | METALLURGICAL ENGINEERING                | Engineering    | 856   | 3           | 725   | 131   | 0.153037   | 648      |
| 4    | 2417       | NAVAL ARCHITECTURE AND MARINE ENGINEERING | Engineering    | 1258  | 16          | 1123  | 135   | 0.107313   | 758      |
| 5    | 2405       | CHEMICAL ENGINEERING                     | Engineering    | 32260 | 289         | 21239 | 11021 | 0.341631   | 25694    |

We learned about SQL's comparison operators in the last lesson:

- Less than: `<`
- Less than or equal to: `<=`
- Greater than: `>`
- Greater than or equal to: `>=`
- Equal to: `=`
- Not equal to: `!=`

These were useful for expressing our filtering criteria, or condition, in the `WHERE` statement. But what if we want to use multiple filtering criteria to specify the data we want from the database?

**Logical operators** are keywords we can use to combine filtering criteria and express more specific conditions. Here are the two basic logical operators we use most often:

- `OR` (returns either Condition1 or Condition2)
- `AND` (returns both Condition1 and Condition2)

## 2: Returning Multiple Conditions With AND

The following psuedo-code will help you conceptualize how we use the `AND`statement with a `WHERE` statement:



```
SELECT [column1, column2,...] FROM [table1]

WHERE [condition1] AND [condition2]
```

Now we can write a SQL query that returns all of the female-majority majors with more than `10000` employed graduates.

Let's see what this query looks like:



```
SELECT Major,ShareWomen,Employed FROM recent_grads 

WHERE ShareWomen>0.5 AND Employed>10000;
```

We want the database to return all of the rows where both conditions are true:

1. `ShareWomen > 0.5`
2. `Employed > 10000`

### Instructions

- Run the query above, which returns all of the female-majority majors with more than `10000` employed graduates.
- Use the `LIMIT` statement to return just the first 10 results.

```

SELECT Major,ShareWomen,Employed FROM recent_grads WHERE ShareWomen>0.5 AND Employed>10000 LIMIT 10;
```

## 3: Returning One Of Several Conditions With OR

We used the `AND` operator to specify that our filter needs to pass two Boolean conditions. Both of the conditions had to evaluate to `True` for the record to appear in the result set. If we wanted to specify a filter that meets **either** of the conditions instead, we would use the `OR` operator.



```
SELECT [column1, column2,...] FROM [table1]

WHERE [condition1] OR [condition2]
```

We'll dive straight into a practice problem because we use the `OR` and `AND`operators in similar ways.

### Instructions

Write a SQL query that returns **the first 20** majors that **either**:

- have a `Median` salary greater than or equal to `10,000`, **or**
- have less than or equal to `1,000` `Unemployed` people

We only want to include the following columns in the results, and in this order:

- `Major`
- `Median`
- `Unemployed`

```

SELECT Major,Median,Unemployed FROM recent_grads WHERE Median >= 10000 OR Unemployed <= 1000 LIMIT 20;
```

## 4: Grouping Operators With Parentheses

There's a certain class of questions that we can't answer using only the techniques we've learned so far. For example, if we wanted to write a query that returned all `Engineering` majors that **either** had mostly female graduates **or** an unemployment rate below 5.1%, we would need to use parentheses to express this more complex logic.

The three raw conditions we'll need are:



```
Major_category = 'Engineering'

ShareWomen >= 0.5

Unemployment_rate < 0.051
```

What the SQL query looks like using parantheses:



```
select Major, Major_category, ShareWomen, Unemployment_rate

from recent_grads

where (Major_category = 'Engineering') and (ShareWomen > 0.5 or Unemployment_rate < 0.051);
```

The first thing you may notice is that we didn't capitalize any of the operators or statements in the query. SQL's built-in keywords are case-insensitive, which means we don't have to capitalize operators like `AND` or statements like `SELECT`.

The second thing you may notice is how we enclosed the logic we wanted to be evaluated together in parentheses. This is very similar to how we group mathematical calculations together in a particular order. The parentheses makes it explictly clear to the database that we want all of the rows where both of the expressions in the statements evaluate to `True`:



```
(Major_category = 'Engineering' and ShareWomen > 0.5) -> True or False

(ShareWomen > 0.5 or Unemployment_rate < 0.051) -> True or False
```

If we had written the `where` statement without any parentheses, the database would guess what our intentions are, and actually execute the following query instead:



```
where (Major_category = 'Engineering' and ShareWomen > 0.5) or (Unemployment_rate < 0.051)
```

Leaving the parentheses out implies that we want the calculation to happen from left to right in the order in which the logic is written, and wouldn't return us the data we want. Now let's run our intended query and see the results!

### Instructions

- Run the query we explored above, which returns all `Engineering` majors that:**either** had mostly women graduates**or** had an unemployment rate below **5.1%**, which was the rate in August 2015
- We're interested in returning the `Major`, `Major_category`, `ShareWomen`, and `Unemployment_rate` columns.

```

select Major, Major_category, ShareWomen, Unemployment_rate
from recent_grads
where (Major_category = 'Engineering') and (ShareWomen > 0.5 or Unemployment_rate < 0.051);
```

## 5: Practice Grouping Operators

In this step, you'll practice grouping operators to express more complex logic.

### Instructions

Find all majors that meet all of the following criteria:

- `Major_category` of `Business` or `Arts` or `Health`
- `Employed` students greater than 20,000 or `Unemployment_rate` below 5.1%

We're only interested in the following columns (in the following order):

- `Major`
- `Major_category`
- `Employed`
- `Unemployment_rate`

Return all of the results (don't apply a limit).

```

select Major, Major_category, Employed, Unemployment_rate
from recent_grads
where (Major_category = 'Business' or Major_category = 'Arts' or Major_category = 'Health') 
and (Employed > 20000 or Unemployment_rate < 0.051);
```

## 6: Order Results With ORDER BY

The database has been ordering all of our results by the `Rank` column, because that's how the original data set ordered the data. This may not make sense for all queries, though. SQL comes with an `ORDER BY` statement that allows us to specify how we want the database to order our results. To use the `ORDER BY`statement, we need to specify the column we want to order the results by, and whether we want to order them in ascending (low to high) or descending order.



```
SELECT [column1, column2,...] FROM [table1]

WHERE [conditions]..

ORDER BY column1 [ASC or DESC]
```

We use `ASC` to order from low to high, and `DESC` to order from high to low. SQL uses the standard methods of ordering -- alphabetically for text fields and numerically for numeric fields. This means that if we order by a text field in descending order, the results will be in *reverse alphabetical* order.

The following code selects the `Employed`column, orders it in ascending order (low to high), and limits the results to the first 10:



```
select Employed

from recent_grads

order by Employed asc

limit 10;
```

This query returns the lowest 10 values in the `Employed` column. First, it puts the values in `Employed` in ascending order, then returns the first 10 values under the new ordering.

### Instructions

- Return the first 10 values in the `Major` column in reverse alphabetical order.

```

select Major
from recent_grads
order by Major desc
limit 10;
```

## 7: Order Results Based On Multiple Columns

SQL also allows us to specify multiple columns in the `ORDER BY` statement. If multiple rows have the same values in one column, for example, we can order by that column first, then by a different column. You may have done something similar with a Microsoft Excel spreadsheet.

Here's what the psuedocode for this looks like:



```
select [column1, column2..]

from table_name

order by column1 (asc or desc), column2 (asc or desc)
```

Ordering by multiple columns is especially useful when working with people's names, because databases often have separate columns for first and last names. We can specify that we want to order or alphabetize query results by `Last Name`and `First Name`. After alphabetizing all last names, the database will alphabetize all rows that have the same values for `Last Name` by `First Name`.

| Last Name | First Name |
| --------- | ---------- |
| Khan      | Sal        |
| Khan      | Tony       |
| Prescot   | Pete       |
| Prescot   | Russ       |

Now it's your turn!

### Instructions

Write a query that orders the majors by `Major` in ascending order, then by `Median` salary in descending order. We're interested in selecting only these columns, in the following order:

- `Major_category`
- `Median`
- `Major`

Limit the query to just **the first 20** results.

```

select Major_category, Median, Major
from recent_grads
order by Major asc, Median desc
limit 20;
```

## 8: Next Steps

This lesson gave you some practice with writing and running SQL queries. The next mission in this course is a challenge that will give you an opportunity to apply what you've learned so far.

# Challenge: Practice Expressing Complex SQL Queries

## 1: Introduction

In the last two missions, we covered the basics of SQL, and explored how to use it to retrieve relevant rows from a database table. In this challenge, you'll practice writing your own SQL queries from scratch.

We'll continue to work with the American Community Survey data on college majors and job outcomes. Here's a preview of [`recent-grads.csv`](https://github.com/fivethirtyeight/data/tree/master/college-majors), the data set we'll be working with:

| Rank | Major_code | Major                                    | Major_category | Total | Sample_size | Men   | Women | ShareWomen | Employed |
| ---- | ---------- | ---------------------------------------- | -------------- | ----- | ----------- | ----- | ----- | ---------- | -------- |
| 1    | 2419       | PETROLEUM ENGINEERING                    | Engineering    | 2339  | 36          | 2057  | 282   | 0.120564   | 1976     |
| 2    | 2416       | MINING AND MINERAL ENGINEERING           | Engineering    | 756   | 7           | 679   | 77    | 0.101852   | 640      |
| 3    | 2415       | METALLURGICAL ENGINEERING                | Engineering    | 856   | 3           | 725   | 131   | 0.153037   | 648      |
| 4    | 2417       | NAVAL ARCHITECTURE AND MARINE ENGINEERING | Engineering    | 1258  | 16          | 1123  | 135   | 0.107313   | 758      |
| 5    | 2405       | CHEMICAL ENGINEERING                     | Engineering    | 32260 | 289         | 21239 | 11021 | 0.341631   | 25694    |

We've loaded a subset of the data into a table named `recent_grads` in a database. The subset contains the 2010-2012 data for recent college grads only. The full table has many more columns (21 to be specific) than the ones displayed above. You can find more details on them at [FiveThirtyEight's Github repo](https://github.com/fivethirtyeight/data/tree/master/college-majors).

Here are the descriptions for the columns in the preview:

- `Rank` - The major's rank by median earnings
- `Major_code` - The major's code or ID
- `Major` - The name of the major
- `Major_category` - The broader category the major belongs to
- `Total` - The total number of people who studied the major
- `Sample_size` - The sample size (unweighted) of graduates with full time jobs
- `Men` - The number of male graduates
- `Women` - The number of female graduates
- `ShareWomen` - Women as a proportion of the total number of graduates (a number ranging from `0` to `1`)
- `Employed` - The number of employed graduates

## 2: Use SELECT And LIMIT To Filter Results

In this step, you'll practice using the `SELECT` and `LIMIT` statements.

### Instructions

Write a query that retrieves the first `20` rows in the table, with only the following columns (in the same order):

- `College_jobs`
- `Median`
- `Unemployment_rate`

```

select College_jobs, Median, Unemployment_rate from recent_grads limit 20;
```

## 3: Use WHERE To Filter Results

In this step, you'll practice using the `WHERE` SQL statement to express row filtering criteria.

### Instructions

- Write a query that returns the first five `Arts` majors. Only include the `Major` column.

```

select major from recent_grads where Major_category='Arts' limit 5;
```

## 4: Express Criteria With Operators

In this step, you'll practice using SQL's logical operators to express complex criteria.

### Instructions

Return all non-engineering majors:

- With a median salary less than or equal to 50,000
- **Or** an unemployment rate higher than 6.5%

Return only these columns (in the same order):

- `Major`
- `Total`
- `Median`
- `Unemployment_rate`

```

select Major,Total,Median,Unemployment_rate from recent_grads 
where (Major_category != 'Engineering') and (Unemployment_rate > 0.065 or Median <= 50000);
```

## 5: Order Your Results

In this final step, you'll practice using the `ORDER BY` statement to customize the ordering of a query's results.

### Instructions

Return the first 20 non-engineering majors in reverse alphabetical order.

- We're only interested in returning the major names in the results.

```

select major
from recent_grads
where Major_category!='Engineering'
order by major desc
limit 20;
```

## 6: Next Steps

In the next mission, we'll walk through how to query a SQLite database from Python. Most companies use a SQL database of some kind to store their data. Learning how to interface with SQL databases from Python will allow you to incorporate more sources into your data science workflow.

# Querying SQLite from Python

## 1: Overview

In past missions, we focused on exploring the SQL syntax for retrieving data from a database. In this mission, we'll explore how to interact with a SQLite database in Python so you can start to incorporate databases into your data science workflow.

SQLite is a database that doesn't require a standalone server; it stores the entire database as a file on disk. This makes it ideal for working with larger data sets that can fit on disk but not in memory.

The pandas library loads the entire data set we're working with into memory, making SQLite a compelling alternative for working with data sets larger than 8 gigabytes (which is roughly the amount of memory modern computers contain). The fact that we can contain an entire database in a single file makes them easy to share; some data sets are available online as SQLite database files (using the extension `.db`).

We can interact with a SQLite database in two main ways:

- Through the SQLite Python module
- Through the SQLite shell

In this mission, we'll focus on learning how to use the [SQLite Python module](https://docs.python.org/3/library/sqlite3.html) to interact with the database. We'll work with the SQLite shell in the guided project that comes after this mission.

## 2: Introduction To The Data

We'll continue to work with the American Community Survey data on college majors and job outcomes, which looks like this:

| Rank | Major_code | Major                                    | Major_category | Total | Sample_size | Men   | Women | ShareWomen | Employed |
| ---- | ---------- | ---------------------------------------- | -------------- | ----- | ----------- | ----- | ----- | ---------- | -------- |
| 1    | 2419       | PETROLEUM ENGINEERING                    | Engineering    | 2339  | 36          | 2057  | 282   | 0.120564   | 1976     |
| 2    | 2416       | MINING AND MINERAL ENGINEERING           | Engineering    | 756   | 7           | 679   | 77    | 0.101852   | 640      |
| 3    | 2415       | METALLURGICAL ENGINEERING                | Engineering    | 856   | 3           | 725   | 131   | 0.153037   | 648      |
| 4    | 2417       | NAVAL ARCHITECTURE AND MARINE ENGINEERING | Engineering    | 1258  | 16          | 1123  | 135   | 0.107313   | 758      |
| 5    | 2405       | CHEMICAL ENGINEERING                     | Engineering    | 32260 | 289         | 21239 | 11021 | 0.341631   | 25694    |

The full table has many more columns than the ones we've displayed above (21 to be specific). You can learn about all of them in [FiveThirtyEight's GitHub repository](https://github.com/fivethirtyeight/data/tree/master/college-majors).

Here are the descriptions for the columns in the preview:

- `Rank` - The major's rank by median earnings
- `Major_code` - The major's code or ID
- `Major` - The name of the major
- `Major_category` - The broader category the major belongs to
- `Total` - The total number of people who studied the major
- `Sample_size` - The sample size (unweighted) of graduates with full time jobs
- `Men` - The number of male graduates
- `Women` - The number of female graduates
- `ShareWomen` - Women as a proportion of the total number of graduates (a number ranging from `0` to `1`)
- `Employed` - The number of employed graduates

We've loaded a subset of the data into a table named `recent_grads` in a database. The subset contains the 2010-2012 data for recent college grads only. The database file we'll be working with is called `jobs.db`.

## 3: Connecting To The Database

Python 2.5 and up come with the SQLite module, which means we don't need to install any separate libraries to get started. Specifically, we'll be working with the SQLite3 Python module, which was developed to work with [SQLite version 3](https://www.sqlite.org/version3.html).

We can import it into our environment using this command:



```
import SQLite3
```

Once we've imported the module, we connect to the database we want to query using the [`connect()` function](https://docs.python.org/3/library/sqlite3.html#sqlite3.connect). This function requires a single parameter, which is the database we want to connect to. Because the database we're working with exists as a file on disk, we need to pass in the file name.

The `connect()` function returns a [Connection instance](https://docs.python.org/3/library/sqlite3.html#sqlite3.Connection), which maintains the connection to the database we want to work with. When we're connected to a database, SQLite locks the database file and prevents any other processes from connecting to the database simultaneously. The SQLite team made this design decision to keep the database lightweight, and avoid the complexity that arises when multiple processes interact with the same database.

### Instructions

- Import the SQLite3 library into the environment.
- Then, use the SQLite3 `connect()` function to connect to `jobs.db`, and assign the Connection instance it returns to `conn`.

```

import sqlite3
conn = sqlite3.connect("jobs.db")
```

## 4: Introduction To Cursor Objects And Tuples

Before we can execute a query, we need to express our SQL query as a *string*. While we use the Connection class to represent the database we're working with, we use the [Cursor class](https://docs.python.org/3/library/sqlite3.html#cursor-objects) to:

- Run a query against the database
- Parse the results from the database
- Convert the results to native Python objects
- Store the results within the Cursor instance as a local variable

After running a query and converting the results to a list of **tuples**, the Cursor instance stores the list as a local variable. Before diving into the syntax of querying the database, let's learn more about *tuples*.

## 5: Working With Sequences Of Values As Tuples

A [*tuple*](https://docs.python.org/3/tutorial/datastructures.html#tuples-and-sequences) is a core data structure that Python uses to represent a sequence of values, similar to a *list*. Unlike *lists*, *tuples* are **immutable**, which means we can't modify existing ones. Python represents each row in the results set as a *tuple*.

To create an empty *tuple*, assign a pair of empty parentheses to a variable:



```
t = ()
```

Python indexes *Tuples* from `0` to `n-1`, just like it does with *lists*. We access the values in a tuple using bracket notation.



```
t = ('Apple', 'Banana')

apple = t[0] 

banana = t[1]
```

*Tuples* are faster than *lists*, so they're helpful with larger databases and larger results sets.

Next, let's dive into how to use the Cursor instance to query the database.

## 6: Creating A Cursor And Running A Query

We need to use the Connection instance method [`cursor()`](https://docs.python.org/3/library/sqlite3.html#sqlite3.Connection.cursor) to return a Cursor instance corresponding to the database we want to query.



```
cursor = conn.cursor()
```

In the following code block, we:

- Write a basic `select` query that will return all of the values from the `recent_grads` table, and store this query as a string named `query`
- Use the Cursor method `execute()` to run the query against our database
- Return the full results set and store it as `results`
- Print the first three tuples in the list `results`



```
# SQL Query as a string

query = "select * from recent_grads;"

# Execute the query, convert the results to tuples, and store as a local variable

cursor.execute(query)

# Fetch the full results set as a list of tuples

results = cursor.fetchall()

# Display the first three results

print(results[0:3])
```

Now it's your turn!

### Instructions

- Write a query that returns all of the values in the `Major`column from the `recent_grads` table.
- Store the full results set (a list of tuples) in `majors`.
- Then, print the first three tuples in `majors`.

```
import sqlite3
conn = sqlite3.connect("jobs.db")
cursor = conn.cursor()

query = "select * from recent_grads;"
cursor.execute(query)
results = cursor.fetchall()
print(results[0:2])
query = "select major from recent_grads;"
majors = cursor.execute(query).fetchall()
print(majors[0:3])
```

## 7: Execute As A Shortcut For Running A Query

So far, we've been running queries by creating a Cursor instance, and then calling the `execute` method on the instance. The SQLite library actually allows us to skip creating a Cursor altogether by using the [`execute`method](https://docs.python.org/3/library/sqlite3.html#sqlite3.Connection.execute) within the Connection object itself. SQLite will create a Cursor instance for us under the hood and our query run against the database, but this shortcut allows us to skip a step. Here's what the code looks like:



```
conn = sqlite3.connect("jobs.db")

query = "select * from recent_grads;"

conn.execute(query).fetchall()
```

Notice that we didn't explicitly create a separate Cursor instance ourselves in this code example.

Now let's learn how to fetch a specific number of results after we run a query.

## 8: Fetching A Specific Number Of Results

To make it easier to work with large results sets, the Cursor class allows us to control the number of results we want to retrieve at any given time. To return a single result (as a *tuple*), we use the [Cursor method `fetchone()`](https://docs.python.org/3/library/sqlite3.html#sqlite3.Cursor.fetchone). To return `n`results, we use the [Cursor method `fetchmany()`](https://docs.python.org/3/library/sqlite3.html#sqlite3.Cursor.fetchmany).

Each Cursor instance contains an internal counter that updates every time we retrieve results. When we call the `fetchone()` method, the Cursor instance will return a single result, and then increment its internal counter by 1. This means that if we call `fetchone()` again, the Cursor instance will actually return the second tuple in the results set (and increment by 1 again).

The `fetchmany()` method takes in an integer (`n`) and returns the corresponding results, starting from the current position. It then increments the Cursor instance's counter by `n`. In the following code, we return the first two results using the `fetchone()` method, then the next five results using the `fetchmany()` method.



```
first_result = cursor.fetchone()

second_result = cursor.fetchone()

next_five_results = cursor.fetchmany(5)
```

### Instructions

- Write and run a query that returns the `Major` and `Major_category` columns from `recent_grads`.
- Then, fetch the first five results and store them as `five_results`.

```
import sqlite3
conn = sqlite3.connect("jobs.db")
cursor = conn.cursor()
query = "select Major,Major_category from recent_grads;"
cursor.execute(query)
five_results = cursor.fetchmany(5)
```

## 9: Closing The Database Connection

Because SQLite restricts access to the database file when we're connected to a database, we need to close the connection when we're done working with it. Closing the connection allows other processes to access the database, which is important when you're in a production environment and working with other team members. Also, if we made any changes to the database, SQLite will automatically save them during this step.

To close a connection to a database, use the Connection instance method [`close()`](https://docs.python.org/3/library/sqlite3.html#sqlite3.Connection.close). When we're working with multiple databases and multiple Connection instances, we want to make sure we call the `close()` method on the correct instance. After closing the connection, attempting to query the database using any linked Cursor instances will return the following error:



```
ProgrammingError: Cannot operate on a closed database.
```

### Instructions

- Close the connection to the database using the Connection instance method `close()`.

```
conn = sqlite3.connect("jobs.db")
conn.close()
```

## 10: Practice

Now let's practice the entire workflow we've learned so far, from start to finish.

### Instructions

- Connect to the database `jobs2.db`, which contains the same data as `jobs.db`.
- Write and execute a query that returns all of the majors (`Major`) in reverse alphabetical order (Z to A).
- Assign the full result set to `reverse_alphabetical`.
- Finally, close the connection to the database.

```

conn = sqlite3.connect("jobs2.db")
query = "select Major from recent_grads order by Major desc;"
reverse_alphabetical = conn.cursor().execute(query).fetchall()
conn.close()
```

## 11: Next Steps

In this mission, we learned how to query a SQLite database from the Python module (the IPython shell). Next up, we'll walk through how to use the SQLite shell in a guided project. The two shells are similar; we can write and run commands interactively in both of them.

# Guided Project: Working With a SQLite Database

## 1: Overview Of The Data Set

In this project, we'll continue working with the [CIA World Factbook](https://www.cia.gov/library/publications/the-world-factbook/), a compendium of facts about countries. The Factbook contains demographic information for each country in the world, including:

- `population` - The population as of `2015`
- `population_growth` - The annual population growth rate, as a percentage
- `area` - The total land and water area

You can download the Factbook as a SQLite database [from GitHub](https://github.com/factbook/factbook.sql) if you want to work with it on your own computer. In this guided project, we'll be working with Python and the SQLite command line tool (SQLite Command Shell) to connect to the database, extract data, and perform analysis on the data.

### Instructions

For now, just hit "Next" to get started with the project!

## 2: Overview Of The SQLite Command Shell

SQLite is a relational database management system that enables us to create databases and query them using SQL syntax. SQLite is simpler than full database systems like MySQL and PostgreSQL. It's good for cases where ease of use is more important than performance. Each SQLite database is stored as a single file, making it easy to transport.

The Factbook database is in the file `factbook.db`. The`db` at the end is a file extension that's short for *database*.

We can open the Factbook database in the SQLite Command Shell by navigating to the same folder as `factbook.db`, then typing `sqlite3 factbook.db` on the command line. This enables us to manage the database and run SQL queries.

Try it out for yourself!

### Instructions

Use the SQLite Command Shell to explore `factbook.db`.

- Connect to `factbook.db` using the SQLite Command Shell.

- Type `.help` into the shell to see a list of commands you can run in the shell.

- Type `.tables` to see a list of the tables in the database.

- If you type `.header on`, you'll see the column headers when you run queries.

  If you type an incomplete command (one that's missing the ending semicolon, for example), SQLite won't execute it. Instead, it will take you to an indented line. Type the semicolon (`;`) in the indented line (and press **Enter**) to exit the indentation and run the command. Here's an example:

![Imgur](https://dq-content.s3.amazonaws.com/nPRG1kN.png)

When you're done with the SQLite Command Shell, type `.quit`. Don't quit the shell just yet, though, because you'll be using it in the next step.

## 3: Running Queries In The SQLite Command Shell

The SQLite Command Shell also allows us to run any valid SQL query. For example, we could run the following:



```
SELECT * FROM facts;
```

This will show us all of the rows in the `facts` table.

### Instructions

Run some queries in the SQLite Command Shell. Be sure to enter `.header on` to see the headers for each column.

While you should think of your own queries, here are a couple of examples:

- SELECT * FROM facts ORDER BY population DESC LIMIT 10;
- SELECT * FROM facts ORDER BY area_land ASC LIMIT 10;

You may notice that these queries return some strange results, such as `Ethiopia` having the least land area. The queries also include non-national entities like the `European Union` and `Akrotiri`.

The data is fairly messy, and some values in the `area_land`column are missing. Add `WHERE area_land != ""` to the query before the `ORDER BY` clause to remove the invalid rows. You may also need to try additional filtering.

When you're done exploring, you can quit the SQLite Command Shell with `.quit`.

## 4: Using Python With SQLite

The `sqlite3` [library](https://docs.python.org/3/library/sqlite3.html), which comes with Python by default, allows us to connect to SQLite databases. To do this, we open a database connection, then create an object that can run queries.

For example, this code will let us connect to `factbook.db` and select all of the rows:



```
import sqlite3

conn = sqlite3.connect('factbook.db')



c = conn.cursor()

c.execute('SELECT * FROM facts;')



print(c.fetchall())
```

The code above creates a [Connection](https://docs.python.org/3/library/sqlite3.html#connection-objects) object. We then create a [Cursor](https://docs.python.org/3/library/sqlite3.html#cursor-objects) instance, which can execute queries. Finally, we execute a query and display the results using the `print` function. To learn more about the `sqlite3` library, read the package documentation [on the official Python website](https://docs.python.org/3/library/sqlite3.html).

### Instructions

- Write code in `query.py` that selects the `10` least populated countries from the `facts` table, and then prints the results.
- Execute `query.py` from the command line by running `python query.py`.

## 5: Computing Population Projections

You can read the results of a SQL query into a pandas dataframe using the `read_sql_query` function, which [the official pandas website](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_sql_query.html) documents. The `read_sql_query` function takes a SQL query string and a connection object, and returns a dataframe containing all of the rows and columns from the query.

When pandas reads in columns this way, it automatically uses the column types from the original the database. Blank entries in the database (like the ones in the `area_land` column) will have `NaN` values in a dataframe, which means "Not a Number." This is because pandas can't have blanks in numeric columns; it uses `NaN` to signify invalid or missing values instead.

You can learn more about how to work with missing data in the [pandas documentation](http://pandas.pydata.org/pandas-docs/stable/missing_data.html). For now, we'll just use the `dropna` method with the `axis=0`argument, which will drop any rows that have `NaN` values.

### Instructions

Read the `facts` table into pandas, and then compute the projected population for each country in `2050`. Here's a rough outline of the steps you'll need to take:

- Create a script called `growth.py`.

- Read `facts` into a pandas dataframe using `read_sql_query`.

- Filter out any rows that have invalid data. Look for things like the value `0` in the `area_land` column, which doesn't make sense.

- Write a function that takes in a country's initial population and growth rate, and then outputs the final population. The annual population growth (expressed as a percentage) for each country is in the `population_growth` column, while the initial population is in the `population` column.

  - The formula for compound annual population growth is:

  N=N0e(rt)

  N is the final population, N0 is the initial population, e is a constant value we can access with `math.e`, r is the rate of annual change, expressed as a decimal (so `1.5` percent should be `.015`), and t is the number of years we want to project out. Assume that you'll be starting in January `2015`and ending in January `2050`, covering a period of `35` years.* For example, let's say you have a country with 5000 people and a 4 percent annual growth rate. The formula would be N=5000∗e(.04∗35). *Use the `apply` method on pandas dataframes to compute what the population will be in `2050`for each row in the data. *Use the dataframe sort_values method to sort on the 2050 population in descending order. *Print the `10` countries that will have the highest populations in `2050`.

## 6: Summing Columns To Compute Total Area

We can add up all of the values in a column by using the `SUM` function in a SQL query. For example, we can calculate the total `population` with this query:



```
SELECT SUM(population) from facts;
```

We can also add a `WHERE` clause, like this:



```
SELECT SUM(population) from facts WHERE area_land != "";
```

### Instructions

Use SQL and Python to compute the ratio of land area to water area that each country claims. Here's a rough outline of the steps you'll need to take:

- Write a script called `area.py`.
- Query to get the total of the `area_land` column.
- Query to get the total of the `area_water` column`.
- Divide `area_land` by `area_water` and print the result.

## 7: Next Steps

That's it for the guided project, but we encourage you to come up with your own questions about the data and find the answers. Be creative in exploring the data set!

Here are a few interesting questions to get you started:

- Which countries will lose population over the next `35` years?
- Which countries have the lowest and highest population densities?
- Which countries receive the most immigrants? Which countries lose the most emigrants?

You can write scripts and explore here, or download the code to your computer using the download icon to the right:

![Imgur](https://dq-content.s3.amazonaws.com/MXwzEqM.png)

We hope this guided project has been a good experience. Please email us at [hello@dataquest.io](mailto:hello@dataquest.io) if you want to share your work -- we'd love to see it!

# SQL Summary Statistics

## 1: Introduction

In this mission, we'll be calculating summary statistics with SQL. We've often needed to count the number of records that matched a particular SQL query. So far, we've been able to do this by:

- Performing a SQL query with Python
- Retrieving the results and storing them as a list
- Finding the length of the list

While this approach works, it requires quite a bit of code, and it's also fairly slow. As we progress through this mission, we'll learn how to count with SQL only.

We'll be working with `factbook.db`, a SQLite database containing information about every country in the world. We'll use a table in the file called `facts`. Each row in `facts` represents a single country and contains several columns, including:

- `name` - The name of the country
- `area` - The total land and sea area of the country
- `population` - The country's population
- `birth_rate` - The country's birth rate
- `created_at` - The date the record was created
- `updated_at` - The date the record was updated

Here are the first few rows of `facts`:

| id   | code | name        | area    | area_land | area_water | population | population_growth | birth_rate | death_rate | migration_rate | created_at                 | updated_at                 |
| ---- | ---- | ----------- | ------- | --------- | ---------- | ---------- | ----------------- | ---------- | ---------- | -------------- | -------------------------- | -------------------------- |
| 1    | af   | Afghanistan | 652230  | 652230    | 0          | 32564342   | 2.32              | 38.57      | 13.89      | 1.51           | 2015-11-01 13:19:49.461734 | 2015-11-01 13:19:49.461734 |
| 2    | al   | Albania     | 28748   | 27398     | 1350       | 3029278    | 0.3               | 12.92      | 6.58       | 3.3            | 2015-11-01 13:19:54.431082 | 2015-11-01 13:19:54.431082 |
| 3    | ag   | Algeria     | 2381741 | 2381741   | 0          | 39542166   | 1.84              | 23.67      | 4.31       | 0.92           | 2015-11-01 13:19:59.961286 | 2015-11-01 13:19:59.961286 |

### Instructions

- Import `sqlite3`.
- Initialize a connection to `factbook.db` using the [connect()](https://docs.python.org/3.5/library/sqlite3.html#sqlite3.connect) method, and store it in the variable `conn`.
- Use `conn`, the [execute()](https://docs.python.org/3.5/library/sqlite3.html#sqlite3.Cursor.execute) method, and the [fetchall()](https://docs.python.org/3.5/library/sqlite3.html#sqlite3.Cursor.fetchall) method to fetch all of the records in the `facts` table. Assign the result to the `facts` variable.
- Print out the `facts` variable.
- Count the number of items in `facts`, and assign the result to `facts_count`.

```

import sqlite3

conn = sqlite3.connect("factbook.db")
facts = conn.execute("SELECT * FROM facts;").fetchall()
print(facts)
facts_count = len(facts)
```

## 2: Counting The Number Of Rows In SQL

Counting the number of records in a table is a common operation, and it feels like it should be more efficient than the code we just wrote on the last screen. Thankfully, SQL has a `COUNT` aggregation function that allows us to count the number of records in a table. We call it an aggregation function because it works across many rows to calculate an aggregate value. Here's an example:



```
SELECT COUNT(*) FROM facts;
```

The query above will count the number of rows in the `facts` table of `factbook.db`. If we want to count the number of non-null values in a single column instead, we can use the following syntax:



```
SELECT COUNT(area_water) 

FROM facts;
```

Note that this query will only count the total number of non-null values in the `area_water` column. That means it can return a different total than `COUNT(*)`.

Each of the queries above will return a list with a single tuple when we execute it in Python. The result will look like this:



```
[(243,)]
```

To get the integer count from the result, we'll need to extract the first element in the first tuple of the results.

This style saves typing, and it's also much faster for larger data sets. That's because we can do the counting inside the database, rather than having to pull all of the data into the Python environment first. In general, doing operations within a SQL database engine will be faster than doing the equivalent operations after pulling the data into a programming environment. This is because SQL database engines are optimized specifically for querying.

### Instructions

- Use the `COUNT` aggregation function to count the number of non-null values in the `birth_rate` column of the `facts` table.
- Extract the integer value from the result, and assign it to `birth_rate_count`.
- Print out `birth_rate_count`.

```
conn = sqlite3.connect("factbook.db")
birth_rate_tuple = conn.execute("SELECT COUNT(birth_rate) FROM facts;").fetchall()
birth_rate_count = birth_rate_tuple[0][0]
print(birth_rate_count)
```

## 3: Finding A Column's Minimum And Maximum Values In SQL

SQL has other aggregation functions, in addition to `COUNT`. `MIN` and `MAX`, for example, find the minimum and maximum values in a column. While we can use the `COUNT` function with any column, `MIN` and `MAX` only work with numeric columns. Here's an example of how we can use these functions:



```
SELECT MAX(birth_rate) 

FROM facts;
```

Just like the `COUNT` function, `MIN` and `MAX` will return a list with a single tuple. In this case, the result is:



```
[(45.45,)]
```

`45.45` is the highest value in the `birth_rate` column of the `facts` table.

### Instructions

- Use the `MIN` function to find the minimum value in the `population_growth` column.Extract the numeric result and assign it to `min_population_growth`.Print `min_population_growth`.
- Use the `MAX` function to find the maximum value in the `death_rate` column.Extract the numeric result and assign it to `max_death_rate`.Print `max_death_rate`.

```
conn = sqlite3.connect("factbook.db")
pop_growth_tuple = conn.execute("SELECT MIN(population_growth) FROM facts;").fetchall()
min_population_growth = pop_growth_tuple[0][0]
print(min_population_growth)

death_rate_tuple = conn.execute("SELECT MAX(death_rate) FROM facts;").fetchall()
max_death_rate = death_rate_tuple[0][0]
print(max_death_rate)
```

## 4: Calculating Sums And Averages In SQL

The final two aggregation functions we'll look at are `SUM` and `AVG`. `SUM` finds the total of all of the values in a numeric column:



```
SELECT SUM(birth_rate) 

FROM facts;
```

This function also returns a list with a single tuple. Our query will return this list:



```
[(4406.909999999998,)]
```

`AVG` finds the mean of all of the non-null values in a column:



```
SELECT AVG(birth_rate) 

FROM facts;
```

The result of this query is:



```
[(19.32855263157894,)]
```

### Instructions

- Use the `SUM` function to find the sum of the `area_land`column.Extract the numeric result and assign it to `total_land_area`.Print `total_land_area`.
- Use the `AVG` function to find the mean of the `area_water`column.Extract the numeric result and assign it to `avg_water_area`.Print `avg_water_area`.

```
conn = sqlite3.connect("factbook.db")
total_land_tuple = conn.execute("SELECT SUM(area_land) FROM facts;").fetchall()
total_land_area = total_land_tuple[0][0]
print(total_land_area)

avg_water_tuple = conn.execute("SELECT AVG(area_water) FROM facts;").fetchall()
avg_water_area = avg_water_tuple[0][0]
print(avg_water_area)
```

## 5: Combining Multiple Aggregation Functions

If we wanted to use the `SUM`, `AVG`, and `MAX` functions on a column, writing three different queries would be inefficient. Recall that we can query multiple columns by separating their names with commas, like this:



```
SELECT birth_rate, death_rate, population_growth 

FROM facts;
```

We can apply the sample principle to combine multiple aggregation functions into a single query:



```
SELECT COUNT(*), SUM(death_rate), AVG(population_growth) 

FROM facts;
```

Because we've specified three aggregation functions in the query, it will return a list containing a tuple with three elements:



```
[(261, 1783.2500000000002, 1.2009745762711865)]
```

The order of the results corresponds to the order of the aggregation functions in the query. In our example, the first element in the tuple is the count of all the rows, the second is the sum of the `death_rate`column, and the third is the mean of the `population_growth` column.

### Instructions

- Write a single query that calculates the following statistics about the `facts` table, in order:The mean of the `population` columnThe sum of the `population` columnThe maximum value in the `birth_rate` column
- Assign the result of the query to `facts_stats`
- Print `facts_stats`

```
conn = sqlite3.connect("factbook.db")
facts_stats = conn.execute("SELECT AVG(population), SUM(population), MAX(birth_rate) FROM facts;").fetchall()
print(facts_stats)
```

## 6: Aggregating Values For A Subset Of The Data

As you may recall from an earlier mission, we can use the `WHERE` statement to limit our query to certain rows in a SQL table:



```
SELECT population 

FROM facts 

WHERE birth_rate > 10;
```

The query above will select any values in the `population` column where the `birth_rate` is higher than `10`. We can also use `WHERE` statements with aggregation functions to calculate statistics for a subset of rows:



```
SELECT COUNT(*) 

FROM facts 

WHERE population > 5000000;
```

The query above will count the number of rows where `population` is greater than `5000000`.

### Instructions

- Calculate the mean `population_growth` for countries with a `population` greater than `10000000`.Extract the numeric result and assign it to `population_growth`.Print `population_growth`.

```
conn = sqlite3.connect("factbook.db")
pop_query = conn.execute("SELECT AVG(population_growth) FROM facts WHERE population > 10000000;").fetchall()
population_growth = pop_query[0][0]
print(population_growth)
```

## 7: Selecting Unique Rows

There are times when we only want to select the unique values in a column or database, rather than each individual row. One example would be if our `facts` table had duplicate entries for each country, like this:

| id   | code | name        | area   | area_land | area_water | population | population_growth | birth_rate | death_rate | migration_rate | created_at                 | updated_at                 |
| ---- | ---- | ----------- | ------ | --------- | ---------- | ---------- | ----------------- | ---------- | ---------- | -------------- | -------------------------- | -------------------------- |
| 1    | af   | Afghanistan | 652230 | 652230    | 0          | 32564342   | 2.32              | 38.57      | 13.89      | 1.51           | 2015-11-01 13:19:49.461734 | 2015-11-01 13:19:49.461734 |
| 2    | af   | Afghanistan | 652230 | 652230    | 0          | 32564342   | 2.32              | 38.57      | 13.89      | 1.51           | 2015-11-01 13:19:49.461734 | 2015-11-01 13:19:49.461734 |

To get a list of all of the countries in the world, we'll need to remove these duplicate rows so that there aren't duplicate entries. We can do this with the `DISTINCT` statement:



```
SELECT DISTINCT name 

FROM facts;
```

This query will return all of the unique values in the `name` column of `facts`. It won't return any duplicate values.

We can also use the `DISTINCT` statement with multiple columns to return unique pairings of those columns:



```
SELECT DISTINCT name, population 

FROM facts;
```

The query above will select the unique combinations of values in the `population`and `name` columns from `facts`.

### Instructions

- Select all of the distinct values in the `birth_rate` column of the `facts` table, and assign the result to `unique_birth_rates`.
- Print `unique_birth_rates`.

```
conn = sqlite3.connect("factbook.db")
unique_birth_rates = conn.execute("SELECT DISTINCT birth_rate FROM facts;").fetchall()
print(unique_birth_rates)
```

## 8: Aggregating Unique Values

If we wanted to count the number of unique items in the `population` column, we could use the `COUNT` aggregation function along with the `DISTINCT`statement. Here's how it would work:



```
SELECT COUNT(DISTINCT population) 

FROM facts;
```

The query above will count all of the distinct values in the `population` column. We can also use other aggregation functions along with the `DISTINCT`statement:



```
SELECT AVG(DISTINCT birth_rate) 

FROM facts;
```

This query will find the mean of all of the distinct values in the `birth_rate` column.

### Instructions

- Find the average of all of the distinct values in the `birth_rate` column where `population` is greater than `20000000`.Extract the numeric result and assign it to `average_birth_rate`.Print `average_birth_rate`.
- Find the sum of all of the distinct values in the `population` column where `area_land` is greater than `1000000`.Extract the numeric result and assign it to `sum_population`.Print `sum_population`.

```
conn = sqlite3.connect("factbook.db")
query = conn.execute("SELECT AVG(DISTINCT birth_rate) FROM facts WHERE population > 20000000;").fetchall()
average_birth_rate = query[0][0]
print(average_birth_rate)

query = conn.execute("SELECT SUM(DISTINCT population) FROM facts WHERE area_land > 1000000;").fetchall()
sum_population = query[0][0]
print(sum_population)
```

## 9: Performing Arithmetic In SQL

Sometimes we'll want to perform some arithmetic on the columns in a SQL table. We might want to make the counts in the `population` column easier to understand by expressing them in terms of millions, for example. Instead of a number like `9766442`, we'd want to display `9.766442`. We could do this in Python, but it would be cumbersome to pull all of the data into the Python environment and then manipulate it. Fortunately, we can perform the math inside the SQL database engine instead. Here's an example:



```
SELECT population / 1000000 

FROM facts;
```

The query above will divide each value in the `population` column by `1000000`, and return the result. Because the `population`column contains integers and we're dividing by an integer, the results will be integers as well. If we want to retain precision, we can specify a float instead:



```
SELECT population / 1000000.0 

FROM facts;
```

The query above will return a series of floats, instead of rounding the values to integers. Here are the rules for what an arithmetic operation will return:

- Two floats - Returns a float (ex. `SELECT birth_rate / 1000000.0 FROM facts;`)
- A float and an integer - Returns a float (ex. `SELECT population / 1000000.0 FROM facts;`)
- Two integers - Returns an integer (ex. `SELECT population / 1000000 FROM facts;`)

### Instructions

- Use arithmetic operators in a SQL query to express `population_growth` in terms of millions. Divide by a float so the query also returns a float.Assign the result of the query to `population_growth_millions`.Print `population_growth_millions`.

```
conn = sqlite3.connect("factbook.db")
population_growth_millions = conn.execute("SELECT population_growth / 1000000.0 FROM facts;").fetchall()
print(population_growth_millions)
```

## 10: Performing Arithmetic Between Columns

A few screens ago, we learned how to apply aggregation functions to columns in the `SELECT` statement, like so:



```
SELECT AVG(birth_rate), SUM(population)

FROM facts;
```

The aggregation functions modified the columns' values before SQLite returned them. SQL lets us perform many different kinds of manipulations on the columns we select. To calculate the ratio between births and deaths for each country, for example, we could divide the `birth_rate`column by the `death_rate` column. Here's how we would accomplish this:



```
SELECT birth_rate / death_rate 

FROM facts;
```

The query above will divide each value in the `birth_rate` column by the corresponding value in the `death_rate`column.

We can also perform more complex queries, such as finding the ratio of `birth_rate` plus `migration_rate` to `death_rate`. The results will help us discover whether the population is increasing or decreasing:



```
SELECT (birth_rate + migration_rate) / death_rate 

FROM facts;
```

The query will add together the `birth_rate` and `migration_rate`columns, then divide by the `death_rate`column. Arithmetic in SQL respects the order of operations and parentheses, so the addition step happens before the division step.

### Instructions

- Use a SQL query to compute the population of each country a year from now.
  - Multiply the `population` and `population_growth`columns, then add the `population` column to the result.
- Assign the result of the query to `next_year_population`.
- Print `next_year_population`.

```
conn = sqlite3.connect("factbook.db")
next_year_population = conn.execute("SELECT (1 + (population_growth/100)) * population FROM facts;").fetchall()
print(next_year_population)
```

## 11: Next Steps

In this mission, we explored how to calculate summary statistics in SQL. It's often advantageous to do these computations in the SQL database instead of a Python environment because it's faster to code and execute. In the next mission, we'll cover how to calculate more advanced statistics in SQL with the `GROUP BY` statement.

# Group Summary Statistics

## 1: Introduction

In the last mission, we computed summary statistics across columns with SQL. In many cases, though, we want to drill down even more and compute summary statistics per group. In this mission, we'll explore how to calculate more granular summary statistics. We'll switch back to writing SQL queries directly instead of using Python so we can focus on the SQL syntax.

We'll be working with a data set on jobs we stored in the `recent_grads` table of `jobs.db`. Each row represents a single college major, and contains information about post-graduation employment of students who studied the major. You can find out more about the data set in [FiveThirtyEight's GitHub repository for the project](https://github.com/fivethirtyeight/data/tree/master/college-majors). Here are some descriptions for just a few of the 21 total columns:

- `Rank` - The major's rank by median earnings
- `Major_code` - The major's code or ID
- `Major` - The name of the major
- `Major_category` - The broader category the major belongs to
- `Total` - The total number of people who studied the major
- `Men` - The number of male graduates
- `Women` - The number of female graduates
- `ShareWomen` - Women as a proportion of the total number of graduates (a number ranging from `0` to `1`)
- `Employed` - The number of employed graduates

Here are the first few rows and columns in the data set:

| Rank | Major_code | Major                                    | Major_category | Total | Sample_size | Men   | Women | ShareWomen | Employed |
| ---- | ---------- | ---------------------------------------- | -------------- | ----- | ----------- | ----- | ----- | ---------- | -------- |
| 1    | 2419       | PETROLEUM ENGINEERING                    | Engineering    | 2339  | 36          | 2057  | 282   | 0.120564   | 1976     |
| 2    | 2416       | MINING AND MINERAL ENGINEERING           | Engineering    | 756   | 7           | 679   | 77    | 0.101852   | 640      |
| 3    | 2415       | METALLURGICAL ENGINEERING                | Engineering    | 856   | 3           | 725   | 131   | 0.153037   | 648      |
| 4    | 2417       | NAVAL ARCHITECTURE AND MARINE ENGINEERING | Engineering    | 1258  | 16          | 1123  | 135   | 0.107313   | 758      |
| 5    | 2405       | CHEMICAL ENGINEERING                     | Engineering    | 32260 | 289         | 21239 | 11021 | 0.341631   | 25694    |

As we progress through this mission, we'll drill down and compute summary statistics by group to answer questions like:

- What's the share of women in each major category?
- Which major categories have the greatest numbers of employed graduates?
- What percentage of people in each major category end up in low-wage jobs?

First, let's explore the data.

## 2: Introduction

### Instructions

Write a SQL query that displays all of the columns and the first five rows of the `recent_grads` table.

```

SELECT * FROM recent_grads LIMIT 5;
```

## 3: Calculating Group-Level Summary Statistics

The `GROUP BY` SQL statement allows us to compute summary statistics by "group," or unique value. When we use this statement, SQL creates a group for each unique value in a column or set of columns (the same values we get when we use the `DISTINCT` statement), and then does the calculations for them. To illustrate, we can find the total number of people employed in each major category with the following query:



```
SELECT SUM(Employed) 

FROM recent_grads 

GROUP BY Major_category;
```

This will give us the total number of employed graduates for each major category. Here's a truncated view of the output:

| SUM(Employed) |
| ------------- |
| 66943         |
| 288114        |
| 302797        |

The output shows aggregate counts of the `Employed` column for each `Major_category`. Unfortunately, it doesn't indicate which major category each row refers to. We can fix this by including the `Major_category`column in our query:



```
SELECT Major_category, SUM(Employed) 

FROM recent_grads 

GROUP BY Major_category;
```

This makes the output much easier to understand:

| Major_category                  | SUM(Employed) |
| ------------------------------- | ------------- |
| Agriculture & Natural Resources | 66943         |
| Arts                            | 288114        |
| Biology & Life Science          | 302797        |

Here's how the query works. The `GROUP BY` statement splits the `Major_category` column into groups (with one group for each unique major category), then calculates the sum for each group. The following diagram shows how `GROUP BY` splits the data. (The diagram uses a small sample from the `recent_grads` table.):

Major_categoryEmployedArts2914SELECTEmployed,Major_category,SUM(Employed)Agriculture3149FROMrecent_gradsGROUPBYMajor_category;Arts36165Agriculture1290Group1Group2Major_category="Arts"Major_category="Agriculture"Arts2914Agriculture3149Arts36165Agriculture1290

For each group, the `GROUP BY` statement queries each column, and runs all of the aggregation functions we include in the query after the `SELECT` statement:

SELECTEmployed,Major_category,SUM(Employed)FROMrecent_gradsGROUPBYMajor_category;Group1Major_category="Arts"Arts2914Arts36165EmployedMajor_categorySUM(Employed)36165Arts2914+36165=39075

If a column is selected, the SQL engine will use the last value for that column in the group. If an aggregation function is selected, the SQL engine will compute the value for that aggregation function across the group.

The query in the diagram will give us the following result:

| Employed | Major_category | SUM(Employed) |
| -------- | -------------- | ------------- |
| 1290     | Agriculture    | 4439          |
| 36165    | Arts           | 39075         |

## 4: Calculating Group-Level Summary Statistics

### Instructions

- Use the `SELECT` statement to select the following columns and aggregates in a query:`Major_category``AVG(ShareWomen)`
- Use the `GROUP BY` statement to group the query by the `Major_category` column.

```

SELECT Major_category, AVG(ShareWomen) FROM recent_grads GROUP BY Major_category;
```

## 5: Renaming Columns With The AS Statement

You may have noticed that on the last screen, specifying `AVG(ShareWomen)`caused the column to have that name in the results. This can lead to confusion, and make it difficult to work with the results of SQL queries. To avoid this issue, we can select and rename columns at the same time with the `AS` statement. Here's an example:



```
SELECT AVG(ShareWomen) AS average_female_share 

FROM recent_grads;
```

This query will result in the following output:

| average_female_share |
| -------------------- |
| 0.5225502029537575   |

### Instructions

- Write a query that selects the following items, in order, and renames them with `AS`:`SUM(Men)` as `total_men`.`SUM(Women)` as `total_women`.

```

SELECT SUM(Men) AS total_men, SUM(Women) AS total_women FROM recent_grads;
```

## 6: Practice: Using GROUP BY

Now that we have a better understanding of the `GROUP BY` statement, let's practice using it by computing summary statistics by group for the `recent_grads` table.

### Instructions

- For each major category, find the percentage of graduates who are employed.
  - Use the `SELECT` statement to select the following columns and aggregates in your query:`Major_category``AVG(Employed) / AVG(Total)` as `share_employed`
  - Use the `GROUP BY` statement to group the query by the `Major_category` column.

```

SELECT Major_category, AVG(Employed) / AVG(Total) AS share_employed FROM recent_grads GROUP BY Major_category;
```

## 7: Querying Virtual Columns With The HAVING Statement

Sometimes we want to select a subset of rows after performing a `GROUP BY` query. On the last screen, for instance, we may have wanted to select only those rows where `share_employed` is greater than `.8`. We can't use the `WHERE` clause to do this because `share_employed` isn't a column in `recent_grads`; it's actually a virtual column generated by the `GROUP BY`statement.

When we want to filter on a column generated by a query, we can use the `HAVING` statement. Here's an example:



```
SELECT Major_category, AVG(Employed) / AVG(Total) AS share_employed 

FROM recent_grads 

GROUP BY Major_category 

HAVING share_employed > .8;
```

Note that we used the same column name in the `HAVING` statement that we originally specified with the `AS`statement. SQL allows us to use custom column names in subsequent statements, including `HAVING` and `WHERE`. The statement above will result in the following output:

| Major_category                  | share_employed     |
| ------------------------------- | ------------------ |
| Agriculture & Natural Resources | 0.8369862842425075 |
| Arts                            | 0.8067482429367457 |
| Business                        | 0.8359659576036412 |
| Communications & Journalism     | 0.8422291333949735 |

Note that the results only include categories where `share_employed` is greater than `.8`. That's because the `HAVING` statement filters out the other rows.

### Instructions

- Find all of the major categories where the share of graduates with low-wage jobs is greater than `.1`.Use the `SELECT` statement to select the following columns and aggregates in a query:`Major_category``AVG(Low_wage_jobs) / AVG(Total)` as `share_low_wage`Use the `GROUP BY` statement to group the query by the `Major_category` column.Use the `HAVING` statement to restrict the selection to rows where `share_low_wage` is greater than `.1`.

```

SELECT Major_category, AVG(Low_wage_jobs) / AVG(Total) AS share_low_wage FROM recent_grads GROUP BY Major_category HAVING share_low_wage > .1;
```

## 8: Rounding Results With The ROUND Function

On the last screen, the percentages in our results were very long and hard to read (e.g., `0.16833085991095678`). We can use the SQL `ROUND` function in our query to round them. Here's an example of what this looks like:



```
SELECT Major_category, ROUND(ShareWomen, 2) AS rounded_share_women 

FROM recent_grads;
```

The query will round the `ShareWomen`column to two decimal places. Here's a truncated view of the results:

| Major_category | rounded_share_women |
| -------------- | ------------------- |
| Engineering    | 0.12                |
| Engineering    | 0.1                 |

By passing different values in to the `ROUND` function, such as `ROUND(ShareWomen, 3)`, we can round to different decimal places.

### Instructions

- Write a SQL query that returns the following columns of `recent_grads` (in the same order):`ShareWomen` rounded to `4` decimal places`Major_category`
- Limit the results to `10` rows.

```

SELECT ROUND(ShareWomen, 4), Major_category FROM recent_grads LIMIT 10;
```

## 9: Nesting Functions

On a previous screen, we ran the following query:



```
SELECT Major_category, AVG(Employed) / AVG(Total) AS share_employed 

FROM recent_grads 

GROUP BY Major_category 

HAVING share_employed > .8;
```

This query returned very long fractional values for `share_employed`. We can update our query with the `ROUND` function to round the results to three decimal places:



```
SELECT Major_category, ROUND(AVG(Employed) / AVG(Total), 3) AS share_employed 

FROM recent_grads 

GROUP BY Major_category 

HAVING share_employed > .8;
```

This will return the following result:

| Major_category                  | share_employed |
| ------------------------------- | -------------- |
| Agriculture & Natural Resources | 0.837          |
| Arts                            | 0.807          |

### Instructions

- Use the `SELECT` statement to select the following columns and aggregates in a query:`Major_category``AVG(College_jobs) / AVG(Total)` as `share_degree_jobs`Use the `ROUND` function to round `share_degree_jobs` to `3` decimal places.
- Group the query by the `Major_category` column.
- Only select rows where `share_degree_jobs` is less than `.3`.

```

SELECT Major_category, ROUND(AVG(College_jobs) / AVG(Total), 3) AS share_degree_jobs FROM recent_grads GROUP BY Major_category HAVING share_degree_jobs < .3;
```

## 10: Next Steps

In this mission, we covered the `GROUP BY` and `HAVING` statements. We can use these statements to quickly calculate powerful summary statistics in SQL. In the next few missions, we'll learn more about working with SQL tables, including how to insert and modify data.

# Challenge: Data Exploration

## 1: Introduction

In this challenge, you'll practice calculating summary statistics in SQL while exploring data from `factbook.db`. Recall that `factbook.db` contains information about all of the countries in the world. You'll work with the `facts`table, where each row represents a single country. Here are the descriptions for some of the columns:

- `name` - The name of the country
- `area` - The total land and sea area of the country
- `population` - The country's population
- `birth_rate` - The country's birth rate
- `created_at` - The date the record was created
- `updated_at` - The date the record was updated

Here are the first few rows of `facts`:

| id   | code | name        | area    | area_land | area_water | population | population_growth | birth_rate | death_rate | migration_rate | created_at                 | updated_at                 |
| ---- | ---- | ----------- | ------- | --------- | ---------- | ---------- | ----------------- | ---------- | ---------- | -------------- | -------------------------- | -------------------------- |
| 1    | af   | Afghanistan | 652230  | 652230    | 0          | 32564342   | 2.32              | 38.57      | 13.89      | 1.51           | 2015-11-01 13:19:49.461734 | 2015-11-01 13:19:49.461734 |
| 2    | al   | Albania     | 28748   | 27398     | 1350       | 3029278    | 0.3               | 12.92      | 6.58       | 3.3            | 2015-11-01 13:19:54.431082 | 2015-11-01 13:19:54.431082 |
| 3    | ag   | Algeria     | 2381741 | 2381741   | 0          | 39542166   | 1.84              | 23.67      | 4.31       | 0.92           | 2015-11-01 13:19:59.961286 | 2015-11-01 13:19:59.961286 |

In this challenge, you'll use the population values for each country to predict the populations for the following year. First, you'll need to explore the data and look for any quality issues.

### Instructions

- In SQL, calculate the means of the `population`, `population_growth`, `birth_rate`, and `death_rate`columns.
- Assign the mean of the `population` column to `pop_avg`.
- Assign the mean of the `population_growth` column to `pop_growth_avg`.
- Assign the mean of the `birth_rate` column to `birth_rate_avg`.
- Assign the mean of the `death_rate` column to `death_rate_avg`.

```
import sqlite3
conn = sqlite3.connect("factbook.db")
averages = "select avg(population), avg(population_growth), avg(birth_rate), avg(death_rate) from facts;"
avg_results = conn.execute(averages).fetchall()
pop_avg = avg_results[0][0]
pop_growth_avg = avg_results[0][1]
birth_rate_avg = avg_results[0][2]
death_rate_avg = avg_results[0][3]
```

## 2: Find Ranges

While the averages give you some sense of the values in these columns, you should also calculate the ranges so you know what their lower and upper bounds are. This will also allow you to look for outliers.

### Instructions

Calculate the minimum and maximum values for the columns from the previous screen:

- Assign the minimum of the `population` column to `pop_min`.
- Assign the maximum of the `population` column to `pop_max`.
- Assign the minimum of the `population_growth` column to `pop_growth_min`.
- Assign the maximum of the `population_growth` column to `pop_growth_max`.
- Assign the minimum of the `birth_rate` column to `birth_rate_min`.
- Assign the maximum of the `birth_rate` column to `birth_rate_max`.
- Assign the minimum of the `death_rate` column to `death_rate_min`.
- Assign the maximum of the `death_rate` column to `death_rate_max`.

You can observe these values using `print` statements, or the variables display below the output box.

```
conn = sqlite3.connect("factbook.db")

averages = "select avg(population), avg(population_growth), avg(birth_rate), avg(death_rate), avg(migration_rate) from facts;"
avg_results = conn.execute(averages).fetchall()
pop_avg = avg_results[0][0]
pop_growth_avg = avg_results[0][1]
birth_rate_avg = avg_results[0][2]
death_rate_avg = avg_results[0][3]
mig_rate_avg = avg_results[0][4]
minimums = "select min(population), min(population_growth), min(birth_rate), min(death_rate) from facts;"
maximums = "select max(population), max(population_growth), max(birth_rate), max(death_rate) from facts;"
min_results = conn.execute(minimums).fetchall()
max_results = conn.execute(maximums).fetchall()

# population column
pop_min = min_results[0][0]
pop_max = max_results[0][0]
# population_growth column
pop_growth_min = min_results[0][1]
pop_growth_max = max_results[0][1]
# birth_rate column
birth_rate_min = min_results[0][2]
birth_rate_max = max_results[0][2]
# death_rate column
death_rate_min = min_results[0][3]
death_rate_max = max_results[0][3]

print(min_results)
print(max_results)
```

## 3: Filter Values

If you observed the values on the previous screen, you may have noticed the outliers. The max for population is 7,256,490,011, while the minimum is 0. We know that China, the most populated country in the world, has less than 2 billion people. The max value for the `population` column is over 7 billion, however. The minimum value for the `population` column is also problematic, because no country has 0 people.

These quirks exist because the database contains rows for entities that aren't countries. There's a row representing the entire world, for example (hence the 7 billion population), and some rows representing oceanic areas (hence the population of 0).

### Instructions

Write a single query that returns the following minimum and maximum values for countries where `population` is less than 2 billion and `population` is greater than 0:

- Assign the minimum of the `population` column to `pop_min`.
- Assign the maximum of the `population` column to `pop_max`.
- Assign the minimum of the `population_growth` column to `pop_growth_min`.
- Assign the maximum of the `population_growth` column to `pop_growth_max`.
- Assign the minimum of the `birth_rate` column to `birth_rate_min`.
- Assign the maximum of the `birth_rate` column to `birth_rate_max`.
- Assign the minimum of the `death_rate` column to `death_rate_min`.
- Assign the maximum of the `death_rate` column to `death_rate_max`.

```
conn = sqlite3.connect("factbook.db")
min_and_max = '''
select min(population), max(population), min(population_growth), max(population_growth),
min(birth_rate), max(birth_rate), min(death_rate), max(death_rate)
from facts where population > 0 and population < 2000000000;
'''
results = conn.execute(min_and_max).fetchall()
print(results)

# population column
pop_min = results[0][0]
pop_max = results[0][1]
# population_growth column
pop_growth_min = results[0][2]
pop_growth_max = results[0][3]
# birth_rate column
birth_rate_min = results[0][4]
birth_rate_max = results[0][5]
# death_rate column
death_rate_min = results[0][6]
death_rate_max = results[0][7]
```

## 4: Predict Future Population Growth

These measures seem to align more with reality. Now let's predict next year's population for each country using the following formula:



```
projected_population = population + (population * (population_growth/100))
```

We need to divide by `100` because the values in `population_growth` are percentage values (e.g. `2.32`) instead of proportional values (e.g. `0.0232`).

### Instructions

Use SQL arithmetic to return the projected population values using the above formula and the following parameters:

- Round the values to the nearest whole number (population can't contain a fractional value).
- Filter out any rows with `NULL` as the value for either `population` or `population_growth`.
- Restrict the query to countries with a `population` that's less than 7 billion and greater than 0.
- Assign the resulting projections to `projected_population`.

```
import sqlite3
conn = sqlite3.connect("factbook.db")
projected_population_query = '''
select round(population + population * (population_growth/100), 0) from facts
where population > 0 and population < 7000000000 
and population is not null and population_growth is not null;
'''

projected_population = conn.execute(projected_population_query).fetchall()

print(projected_population[0:10])
```

## 5: Explore Projected Population

To understand how global population would shift under the projections, calculate the minimum, maximum, and average values.

### Instructions

Write a single query that returns:

- the minimum of the projected population values, and assigns it to `pop_proj_min`.
- the maximum of the projected population values, and assigns it to `pop_proj_max`.
- the average of the projected population values, and assigns it to `pop_proj_avg`.

Be sure to:

- Round all fractional values to the nearest whole number.
- Filter out any rows with `NULL` as the value for either `population` or `population_growth`.
- Restrict the query to countries with a `population` of less than 7 billion and greater than 0.
- Use `print` statements or the variables display below the output box to observe these values.

```
import sqlite3
conn = sqlite3.connect("factbook.db")
proj_pop_query = '''
select round(min(population + population * (population_growth/100)), 0), 
round(max(population + population * (population_growth/100)), 0), 
round(avg(population + population * (population_growth/100)), 0)
from facts 
where population > 0 and population < 7000000000 and 
population is not null and population_growth is not null;
'''

proj_results = conn.execute(proj_pop_query).fetchall()

pop_proj_min = proj_results[0][0]
pop_proj_max = proj_results[0][1]
pop_proj_avg = proj_results[0][2]

print("Projected Population,", "Minimum: ", pop_proj_min)
print("Projected Population,", "Maximum: ", pop_proj_max)
print("Projected Population,", "Average: ", pop_proj_avg)
```

## 6: Next Steps

In this challenge, you calculated summary statistics to understand the data better, and then projected the following year's population for each country using SQL arithmetic. In the next mission, you'll learn about group summary techniques for segmenting data in your queries.