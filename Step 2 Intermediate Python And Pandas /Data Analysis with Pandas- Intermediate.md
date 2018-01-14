# Getting started with NumPy

## 1: The Dataset

In this mission, we'll be working with `world_alcohol.csv`, which contains data on how much alcohol is consumed per capita in each country.

Here are the first few rows of the data:

| Year | WHO Region        | Country        | Beverage Types | Display Value |
| ---- | ----------------- | -------------- | -------------- | ------------- |
| 1986 | 'Western Pacific' | 'Viet Nam'     | 'Wine'         | 0             |
| 1986 | 'Americas'        | 'Uruguay'      | 'Other'        | 0.5           |
| 1985 | 'Africa'          | 'Cte d'Ivoire' | 'Wine'         | 1.62          |

Each row specifies how many liters of a type of alcohol each citizen of a country drank in a given year. The first row shows how many liters of wine an average person in Vietnam drank in `1986`.

Here's what each column represents:

- `Year` -- the year the data in the row is for.
- `WHO Region` -- the region in which the country is located.
- `Country` -- the country the data is for.
- `Beverage Types` -- the type of beverage the data is for.
- `Display Value` -- the number of liters, on average, of the beverage type a citizen of the country drank in the given year.

## 2: Lists Of Lists

In previous missions, we worked with datasets like `world_alcohol.csv` by using *lists of lists*. This is a *list* that contains other lists inside. Here's how the first few rows of the data would look as a *list of lists*:

```
world_alcohol = [
                     [1986, "Western Pacific", "Viet Nam", "Wine", 0],
                     [1986, "Americas", "Uruguay", "Other", 0.5],
                     [1986, "Africa", "Cte d'Ivoire", "Wine", 1.62]
                    ]
```

We could retrieve the first row of the data and work with it as a *list*:

```
first_row = world_alcohol[0]
```

Here's what `first_row` would looks like, with the corresponding *index* above each item:

| 0    | 1                 | 2          | 3      | 4    |
| ---- | ----------------- | ---------- | ------ | ---- |
| 1986 | "Western Pacific" | "Viet Nam" | "Wine" | 0    |

`first_row[0]` would retrieve the value `1986`, and `first_row[3]` would retrieve the value `Wine`.

To get a value from a *list of lists*, we first specify an *index* to retrieve the correct row, then an index to retrieve the correct item in that row. We could get the second item in the second row with `world_alcohol[1][1]`.

While extracting single values and rows is easy with *lists of lists*, it's cumbersome to compute statistics and extract columns. If we wanted to compute the average of the `Display Value` column, here's what we'd have to do:

```
# Extract the values in the 5th column.
liters_drank = []
for row in world_alcohol:
    liters = row[4]
    liters_drank.append(liters)
liters_drank = liters_drank[1:len(liters_drank)]

# Calculate the average of the values in the 5th column.
total = 0
for item in liters_drank:
    total = total + float(item)
average = total / len(liters_drank)
print(average)
```

Step by step, we:

- Use a `for` loop to iterate over `world_alcohol`. Within the `for` loop, we:Extract each row's value for the fifth column and assign to `liters`.We then append `liters` to `liters_drank`, which is a *list* that we use to store all of the values in the 5th column.
- Outside the `for` loop, we remove the first item in `liters_drank` since it's just the column name (`Display Value`).
- We then use a new `for` loop to iterate over `liters_drank`. Within the `for` loop, we:Convert each value to a *float* and add it to the *list* `total`.
- Outside the `for` loop, we calculate the average by dividing `total` by the number of elements in `liters_drank`.
- Lastly, we print the average of `liters_drank`.

### Instructions

