# Querying SQLite from Python
------

## 1: Overview

In past missions, we focused on exploring the SQL syntax for retrieving data from a database. In this mission, we'll explore how to interact with a SQLite database in Python so you can start to incorporate databases into your data science workflow.

SQLite is a database that doesn't require a standalone server; it stores the entire database as a file on disk. This makes it ideal for working with larger data sets that can fit on disk but not in memory.

The pandas library loads the entire data set we're working with into memory, making SQLite a compelling alternative for working with data sets larger than 8 gigabytes (which is roughly the amount of memory modern computers contain). The fact that we can contain an entire database in a single file makes them easy to share; some data sets are available online as SQLite database files (using the extension `.db`).

We can interact with a SQLite database in two main ways:

* Through the SQLite Python module
* Through the SQLite shell

In this mission, we'll focus on learning how to use the [SQLite Python module](https://docs.python.org/3/library/sqlite3.html) to interact with the database. We'll work with the SQLite shell in the guided project that comes after this mission.

## 2: Introduction To The Data
We'll continue to work with the American Community Survey data on college majors and job outcomes, which looks like this:
| **Rank** | **Major_code** | **Major**                                | **Major_category** | **Total** | **Sample_size** | **Men** | **Women** | **ShareWomen** | **Employed** |
| -------- | -------------- | ---------------------------------------- | ------------------ | --------- | --------------- | ------- | --------- | -------------- | ------------ |
| 1        | 2419           | PETROLEUM ENGINEERING                    | Engineering        | 2339      | 36              | 2057    | 282       | 0.120564       | 1976         |
| 2        | 2416           | MINING AND MINERAL ENGINEERING           | Engineering        | 756       | 7               | 679     | 77        | 0.101852       | 640          |
| 3        | 2415           | METALLURGICAL ENGINEERING                | Engineering        | 856       | 3               | 725     | 131       | 0.153037       | 648          |
| 4        | 2417           | NAVAL ARCHITECTURE AND MARINE ENGINEERING | Engineering        | 1258      | 16              | 1123    | 135       | 0.107313       | 758          |
| 5        | 2405           | CHEMICAL ENGINEERING                     | Engineering        | 32260     | 289             | 21239   | 11021     | 0.341631       | 25694        |
The full table has many more columns than the ones we've displayed above (21 to be specific). You can learn about all of them in [FiveThirtyEight's GitHub repository](https://github.com/fivethirtyeight/data/tree/master/college-majors).

Here are the descriptions for the columns in the preview:
* `Rank` - The major's rank by median earnings
* `Major_code` - The major's code or ID
* `Major` - The name of the major
* `Major_category` - The broader category the major belongs to
* `Total` - The total number of people who studied the major
* `Sample_size` - The sample size (unweighted) of graduates with full time jobs
* `Men` - The number of male graduates
* `Women` - The number of female graduates
* `ShareWomen` - Women as a proportion of the total number of graduates (a number ranging from `0` to `1`)
* `Employed` - The number of employed graduates

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

So far, we've been running queries by creating a Cursor instance, and then calling the `execute` method on the instance. The SQLite library actually allows us to skip creating a Cursor altogether by using the [`execute` method](https://docs.python.org/3/library/sqlite3.html#sqlite3.Connection.execute) within the Connection object itself. SQLite will create a Cursor instance for us under the hood and our query run against the database, but this shortcut allows us to skip a step. Here's what the code looks like:

```
conn = sqlite3.connect("jobs.db")
query = "select * from recent_grads;"
conn.execute(query).fetchall()
```

Notice that we didn't explicitly create a separate Cursor instance ourselves in this code example.

Now let's learn how to fetch a specific number of results after we run a query.

## 8: Fetching A Specific Number Of Results

To make it easier to work with large results sets, the Cursor class allows us to control the number of results we want to retrieve at any given time. To return a single result (as a *tuple*), we use the [Cursor method `fetchone()`](https://docs.python.org/3/library/sqlite3.html#sqlite3.Cursor.fetchone). To return `n` results, we use the [Cursor method `fetchmany()`](https://docs.python.org/3/library/sqlite3.html#sqlite3.Cursor.fetchmany).

Each Cursor instance contains an internal counter that updates every time we retrieve results. When we call the `fetchone()`method, the Cursor instance will return a single result, and then increment its internal counter by 1. This means that if we call `fetchone()` again, the Cursor instance will actually return the second tuple in the results set (and increment by 1 again).

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