- Use the [csv](https://docs.python.org/2/library/csv.html) module to read `world_alcohol.csv` into the variable `world_alcohol`.You can use the [csv.reader](https://docs.python.org/2/library/csv.html#csv.reader) function to accomplish this.`world_alcohol` should be a *list of lists*.
- Extract the first column of `world_alcohol`, and assign it to the variable `years`.
- Use list slicing to remove the first item in `years` (this is a header).
- Find the sum of all the items in `years`. Assign the result to `total`.Remember to convert each item to a *float* before adding them together.
- Divide `total` by the length of `years` to get the average. Assign the result to `avg_year`.

```
import csv
f = open("world_alcohol.csv")
reader = csv.reader(f)
world_alcohol = list(reader)

years = []
for row in world_alcohol:
    years.append(row[0])

years = years[1:]

total = 0
for year in years:
    total = total + float(year)

avg_year = total / len(years)
```

## 3: Introducing NumPy

Using [NumPy](http://www.numpy.org/), we can much more efficiently analyze data than we can using *lists*. NumPy is a Python module that is used to create and manipulate multidimensional *arrays*.

An *array* is a collection of values. *Arrays* have one or more dimensions. An *array* dimension is the number of indices it takes to extract a value. In a *list*, we specify a single *index*, so it is one-dimensional:

```
first_row =  [1986, "Western Pacific", "Viet Nam", "Wine", 0]
print(first_row[0])
```

The code above will print out `1986`. A *list* is similar to a NumPy *one-dimensional array*, or *vector*, because we only need a single *index* to get a value.

`world_alcohol.csv`, on the other hand, looks like this:

ColumnIndex0123401986'WesternPacific''VietNam''Wine'0RowIndex11986'Americas''Uruguay''Other'0.521985'Africa''Cted'Ivoire''Wine'1.62

To extract a single value, we need to specify a row *index* then a column *index*. `1, 2` results in `Uruguay`. `2, 3` results in `Wine`.

This is a *two-dimensional array*, also known as a *matrix*. Data sets are usually distributed in the form of *matrices*, and NumPy makes it extremely easy to read in and work with *matrices*.

## 4: Using NumPy

To get started with NumPy, we first need to import it using `import numpy`. We can then read in datasets using the [numpy.genfromtxt()](http://docs.scipy.org/doc/numpy-1.10.0/reference/generated/numpy.genfromtxt.html) function.

Since `world_alcohol.csv` is a *csv* file, rows are separated by line breaks, and columns are separated by commas, like this:

```
Year,WHO region,Country,Beverage Types,Display Value
1986,Western Pacific,Viet Nam,Wine,0
1986,Americas,Uruguay,Other,0.5
1985,Africa,Cte d'Ivoire,Wine,1.62
```

In files like this, the comma is called the *delimiter*, because it indicates where each field ends and a new one begins. Other delimiters, such as tabs, are occasionally used, but commas are the most common.

To use , we need to pass a keyword argument called `delimiter` that indicates what character is the delimiter:

```python
import numpy
nfl = numpy.genfromtxt("nfl.csv", delimiter=",")
```

The above code would read in the `nfl.csv` file into a NumPy *array*. NumPy arrays are represented using the [`numpy.ndarray`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.html) class. We'll refer to `ndarray` objects as NumPy arrays in our material.

### Instructions

- Import NumPy.
- Use the [genfromtxt()](http://docs.scipy.org/doc/numpy-1.10.0/reference/generated/numpy.genfromtxt.html) function to read `world_alcohol.csv`into the `world_alcohol` variable.
- Use the `type()` and `print()` functions to display the type for `world_alcohol`.

```
import numpy
world_alcohol = numpy.genfromtxt("world_alcohol.csv", delimiter=",")
print(type(world_alcohol))
```

## 5: Creating Arrays

We can directly construct arrays from lists using the [numpy.array()](http://docs.scipy.org/doc/numpy-1.10.0/reference/generated/numpy.array.html#numpy.array) function.

The `numpy.array()` function can take a *list* or *list of lists* as input. When we input a list, we get a *one-dimensional array* as a result:

```python
vector = numpy.array([5, 10, 15, 20])
```

When we input a *list of lists*, we get a *matrix* as a result:

```python
matrix = numpy.array([[5, 10, 15], [20, 25, 30], [35, 40, 45]])
```

### Instructions

- Create a vector from the *list* `[10, 20, 30]`.Assign the result to the variable `vector`.
- Create a matrix from the *list of lists* `[[5, 10, 15], [20, 25, 30], [35, 40, 45]]`.Assign the result to the variable `matrix`.

```
vector = numpy.array([10, 20, 30])
matrix = numpy.array([[5, 10, 15], [20, 25, 30], [35, 40, 45]])
```

## 6: Array Shape

*Vectors* have a certain number of elements. The *vector* below has `5`elements:

1986'WesternPacific''VietNam''Wine'0

*Matrices* have a certain number of *rows*, and a certain number of *columns*. The *matrix* below has `5` *columns* and `3`*rows*:

ColumnIndex0123401986'WesternPacific''VietNam''Wine'0RowIndex11986'Americas''Uruguay''Other'0.521985'Africa''Cted'Ivoire''Wine'1.62

It's often useful to know how many elements an *array* contains. We can use the [ndarray.shape](http://docs.scipy.org/doc/numpy-1.10.1/reference/generated/numpy.ndarray.shape.html) property to figure out how many elements are in the *array*.

For vectors, the `shape` property contains a *tuple* with `1` element. A *tuple* is a kind of *list* where the elements can't be changed.

```
vector = numpy.array([1, 2, 3, 4])
print(vector.shape)
```

The code above would result in the *tuple*`(4,)`. This *tuple* indicates that the array `vector` has one dimension, with length `4`, which matches our intuition that `vector` has `4` elements.

For matrices, the `shape` property contains a *tuple* with `2` elements.

```python
matrix = numpy.array([[5, 10, 15], [20, 25, 30]])
print(matrix.shape)
# (2, 3)
```

```R
dim(matrix)
```

The above code will result in the *tuple*`(2,3)` indicating that `matrix` has `2`*rows* and `3` *columns*.

### Instructions

- Assign the shape of `vector` to `vector_shape`.
- Assign the shape of `matrix` to `matrix_shape`.

```
vector = numpy.array([10, 20, 30])
matrix = numpy.array([[5, 10, 15], [20, 25, 30], [35, 40, 45]])
vector_shape = vector.shape
matrix_shape = matrix.shape
```

## 7: Data Types

Each value in a NumPy *array* has to have the same data type. NumPy data types are similar to Python data types, but have slight differences. You can find a full list of NumPy data types [here](http://docs.scipy.org/doc/numpy-1.10.1/user/basics.types.html), but here are some of the common ones:

- `bool` -- Boolean. Can be `True` or `False`.
- `int` -- Integer values. An example is `10`. Can be `int16`, `int32`, or `int64`. The suffix `16`, `32`, or `64`indicates how long the number can be, in *bits* (there are 8 bits in a byte; we'll dive more into bits and bytes later on). The higher the suffix, the larger the integers can be.
- `float` -- Floating point values. An example is `10.6`. Can be `float16`, `float32`, or `float64`. The suffix `16`, `32`, or `64` indicates how many numbers after the decimal point the number can have. The larger the suffix, the more precise the float can be.
- `string` -- String values. Can be `string` or `unicode`. We'll get more into what unicode is later on, but the difference between `string` and `unicode` is how the characters are stored by the computer.

NumPy will automatically figure out an appropriate data type when reading in data or converting *lists* to *arrays*. You can check the data type of a NumPy *array* using the [dtype](http://docs.scipy.org/doc/numpy-1.10.1/user/basics.types.html) property.

```python
numbers = numpy.array([1, 2, 3, 4])
numbers.dtype
# dtype('int64')
```

Because `numbers` only contains integers, its data type is `int64`.

### Instructions

- Assign the data type of `world_alcohol` to the variable `world_alcohol_dtype`.

```
world_alcohol_dtype = world_alcohol.dtype
```

## 8: Inspecting The Data

Here's how the first few rows of `world_alcohol` look:

```
array([[             nan,              nan,              nan,              nan,              nan],
       [  1.98600000e+03,              nan,              nan,              nan,   0.00000000e+00],
       [  1.98600000e+03,              nan,              nan,              nan,   5.00000000e-01]])
```

There are a few concepts we haven't been introduced to yet that we'll get into one by one:

- Many items in `world_alcohol` are `nan`.
- The entire first row is `nan`.
- Some of the numbers are written like `1.98600000e+03`.

The data type of `world_alcohol` is `float`. Because all of the values in a NumPy *array* have to have the same data type, NumPy attempted to convert all of the columns to floats when they were read in. The [numpy.genfromtxt()](http://docs.scipy.org/doc/numpy-1.10.0/reference/generated/numpy.genfromtxt.html) function will attempt to guess the correct data type of the *array* it creates.

In this case, the `WHO Region`, `Country`, and `Beverage Types` columns are actually *strings*, and couldn't be converted to *floats*. When NumPy can't convert a value to a numeric data type like *float* or *integer*, it uses a special `nan` value that stands for *Not a Number*. NumPy assigns an `na` value, which stands for *Not Available*, when the value doesn't exist. `nan` and `na` values are types of missing data. We'll dive more into how to deal with missing data in later missions.

The whole first row of `world_alcohol.csv` is a *header* row that contains the names of each column. This is not actually part of the data, and consists entirely of *strings*. Since the *strings* couldn't be converted to *floats* properly, NumPy uses `nan` values to represent them.

If you haven't seen [scientific notation](https://en.wikipedia.org/wiki/Scientific_notation) before, you might not recognize numbers like `1.98600000e+03`. Scientific notation is a way to condense how very large or very precise numbers are displayed. We can represent `100` in scientific notation as `1e+02`. The `e+02` indicates that we should multiply what comes before it by `10 ^ 2`(`10` to the power `2`, or `10` squared). This results in `1 * 100`, or `100`. Thus, `1.98600000e+03` is actually `1.986 * 10 ^ 3`, or `1986`. `1000000000000000` can be written as `1e+15`.

In this case, `1.98600000e+03` is actually longer than `1986`, but NumPy displays numeric values in scientific notation by default to account for larger or more precise numbers.

## 9: Reading In The Data Properly

Our data wasn't read in properly, which resulted in NumPy trying to convert *strings* to *floats*, and `nan` values. We can fix this by specifying in the [numpy.genfromtxt()](http://docs.scipy.org/doc/numpy-1.10.0/reference/generated/numpy.genfromtxt.html) function that we want to read in all the data as *strings*. While we're at it, we can also specify that we want to skip the header row of `world_alcohol.csv`.

We can do this by:

- Specifying the keyword argument `dtype` when reading in `world_alcohol.csv`, and setting it to `"U75"`. This specifies that we want to read in each value as a `75` byte `unicode` data type. We'll dive more into `unicode` and *bytes* later on, but for now, it's enough to know that this will read in our data properly.
- Specifying the keyword argument `skip_header`, and setting it to `1`. This will skip the first row of `world_alcohol.csv` when reading in the data.

### Instructions

- Use the `numpy.genfromtxt()` function to read in `world_alcohol.csv`:Set the `dtype` parameter to `"U75"`.Set the `skip_header` parameter to `1`.Set the `delimiter` parameter to `,`.
- Assign the result to `world_alcohol`.
- Use the `print()` function to display `world_alcohol`.

```python
world_alcohol = numpy.genfromtxt("world_alcohol.csv", delimiter=",", dtype="U75", skip_header=1)
print(world_alcohol)
#[['1986' 'Western Pacific' 'Viet Nam' 'Wine' '0']
# ['1986' 'Americas' 'Uruguay' 'Other' '0.5']
# ['1985' 'Africa' "Cte d'Ivoire" 'Wine' '1.62']
# ..., 
# ['1986' 'Europe' 'Switzerland' 'Spirits' '2.54']
# ['1987' 'Western Pacific' 'Papua New Guinea' 'Other' '0']
# ['1986' 'Africa' 'Swaziland' 'Other' '5.15']]
```

## 10: Indexing Arrays

We can index NumPy *arrays* very similarly to how we index regular Python *lists*. Here's how we would index a NumPy vector:

```
vector = numpy.array([5, 10, 15, 20])
print(vector[0])
```

The above code would print the first element of `vector`, or `5`.

Indexing *matrices* is similar to indexing *lists of lists*. Here's a refresher on indexing *lists of lists*:

```
list_of_lists = [
        [5, 10, 15], 
        [20, 25, 30]
       ]
```

The first item in *list_of_lists* is `[5, 10, 15]`. If we wanted to access the element `15`, we could do this:

```
first_item = list_of_lists[0]
first_item[2]
```

We could also condense the notation like this:

```
list_of_lists[0][2]
```

We can index *matrices* in a similar way, but we place both *indices* inside square brackets. The first *index* specifies which *row* the data comes from, and the second *index* specifies which *column* the data comes from:

```
matrix = numpy.array([
                        [5, 10, 15], 
                        [20, 25, 30]
                     ])
print(matrix[1,2])
```

In the above code, we pass two *indices* into the square brackets when we *index*`matrix`. This will result in the value in the second row and the third column, or `30`.

### Instructions

- Assign the amount of alcohol Uruguayans drank in other beverages per capita in `1986` to `uruguay_other_1986`. This is the second row and fifth column.
- Assign the country in the third row to `third_country`.`Country` is the third column.

```
uruguay_other_1986 = world_alcohol[1,4]
third_country = world_alcohol[2,2]
```

## 11: Slicing Arrays

We can slice *vectors* very similarly to how we slice *lists*:

```
vector = numpy.array([5, 10, 15, 20])
print(vector[0:3])
```

The above code will print out `5, 10, 15`. Like lists, *vector* slicing is from the first index up to but not including the second index.

*Matrix* slicing is a bit more complex, and has four forms:

- When we want to select one entire dimension, and a single element from the other.
- When we want to select one entire dimension, and a slice of the other.
- When you want to select a slice of one dimension, and a single element from the other.
- When we want to slice both dimensions.

We'll dive into the first form in this screen. When we want to select one whole dimension, and an element from the other, we can do this:

```
matrix = numpy.array([
                    [5, 10, 15], 
                    [20, 25, 30],
                    [35, 40, 45]
                 ])
print(matrix[:,1])
```

This will select all of the rows, but only the column with *index* `1`. So we'll end up with `10, 25, 40`, which is the whole second column.

The colon by itself `:` specifies that the entirety of a single dimension should be selected. Think of the colon as selecting from the first element in a dimension up to and including the last element.

### Instructions

- Assign the whole third column from `world_alcohol` to the variable `countries`.
- Assign the whole fifth column from `world_alcohol` to the variable `alcohol_consumption`.

```
countries = world_alcohol[:,2]
alcohol_consumption = world_alcohol[:,4]
```

## 12: Slicing One Dimension

When we want to select one whole dimension, and a slice of the other, we can do this:

```python
matrix = numpy.array([
                    [5, 10, 15], 
                    [20, 25, 30],
                    [35, 40, 45]
                 ])
print(matrix[:,0:2])
```

This will select all the rows, and columns with *index* `0`, and *index* `1`. We'll end up with:

```
[
    [5, 10],
    [20, 25],
    [35, 40] 
]
```

We can select rows by specifying a colon in the *columns* area:

```
print(matrix[1:3,:])
```

The code above will select rows `1` and `2`, and all of the columns. We'll end up with this:

```
[
    [20, 25, 30],
    [35, 40, 45]
]
```

We can also select a single value alongside an entire dimension:

```
print(matrix[1:3,1])
```

The code above will print rows `1` and `2`and column `1`:

```
[
     [25, 40]
]
```

### Instructions

- Assign all the rows and the first `2` columns of `world_alcohol` to `first_two_columns`.
- Assign the first `10` rows and the first column of `world_alcohol` to `first_ten_years`.
- Assign the first `10` rows and all of the columns of `world_alcohol` to `first_ten_rows`.

```

first_two_columns = world_alcohol[:,0:2]
first_ten_years = world_alcohol[0:10,0]
first_ten_rows = world_alcohol[0:10,:]
```

## 13: Slicing Arrays

When we want to slice both dimensions, we can do this:

```
matrix = numpy.array([
                    [5, 10, 15], 
                    [20, 25, 30],
                    [35, 40, 45]
                 ])
print(matrix[1:3,0:2])
```

This will select rows with *index* `1` and `2`, and columns with *index* `0` and `1`:

```
[
    [20, 25],
    [35, 40]
]
```

### Instructions

- Assign the first `20` rows of the columns at index `1` and `2`of `world_alcohol` to `first_twenty_regions`.

```
first_twenty_regions = world_alcohol[0:20,1:3]
```

## 14: Next Steps

We've learned some of the basics of the NumPy library and how to work with NumPy *arrays*. In the next mission, we'll build on this foundation to determine which country consumes the most alcohol.

# Computation with NumPy

## 1: The Dataset

In the previous mission, we worked with `world_alcohol.csv`, which records per capita alcohol consumption for each country. We'll use the same dataset in this mission, and eventually determine which country consumes the most alcohol per capita.

Here's what the first few rows look like:

| Year | WHO Region       | Country       | Beverage Types | Display Value |
| ---- | ---------------- | ------------- | -------------- | ------------- |
| 1986 | 'WesternPacific' | 'VietNam'     | 'Wine'         | 0             |
| 1986 | 'Americas'       | 'Uruguay'     | 'Other'        | 0.5           |
| 1985 | 'Africa'         | 'Cted'Ivoire' | 'Wine'         | 1.62          |

Each row specifies how many liters of a type of alcohol the average citizen of a particular country drank in a given year. The first row, for example, shows how many liters of wine the typical Vietnamese citizen drank in `1986`.

Here's a description of each column in the dataset:

- `Year` -- The year the data in the row is for
- `WHO Region` -- The region in which the country is located
- `Country` -- The country of the data is for
- `Beverage Types` -- The type of beverage
- `Display Value` -- The average number of liters drunk per capita

## 2: Array Comparisons

One of the most powerful aspects of the NumPy module is the ability to make comparisons across an entire *array*. These comparisons result in Boolean values.

Here's an example of how we can do this with a *vector*:

```python
vector = numpy.array([5, 10, 15, 20])
vector == 10
# array([False,  True, False, False], dtype=bool)
```

If you'll recall from an earlier mission, the double equals sign (`==`) compares two values. When used with NumPy, it will compare the second value to each element in the vector. If the value are equal, the Python interpreter returns `True`; otherwise, it returns `False`. It stores the Boolean results in a new vector.

For example, the code above will generate the *vector* `[False, True, False, False]`, since only the second element in `vector`equals `10`.

Here's an example with a *matrix*:

```
matrix = numpy.array([
                    [5, 10, 15], 
                    [20, 25, 30],
                    [35, 40, 45]
                 ])
matrix == 25
```

The final statement will compare `25` to every element in `matrix`. The result will be a *matrix* where elements are `True` or `False`:

```
[
    [False, False, False], 
    [False, True,  False],
    [False, False, False]
]
```

In the result, `True` only appears in a single position - where the corresponding element in `matrix` is `25`.

Now it's time to practice what we've learned!

### Instructions

The variable `world_alcohol` already contains the data set we're working with.

- Extract the third column in `world_alcohol`, and compare it to the *string* `Canada`. Assign the result to `countries_canada`.
- Extract the first column in `world_alcohol`, and compare it to the *string* `1984`. Assign the result to `years_1984`.

```
years_1984 = (world_alcohol[:,0] == "1984")
countries_canada = (world_alcohol[:,2] == "Canada")
```

## 3: Selecting Elements

We mentioned that comparisons are very powerful, but it may not have been obvious why on the last screen. Comparisons give us the power to select elements in *arrays* using Boolean *vectors*. This allows us to conditionally select certain elements in *vectors*, or certain rows in *matrices*.

Here's an example of how we would do this with a *vector*:

```
vector = numpy.array([5, 10, 15, 20])
equal_to_ten = (vector == 10)

print(vector[equal_to_ten])
```

The code above:

- Creates `vector`.
- Compares `vector` to the value `10`, which generates a Boolean *vector*`[False, True, False, False]`. It assigns the result to `equal_to_ten`.
- Uses `equal_to_ten` to only select elements in `vector` where `equal_to_ten` is `True`. This results in the *vector* `[10]`.

We can use the same principle to select rows in *matrices*:

```
matrix = numpy.array([
                [5, 10, 15], 
                [20, 25, 30],
                [35, 40, 45]
             ])
second_column_25 = (matrix[:,1] == 25)
print(matrix[second_column_25, :])
```

The code above:

- Creates `matrix`.
- Uses `second_column_25` to select any rows in `matrix` where `second_column_25` is `True`.

We end up with this matrix:

```
[
    [20, 25, 30]
]
```

We selected a single row from `matrix`, which was returned in a new *matrix*.

### Instructions

- Compare the third column of `world_alcohol` to the *string*`Algeria`.
- Assign the result to `country_is_algeria`.
- Select only the rows in `world_alcohol` where `country_is_algeria` is `True`.
- Assign the result to `country_algeria`.

```
country_is_algeria = world_alcohol[:,2] == "Algeria"
country_algeria = world_alcohol[country_is_algeria,:]
```

## 4: Comparisons With Multiple Conditions

On the last screen, we made comparisons based on a single condition. We can also perform comparisons with multiple conditions by specifying each one separately, then joining them with an ampersand (`&`). When constructing a comparison with multiple conditions, it's critical to put each one in parentheses.

Here's an example of how we would do this with a *vector*:

```
vector = numpy.array([5, 10, 15, 20])
equal_to_ten_and_five = (vector == 10) & (vector == 5)
```

In the above statement, we have two conditions, `(vector == 10)` and `(vector == 5)`. We use the ampersand (`&`) to indicate that both conditions must be `True` for the final result to be `True`. The statement returns `[False, False, False, False]`, because none of the elements can be `10` and `5` at the same time. Here's a diagram of the comparison logic:

Vector5101520Condition1Condition2Vector==10Vector==5FalseTrueFalseFalseandTrueFalseFalseFalseCombineConditionsFalseFalseFalseFalse

We can also use the pipe symbol (`|`) to specify that either one condition or the other should be `True`:

```
vector = numpy.array([5, 10, 15, 20])
equal_to_ten_or_five = (vector == 10) | (vector == 5)
```

The code above will result in `[True, True, False, False]`.

## 5: Comparisons With Multiple Conditions

### Instructions

- Perform a comparison with multiple conditions, and join the conditions with `&`.Compare the first column of `world_alcohol` to the *string* `1986`.Compare the third column of `world_alcohol` to the *string* `Algeria`.Enclose each condition in parentheses, and join the conditions with `&`.Assign the result to `is_algeria_and_1986`.
- Use `is_algeria_and_1986` to select rows from `world_alcohol`.
- Assign the rows that `is_algeria_and_1986` selects to `rows_with_algeria_and_1986`.

```
is_algeria_and_1986 = (world_alcohol[:,0] == "1986") & (world_alcohol[:,2] == "Algeria")
rows_with_algeria_and_1986 = world_alcohol[is_algeria_and_1986,:]
```

## 6: Replacing Values

We can also use comparisons to replace values in an *array*, based on certain conditions. Here's an example of how we would do this for a *vector*:

```
vector = numpy.array([5, 10, 15, 20])
equal_to_ten_or_five = (vector == 10) | (vector == 5)
vector[equal_to_ten_or_five] = 50
print(vector)
```

This code will complete the following steps:

- Create an array `vector`.
- Compare `vector` to `10` and `5`, and generate a *vector* that's `True` where `vector` is equal to either value.
- Select only the elements in `vector`where `equal_to_ten_or_five` is `True`.
- Replace the selected values with the value `50`.

The result will be `[50, 50, 15, 20]`.

Here's a diagram showing what takes place at each step in the process:

vector5101520equal_to_five_or_TrueTrueFalseFalsetenReplacement50505050Ifequal_to_five_or_tenisTrue,Replacepickthereplacementvalue.Otherwise,pickthevaluefromvector.Final50501520vector

We can perform the same replacement on a matrix. To do this, we'll need to use *indexing* to select a column or row first:

```
matrix = numpy.array([
            [5, 10, 15], 
            [20, 25, 30],
            [35, 40, 45]
         ])
    second_column_25 = matrix[:,1] == 25
    matrix[second_column_25, 1] = 10
```

The code above will result in:

```
[
    [5, 10, 15], 
    [20, 10, 30],
    [35, 40, 45]
 ]
```

### Instructions

- Replace all instances of the *string* `1986` in the first column of `world_alcohol` with the *string* `2014`.
- Replace all instances of the *string* `Wine` in the fourth column of `world_alcohol` with the *string* `Grog`.

```
world_alcohol[:,0][world_alcohol[:,0] == '1986'] = '2014'
world_alcohol[:,3][world_alcohol[:,3] == 'Wine'] = 'Grog'
```

## 7: Replacing Empty Strings

We'll soon be working with the `Display Value` column, which shows how much alcohol the average citizen of a country drinks. However, because `world_alcohol`currently has a `unicode` datatype, all of the values in the column are *strings*. To add these values together or perform any other mathematical operations on them, we'll have to convert the data in the column to *floats*.

Before we can do this, we need to address the empty *string* values (`''`) that appear where there was no original data for the given country and year. If we try to convert the data in the column to *floats* without removing these values first, we'll get a `ValueError`. Thankfully, we can remove these items using the replacement technique we learned on the last screen.

### Instructions

- Compare all the items in the fifth column of `world_alcohol` with an empty *string* `''`. Assign the result to `is_value_empty`.
- Select all the values in the fifth column of `world_alcohol`where `is_value_empty` is `True`, and replace them with the *string* `0`.

```
is_value_empty = world_alcohol[:,4] == ''
world_alcohol[is_value_empty, 4] = '0'
```

## 8: Converting Data Types

We can convert the data type of an *array* with the [astype()](http://docs.scipy.org/doc/numpy-1.10.1/reference/generated/numpy.ndarray.astype.html) method. Here's an example of how this works:

```python
vector = numpy.array(["1", "2", "3"])
vector = vector.astype(float)
```

The code above will convert all of the values in `vector` to *floats*: `[1.0, 2.0, 3.0]`.

We'll do something similar with the fifth column of `world_alcohol`, which contains information on how much alcohol the average citizen of a country drank in a given year. To determine which country drinks the most, we'll have to convert the values in this column to *float* values. That's because we can't add or perform calculations on these values while they're *strings*.

### Instructions

- Extract the fifth column from `world_alcohol`, and assign it to the variable `alcohol_consumption`.
- Use the [astype()](http://docs.scipy.org/doc/numpy-1.10.1/reference/generated/numpy.ndarray.astype.html) method to convert `alcohol_consumption`to the *float* data type.

```
alcohol_consumption = world_alcohol[:,4]
alcohol_consumption = alcohol_consumption.astype(float)
```

## 9: Computing With NumPy

Now that `alcohol_consumption` consists of numeric values, we can perform computations on it. NumPy has a few built-in methods that operate on *arrays*. You can view all of them [in the documentation](http://docs.scipy.org/doc/numpy-1.10.1/index.html). For now, here are a few important ones:

- [sum()](http://docs.scipy.org/doc/numpy-1.10.1/reference/generated/numpy.sum.html) -- Computes the sum of all the elements in a *vector*, or the sum along a dimension in a *matrix*
- [mean()](http://docs.scipy.org/doc/numpy-1.10.0/reference/generated/numpy.mean.html) -- Computes the average of all the elements in a *vector*, or the average along a dimension in a *matrix*
- [max()](http://docs.scipy.org/doc/numpy-1.10.1/reference/generated/numpy.ndarray.max.html) -- Identifies the maximum value among all the elements in a *vector*, or the maximum along a dimension in a *matrix*

Here's an example of how we'd use one of these methods on a *vector*:

```
vector = numpy.array([5, 10, 15, 20])
vector.sum()
```

This would add together all of the elements in `vector`, and result in `50`.

With a matrix, we have to specify an additional keyword argument, `axis`. The `axis` dictates which dimension we perform the operation on. `1` means that we want to perform the operation on each *row*, and `0` means on each *column*. The example below performs an operation across each *row*:

```python
matrix = numpy.array([
                [5, 10, 15], 
                [20, 25, 30],
                [35, 40, 45]
             ])
matrix.sum(axis=1)
```

This would compute the sum for each *row*, resulting in `[30, 75, 120]`.

### Instructions

- Use the [sum()](http://docs.scipy.org/doc/numpy-1.10.1/reference/generated/numpy.sum.html) method to calculate the sum of the values in `alcohol_consumption`. Assign the result to `total_alcohol`.
- Use the [mean()](http://docs.scipy.org/doc/numpy-1.10.1/reference/generated/numpy.sum.html) method to calculate the average of the values in `alcohol_consumption`. Assign the result to `average_alcohol`.

```
total_alcohol = alcohol_consumption.sum()
average_alcohol = alcohol_consumption.mean()
```

## 10: Total Annual Alcohol Consumption

Each country is associated with several rows for different types of beverages:

```
[['1986', 'Americas', 'Canada', 'Other', ''],
   ['1986', 'Americas', 'Canada', 'Spirits', '3.11'],
   ['1986', 'Americas', 'Canada', 'Beer', '4.87'],
   ['1986', 'Americas', 'Canada', 'Wine', '1.33']]
```

To find the total amount the average person in `Canada` drank in `1986`, for example, we'd have to add up all `4` of the rows shown above, then repeat this process for each country.

### Instructions

- Create a *matrix* called `canada_1986` that only contains the rows in `world_alcohol` where the first column is the *string* `1986` and the third column is the *string* `Canada`.
- Extract the fifth column of `canada_1986`, replace any empty strings (`''`) with the *string* `0`, and convert the column to the *float* data type. Assign the result to `canada_alcohol`.
- Compute the sum of `canada_alcohol`. Assign the result to `total_canadian_drinking`.

```
is_canada_1986 = (world_alcohol[:,2] == "Canada") & (world_alcohol[:,0] == '1986')
canada_1986 = world_alcohol[is_canada_1986,:]
canada_alcohol = canada_1986[:,4]
empty_strings = canada_alcohol == ''
canada_alcohol[empty_strings] = "0"
canada_alcohol = canada_alcohol.astype(float)
total_canadian_drinking = canada_alcohol.sum()
```

## 11: Calculating Consumption For Each Country

Now that we know how to calculate the average consumption of all types of alcohol for a single country and year, we can scale up the process and make the same calculation for all countries in a given year. Here's a rough process:

- Create an empty dictionary called `totals`.
- Select only the rows in `world_alcohol` that match a given year. Assign the result to `year`.
- Loop through a list of countries. For each country:
  - Select only the rows from `year`that match the given country.
  - Assign the result to `country_consumption`.
  - Extract the fifth column from `country_consumption`.
  - Replace any empty *string* values in the column with the *string* `0`.
  - Convert the column to the *float* data type.
  - Find the sum of the column.
  - Add the sum to the `totals`dictionary, with the country name as the key.
- After the code executes, you'll have a dictionary containing all of the country names as keys, with the associated alcohol consumption totals as the values.

### Instructions

- We've assigned the list of all countries to the variable `countries`.
- Find the total consumption for each country in `countries`for the year `1989`.Refer to the steps outlined above for help.
- When you're finished, `totals` should contain all of the country names as keys, with the corresponding alcohol consumption totals for `1989` as values.

```
totals = {}
is_year = world_alcohol[:,0] == "1989"
year = world_alcohol[is_year,:]

for country in countries:
    is_country = year[:,2] == country
    country_consumption = year[is_country,:]
    alcohol_column = country_consumption[:,4]
    is_empty = alcohol_column == ''
    alcohol_column[is_empty] = "0"
    alcohol_column = alcohol_column.astype(float)
    totals[country] = alcohol_column.sum()
```

## 12: Finding The Country That Drinks The Most

Now that we've computed total alcohol consumption for each country in `1989`, we can loop through the `totals`dictionary to find the country with the highest value.

The process we've outlined below will help you find the key with the highest value in a dictionary:

- Create a variable called `highest_value` that will keep track of the highest value. Set its value to `0`.
- Create a variable called `highest_key`that will keep track of the key associated with the highest value. Set its value to `None`.
- Loop through each key in the dictionary.
  - If the value associated with the key is greater than `highest_value`, assign the value to `highest_value`, and assign the key to `highest_key`.
- After the code runs, `highest_key` will be the key associated with the highest value in the dictionary.

### Instructions

- Find the country with the highest total alcohol consumption.
- To do this, you'll need to find the key associated with the highest value in the `totals` dictionary.
- Follow the process outlined above to find the highest value in `totals`.
- When you're finished, `highest_value` will contain the highest average alcohol consumption, and `highest_key`will contain the country that had the highest per capital alcohol consumption in `1989`.

```
highest_value = 0
highest_key = None
for country in totals:
    consumption = totals[country]
    if highest_value < consumption:
        highest_value = consumption
        highest_key = country
```

## 13: NumPy Strengths And Weaknesses

You should now have a good foundation in NumPy, and in handling issues with your data. NumPy is much easier to work with than *lists of lists*, because:

- It's easy to perform computations on data.
- Data indexing and slicing is faster and easier.
- We can convert data types quickly.

Overall, NumPy makes working with data in Python much more efficient. It's widely used for this reason, especially for machine learning.

You may have noticed some limitations with NumPy as you worked through the past two missions, though. For example:

- All of the items in an array must have the same data type. For many datasets, this can make arrays cumbersome to work with.
- Columns and rows must be referred to by number, which gets confusing when you go back and forth from column name to column number.

In the next few missions, we'll learn about the **Pandas** library, one of the most popular data analysis libraries. Pandas builds on NumPy, but does a better job addressing the limitations of NumPy.

# Introduction to Pandas

## 1: Introduction To Pandas

Pandas is a library that unifies the most common workflows that data analysts and data scientists previously relied on many different libraries for. Pandas has quickly became an important tool in a data professional's toolbelt and is the most popular library for working with tabular data in Python. Tabular data is any data that can be represented as rows and columns. The CSV files we've worked with in previous missions are all examples of tabular data.

To represent tabular data, pandas uses a custom data structure called a **dataframe**. A dataframe is a highly efficient, 2-dimensional data structure that provides a suite of methods and attributes to quickly explore, analyze, and visualize data. The dataframe is similar to the NumPy 2D array but adds support for many features that help you work with tabular data.

One of the biggest advantages that pandas has over NumPy is the ability to store mixed data types in rows and columns. Many tabular datasets contain a range of data types and pandas dataframes handle mixed data types effortlessly while NumPy doesn't. Pandas dataframes can also handle missing values gracefully using a custom object, `NaN`, to represent those values. A common complaint with NumPy is its lack of an object to represent missing values and people end up having to find and replace these values manually. In addition, pandas dataframes contain axis labels for both rows and columns and enable you to refer to elements in the dataframe more intuitively. Since many tabular datasets contain column titles, this means that dataframes preserve the metadata from the file around the data.

## 2: Introduction To The Data

In this mission, you'll learn the basics of pandas while exploring a dataset from the [United States Department of Agriculture (USDA)](http://www.ars.usda.gov/Services/docs.htm?docid=8964). This dataset contains nutritional information on the most common foods Americans consume. Each column in the dataset shows a different attribute of the foods and each row describes a different food item.

Here are some of the columns in the dataset:

- `NDB_No` - unique id of the food.
- `Shrt_Desc` - name of the food.
- `Water_(g)` - water content in grams.
- `Energ_Kcal` - energy measured in kilo-calories.
- `Protein_(g)` - protein measured in grams.
- `Cholestrl_(mg)` - cholesterol in milligrams.

Here's a preview of the first few rows and columns in the dataset:

| NDB_No | Shrt_Desc                | Water_(g) | Energy_Kcal | Protein_(g) | Lipid_Tot_(g) | Ash_(g) | Carbohydrt_(g) | Fiber_TD_(g) | Sugar_Tot_(g) |
| ------ | ------------------------ | --------- | ----------- | ----------- | ------------- | ------- | -------------- | ------------ | ------------- |
| 1001   | BUTTER WITH SALT         | 15.87     | 717         | 0.85        | 81.11         | 2.11    | 0.06           | 0.0          | 0.06          |
| 1002   | BUTTER WHIPPED WITH SALT | 15.87     | 717         | 0.85        | 81.11         | 2.11    | 0.06           | 0.0          | 0.06          |
| 1003   | BUTTER OIL ANHYDROUS     | 0.24      | 876         | 0.28        | 99.48         | 0.00    | 0.00           | 0.0          | 0.0           |
| 1004   | CHEESE BLUE              | 42.41     | 353         | 21.40       | 28.74         | 5.11    | 2.34           | 0.0          | 0.50          |
| 1005   | CHEESE BRICK             | 41.11     | 371         | 23.24       | 29.68         | 3.18    | 2.79           | 0.0          | 0.51          |

## 3: Read In A CSV File

To use the Pandas library, we need to import it into the environment using the `import` keyword:

```
import pandas
```

We can then refer to the module using `pandas` and use dot notation to call its methods. To read a CSV file into a dataframe, we use the [`pandas.read_csv()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html)function and pass in the file name as a string:

```
# To read in the file `crime_rates.csv` into a dataframe named crime_rates.
crime_rates = pandas.read_csv("crime_rates.csv")
```

You can read more about the parameters the `read_csv()` method takes to customize how a file is read in on the [documentation page](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html).

### Instructions

- Import the pandas library.
- Use the `pandas.read_csv()` function to read the file `"food_info.csv"` into a dataframe named `food_info`.
- Use the `type()` and `print()` functions to display the type of `food_info` to confirm that it's a dataframe object.

```python
import pandas
food_info = pandas.read_csv("food_info.csv")
print(type(food_info))
# <class 'pandas.core.frame.DataFrame'>
```

## 4: Exploring The DataFrame

Now that we've read the dataset into a dataframe, we can start using the dataframe methods to explore the data. To select the first 5 rows of a dataframe, use the dataframe method [`head()`](http://pandas.pydata.org/pandas-docs/version/0.17.1/generated/pandas.DataFrame.head.html). When you call the `head()` method, pandas will return a new dataframe containing just the first 5 rows:

```
first_rows = food_info.head()
```

If you peek at the [documentation](http://pandas.pydata.org/pandas-docs/version/0.17.1/generated/pandas.DataFrame.head.html), you'll notice that you can pass in an integer (`n`) into the `head()` method to display the first `n` rows instead of the first 5:

```python
# First 3 rows.
print(food_info.head(3))
```

Because this dataframe contains many columns and rows, pandas uses ellipsis (`...`) to hide the columns and rows in the middle. Only the first few and the last few columns and rows are displayed to conserve space.

To access the full list of column names, use the `columns` attribute:

```python
column_names = food_info.columns
#Index(['NDB_No', 'Shrt_Desc', 'Water_(g)', 'Energ_Kcal', 'Protein_(g)',
#       'Lipid_Tot_(g)', 'Ash_(g)', 'Carbohydrt_(g)', 'Fiber_TD_(g)',
#       'Sugar_Tot_(g)', 'Calcium_(mg)', 'Iron_(mg)', 'Magnesium_(mg)',
#       'Phosphorus_(mg)', 'Potassium_(mg)', 'Sodium_(mg)', 'Zinc_(mg)',
#       'Copper_(mg)', 'Manganese_(mg)', 'Selenium_(mcg)', 'Vit_C_(mg)',
#       'Thiamin_(mg)', 'Riboflavin_(mg)', 'Niacin_(mg)', 'Vit_B6_(mg)',
#       'Vit_B12_(mcg)', 'Vit_A_IU', 'Vit_A_RAE', 'Vit_E_(mg)', 'Vit_D_mcg',
#       'Vit_D_IU', 'Vit_K_(mcg)', 'FA_Sat_(g)', 'FA_Mono_(g)', 'FA_Poly_(g)',
#       'Cholestrl_(mg)'],
#      dtype='object')
```

Lastly, you can use the `shape` attribute to understand the dimensions of the dataframe. The `shape` attribute returns a tuple of integers representing the number of rows followed by the number of columns:

```
# Returns the tuple (8618,36) and assigns to `dimensions`.
dimensions = food_info.shape
# The number of rows, 8618.
num_rows = dimensions[0]
# The number of columns, 36.
num_cols = dimensions[1]
```

### Instructions

- Select the first 20 rows from `food_info` and assign to the variable `first_twenty`.

```
print(food_info.head(3))
dimensions = food_info.shape
print(dimensions)
num_rows = dimensions[0]
print(num_rows)
num_cols = dimensions[1]
print(num_cols)
first_twenty = food_info.head(20)
```

## 5: Indexing

When you read in a file into a dataframe, pandas uses the values in the first row (also known as the header) for the column labels and the row number for the row labels. Collectively, the labels are referred to as the **index**. dataframes contain both a row index and a column index. Here's a diagram that displays some of the column and row labels for `food_info`:

columnlabels(columnindex)NDB_NoShrt_DescWater_(g)Energy_KcalProtein_(g)0rowlabels1(rowindex)2

The labels allow us to refer to values in the dataframe, which we'll learn more about in the rest of this mission.

## 6: Series

The **Series** object is a core data structure that pandas uses to represent rows and columns. A Series is a labelled collection of values similar to the NumPy vector. The main advantage of Series objects is the ability to utilize non-integer labels. NumPy arrays can only utilize integer labels for indexing.

Pandas utilizes this feature to provide more context when returning a row or a column from a dataframe. For example, when you select a row from a dataframe, instead of just returning the values in that row as a list, pandas returns a Series object that contains the column labels as well as the corresponding values:

| NDB\_No | Shrt\_Desc       | Water\_(g) | Energy\_Kcal | Protein\_(g) | ...  |
| ------- | ---------------- | ---------- | ------------ | ------------ | ---- |
| 1001    | BUTTER WITH SALT | 15.87      | 717          | 0.85         | ...  |

The Series object representing the first row looks like:

NDB\_No	1001

Shrt\_Desc	BUTTER WITH SALT

Water\_(g)	15.87

Energ\_Kcal	717

Protein\_(g)	0.85

...	...

## 7: Selecting A Row

While we use bracket notation to access elements in a NumPy array or a standard list, we need to use the pandas method [`loc[\]`](http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-label) to select rows in a dataframe. The `loc[]` method allows you to select rows by row labels. Recall that when you read a file into a dataframe, pandas uses the row number (or position) as each row's label. Pandas uses zero-indexing, so the first row is at index 0, the second row at index 1, and so on.

If you're interested in accessing a single row, pass in the row label to the `loc[]`method. Python will return an error if you don't pass in a valid row label:

```python
# Series object representing the row at index 0.
food_info.loc[0]

# Series object representing the seventh row.
food_info.loc[6]

# Will throw an error: "KeyError: 'the label [8620] is not in the [index]'"
food_info.loc[8620]
```

When accessing an individual row, pandas returns a Series object containing the column names and that row's value for each column. In the following code cell, we select the first and seventh rows and display them using the `print()` function.

### Instructions

- Assign the 100th row of `food_info` to the variable `hundredth_row`.
- Display `hundredth_row` using the `print()` function.

```
hundredth_row = food_info.loc[99]
print(hundredth_row)
```

## 8: Data Types

When you displayed individual rows, represented as Series objects, you may have noticed the text `"dtype: object"`after the last value. `dtype: object` refers to the data type, or **dtype**, of that Series. The object dtype is equivalent to the string type in Python. Pandas borrows from the NumPy type system and contains the following dtypes:

- `object` - for representing string values.
- `int` - for representing integer values.
- `float` - for representing float values.
- `datetime` - for representing time values.
- `bool` - for representing Boolean values.

When reading a file into a dataframe, pandas analyzes the values and infers each column's types. To access the types for each column, use the `DataFrame.dtypes`attribute to return a Series containing each column name and its corresponding type. Read more about data types on the [Pandas documentation](http://pandas.pydata.org/pandas-docs/stable/basics.html#dtypes).

## 9: Selecting Multiple Rows

If you're interested in accessing multiple rows of the dataframe, you can pass in either a slice of row labels or a list of row labels and pandas will return a dataframe. Note that unlike slicing lists in Python, a slice of a dataframe using `.loc[]` will include both the start and the end row:

```python
# DataFrame containing the rows at index 3, 4, 5, and 6 returned.
food_info.loc[3:6]

# DataFrame containing the rows at index 2, 5, and 10 returned. Either of the following work.
# Method 1
two_five_ten = [2,5,10] 
food_info.loc[two_five_ten]

# Method 2
food_info.loc[[2,5,10]]
```

### Instructions

- Select the last 5 rows of `food_info` and assign to the variable `last_rows`.

```
print("Rows 3, 4, 5 and 6")
print(food_info.loc[3:6])

print("Rows 2, 5, and 10")
two_five_ten = [2,5,10]
print(food_info.loc[two_five_ten])
num_rows = food_info.shape[0]
last_rows = food_info.loc[num_rows-5:num_rows]
```

## 10: Selecting Individual Columns

When accessing a column in a dataframe, pandas returns a Series object containing the row label and each row's value for that column. To access a single column, use bracket notation and pass in the column name as a string:

```python
# Series object representing the "NDB_No" column.
ndb_col = food_info["NDB_No"]

# You can instead access a column by passing in a string variable.
col_name = "NDB_No"
ndb_col = food_info[col_name]
```

### Instructions

- Assign the `"FA_Sat_(g)"` column to the variable `saturated_fat`.
- Assign the `"Cholestrl_(mg)"` column to the variable `cholesterol`.

```
# Series object.
ndb_col = food_info["NDB_No"]
print(ndb_col)

# Display the type of the column to confirm it's a Series object.
print(type(ndb_col))
saturated_fat = food_info["FA_Sat_(g)"]
cholesterol = food_info["Cholestrl_(mg)"]
```

## 11: Selecting Multiple Columns By Name

To select multiple columns, pass in a list of strings representing the column names and pandas will return a dataframe containing only the values in those columns. The following code returns a dataframe containing the `"Zinc_(mg)"`and `"Copper_(mg)"` columns, in that order:

```python
columns = ["Zinc_(mg)", "Copper_(mg)"]
zinc_copper = food_info[columns]

# Skipping the assignment.
zinc_copper = food_info[["Zinc_(mg)", "Copper_(mg)"]]
```

When selecting multiple columns, the order of the columns in the returned dataframe matches the order of the column names in the list of strings that you passed in. This allows you to easily explore specific columns that may not be positioned next to each other in the dataframe.

### Instructions

- Select the `'Selenium_(mcg)'` and `'Thiamin_(mg)'`columns and assign the resulting dataframe to `selenium_thiamin`.

```
zinc_copper = food_info[["Zinc_(mg)", "Copper_(mg)"]]

columns = ["Zinc_(mg)", "Copper_(mg)"]
zinc_copper = food_info[columns]
selenium_thiamin = food_info[['Selenium_(mcg)', 'Thiamin_(mg)']]
```

## 12: Practice

To help solidify the concepts learned in this mission, complete the following exercise.

### Instructions

- Select and display only the columns that use grams for measurement (that end with `"(g)"`). To accomplish this:
  - Use the `columns` attribute to return the column names in `food_info` and convert to a list by calling the method [`tolist()`](http://pandas.pydata.org/pandas-docs/version/0.13.1/generated/pandas.Index.tolist.html)
  - Create a new list, `gram_columns`, containing only the column names that end in `"(g)"`. The string method [`endswith()`](https://docs.python.org/2/library/stdtypes.html#str.endswith) returns `True` if the string object calling the method ends with the string passed into the parentheses.
  - Pass `gram_columns` into bracket notation to select just those columns and assign the resulting dataframe to `gram_df`
  - Then use the dataframe method [`head()`](http://pandas.pydata.org/pandas-docs/version/0.17.1/generated/pandas.DataFrame.head.html) to display the first 3 rows of `gram_df`.

```python
print(food_info.columns)
print(food_info.head(2))
col_names = food_info.columns.tolist() # tolist()
gram_columns = []

for c in col_names:
    if c.endswith("(g)"): # endwith()
        gram_columns.append(c)
gram_df = food_info[gram_columns]
print(gram_df.head(3))
```

## 13: Next Steps

In this mission, we introduced the key data structures in pandas and learned the basics of exploring data. In the next mission, you'll learn how to transform columns, normalize columns, and sort a dataframe.

# Data Manipulation with pandas

## 1: Overview

In the previous mission, we learned how to explore a pandas DataFrame. In this mission, we'll explore how to manipulate a DataFrame and make transformations to it. We'll continue to work with the same data set from the USDA on nutritional information. We'll build a basic nutritional index for people who want to eat high-protein, low-fat foods. The `"Lipid_Tot_(g)"` column contains each food's total fat content, and the `"Protein_(g)"` (in grams) contains each food's total protein content (in grams). Let's use the following formula to score each food in our data set:

Score=2×(Protein_(g))−0.75×(Lipid_Tot_(g))

While this formula is by no means scientific, it will act as a guide as we explore pandas further.

Here's a preview of the data set:

| NDB_No | Shrt_Desc                | Water_(g) | Energy_Kcal | Protein_(g) | Lipid_Tot_(g) | Ash_(g) | Carbohydrt_(g) | Fiber_TD_(g) | Sugar_Tot_(g) |
| ------ | ------------------------ | --------- | ----------- | ----------- | ------------- | ------- | -------------- | ------------ | ------------- |
| 1001   | BUTTER WITH SALT         | 15.87     | 717         | 0.85        | 81.11         | 2.11    | 0.06           | 0.0          | 0.06          |
| 1002   | BUTTER WHIPPED WITH SALT | 15.87     | 717         | 0.85        | 81.11         | 2.11    | 0.06           | 0.0          | 0.06          |
| 1003   | BUTTER OIL ANHYDROUS     | 0.24      | 876         | 0.28        | 99.48         | 0.00    | 0.00           | 0.0          | 0.0           |
| 1004   | CHEESE BLUE              | 42.41     | 353         | 21.40       | 28.74         | 5.11    | 2.34           | 0.0          | 0.50          |
| 1005   | CHEESE BRICK             | 41.11     | 371         | 23.24       | 29.68         | 3.18    | 2.79           | 0.0          | 0.51          |

### Instructions

To practice what we learned in the previous mission:

- Import the pandas library.
- Read `food_info.csv` into a DataFrame object named `food_info`.
- Use the `DataFrame.columns` attribute, followed by the `Index.tolist()` method, to return a *list* containing only the column names.
- Assign the resulting *list* to `col_names`, and use the `print()` function to display the value.
- Display the first three rows of `food_info`.

```
import pandas
food_info = pandas.read_csv("food_info.csv")
col_names = food_info.columns.tolist()
print(col_names)
print(food_info.head(3))
```

## 2: Transforming A Column

We can use the arithmetic operators to transform a numerical column. The values in the `"Iron_(mg)"` column, for example, are currently in milligrams. We can divide each value by `1000` to convert the values to grams. The following code will divide each value in the `"Iron_(mg)"` column by `1000`, and return a new Series object with those values:

```
div_1000 = food_info["Iron_(mg)"] / 1000
```

pandas allows us to use any of the arithmetic operators to scale the values in a numerical column:

```
# Adds 100 to each value in the column and returns a Series object.
add_100 = food_info["Iron_(mg)"] + 100

# Subtracts 100 from each value in the column and returns a Series object.
sub_100 = food_info["Iron_(mg)"] - 100

# Multiplies each value in the column by 2 and returns a Series object.
mult_2 = food_info["Iron_(mg)"]*2
```

### Instructions

- Divide the `"Sodium_(mg)"` column by `1000` to convert the values to grams, and assign the result to `sodium_grams`.
- Multiply the `"Sugar_Tot_(g)"` column by `1000` to convert to milligrams, and assign the result to `sugar_milligrams`.

```
div_1000 = food_info["Iron_(mg)"] / 1000
add_100 = food_info["Iron_(mg)"] + 100
sub_100 = food_info["Iron_(mg)"] - 100
mult_2 = food_info["Iron_(mg)"]*2
sodium_grams = food_info["Sodium_(mg)"] / 1000
sugar_milligrams = food_info["Sugar_Tot_(g)"] * 1000
```

## 3: Performing Math With Multiple Columns

In addition to transforming columns by numerical values, we can transform columns by other columns. When we use an arithmetic operator between two columns (Series objects), pandas will perform that computation in a pair-wise fashion, and return a new Series object. It applies the arithmetic operator to the first value in both columns, the second value in both columns, and so on.

In the following code, we multiply the `"Water_(g)"` column by the `"Energ_Kcal"`column, and assign the resulting Series to `water_energy`:

```
water_energy = food_info["Water_(g)"] * food_info["Energ_Kcal"]
```

The following diagram may help you understand pair-wise computation a bit better:

water_energy=food_info["Water_(g)"]xfood_info["Energ_Kcal"]11378.79=15.87x71711378.79=15.87x717210.24=0.24x87614970.73=42.41x35315251.81=41.11x371.........

### Instructions

- Assign the number of grams of protein per gram of water (`"Protein_(g)"` column divided by `"Water_(g)"` column) to `grams_of_protein_per_gram_of_water`.
- Assign the total amount of calcium and iron (`"Calcium_(mg)"` column plus `"Iron_(mg)"` column) to `milligrams_of_calcium_and_iron`.

```
water_energy = food_info["Water_(g)"] * food_info["Energ_Kcal"]
print(water_energy[0:5])
grams_of_protein_per_gram_of_water = food_info["Protein_(g)"] / food_info["Water_(g)"]
milligrams_of_calcium_and_iron = food_info["Calcium_(mg)"] + food_info["Iron_(mg)"]
```

## 4: Create A Nutritional Index

Now that we've learned how to transform columns with a numerical value and how to combine columns, we can use the following formula to create a nutritional index:

Score=2×(Protein_(g))−0.75×(Lipid_Tot_(g))

### Instructions

- Multiply the `"Protein_(g)"` column by two, and assign the resulting Series to `weighted_protein`.
- Multiply the `"Lipid_Tot_(g)"` column by -0.75, and assign the resulting Series to `weighted_fat`.
- Add both Series objects together and assign the result to `initial_rating`.

```

weighted_protein = food_info["Protein_(g)"] * 2
weighted_fat = -0.75 * food_info["Lipid_Tot_(g)"]
initial_rating = weighted_protein + weighted_fat
```

## 5: Normalizing Columns In A Data Set

The columns in the data set use different units (kilo-calories, milligrams, etc.). As a result, the range of values varies greatly between columns. For example, the `"Vit_A_IU"` column ranges from `0` to `100000`, while the `"Fiber_TD_(g)"`column ranges from `0` to `79`. For certain calculations, columns like `"Vit_A_IU"` can have a greater effect on the result, due to the scale of the values.

While there are many ways to normalize data, one of the simplest ways is to divide all of the values in a column by that column's maximum value. This way, all of the columns will range from `0` to `1`. To calculate the maximum value of a column, we use the [`Series.max()`](http://pandas.pydata.org/pandas-docs/version/0.17.1/generated/pandas.Series.max.html) method. In the following code, we use the `Series.max()`method to calculate the largest value in the `"Energ_Kcal"` column, and assign it to `max_calories`:

```python
# The largest value in the "Energ_Kcal" column.
max_calories = food_info["Energ_Kcal"].max() # max()
```

We can then use the division operator (`/`) to divide the values in the `"Energ_Kcal"` column by the maximum value, `max_calories`:

```
# Divide the values in "Energ_Kcal" by the largest value.
normalized_calories = food_info["Energ_Kcal"] / max_calories
```

### Instructions

- Normalize the values in the `"Protein_(g)"` column, and assign the result to `normalized_protein`.
- Normalize the values in the `"Lipid_Tot_(g)"` column, and assign the result to `normalized_fat`.

```
print(food_info["Protein_(g)"][0:5])
max_protein = food_info["Protein_(g)"].max()
normalized_protein = food_info["Protein_(g)"] / food_info["Protein_(g)"].max()
normalized_fat = food_info["Lipid_Tot_(g)"] / food_info["Lipid_Tot_(g)"].max()
```

## 6: Creating A New Column

So far, we've assigned the Series object that results from a column transform to a variable. However, we can add it to the DataFrame as a new column instead.

We add bracket notation to specify the name we want for that column, then use the assignment operator (`=`) to specify the Series object containing the values we want to assign to that column:

```
iron_grams = food_info["Iron_(mg)"] / 1000  
food_info["Iron_(g)"] = iron_grams
```

The DataFrame `food_info` now includes the `"Iron_(g)"` column, which contains the values from `iron_grams`.

### Instructions

- Assign the normalized `"Protein_(g)"` column to a new column named `"Normalized_Protein"` in `food_info`.
- Assign the normalized `"Lipid_Tot_(g)"` column to a new column named `"Normalized_Fat"` in `food_info`.

```

food_info["Normalized_Protein"] = normalized_protein
food_info["Normalized_Fat"] = normalized_fat
```

## 7: Create A Normalized Nutritional Index

Combining what you've learned so far, you can now create a nutritional index that uses the normalized fat and protein values, instead of the original values.

Here's the formula for reference:

Score=2×(Normalized_Protein)−0.75×(Normalized_Fat)

### Instructions

- Use the `Normalized_Protein` and `Normalized_Fat`columns with the formula above to create the `Norm_Nutr_Index` column.

```
food_info["Normalized_Protein"] = food_info["Protein_(g)"] / food_info["Protein_(g)"].max()
food_info["Normalized_Fat"] = food_info["Lipid_Tot_(g)"] / food_info["Lipid_Tot_(g)"].max()
food_info["Norm_Nutr_Index"] = 2*food_info["Normalized_Protein"] + (-0.75*food_info["Normalized_Fat"])
```

## 8: Sorting A DataFrame By A Column

The DataFrame currently appears in numerical order according to the `NDB_No`column. `NDB_No` is a unique USDA identifier that isn't really useful for our needs. To explore which foods rank the highest in the `Norm_Nutr_Index` column, we need to sort the DataFrame by that column. DataFrame objects have a [`sort_values()` method](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.sort_values.html) that we can use to sort the entire DataFrame.

To sort the DataFrame on the `Sodium_(mg)` column, pass in the column name to the `DataFrame.sort_values()`method, and assign the resulting DataFrame to a new variable:

```
food_info.sort_values("Sodium_(mg)")
```

By default, pandas will sort the data by the column we specify in ascending order and return a new DataFrame, rather than modifying `food_info` itself. To customize the method's behavior, use the parameters listed in [the documentation](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.sort_values.html):

```python
# Sorts the DataFrame in-place, rather than returning a new DataFrame.
food_info.sort_values("Sodium_(mg)", inplace=True) # sort_values

# Sorts by descending order, rather than ascending.
food_info.sort_values("Sodium_(mg)", inplace=True, ascending=False)
```

### Instructions

- Sort the `food_info` DataFrame in-place on the `Norm_Nutr_Index` column in descending order.

```
food_info["Normalized_Protein"] = food_info["Protein_(g)"] / food_info["Protein_(g)"].max()
food_info["Normalized_Fat"] = food_info["Lipid_Tot_(g)"] / food_info["Lipid_Tot_(g)"].max()
food_info["Norm_Nutr_Index"] = 2*food_info["Normalized_Protein"] + (-0.75*food_info["Normalized_Fat"])
food_info.sort_values("Norm_Nutr_Index", inplace=True, ascending=False)
```

## 9: Next Steps

In this mission, we learned how to transform columns, normalize columns, and use the arithmetic operators to create new columns. In the next mission, we'll dive into how to work with missing values, use pivot tables, and calculate summary statistics.

# Working with Missing Data

## 1: Introduction

In this mission, we'll clean and analyze data on passenger survival from the [Titanic](https://en.wikipedia.org/wiki/RMS_Titanic). Each row contains information for a specific Titanic passenger.

Here are the first few rows of the dataset:

|      | pclass | survived | name                                     | sex    | age     | sibsp | parch | ticket | fare     | cabin   | embarked | boat | body | home.dest                         |
| ---- | ------ | -------- | ---------------------------------------- | ------ | ------- | ----- | ----- | ------ | -------- | ------- | -------- | ---- | ---- | --------------------------------- |
| 0    | 1      | 1        | "Allen, Miss. Elisabeth Walton"          | female | 29.0000 | 0     | 0     | 24160  | 211.3375 | B5      | S        | 2    |      | "St Louis, MO"                    |
| 1    | 1      | 1        | "Allison, Master. Hudson Trevor"         | male   | 0.9167  | 1     | 2     | 113781 | 151.5500 | C22 C26 | S        | 11   |      | "Montreal, PQ / Chesterville, ON" |
| 2    | 1      | 0        | "Allison, Miss. Helen Loraine"           | female | 2       | 1     | 2     | 113781 | 151.5500 | C22 C26 | S        |      |      | "Montreal, PQ / Chesterville, ON" |
| 3    | 1      | 0        | "Allison, Mr. Hudson Joshua Creighton"   | male   | 30.0000 | 1     | 2     | 113781 | 151.5500 | C22 C26 | S        |      | 135  | "Montreal, PQ / Chesterville, ON" |
| 4    | 1      | 0        | "Allison, Mrs. Hudson J C (Bessie Waldo Daniels)" | female | 25      | 1     | 2     | 113781 | 151.5500 | C22 C26 | S        |      |      | Montreal, PQ / Chesterville, ON   |

Lets take a closer look at a few of the key columns:

- `pclass` -- The passenger's cabin class from `1` to `3` where `1` was the highest class
- `survived` -- `1` if the passenger survived, and `0` if they did not.
- `sex` -- The passenger's gender
- `age` -- The passenger's age
- `fare` -- The amount the passenger paid for their ticket
- `embarked` -- Either `C`, `Q`, or `S`, to indicate which port the passenger boarded the ship from.

Many of the columns, such as `age` and `sex`, have missing values.

Because missing values can cause errors in numerical functions, we'll need to deal with them before we can analyze the data. For instance, finding the mean of a column with a missing value will fail because it's impossible to average a missing value. Addressing missing values will let us perform calculations on the entire data set.

Lets import the data set into pandas. You may notice at the start of the code, we import pandas differently from how we have previously.

```
import pandas as pd
```

This gives the pandas library the alias `pd`, so that instead of typing `pandas` every time we want to use a function, we can instead type `pd`, for example `pd.read_csv()`.

## 2: Introduction

### Instructions

- Read the file `titanic_survival.csv` into a dataframe called `titanic_survival`.

```
import pandas as pd
titanic_survival = pd.read_csv("titanic_survival.csv")
```

## 3: Finding The Missing Data

Missing data can take a few different forms:

- In Python, the `None` keyword and type indicates no value.
- The Pandas library uses `NaN`, which stands for "not a number", to indicate a missing value.

In general terms, both `NaN` and `None`can be called *null* values.

If we want to see which values are `NaN`, we can use the [`pandas.isnull()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.isnull.html) function which takes a pandas series and returns a series of `True` and `False` values, the same way that NumPy did when we compared arrays.

```python
sex = titanic_survival["sex"]
sex_is_null = pandas.isnull(sex) # pd.isnull
```

We can use this resultant series to select only the rows that have null values.

```
sex_null_true = sex[sex_is_null]
```

We'll use this structure to look at the the null values for the `"age"` column.

### Instructions

- Count how many values in the `"age"` column have null values:Use `pandas.isnull()` on `age` variable to create a Series of `True` and `False` values.Use the resulting series to select only the elements in `age` that are null, and assign the result to `age_null_true`Assign the length of `age_null_true` to `age_null_count`.
- Print `age_null_count` to see how many null values are in the `"age"` column.

```
age = titanic_survival["age"]
print(age.loc[10:20])
age_is_null = pd.isnull(age)
age_null_true = age[age_is_null]
age_null_count = len(age_null_true)
print(age_null_count)
```

## 4: Whats The Big Deal With Missing Data?

So, we know that quite a few values are missing from the `"age"` column, and other columns are missing data too. But why is this a problem?

Lets look at a typical approach to calculate the average for the `"age"` column:

```
mean_age = sum(titanic_survival["age"]) / len(titanic_survival["age"])
```

The result of this is that `mean_age` would be `nan`. This is because any calculations we do with a null value also result in a null value. This makes sense when you think about it -- how can you add a null value to a known value?

Instead, we have to filter out the missing values before we calculate the mean.

### Instructions

- Use `age_is_null` to create a vector that only contains values from the `"age"` column that aren't `NaN`.
- Calculate the mean of the new vector, and assign the result to `correct_mean_age`.

```
age_is_null = pd.isnull(titanic_survival["age"])
good_ages = titanic_survival["age"][age_is_null == False]
correct_mean_age = sum(good_ages) / len(good_ages)
```

## 5: Easier Ways To Do Math

Luckily, missing data is so common that many pandas methods automatically filter for it. For example, if we use use the [`Series.mean()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.mean.html) method to calculate the mean of a column, missing values will not be included in the calculation.

To calculate the mean age that we did earlier, we can replace all of our code with one line

```
correct_mean_age = titanic_survival["age"].mean()
```

Using the built in method is much easier, but it's import to understand what is happening behind the scenes.

### Instructions

- Assign the mean of the `"fare"` column to `correct_mean_fare`.

```
correct_mean_age = titanic_survival["age"].mean()
correct_mean_fare = titanic_survival["fare"].mean()
```

## 6: Calculating Summary Statistics

Let's calculate more summary statistics for the data. The `pclass` column indicates the cabin class for each passenger, which was either first class (`1`), second class (`2`), or third class (`3`). You'll use the list `passenger_classes`, which contains these values, in the following exercise.

### Instructions

- Use a `for` loop to iterate over `passenger_classes`. Within the `for` loop:
    - Select just the rows in `titanic_survival` where the `pclass` value is equivalent to the current iterator value (class).
    - Select just the `fare` column for the current subset of rows.
    - Use the [`Series.mean`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.mean.html) method to calculate the mean of this subset.
    - Add the mean of the class to the `fares_by_class`dictionary with class as the key.
- Once the loop completes, the dictionary `fares_by_class`should have `1`, `2`, and `3` as keys, with the average fares as the corresponding values.

```
passenger_classes = [1, 2, 3]
fares_by_class = {}
for this_class in passenger_classes:
    pclass_rows = titanic_survival[titanic_survival["pclass"] == this_class]
    pclass_fares = pclass_rows["fare"]
    fare_for_class = pclass_fares.mean()
    fares_by_class[this_class] = fare_for_class
```

## 7: Making Pivot Tables

[Pivot tables](https://en.wikipedia.org/wiki/Pivot_table) provide an easy way to subset by one column and then apply a calculation like a sum or a mean. The concept of Pivot tables was popularized with the introduction of the 'PivotTable' feature in Microsoft Excel in the mid 1990's.

Pivot tables first group and then apply a calculation. In the previous screen, we actually made a pivot table manually by grouping by the column `"pclass"` and then calculating the mean of the `"fare"`column for each class.

Luckily, we can use the [Dataframe.pivot_table()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.pivot_table.html) method instead, which simplifies the kind of work we did on the last screen. To produce the same data, we could use one line.

```python
passenger_class_fares = titanic_survival.pivot_table(index="pclass", values="fare", aggfunc=np.mean)
```

The first parameter of the method, `index`tells the method which column to group by. The second parameter `values` is the column that we want to apply the calculation to, and `aggfunc` specifies the calculation we want to perform. The default for the `aggfunc` parameter is actually the mean, so if we're calculating this we can omit this parameter.

### Instructions

- Use the `DataFrame.pivot_table()` method to calculate the mean age for each passenger class (`"pclass"`).
- Assign the result to `passenger_age`.
- Display the `passenger_age` pivot table using the `print()`function.

```
passenger_survival = titanic_survival.pivot_table(index="pclass", values="survived")
passenger_age = titanic_survival.pivot_table(index="pclass", values="age")
print(passenger_age)
```

## 8: More Complex Pivot Tables

We can use the `DataFrame.pivot_table()`method to perform even more advanced tasks. If we pass a list of column names to the `values` parameter instead of a single value, we can perform calculations on multiple columns at once.

We can also specify a custom calculation to be made. For instance, if we pass `np.sum()` to the `aggfunc` parameter it will total the values in each column.

### Instructions

- Make a pivot table that calculates the total fares collected (`"fare"`) and total number of survivors (`"survived"`) for each embarkation port (`"embarked"`).
- Assign the result to `port_stats`.
- Display `port_stats` using the `print()` function.

```
import numpy as np
port_stats = titanic_survival.pivot_table(index="embarked", values=["fare","survived"], aggfunc=np.sum)
print(port_stats)
```

## 9: Drop Missing Values

We learned how to remove the missing values in a vector of data, but how about in a matrix?

We can use the [`DataFrame.dropna()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.dropna.html)method on pandas DataFrames to do this. The method will drop any rows that contain missing values.

The `dropna()` method takes an `axis`parameter, which indicates whether you would like to drop rows or columns. Specifying `axis=0` or `axis='index'` will drop any rows that have null values, while specifying `axis=1` or `axis='columns'` will drop any columns that have null values. We will use `0` and `1` since they're more commonly used, but you can use either.

The code below will drop all rows in `titanic_survival` that have null values.

```python
drop_na_rows = titanic_survival.dropna(axis=0)
```

There is also a parameter that allows you to specify a list of columns or rows to look at when using `dropna()`. You will need to use this in the next exercise - take a look at [the documentation](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.dropna.html) to work out the name of this parameter and how it works.

### Instructions

- Drop all columns in `titanic_survival` that have missing values and assign the result to `drop_na_columns`.
- Drop all rows in `titanic_survival` where the columns `"age"` or `"sex"` have missing values and assign the result to `new_titanic_survival`.

```
drop_na_rows = titanic_survival.dropna(axis=0)
drop_na_columns = titanic_survival.dropna(axis=1)
new_titanic_survival = titanic_survival.dropna(axis=0,subset=["age", "sex"])
```

## 10: Using Iloc To Access Rows By Position

In previous missions, we have used **row labels** to select data in pandas using `Dataframe.loc[]`. These work just like column labels, and can be values like numbers, characters, and strings.

Sometimes your dataset will have row labels that are not numbers, or that are not in order. We have sorted the `new_titanic_survival` dataframe by the `"age"` column from highest to lowest. Here is a preview of the a few of the columns for the first five rows of the data, or the five oldest passengers onboard.

|      | pclass | survived | name                                     | sex    | age  |
| ---- | ------ | -------- | ---------------------------------------- | ------ | ---- |
| 14   | 1.0    | 1.0      | Barkworth, Mr. Algernon Henry Wilson     | male   | 80.0 |
| 61   | 1.0    | 1.0      | Cavendish, Mrs. Tyrell William (Julia Florence... | female | 76.0 |
| 1235 | 3.0    | 0.0      | Svensson, Mr. Johan                      | male   | 74.0 |
| 135  | 1.0    | 0.0      | Goldschmidt, Mr. George B                | male   | 71.0 |
| 9    | 1.0    | 0.0      | Artagaveytia, Mr. Ramon                  | male   | 71.0 |

You can see that the row labels for the first 5 rows are `14`, `61`, `1235`, `135`and `9`. If we wanted to select the first five rows, we can use [`DataFrame.iloc[\]`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.iloc.html)method to select by position. The easy way to remember which is which is to remember that `iloc[]` stands for *integer location*, because you use integers and not labels to select the data.

The following code will select the first 5 rows as shown above:

```python
first_five_rows = new_titanic_survival.iloc[0:5]
```

### Instructions

- Assign the first ten rows from `new_titanic_survival` to `first_ten_rows`.
- Assign the fifth row from `new_titanic_survival` to `row_position_fifth`.
- Assign the row with index label `25` from `new_titanic_survival`to `row_index_25`.

```
# We have already sorted new_titanic_survival by age
first_five_rows = new_titanic_survival.iloc[0:5]
first_ten_rows = new_titanic_survival.iloc[0:10]
row_index_25 = new_titanic_survival.loc[25]
row_position_fifth = new_titanic_survival.iloc[4]
```

## 11: Using Column Indexes

We can also index columns using both the `loc[]` and `iloc[]` methods. With `.loc[]`, we specify the **column label** strings as we have in the earlier exercises in this missions. With `iloc[]`, we simply use the **integer number** of the column, starting from the left-most column which is `0`. Similar to indexing with NumPy arrays, you separate the row and columns with a comma, and can use a colon to specify a range or as a wildcard.

```
first_row_first_column = new_titanic_survival.iloc[0,0]
all_rows_first_three_columns = new_titanic_survival.iloc[:,0:3]
row_index_83_age = new_titanic_survival.loc[83,"age"]
row_index_1000_pclass = new_titanic_survival.loc[766,"pclass"]
```

### Instructions

- Assign the value at row index label `1100`, column index label `"age"` from `new_titanic_survival` to `row_index_1100_age`.
- Assign the value at row index label `25`, column index label `"survived"` from `new_titanic_survival` to`row_index_25_survived`.
- Assign the first 5 rows and first three columns from `new_titanic_survival` to `five_rows_three_cols`.

```
first_row_first_column = new_titanic_survival.iloc[0,0]
all_rows_first_three_columns = new_titanic_survival.iloc[:,0:3]
row__index_83_age = new_titanic_survival.loc[83,"age"]
row_index_1000_pclass = new_titanic_survival.loc[766,"pclass"]
row_index_1100_age = new_titanic_survival.loc[1100, "age"]
row_index_25_survived = new_titanic_survival.loc[25, "survived"]
five_rows_three_cols = new_titanic_survival.iloc[0:5,0:3]
```

## 12: Reindexing Rows

After we sorted `new_titanic_survival` by age, the row indexes were no longer sequential. Each row retained its original index from `titanic_survival`.

Sometimes it's useful to reindex, starting from `0`. We can use the [`DataFrame.reset_index()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.reset_index.html) method to do this. By default, the method retains the old index by adding an extra column to the dataframe with the old index values.

In this exercise, we don't want to retain the index. Check the documentation to see what parameter you need to add so that we don't retain the old index.

### Instructions

- Reindex the `new_titanic_survival` dataframe so the row indexes start from `0`, and the old index is dropped.
- Assign the final result to `titanic_reindexed`.
- Print the first 5 rows and the first 3 columns of `titanic_reindexed`.

```python
titanic_reindexed = new_titanic_survival.reset_index(drop=True)
print(titanic_reindexed.iloc[0:5,0:3])
```

## 13: Apply Functions Over A DataFrame

To perform a complex calculation across pandas objects, we'll need to learn about the [`DataFrame.apply()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.apply.html) method. By default, `DataFrame.apply()` will iterate through each **column** in a DataFrame, and perform on each function. When we create our function, we give it one parameter, `apply()` method passes each column to the parameter as a pandas series.

The result from the function will be combined with all of the other results, and placed into a new series. The function results will have the same position as the column or row we generated them from. Let's look at a simple example:

```python
# This function returns the hundredth item from a series
def hundredth_row(column):
    # Extract the hundredth item
    hundredth_item = column.iloc[99]
    return hundredth_item

# Return the hundredth item from each column
hundredth_row = titanic_survival.apply(hundredth_row)
```

### Instructions

- Write a function that counts the number of null elements in a Series.
- Use the `DataFrame.apply()` method along with your function to run across all the columns in `titanic_survival`.
- Assign the result to `column_null_count`.

```
def hundredth_row(column):
    hundredth_item = column.iloc[99]
    return hundredth_item

hundredth_row = titanic_survival.apply(hundredth_row)
def not_null_count(column):
    column_null = pd.isnull(column)
    null = column[column_null]
    return len(null)

column_null_count = titanic_survival.apply(not_null_count)
```

## 14: Applying A Function To A Row

By passing in the `axis=1` argument, we can use the `DataFrame.apply()` method to iterate over rows instead of columns.

We can use this to calculate some summary information about the ages of the passengers on the Titanic. You will need to use an `if/elif/else` statement in your function. The `elif` statement just means *else if*. Below is an example of how these statements work.

```python
def which_class(row):
    pclass = row['pclass']
    if pd.isnull(pclass):
        return "Unknown"
    elif pclass == 1:
        return "First Class"
    elif pclass == 2:
        return "Second Class"
    else:
        return "Third Class"

classes = titanic_survivors.apply(which_class, axis=1) # to row
```

When the function is called, each test runs until one of the `if`, `elif` or `else`statements is met.

### Instructions

- Create a function that returns the string `"minor"` if someone is under 18, `"adult"` if they are equal to or over 18, and `"unknown"` if their age is null.
- Then, use the function along with `.apply()` to find the correct label for everyone in the `titanic_survival`dataframe.
- Assign the result to `age_labels`.
- You can use `pd.isnull` to check if a value is null or not.

```
def is_minor(row):
    if row["age"] < 18:
        return True
    else:
        return False

minors = titanic_survival.apply(is_minor, axis=1)
import pandas as pd

def generate_age_label(row):
    age = row["age"]
    if pd.isnull(age):
        return "unknown"
    elif age < 18:
        return "minor"
    else:
        return "adult"

age_labels = titanic_survival.apply(generate_age_label, axis=1)
```

## 15: Calculating Survival Percentage By Age Group

Now that we have age labels for everyone, let's make a pivot table to find the probability of survival for each age group.

We have added an `"age_labels"` column to the dataframe containing the `age_labels` variable from the previous step.

### Instructions

- Create a pivot table that calculates the mean survival chance(`"survived"`) for each age group (`"age_labels"`) of the dataframe `titanic_survival`.
- Assign the resulting Series object to `age_group_survival`.

```
age_group_survival = titanic_survival.pivot_table(index="age_labels", values="survived")
```

## 16: Next Steps

In this mission, we learned several strategies for identifying missing data, using integer based selection, and performing calculations to perform analysis in pandas dataframes.

The next mission is a challenge, where we'll practice some of the pandas concepts we've learned so far.

# Challenge: Summarizing Data

## 1: How Challenges Work

At Dataquest, we're huge believers in learning through doing, and we hope this shows in your learning experience. While missions focus on introducing concepts, challenges allow you to perform deliberate practice by completing structured problems. You can read more about deliberate practice [here](http://bit.ly/2cJ2Qt7) and [here](http://nautil.us/issue/35/boundaries/not-all-practice-makes-perfect). Challenges will feel similar to missions, but with little instructional material and a greater focus on exercises.

If you have questions or run into issues, head over to the [Dataquest forums](https://www.dataquest.io/forum/) or our [Slack community](https://www.dataquest.io/chat).

## 2: Introduction To The Data

The American Community Survey is a U.S. Census Bureau survey that collects data on everything from housing affordability to industry employment rates. For this challenge, you'll be using the data that the team at [FiveThirtyEight](http://www.fivethirtyeight.com/) derived from the 2010-2012 American Community Surveys. FiveThirtyEight cleaned the data set and made it available in a [Github repository](https://github.com/fivethirtyeight/data/tree/master/college-majors).

Here's a quick overview of the files we'll be working with:

- `all-ages.csv` - Employment data by major for all ages
- `recent-grads.csv` - Employment data by major for recent college graduates only

Here are descriptions for a few of the columns (out of `21` total columns):

- `Rank` - The major's numerical rank, by post-graduation median earnings
- `Major_code` - The major's numerical code
- `Major` - The major's description
- `Major_category` - The major's category
- `Total` - The total number of people who studied the major
- `Men` - The number of men who studied the major
- `Women` - The number of women who studied the major
- `ShareWomen` - The share of women (from `0` to `1`) who studied the major
- `Employed` - The number of people who studied the major and obtained a job after graduating

Here are the first few rows and columns in `recent-grads.csv`. The data set `all-ages.csv` has the same structure, but with different values for some of the columns:

| Rank | Major_code | Major                                    | Major_category | Total | Sample_size | Men   | Women | ShareWomen | Employed |
| ---- | ---------- | ---------------------------------------- | -------------- | ----- | ----------- | ----- | ----- | ---------- | -------- |
| 1    | 2419       | PETROLEUM ENGINEERING                    | Engineering    | 2339  | 36          | 2057  | 282   | 0.120564   | 1976     |
| 2    | 2416       | MINING AND MINERAL ENGINEERING           | Engineering    | 756   | 7           | 679   | 77    | 0.101852   | 640      |
| 3    | 2415       | METALLURGICAL ENGINEERING                | Engineering    | 856   | 3           | 725   | 131   | 0.153037   | 648      |
| 4    | 2417       | NAVAL ARCHITECTURE AND MARINE ENGINEERING | Engineering    | 1258  | 16          | 1123  | 135   | 0.107313   | 758      |
| 5    | 2405       | CHEMICAL ENGINEERING                     | Engineering    | 32260 | 289         | 21239 | 11021 | 0.341631   | 25694    |

By completing this challenge, you'll test your comfort level with using pandas to manipulate DataFrames and calculate summary statistics. First, we'll need to read the data set into pandas.

## 3: Introduction To The Data

### Instructions

- Read `all-ages.csv` into a DataFrame object, and assign it to `all_ages`.
- Read `recent-grads.csv` into a DataFrame object, and assign it to `recent_grads`.
- Display the first five rows of `all_ages` and `recent_grads`.

```
import pandas as pd
all_ages = pd.read_csv("all-ages.csv")
recent_grads = pd.read_csv("recent-grads.csv")
print(all_ages.head(5))
print(recent_grads.head(5))
```

## 4: Summarizing Major Categories

Both of these data sets group the various majors into categories in the `Major_category` column. Let's start by understanding the number of people in each `Major_category` for both data sets.

To do so, you'll need to:

- Return the unique values in `Major_category`.Use the [Series.unique()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.unique.html) method to return the unique values in a column, like this: `recent_grads['Major_category'].unique()`
- For each unique value:
  - Return all of the rows where `Major_category` equals that unique value.
  - Calculate the total number of students those rows represent (using the `Total` column).Use the `Series.sum()` to calculate the sum of the values in a column.`recent_grads['Total'].sum()`returns the sum of the values in the `Total` column.
  - Keep track of the totals by adding the `Major_category` value and the total number of students to a dictionary.

### Instructions

- Use the `Total` column to calculate the number of people who fall under each `Major_category` in each data set.
    - Store the result as a separate dictionary for each data set.
    - The key for the dictionary should be the `Major_category`, and the value should be the total count.
    - For the counts from `all_ages`, store the results as a dictionary named `aa_cat_counts`.
    - For the counts from `recent_grads`, store the results as a dictionary named `rg_cat_counts`.


```
# Unique values in Major_category column.
print(all_ages['Major_category'].unique())

aa_cat_counts = dict()
rg_cat_counts = dict()
def calculate_major_cat_totals(df):
    cats = df['Major_category'].unique()
    counts_dictionary = dict()

    for c in cats:
        major_df = df[df["Major_category"] == c]
        total = major_df["Total"].sum()
        counts_dictionary[c] = total
    return counts_dictionary

aa_cat_counts = calculate_major_cat_totals(all_ages)
rg_cat_counts = calculate_major_cat_totals(recent_grads)
```

## 5: Low-Wage Job Rates

The [press likes to talk](http://bit.ly/1fNLmaT) about the number of college graduates working low-pay, unskilled jobs because they can't find better ones. As a data person, you should be skeptical of any broad claims, and analyze relevant data to obtain a more nuanced view.

Let's run some basic calculations to explore that idea further.

### Instructions

- Use the `Low_wage_jobs` and `Total` columns to calculate the proportion of recent college graduates that worked low wage jobs.Recall that you can use the [Series.sum()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.sum.html) method to return the sum of the values in a column.
- Store the resulting *float* as `low_wage_percent`, and display the value with the `print()` function.

```
low_wage_percent = 0.0
low_wage_jobs_sum = recent_grads['Low_wage_jobs'].sum()
recent_grads_sum = recent_grads['Total'].sum()
low_wage_percent = low_wage_jobs_sum / recent_grads_sum
print(low_wage_percent)
```

## 6: Comparing Data Sets

It looks like only about 9.85% of graduates took on a low wage job after finishing college.

Both the `all_ages` and `recent_grads`data sets have 173 rows, corresponding to the 173 college major codes. This enables us to do some comparisons between the two data sets, and perform some initial calculations to see how the statistics for recent college graduates compare with those for the entire population.

Next, let's calculate the number of majors where recent graduates did better than the overall population.

### Instructions

- Use a `for` loop to iterate over `majors`.
  - For each major, use Boolean filtering to find the corresponding row in both DataFrames.
  - Compare the values for `Unemployment_rate` to see which DataFrame has a lower value.
  - Increment `rg_lower_count` if the value for `Unemployment_rate` is lower for `recent_grads` than it is for `all_ages`.
- Display `rg_lower_count` with the `print()` function.

```
# All majors, common to both DataFrames
majors = recent_grads['Major'].unique()
rg_lower_count = 0
for m in majors:
    recent_grads_row = recent_grads[recent_grads['Major'] == m]
    all_ages_row = all_ages[all_ages['Major'] == m]
    
    rg_unemp_rate = recent_grads_row.iloc[0]['Unemployment_rate']
    aa_unemp_rate = all_ages_row.iloc[0]['Unemployment_rate']
    
    if rg_unemp_rate < aa_unemp_rate:
        rg_lower_count += 1

print(rg_lower_count)
```

## 7: Next Steps

It appears that less recent graduates who studied 43 of the 173 majors ended up having lower unemployment rates than the general population.

In the next few missions, we'll dive further into the two key data structures in pandas: Series and DataFrame objects.

# Pandas Internals: Series

## 1: Data Structures

Over the next two missions, we'll dive into some of pandas' internals to better understand how it does things under the hood.

The three key data structures in pandas are:

- Series objects (collections of values)
- DataFrames (collections of Series objects)
- Panels (collections of DataFrame objects)

We'll focus on the Series object in this mission.

**Series** objects use NumPy arrays for fast computation, but add valuable features to them for analyzing data. While NumPy arrays use an *integer* index, for example, Series objects can use other index types, such as a *string* index. Series objects also allow for mixed data types, and use the `NaN` Python value for handling missing values.

A Series object can hold many data types, including:

- `float` - For float values
- `int` - For integer values
- `bool` - For Boolean values
- `datetime64[ns]` - For date & time, without time zone
- `datetime64[ns, tz]` - For date & time, with time zone
- `timedelta[ns]` - For representing differences in dates & times (seconds, minutes, etc.)
- `category` - For categorical values
- `object` - For string values

Before we go into further depth, let's introduce the data set we'll be working with. The FiveThirtyEight team recently released a data set containing scores for all movies that have substantive user and critic reviews on IMDB, Rotten Tomatoes, Metacritic, and Fandango. We'll be working with the file `fandango_score_comparison.csv`, which you can download from [their Github repository](https://github.com/fivethirtyeight/data/tree/master/fandango). Here are some of the columns in the data set:

- `FILM` - Film name
- `RottenTomatoes` - Average critic score on Rotten Tomatoes
- `RottenTomatoes_User` - Average user score on Rotten Tomatoes
- `RT_norm` - Average critic score on Rotten Tomatoes (normalized to a 0 to 5-point system)
- `RT_user-norm` - Average user score on Rotten Tomatoes (normalized to a 0 to 5-point system)
- `Metacritic` - Average critic score on Metacritic
- `Metacritic_User` - Average user score on Metacritic

The full list of columns, along with their descriptions, is available on the [Github repository](https://github.com/fivethirtyeight/data/tree/master/fandango).

### Instructions

- Use the `pd.read_csv()` function to read `"fandango_score_comparison.csv"` into a DataFrame object called `fandango`.
- Then, use the `.head()` method to print the first two rows.

```
import pandas as pd
fandango = pd.read_csv('fandango_score_comparison.csv')
fandango.head(2)
```

## 2: Integer Indexes

**DataFrames** use Series objects to represent columns. When we select a single column from a DataFrame, pandas will return the Series object representing that column. By default, pandas indexes each individual Series object in a DataFrame with the integer data type. Each value in the Series has a unique integer index, or position. Like most Python data structures, the Series object uses *0-indexing*. The indexing ranges from `0` to `n-1`, where `n` is the number of rows. We can use an integer index to select an individual value in a Series if we know its position.

With both NumPy arrays and Series objects, we can pass integer indexes into bracket notation to slice and select values. With Series objects, however, we can also specify custom indexes.

To explore this idea further, let's use two Series objects representing the film names and Rotten Tomatoes scores.

### Instructions

- Select the `FILM` column, assign it to the variable `series_film`, and print the first five values.
- Then, select the `RottenTomatoes` column, assign it to the variable `series_rt`, and print the first five values.

```
fandango = pd.read_csv('fandango_score_comparison.csv')
series_film = fandango['FILM']
print(series_film[0:5])
series_rt = fandango['RottenTomatoes']
print(series_rt[0:5])
```

## 3: Custom Indexes

Both of these Series objects use the same integer indexes. This means that the value at index `5`, for example, would describe the same film in both Series objects (`The Water Diviner (2015)`). To look up information about a specific movie, we would need to know its integer index.

If we only had these two Series objects and wanted to look up the Rotten Tomatoes scores for `Minions (2015)` and `Leviathan (2014)`, we'd have to:

- Find the integer index corresponding to `Minions (2015)` in `series_film`
- Look up the value at that integer index from `series_rt`
- Find the integer index corresponding to `Leviathan (2014)` in `series_film`
- Look up the value at that integer index from `series_rt`

This becomes especially cumbersome as we scale up the problem to look for a larger number of movies. What we really want is a way to retrieve the Rotten Tomatoes scores for many movies at the same time with just one command (and one Series object). To accomplish this, we need to move away from using integer indexes, and use string indexes corresponding to the film names instead. Then we can pass in a list of strings matching the film names to retrieve the scores, like so:

```
series_custom[['Minions (2015)', 'Leviathan (2014)']]
```

### Instructions

- Create a new Series object named `series_custom` that has a *string* index (based on the values from `film_names`), and contains all of the Rotten Tomatoes scores from `series_rt`.To create a new Series object:Import `Series` from pandas.Instantiate a new Series object, which takes in a `data` parameter and an `index` parameter. See the [documentation](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.html#pandas.Series) for help.Both of these parameters need to be lists.

```
# Import the Series object from pandas
from pandas import Series

film_names = series_film.values
rt_scores = series_rt.values
series_custom = Series(rt_scores , index=film_names)
series_custom[['Minions (2015)', 'Leviathan (2014)']]
```

## 4: Integer Index Preservation

Even though we specified that the Series object uses a custom string index, the object still has an internal integer index that we can use for selection. When it comes to indexes, Series objects act like both dictionaries and lists. We can access values with our custom index (like the keys in a dictionary), or the integer index (like the index in a list).

### Instructions

- Assign the values in `series_custom` at indexes `5` through `10` to the variable `fiveten`. Then, print `fiveten` to verify that you can still use integer values for selection.

```
series_custom = Series(rt_scores , index=film_names)
series_custom[['Minions (2015)', 'Leviathan (2014)']]
fiveten = series_custom[5:11]
print(fiveten)
```

## 5: Reindexing

Reindexing is the pandas way of modifying the alignment between labels (indexes) and the data (values). The [`reindex()` method](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.Series.reindex.html) allows us to specify a different order for the labels (indexes) in a Series object. This method takes in a list of strings corresponding to the order we'd like for that Series object.

We can use the reindex() method to sort series_custom **alphabetically** by film. To accomplish this, we need to:

- Return a list representation of the current index using [`tolist()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.tolist.html#pandas.Series.tolist).
- Sort the index with [`sorted()`](https://docs.python.org/3/howto/sorting.html#sorting-basics).
- Use [`reindex()`](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.Series.reindex.html) to set the newly-ordered index.

The following code cell contains the logic for accomplishing the first task. We'll leave it up to you to finish the rest.

### Instructions

- The list `original_index` contains the original index. Sort this index using the [Python 3 core method `sorted()`](https://wiki.python.org/moin/HowTo/Sorting), then pass the result in to the Series method `reindex()`.
- Store the result in a variable named `sorted_by_index`.

```
original_index = series_custom.index
sorted_index = sorted(original_index)
sorted_by_index = series_custom.reindex(sorted_index)
```

## 6: Sorting

We just learned how to sort a Series object by the index using the `reindex()` method. This can be cumbersome if we just want to do some quick exploratory data analysis, or reorder by rating instead of film name.

To make sorting easier, pandas comes with a [`sort_index()` method](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.Series.sort_index.html) that sorts a Series by **index**, and a [`sort_values()`method](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.Series.sort_values.html) that sorts a Series by its **values**. Since the values representing the Rotten Tomatoes scores are integers, sorting by values will return the data in numerically ascending order (low to high).

In both cases, pandas preserves the link between each element's index (film name) and value (score). We call this **data alignment**, which is a key tenet of pandas that's incredibly important when analyzing data. Pandas allows us to assume the linking will be preserved, unless we specifically change a value or an index.

### Instructions

- Sort `series_custom` by index using `sort_index()`, and assign the result to the variable `sc2`.
- Sort `series_custom` by values, and assign the result to the variable `sc3`.
- Finally, print the first 10 values in `sc2` and the first 10 values in `sc3`.

```

sc2 = series_custom.sort_index()
sc3 = series_custom.sort_values()
print(sc2[0:10])
print(sc3[0:10])
```

## 7: Transforming Columns With Vectorized Operations

A column is really a vector of values. For this reason, we often want to transform an entire column in a data set. Series objects offer robust support for vectorized operations, which enable us to run computations over an entire column very quickly.

Since pandas builds on NumPy, it takes advantage of NumPy's vectorizaton capabilities. These capabilities generate incredibly optimized, low level code in the **C** programming language to loop over the values. Using a traditional *for* loop would be much slower, especially for large data sets.

We can use any of the standard Python arithmetic operators (`+`, `-`, `*`, and `/`) to transform each of the values in a Series object. If we wanted to transform the Rotten Tomatoes scores from a 100-point scale to a 10-point scale, for example, we could use the Python division operator (`/`) to divide the Series by 10:

```
series_custom/10
```

This will return a new Series object where each value is 1/10 of the original value. We can even use NumPy functions to transform and run calculations over Series objects:

```
# Add each value with each other
np.add(series_custom, series_custom)
# Apply sine function to each value
np.sin(series_custom)
# Return the highest value (will return a single value, not a Series)
np.max(series_custom)
```

The values in a Series object are part of an `ndarray`, the core data type in NumPy. Applying some NumPy functions to a Series object will return a new Series object, while other functions will return a single value. [NumPy's documentation](http://docs.scipy.org/doc/numpy/reference/generated/numpy.sin.html#numpy.sin) gives us a good sense of the return value for each function. If a particular NumPy function usually returns an `ndarray`, it will return a Series object instead when we apply it to a Series.

The original DataFrame contains the column `RT_norm`, which represents a normalized score (from 0 to 5) of the Rotten Tomatoes average critic score. Let's use vectorized operations to normalize `series_custom` back to the 0-5 scale.

### Instructions

- Normalize `series_custom` (which is currently on a 0 to 100-point scale) to a 0 to 5-point scale by dividing each value by 20.
- Assign the new normalized Series object to `series_normalized`.

```
series_normalized = (series_custom/20)
```

## 8: Comparing And Filtering

Pandas uses vectorized operations for many tasks, such as filtering values within a single Series object and comparing two different Series objects. For example, to find all films with an average critic rating of 50 or above on Rotten Tomatoes, running:

```
series_custom > 50
```

will actually return a Series object with a *Boolean* value for each film. That's because pandas applies the filter (`> 50`) to each value in the Series object. To retrieve the actual film names, we need to pass this Boolean series into the original Series object.

```
series_greater_than_50 = series_custom[series_custom > 50]
```

Pandas returns *Boolean Series* objects that serve as intermediate representations of the logic. These objects make it easier to separate complex logic into modular pieces. We can specify filtering criteria in different variables, then chain them together with the `and` operator (`&`) or the `or` operator (`|`). Finally, we can use a Series object's bracket notation to pass in an expression representing a Boolean Series object and get back the filtered data set.

### Instructions

- In the following code cell, the `criteria_one` and `criteria_two` statements return Boolean Series objects.
- Return a filtered Series object named `both_criteria` that only contains the values where both criteria are true. Use bracket notation and the `&` operator to obtain this Series object.

```
criteria_one = series_custom > 50
criteria_two = series_custom < 75
both_criteria = series_custom[criteria_one & criteria_two]
```

## 9: Alignment

One of pandas' core tenets is **data alignment**. Series objects align along indices, and DataFrame objects align along both indices and columns. With Series objects, pandas implicitly preserves the link between the index labels and the values across operations and transformations, unless we explicitly break it. With DataFrame objects, the values link to the index labels and the column labels. Pandas also preserves these links, unless we explicitly break them (by reassigning or editing a column or index label, for example).

This core tenet allows us to use pandas effectively when working with data, and offers a big advantage over using NumPy objects. For Series objects in particular, this means we can use the standard Python arithmetic operators (`+`, `-`, `*`, and `/`) to add, subtract, multiply, and divide the values at each index label for two different Series objects.

Let's use this functionality to calculate the mean ratings from both critics and users on Rotten Tomatoes.

### Instructions

- `rt_critics` and `rt_users` are Series objects containing the average ratings from critics and users for each film.
- Both Series objects use the same custom string index, which they base on the film names. Use the Python arithmetic operators to return a new Series object, `rt_mean`, that contains the mean ratings from both critics and users for each film.

```
rt_critics = Series(fandango['RottenTomatoes'].values, index=fandango['FILM'])
rt_users = Series(fandango['RottenTomatoes_User'].values, index=fandango['FILM'])
rt_mean = (rt_critics + rt_users)/2

print(rt_mean)
```

# Pandas Internals: Dataframes

## 1: Shared Indexes

Dataframe objects can easily query and interact with many columns. They represent each of these columns as a Series object. We discussed how Series objects work in the previous mission. In this mission, we'll learn how dataframes build on Series objects to provide a powerful data analysis toolkit.

Series objects maintain data alignment between values and their index labels. Because dataframes are basically collections of Series objects, they maintain alignment along both columns and rows.

Pandas dataframe share a row index across columns. By default, this is an *integer* index. Pandas enforces this shared row index by throwing an error if we read in a CSV file with columns that contain a different number of elements.

Whenever you call a method that returns or prints a dataframe, the index values (such as a sequence of integers) appear in the leftmost column. You can also use the `index` attribute to access the index values directly. For this mission, we'll continue to work with the data set containing average user and critic ratings from the major film review sites. FiveThirtyEight compiled the data set and made it available in [their Github repository](https://github.com/fivethirtyeight/data/tree/master/fandango).

### Instructions

- Read `fandango_score_comparison.csv` into a dataframe named `fandango`.
- Use the `head` method to return the first two rows in the dataframe, then display them with the `print` function.
- Use the `index` attribute to return the index of the dataframe, and display it with the `print` function.

```
import pandas as pd
fandango = pd.read_csv('fandango_score_comparison.csv')
print(fandango.head(2))
print(fandango.index)
```

## 2: Using Integer Indexes To Select Rows

In the previous cell, we explored the default integer index that pandas uses for the dataframe. With Series, each unique index value refers to a data value. With dataframes, however, each index value refers to an entire row. We can use the integer index to select rows in a few different ways:

```
# First five rows
fandango[0:5]
# From row at 140 and higher
fandango[140:]
# Just row at index 50
fandango.iloc[50]
# Just row at index 45 and 90
fandango.iloc[[45,90]]
```

We use bracket notation to select a slice (continuous sequence) of rows, just as we would for a list. To select an individual row, however, we'll need to use the [`iloc[\]` method](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.DataFrame.iloc.html). This method accepts the following objects for selection:

- An integer
- A list of integers
- A slice object
- A Boolean array

When selecting an individual row, pandas will return a Series object. When selecting multiple rows, it will return a subset of the original dataframe as a new dataframe.

### Instructions

- Return a dataframe containing just the first and last rows, and assign it to `first_last`.

```
fandango = pd.read_csv('fandango_score_comparison.csv')
first_row = 0
last_row = fandango.shape[0] - 1
first_last = fandango.iloc[[first_row, last_row]]
```

## 3: Using Custom Indexes

The dataframe object has a `set_index()`method that allows us to pass in the name of the *column* we want pandas to use as the Dataframe index. By default, pandas will create a new dataframe, index it by the values in the column we specify, then drop that column. The `set_index()`method has a few parameters that allow us to tweak this behavior:

- `inplace`: If set to `True`, this parameter will set the index for the current, "live" dataframe, instead of returning a new dataframe.
- `drop`: If set to `False`, this parameter will keep the column we specified as the index, instead of dropping it.

### Instructions

- Use the pandas dataframe method [`set_index`](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.DataFrame.set_index.html) to assign the `FILM` column as the custom index for the dataframe. Also, specify that we don't want to drop the `FILM` column from the dataframe. We want to keep the original dataframe, so assign the new one to `fandango_films`.
- Display the index for `fandango_films` using the `index`attribute and the `print` function.

```
fandango = pd.read_csv('fandango_score_comparison.csv')
fandango_films = fandango.set_index('FILM', drop=False)
print(fandango_films.index)
```

## 4: Using A Custom Index For Selection

Now that we have a custom index, we can select a row by film name instead of row number (which is the default integer index). We can select rows using the custom index by either:

1. Using the `loc[]` method (the same way we would the `iloc[]` method)
2. Creating a slice using bracket notation

```
# Slice using either bracket notation or loc[]
fandango_films["Avengers: Age of Ultron (2015)":"Hot Tub Time Machine 2 (2015)"]
fandango_films.loc["Avengers: Age of Ultron (2015)":"Hot Tub Time Machine 2 (2015)"]

# Specific movie
fandango_films.loc['Kumiko, The Treasure Hunter (2015)']

# Selecting list of movies
movies = ['Kumiko, The Treasure Hunter (2015)', 'Do You Believe? (2015)', 'Ant-Man (2015)']
fandango_films.loc[movies]
```

When we select multiple rows, pandas returns a dataframe. When we select an individual row, however, it returns a Series object instead. Either way, pandas will maintain the original integer index, even if we specify a custom index. That means we can still select by row number.

### Instructions

- Select the following movies from `fandango_films` (in the order in which they appear), and assign them to `best_movies_ever`:"The Lazarus Effect (2015)""Gett: The Trial of Viviane Amsalem (2015)""Mr. Holmes (2015)"

```

movies = ["The Lazarus Effect (2015)", "Gett: The Trial of Viviane Amsalem (2015)", "Mr. Holmes (2015)"]
best_movies_ever = fandango_films.loc[movies]
```

## 5: Apply() Logic Over The Columns In A Dataframe

Recall that a dataframe object represents both rows and columns as Series objects. The `apply()` method in pandas allows us to specify Python logic that we want to evaluate over the Series objects in a dataframe. Here are some examples of what we can accomplish using the `apply()` method:

- Calculate the standard deviations for each numeric column
- Lowercase all film names in the `FILM`column

The `apply()` method requires us to pass in the vectorized operation we want to apply over each Series object. The method runs over the dataframe's columns by default, but we can use the `axis`parameter to change this (which we'll do later). If the vectorized operation usually returns a single value (such as the NumPy `std()` function), it will return a Series object containing the computed value for each column. If it usually returns a value for each element (such as multiplying or dividing by 2), it will transform all of the values and return them as a dataframe.

In the following code cell, we select only the *float* columns, and assign the dataframe containing them to `float_df`. Then, we pass in the NumPy function `std()` as a lambda function to the dataframe method `apply()` in order to calculate the standard deviation of each column. Under the hood, pandas uses vectorized operations to apply the NumPy function for each iteration of the `apply()`method. It then returns a final Series object containing the standard deviations for each column (i.e., the film ratings).

## 6: Apply() Logic Over Columns: Practice

Recall that the NumPy `std()` method returns a single computed value when we apply it over a Series. In the previous code cell, the `apply()` method returned a single value for each column for this reason.

If we use a NumPy function that returns a value for each element in a Series, we can transform all of the values in each column and return a dataframe with those new values instead. Here's an example:

```
float_df.apply(lambda x: x*2)
```

This will double each of the ratings in the float columns and return a new dataframe, instead of modifying the object in place.

### Instructions

- Use the `apply()` method on `float_df` to halve each value, and assign the result to `halved_df`. Then, print the first row.

```
double_df = float_df.apply(lambda x: x*2)
print(double_df.head(1))
halved_df = float_df.apply(lambda x: x/2)
print(halved_df.head(1))
```

## 7: Apply() Over Dataframe Rows

So far we've used the default behavior of the `apply()` method, which operates over the columns in a Datframe. To apply a function over the rows in a dataframe (which pandas treats as Series objects), we need to set the `axis` parameter to `1`. Applying over the rows allows us to do things like calculate the standard deviation of multiple column values for each movie:

```
rt_mt_user = float_df[['RT_user_norm', 'Metacritic_user_nom']]
rt_mt_user.apply(lambda x: np.std(x), axis=1)
```

The code above filters the dataframe down to the two columns we want. Because `std()` returns a value for each iteration, it then returns a Series object containing the standard deviation of each movie's ratings from `RT_user_norm` and `Metacritic_user_nom`.

### Instructions

- Use the `apply()` method to calculate the average of each movie's values for `RT_user_norm` and `Metacritic_user_nom`, and assign the result to the variable `rt_mt_means`.
- Display the first five values in `rt_mt_means`.

```
rt_mt_user = float_df[['RT_user_norm', 'Metacritic_user_nom']]
rt_mt_deviations = rt_mt_user.apply(lambda x: np.std(x), axis=1)
print(rt_mt_deviations[0:5])
rt_mt_user = float_df[['RT_user_norm', 'Metacritic_user_nom']]
rt_mt_means = rt_mt_user.apply(lambda x: np.mean(x), axis=1)
print(rt_mt_means[0:5])
```



# Project: Python and pandas Installation

## 1: Introduction

So far, you've been learning Python and pandas through our interactive, browser-based environment. In this project, we'll walk you through installing the same tools on your own computer. If you get stuck, here are a few things you can try:

- Search Google and [StackOverflow](https://www.dataquest.io/m/203/www.stackoverflow.com) for the error message you received and debug the issue yourself.
- Post to Dataquest's [Slack community](https://www.dataquest.io/m/203/dataquest.io/chat). Use the **#setup** channel for all installation-related questions.
- Post in [our forum](https://www.dataquest.io/forum/t/discussion-of-the-project-python-and-pandas-installation-mission), and a Dataquest employee or another student will get back to you.

At Dataquest, we teach all of our lessons using Python 3, which is the third major release of the Python programming language. To install Python 3 and the data science libraries we can use with it, we'll use a package manager named [Anaconda](https://www.continuum.io/downloads). Head to the [Anaconda downloads page](https://www.continuum.io/downloads) and scroll down until you find the section for your operating system (Windows or Mac). We won't cover the full installation instructions for Linux because there are many different flavors of it. We will point you to relevant external links along the way, though.

You'll need to choose from a few different options:

- Python 2.x and Python 3.x
- 32 bit and 64 bit
- Command line vs graphical installer (except for Linux)

As of this writing, Python 3.5 is the most recent version of Python 3, and that's the one we'll use. There are options for both 32-bit and 64-bit, which refer to a computer's [instruction set](https://en.wikipedia.org/wiki/Instruction_set). To find out which instruction set your computer has, click on the relevant link below:

- [Windows](https://support.microsoft.com/en-us/kb/827218)
- [Mac](https://support.apple.com/en-us/HT201948)

Download the **graphical installer** that matches the most recent version of Python 3 and your operating system. The graphical installer is similar to other installers you may have used in the past to install your Web browser, a text editor, or even Microsoft Office products. You interact with them visually by pointing and clicking in a graphical user interface (GUI).

If you're not sure which installer to download, select the one for your operating system that matches the following criteria:

- Python 3.x installer
- 64-bit
- Graphical installer (if available); otherwise, use the command line installer

If you're on Mac or Windows, launch the Anaconda graphical installer you just downloaded, accept the default options, and complete the setup process. You can read about the process [at the installation documentation page](https://docs.continuum.io/anaconda/install).

Unfortunately, **Linux** doesn't have a graphical installer for Anaconda. You can only install it using the command line - a user interface for interacting with a computer via text commands. This used to be the only way to interact with computers before the rise of modern monitors. You can find the command line instructions for installing Anaconda on Linux at the [Anaconda installation documentation page](https://docs.continuum.io/anaconda/install).

## 2: Experiment With The Command Line

Today, we almost always use a GUI to interact with applications visually, instead of through text. For many programming tasks, however, you'll need to use the **command line environment**.

For example, to test your Anaconda installation on Windows and Mac or to install Anaconda on Linux, you'll need a basic understanding of this environment. In this project, we'll only cover the concepts required to install the tools you need. We explore the environment in much more depth in our [command line courses](https://www.dataquest.io/subject/the-command-line).

The command line application is called `Terminal` on Linux and Mac; it's called `Command Prompt` on Windows. Launch either `Terminal` or `Command Prompt` on your computer. To test your installation, type `python3` and press Enter. The output should resemble the following:

![Imgur](http://i.imgur.com/w0NCwXz.png)

The `>>>`symbols at the start of the line indicate that you're now in interactive mode. This mode allows you to write a line of Python code and press Enter to see the output. Try it out!

- Type `a = 1`
- Press Enter
- Type `print(a)`
- Press Enter

Once you've experimented with these steps, type `exit()` and press the Enter key to exit interactive mode.

## 3: Test Your Installation With The Conda Command

To verify that the Anaconda package manager has installed properly, type `conda` in your command line environment. You should see output information on the library. If you don't see the expected output, use StackOverflow and our Slack community to get help.

Anaconda comes with several packages for scientific computing. You can see the [full list of libraries here](http://docs.continuum.io/anaconda/pkg-docs). For example, it automatically installs libraries like pandas and Matplotlib. You can verify that you have them by launching interactive mode (using the command `python3`) and importing the libraries.

## 4: Write Python Code With Jupyter Notebook

Jupyter Notebook is an in-browser, notebook-based interface for writing and running Python code. We use it to create our content at Dataquest. Its design also influenced our mission interface.

To launch the notebook, enter the following:

```
jupyter notebook
```

Running this command will automatically send you to the Jupyter Notebook landing page in your default Web browser. In the top right corner, you'll see a text box named `New`. Click on that box, then look for `Python 3`under the `Notebooks` section. Click on `Python 3` to open a new notebook.

Jupyter Notebook used to be called IPython Notebook, so you may see some references to that name in Stack Overflow threads and other Internet resources.

## 5: Next Steps

Now that you've installed Jupyter Notebook and gotten it up and running, write some code in a code cell, then press `Shift + Enter` to see the result. Try going through some of the courses you've already completed on Dataquest, but write code that explores the data sets and concepts further this time. What's wonderful about Jupyter Notebook is how easily you can let your curiosity drive your exploration, and the direct connection you have with your interactive coding environment.

# Guided Project: Analyzing Thanksgiving Dinner

## 1: Introducing Thanksgiving Dinner Data

In this project, you'll be working with Jupyter notebook, and analyzing data on Thanksgiving dinner in the US. By the end, you'll have a notebook that you can add to your portfolio or build on top of on your own. If you need help at any point, you can consult our solution notebook [here](https://github.com/dataquestio/solutions/blob/master/Mission219Solution.ipynb). The dataset came from [FiveThirtyEight](https://www.fivethirtyeight.com/), and can be found [here](https://github.com/fivethirtyeight/data/tree/master/thanksgiving-2015).

The dataset is stored in the `thanksgiving.csv` file. It contains `1058`responses to an online survey about what Americans eat for Thanksgiving dinner. Each survey respondent was asked questions about what they typically eat for Thanksgiving, along with some demographic questions, like their gender, income, and location. This dataset will allow us to discover regional and income-based patterns in what Americans eat for Thanksgiving dinner.

The dataset has `65` columns, and `1058`rows. Most of the column names are questions, and most of the column values are string responses to the questions. Most of the columns are categorical, as a survey respondent had to select one of a few options. For example, one of the first column names is `What is typically the main dish at your Thanksgiving dinner?`. The potential responses are:

- `Turkey`
- `Other (please specify)`
- `Ham/Pork`
- `Tofurkey`
- `Chicken`
- `Roast beef`
- `I don't know`
- `Turducken`

Most of the columns follow the same question/response format as the above. There are also quite a few `NaN` values in the columns, which occurred when a survey respondent didn't fill out a question because they didn't want to, or it didn't apply to them.

Here are the first few rows of the dataset:

|      | RespondentID | Do you celebrate Thanksgiving? | What is typically the main dish at your Thanksgiving dinner? | What is typically the main dish at your Thanksgiving dinner? - Other (please specify) | How is the main dish typically cooked? | How is the main dish typically cooked? - Other (please specify) | What kind of stuffing/dressing do you typically have? | What kind of stuffing/dressing do you typically have? - Other (please specify) | What type of cranberry saucedo you typically have? | What type of cranberry saucedo you typically have? - Other (please specify) | ...  | Have you ever tried to meet up with hometown friends on Thanksgiving night? | Have you ever attended a "Friendsgiving?" | Will you shop any Black Friday sales on Thanksgiving Day? | Do you work in retail? | Will you employer make you work on Black Friday? | How would you describe where you live? | Age     | What is your gender? | How much total combined money did all members of your HOUSEHOLD earn last year? | US Region          |
| ---- | ------------ | ------------------------------ | ---------------------------------------- | ---------------------------------------- | -------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------- | ---------------------------------------- | -------------------------------------- | ------- | -------------------- | ---------------------------------------- | ------------------ |
| 0    | 4337954960   | Yes                            | Turkey                                   | NaN                                      | Baked                                  | NaN                                      | Bread-based                              | NaN                                      | None                                     | NaN                                      | ...  | Yes                                      | No                                       | No                                       | No                     | NaN                                      | Suburban                               | 18 - 29 | Male                 | 75,000 to 99,999                         | Middle Atlantic    |
| 1    | 4337951949   | Yes                            | Turkey                                   | NaN                                      | Baked                                  | NaN                                      | Bread-based                              | NaN                                      | Other (please specify)                   | Homemade cranberry gelatin ring          | ...  | No                                       | No                                       | Yes                                      | No                     | NaN                                      | Rural                                  | 18 - 29 | Female               | 50,000 to 74,999                         | East South Central |
| 2    | 4337935621   | Yes                            | Turkey                                   | NaN                                      | Roasted                                | NaN                                      | Rice-based                               | NaN                                      | Homemade                                 | NaN                                      | ...  | Yes                                      | Yes                                      | Yes                                      | No                     | NaN                                      | Suburban                               | 18 - 29 | Male                 | 0 to 9,999                               | Mountain           |
| 3    | 4337933040   | Yes                            | Turkey                                   | NaN                                      | Baked                                  | NaN                                      | Bread-based                              | NaN                                      | Homemade                                 | NaN                                      | ...  | Yes                                      | No                                       | No                                       | No                     | NaN                                      | Urban                                  | 30 - 44 | Male                 | $200,000 and up                          | Pacific            |
| 4    | 4337931983   | Yes                            | Tofurkey                                 | NaN                                      | Baked                                  | NaN                                      | Bread-based                              | NaN                                      | Canned                                   | NaN                                      | ...  | Yes                                      | No                                       | No                                       | No                     | NaN                                      | Urban                                  | 30 - 44 | Male                 | 100,000 to 124,999                       | Pacific            |

5 rows × 65 columns

We won't enumerate every single column now, but here are descriptions of some of the most important:

- `RespondentID` -- a unique ID of the respondent to the survey.
- `Do you celebrate Thanksgiving?` -- a `Yes`/`No` reponse to the question.
- `How would you describe where you live?` -- responses are `Suburban`, `Urban`, and `Rural`.
- `Age` -- resposes are one of several categories, such as `18-29`, and `30-44`.
- `How much total combined money did all members of your HOUSEHOLD earn last year?` -- one of several categories, such as `$75,000 to $99,999`.

In this project, we'll explore the data, and try to find interesting patterns. Our first step is to read in and display the data.

### Instructions

- Import the [pandas](http://pandas.pydata.org/) package.
- Use the [pandas.read_csv()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html) function to read the `thanksgiving.csv` file in.Make sure to specify the keyword argument `encoding="Latin-1"`, as the CSV file isn't encoded normally.Assign the result to the variable `data`.
- Display the first few rows of `data` to see what the columns and rows look like.
- In a separate notebook cell, display all of the column names to get a sense of what the data consists of.
  - You can use the [pandas.DataFrame.columns](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) property to display the column names.

```
import pandas as pd

data = pd.read_csv("thanksgiving.csv", encoding="Latin-1")
data.head()
data.columns
data["Do you celebrate Thanksgiving?"].value_counts()
```

## 2: Filtering Out Rows From A DataFrame

Because we want to understand what people ate for Thanksgiving, we'll remove any responses from people who don't celebrate it. The column `Do you celebrate Thanksgiving?` contains this information. We only want to keep data for people who answered `Yes` to this questions.

### Instructions

- Use the [pandas.Series.value_counts()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.value_counts.html) method to display counts of how many times each category occurs in the `Do you celebrate Thanksgiving?` column.
- Filter out any rows in `data` where the response to `Do you celebrate Thanksgiving?` is not `Yes`. At the end, all of the values in the `Do you celebrate Thanksgiving?`column of `data` should be `Yes`.

```
data["Do you celebrate Thanksgiving?"].value_counts()
data = data[data["Do you celebrate Thanksgiving?"] == "Yes"]
```

## 3: Using Value_counts To Explore Main Dishes

Let's explore what main dishes people tend to eat during Thanksgiving dinner. We can again use the `value_counts`method to help us with this.

### Instructions

- Use the [pandas.Series.value_counts()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.value_counts.html) method to display counts of how many times each category occurs in the `What is typically the main dish at your Thanksgiving dinner?` column.
- Display the `Do you typically have gravy?` column for any rows from `data` where the `What is typically the main dish at your Thanksgiving dinner?` column equals `Tofurkey`.Create a filter that only selects rows from `data`where `What is typically the main dish at your Thanksgiving dinner?` equals `Tofurkey`.Select the `Do you typically have gravy?` column, and display it.

```
data["What is typically the main dish at your Thanksgiving dinner?"].value_counts()
data[data["What is typically the main dish at your Thanksgiving dinner?"] == "Tofurkey"]["Do you typically have gravy?"]
```

## 4: Figuring Out What Pies People Eat

Now that we've looked into the main dishes, let's explore the dessert dishes. Specifically, we'll look at how many people eat `Apple`, `Pecan`, or `Pumpkin`pie during Thanksgiving dinner. This data is encoded in the following three columns:

- `Which type of pie is typically served at your Thanksgiving dinner? Please select all that apply. - Apple`
- `Which type of pie is typically served at your Thanksgiving dinner? Please select all that apply. - Pumpkin`
- `Which type of pie is typically served at your Thanksgiving dinner? Please select all that apply. - Pecan`

In all three columns, the value is either the name of the pie if the person eats it for Thanksgiving dinner, or null otherwise.

We can find out how many people eat one of these three pies for Thanksgiving dinner by figuring out for how many people all three columns are null.

You may recall from an earlier mission that the [pandas.isnull()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.isnull.html) function will return a Boolean Series indicating whether or not each value in a specified DataFrame or Series is null.

We can also use the `&` operator to combine two Boolean Series into a single one. If both Series contain `True` in a position, the result will be `True`. Otherwise, the result will be `False`. Here's an example:

apple_isnull&pecan_isnullboth_arenullTrueTrueTrueTrueFalseFalseFalseTrueFalseFalseFalseFalse

If we use the `pandas.isnull()` function to check where all three columns are null, then use the `&` operator to join all of the Series, we'll end up with a single Boolean Series. Where that Series contains `False`, the person ate at least one of the types of pies for Thanksgiving dinner. Where it contains `True`, they ate none of the types of pies.

### Instructions

- Generate a Boolean Series indicating where the `Which type of pie is typically served at your Thanksgiving dinner? Please select all that apply. - Apple` column is null. Assign to the `apple_isnull` variable.
- Generate a Boolean Series indicating where the `Which type of pie is typically served at your Thanksgiving dinner? Please select all that apply. - Pumpkin` column is null. Assign to the `pumpkin_isnull` variable.
- Generate a Boolean Series indicating where the `Which type of pie is typically served at your Thanksgiving dinner? Please select all that apply. - Pecan` column is null. Assign to the `pecan_isnull` variable.
- Join all three Series using the `&` operator, and assign the result to `ate_pies`.
- Display the unique values and how many times each occurs in the `ate_pies` column.

```
data["Which type of pie is typically served at your Thanksgiving dinner? Please select all that apply. - Apple"].value_counts()

ate_pies = (pd.isnull(data["Which type of pie is typically served at your Thanksgiving dinner? Please select all that apply. - Apple"])
&
pd.isnull(data["Which type of pie is typically served at your Thanksgiving dinner? Please select all that apply. - Pecan"])
 &
 pd.isnull(data["Which type of pie is typically served at your Thanksgiving dinner? Please select all that apply. - Pumpkin"])
)

ate_pies.value_counts()
```

## 5: Converting Age To Numeric

Let's analyze the `Age` column in more depth. In order to analyze the `Age`column, we'll first need to convert it to numeric values. This will make it simple to figure out things like the average age of survey respondents. The `Age` column contains values that fall into one of a few categories:

- `18 - 29`
- `30 - 44`
- `45 - 59`
- `60+`
- `null`

Because we're missing the exact age value, we won't be able to extract an exact integer value, and we'll instead have to extract the first age value in the strings given.

We can do this by splitting each value on the space character (``), then taking the first item in the resulting list. We'll also have to replace the `+` character to account for `60+`, which follows a different format than the rest.

### Instructions

- Write a function to convert a single string to an appropriate integer value. This will allow us to convert the values in the `Age` column to integers.Use the `isnull()` function to check if the value is null. If it is, return `None`.Split the string on the space character (``), and extract the first item of the resulting list.Replace the `+` character in the result with an empty string to remove it.Use `int()` to convert the result to an integer.Return the result.
- Use the [pandas.Series.apply()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.apply.html) method to apply the function to each value in the `Age` column of `data`.Assign the result to the `int_age` column of `data`.
- Call the [pandas.Series.describe()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.describe.html) method on the `int_age`column of `data`, and display the result.
- In a separate markdown cell, write up your findings.
  - Is there anything that we should be aware of about the results or our methodology?
  - Is this a true depiction of the ages of survey participants?

```
data["Age"].value_counts()
def extract_age(age_str):
    if pd.isnull(age_str):
        return None
    age_str = age_str.split(" ")[0]
    age_str = age_str.replace("+", "")
    return int(age_str)

data["int_age"] = data["Age"].apply(extract_age)
data["int_age"].describe()
```

### Findings

Although we only have a rough approximation of age, and it skews downward because we took the first value in each string (the lower bound), we can see that that age groups of respondents are fairly evenly distributed.

## 6: Converting Income To Numeric

The `How much total combined money did all members of your HOUSEHOLD earn last year?` column is very similar to the `Age`column. It contains categories, but can be converted to numerical values. Here are the unique values in the column:

- `Prefer not to answer`
- `$0 to $9,999`
- `$10,000 to $24,999`
- `$25,000 to $49,999`
- `$50,000 to $74,999`
- `$75,000 to $99,999`
- `$100,000 to $124,999`
- `$125,000 to $149,999`
- `$150,000 to $174,999`
- `$175,000 to $199,999`
- `$200,000 and up`
- `null`

We can convert these values to numeric by again splitting on the space character (``). We'll then have to account for the string `Prefer`. Finally, we'll be able to replace the dollar sign character `$` and the comma `,`, and return the result.

### Instructions

- Write a function to convert a single string to an appropriate integer income value.
  - Use the `isnull()` function to check if the value is null. If it is, return `None`.
  - Split the string on the space character (``), and extract the first item of the resulting list.
  - If the result equals `Prefer`, return `None`.
  - Replace the `$` and `,` characters in the result with empty strings to remove them.
  - Use `int()` to convert the result to an integer.
  - Return the result.
- Use the [pandas.Series.apply()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.apply.html) method to apply the function to each value in the `How much total combined money did all members of your HOUSEHOLD earn last year?`column of `data`.Assign the result to the `int_income` column of `data`.
- Call the [pandas.Series.describe()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.describe.html) method on the `int_income` column of `data`, and display the result.
- In a separate markdown cell, write up your findings.
  - Is there anything that we should be aware of about the results or our methodology?
  - Is this a true depiction of the incomes of survey participants?

```
data["How much total combined money did all members of your HOUSEHOLD earn last year?"].value_counts()
data["How much total combined money did all members of your HOUSEHOLD earn last year?"].value_counts()
```

### Findings

Although we only have a rough approximation of income, and it skews downward because we took the first value in each string (the lower bound), the average income seems to be fairly high, although there is also a large standard deviation.

## 7: Correlating Travel Distance And Income

We can now see how the distance someone travels for Thanksgiving dinner relates to their income level. It's safe to hypothesize that people earning less money could be younger, and would travel to their parent's houses for Thanksgiving. People earning more are more likely to have Thanksgiving at their house as a result.

We can test this by filtering `data` based on `int_income`, and seeing what the values in the `How far will you travel for Thanksgiving?` column are.

### Instructions

- See how far people earning under `150000` will travel.Filter `data`, and only select rows where `int_income`is less than `150000`.Use indexing to select the `How far will you travel for Thanksgiving?` column.Use the `value_counts()` method to count up how many times each value occurs in the column.Display the results.
- See how far people earning over `150000` will travel.Filter `data`, and only select rows where `int_income`is greater than `150000`.Use indexing to select the `How far will you travel for Thanksgiving?` column.Use the `value_counts()` method to count up how many times each value occurs in the column.Display the results
- Write up your findings in a markdown cell.

```
data[data["int_income"] < 50000]["How far will you travel for Thanksgiving?"].value_counts()
data[data["int_income"] > 150000]["How far will you travel for Thanksgiving?"].value_counts()
```

### Findings

It appears that more people with high income have Thanksgiving at home than people with low income. This may be because younger students, who don't have a high income, tend to go home, whereas parents, who have higher incomes, don't.

## 8: Linking Friendship And Age

There are two columns which directly pertain to friendship, `Have you ever tried to meet up with hometown friends on Thanksgiving night?`, and `Have you ever attended a "Friendsgiving?`. In the US, a "Friendsgiving" is when instead of traveling home for the holiday, you celebrate it with friends who live in your area. Both questions seem skewed towards younger people. Let's see if this hypothesis holds up.

In order to see the average ages of people who have done both, we can use a pivot table. As you may recall from an earlier mission, we can generate a pivot table with the [pandas.DataFrame.pivot_table()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.pivot_table.html) method. By calling this method on `data`, and passing in the right keyword arguments, we can generate a table showing the average ages of people who answered `Yes` to both questions, answered `Yes` to one question, and so on.

### Instructions

- Generate a pivot table showing the average age of respondents for each category of `Have you ever tried to meet up with hometown friends on Thanksgiving night?` and `Have you ever attended a "Friendsgiving?`.Call the `pivot_table()` method on data.Pass in `"Have you ever tried to meet up with hometown friends on Thanksgiving night?"` to the `index` keyword argument.Pass in `'Have you ever attended a "Friendsgiving?"'` to the `columns` keyword argument.Pass in `"int_age"` to the `values` keyword argument.Display the results.
- Generate a pivot table showing the average income of respondents for each category of `Have you ever tried to meet up with hometown friends on Thanksgiving night?` and `Have you ever attended a "Friendsgiving?`.
- Write up a markdown cell with your findings.

```

data.pivot_table(
    index="Have you ever tried to meet up with hometown friends on Thanksgiving night?", 
    columns='Have you ever attended a "Friendsgiving?"',
    values="int_age"
)

data.pivot_table(
    index="Have you ever tried to meet up with hometown friends on Thanksgiving night?", 
    columns='Have you ever attended a "Friendsgiving?"',
    values="int_income"
)
```

### Findings

It appears that people who are younger are more likely to attend a Friendsgiving, and try to meet up with friends on Thanksgiving.

## 9: Next Steps

That's it for the guided steps! We recommend exploring the data more on your own.

Here are some potential next steps:

- Figure out the most common dessert people eat.
- Figure out the most common complete meal people eat.
- Identify how many people work on Thanksgiving.
- Find regional patterns in the dinner menus.
- Find age, gender, and income based patterns in dinner menus.

We recommend creating a [Github](https://github.com/) repository and placing this project there. It will help other people, including employers, see your work. As you start to put multiple projects on Github, you'll have the beginnings of a strong portfolio. You're welcome to keep working on the project here, but we recommend downloading it to your computer using the download icon above and working on it there.

