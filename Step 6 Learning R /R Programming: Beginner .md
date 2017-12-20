We can use the `class` function to look up a variable's type.

We can define vectors, or collections of elements, using the `c` function.

The `length`function in R returns the number of elements in a vector.

```R
# Create a simple matrix with three rows and two columns
B <- matrix(c(1,2,3,4,5,6), 3, 2)
```
```R
# Returns a dataframe
salaryFrame <- whiteHouse["Salary"]
# Returns a vector
salaryVector <- whiteHouse[,"Salary"]
```
```R
# Find the index of the person with the lowest salary
# This is where knowing what kind of indexing returns what value comes in handy  
# We need a vector
minimumIndex <- which.min(whiteHouse[,"Salary"])
# Extract the row containing the lowest salary
minimumSalaryRow <- whiteHouse[minimumIndex,]
# Get the name column from that row
lowestEarner <- minimumSalaryRow["Name"]
# Print the name of the White House employee who earned the least
print(lowestEarner)
```



# Working with Dataframes

## 1: Introduction To Functions

In the last mission, we learned that dataframes are a powerful way to represent and work with data.

Before we dive into dataframes more deeply, lets take a look at *functions*. Functions are named chunks of code that take inputs and return outputs.

For example, we used the `sum` function in the last mission. This function adds all of the values in a vector together. `print` is another function that displays a string on the screen.

We use functions to save ourselves the trouble of writing the same code over and over again. Adding up all of the elements in a vector, for example, takes quite a few lines of code. Having a function do it for us means that we can save ourselves some typing.

`sum` and `print` are *built-in* functions; they already exist whenever we start programming in R. We can also define our own functions that perform custom tasks.

We can write functions like this:

```R
add <- function(a, b){
    d <- a + b
    return(d)
}
```

Let's deconstruct this code. We have `add`, which is a variable name. The `<-` denotes assignment, so we're assigning the function to the variable name. `function` is a special keyword that we use to define a function in R.

`a` and `b` are placeholders for the arguments the function will accept as input. We separate them with a comma. A function can have any number of arguments, including zero.

Once we define the arguments, we open the *body* of the function with a curly brace. Inside the function, we write whatever computations we want the function to do. R passes the input arguments into the body of the function, so we can access them however we want there.

We choose to add `a` and `b` together, and assign the result to `d`. Then, we `return` that result. The `return`function returns a value. In this case, it will return the output of the function (the value associated with the variable `d`).

## 2: Calling Functions

We can call functions we've defined the same way we've been calling built-in functions. To call our `add` function, for instance, we would just type `add(1,2)`.

In the function body, the `a` would take on the value `1`, and `b` would take on the value `2`. This is because `1` is the first input to our add function, and `2` is the second input. `a` is the first variable in the function definition, and `b` is the second. Programming languages like R match inputs to variables by position. They assign the first input to the first variable in the function definition, the second input to the second variable, and so on.

### Instructions

- Call the function `add` with the arguments `5` and `10`, and assign the result to `d`.

```R
# Define the add function
add <- function(a, b){
    return(a + b)
}

# Call the add function with the arguments 1 and 2
print(add(1, 2))
d <- add(5, 10)
```

## 3: Defining A Function

Let's define a function that subtracts two numbers.

### Instructions

- Define a function that takes two arguments, `a` and `b`:
  - It should subtract `b` from `a` and return the result.
  - Assign the function to the variable `subtract`.
- Call the function with the arguments `50` and `10`, and assign the result to `d`.

```R
subtract <- function(a, b){
        return(a - b)
    }

d <- subtract(50, 10)
```

## 4: Reading In The Data

In this mission, we'll be using data on UFO sightings. Here's a preview of the data:

```
date sighted,date reported,duration,city,state,geocode score,geocode precision,latitude,longitude
20040616,20040617,1 minute, Willoughby Hills, OH,0.743,zip,25.3166667,85.2833333
20021116,20021120,30 seconds, Halls Gap (near Melbourne) (NSW, Australia),0.768,zip,-37.8139965641595,144.963322877884
19920615,19961203,2-3 mins, River Falls, WI,0.757,zip,39.3666667,22.9458333
```

Each row corresponds to a report of a UFO sighting. There are 10,000 rows, counting the header. We randomly sampled them from a larger data set.

Here are descriptions for some of the columns:

- `date sighted` - The date of the sighting
- `date reported` - The date the sighting was reported
- `duration` - The duration of the sighting
- `city` - The city where the sighting occurred
- `state` - The state where the sighting occurred
- `latitude` - The latitude of the sighting
- `longitude` - The longitude of the sighting

### Instructions

- Read in `ufo_sightings.csv` and assign the resulting dataframe to the variable `ufos`.

```R
ufos <- read.csv("ufo_sightings.csv")
```

## 5: Previewing The Dataframe With Head And Tail

If we want to take a quick peek at a dataframe, we can use the `head` function to see the beginning, and the `tail`function to see the end.

Both of these functions take two arguments: the dataframe we want to explore, and the number of rows we want to see, in that order. `head` will display the number of rows we specify at the beginning of the dataframe, and `tail`will display that number of rows at the end.

### Instructions

- Print the last five rows of `ufos` using the `tail` function.

```R
# Print the first five rows in the dataframe
print(head(ufos, 5))
print(tail(ufos, 5))
```

## 6: Calculating UFO Sightings Per Year

When we looked at the data, you may have noticed that the `date.sighted` column contains the year, month, and day the sighting occurred. We can use this information to figure out how many UFO sightings occured during each year.

Unfortunately, the data is a bit hard to work with because it combines the year, month, and day in a single field.

To process this field, we'll first need to figure out what type it has. We'll need to process it differently if it's a number, rather than a character.

We can **look up the types for all of the columns in a dataframe** with the `str`function. This function will convert any R object into a human-readable form, and generate information about it. For a dataframe, it will display each column, as well as the columns' types.

### Instructions

- Use the `str` function on the `ufos` dataframe.
- Use the `print` function to display `ufos`.

```R
print(str(ufos))
```

## 7: Converting A Vector's Type

The results from the `str` function indicate that the column is an *int*, which is short for *integer*. While an integer is *numeric*, R stores it a bit differently on the backend. It's generally safe to treat *integers* and *numerics* the same way, so we won't get too much into the distinction right now. You can read more about integers in the [integer vectors section of the R documentation](https://stat.ethz.ch/R-manual/R-devel/library/base/html/integer.html).

At the moment, we're only interested in the year, not the month and day. It would be difficult to extract the year from the full date, though, because the column type is numeric. We'd need to subtract the month and day, but this operation doesn't make much logical sense. An easier way to do the same thing would be to convert the date sighted to a character type, and then extract the first four characters (which represent the year).

We can use functions to convert between types. In this case, we need the date sighted to be a *character* vector so that we can process it more easily. We can do this conversion with the `as.character`function.

Going forward, we'll use a slightly different indexing notation sometimes. For example, `ufos$date.sighted` will return the `date.sighted` column of `ufos`. This is equivalent to `ufos[,"date.sighted"]`, which will return a vector.

### Instructions

- Convert the `date.sighted` column of `ufos` to the `character` type, and assign the result to `dateSighted`.

```R
dateReported <- as.character(ufos$date.reported)
dateSighted <- as.character(ufos$date.sighted)
```

## 8: Extracting Part Of A Character With The Substring Function

We want to extract the year from our dates, which have the format `20060620`. In this example, we just want the `2006`. To achieve this result, we need to **extract just a few letters from our character type**.

In this case, we can use the `substr`(substring) function. *strings* are just another term for character types. It's very common for people to refer to them as strings.

The `substr` function takes three arguments: the character we want to extract "letters" from, the index we want to start from, and the index we want to end at (we call each piece of information within a character or string variable a "letter.")


```R
date <- "20040415"
substr(date, 1, 4)
```

This code will start at the first letter in `date`, and go up to the fourth letter. It will pull out all the letters in between them, inclusive of the ends. In the example above, the function will extract `2`, `0`, `0`, and `4` (`2004`). In other words, it will retrieve the year for us.

Recall that single values in R are vectors by default. That means that our `substr`function is already working on a vector. We can use `substr` on a single value, or our entire `dateSighted` vector. Here's another example:

```R
dates <- c("20040415", "20080515")
substr(dates, 1, 4)
```

`substr` is actually working on each element within `dates`. It performs the exact same operation each time, and extracts the first four letters. Our result will be `c("2004", "2008")`.

### Instructions

- Extract the year from each element in `dateSighted`, and assign the result to `years`.

```R
years <- substr(dateSighted, 1, 4)
```

## 9: Generating A Table Of Sightings Per Year

We now have a vector of years that looks like this:

```
[1] "2004" "2002" "1992" "2005" "2006" "2005" "2004" "1999" "2007" "2008"
   [11] "2008" "1998" "2002" "2004" "2007" "2009" "2007" "2004" "2000" "2005"
```

While we've truncated the data, we have `10000` years, which matches the total number of UFO sightings in the data set.

We want to calculate how many UFO sightings occurred during each year. To do this, we can use R's `table` function. This function **goes through the vector, finds the unique values, and then calculates how many times each unique value occurs**. The `table` function will do exactly what we want - give us a count of how many sightings occured in each year.

### Instructions

- Create a table of the `years` variable and print it out.

```R
# Create a small vector containing a few years
selectedYears <- c("2004", "2002", "1992", "2005", "2006", "2005", "2004")

# Create and print a table
print(table(selectedYears))
print(table(years))
```

## 10: Working With Dates

Let's try to find the difference between the date someone sighted a UFO and the date they reported it. To do this, we'll first have to convert `date.sighted` and `date.reported` to a different data type - specifically, the date type.

If we left the dates as numerics and subtracted them, we'd end up with nonsensical results. `20150515 - 20150510`would give us `5`, which makes sense, because the dates are five days apart. However, `20150515 - 20150415` would give us `100`, which doesn't make sense because the dates aren't `100` days apart.

First, we'll convert the columns to strings. Then we'll convert the strings to dates using the `as.Date` function.

We can store dates as strings in many different ways, such as:

- `2015/04/15`
- `04/15/2015`
- `20150415`

All of these strings represent the same date, because humans write dates in many different ways. Date formatting solves this problem. Date formatting enables us to specify how our date should look. Once we apply date formatting, R will be able to identify the year, month, and day and convert the string to a date type properly.

If we wanted to convert `04/15/2015` to a date type, for example, we would run `as.Date("04/15/2015", "%m/%d/%Y")`. This tells R that the string contains:

- The month first (`04`)
- Then a forward slash (`/`)
- Then the day (`15`)
- Another forward slash
- Then the full, 4-digit year (`2015`)

If we wanted to convert `2015-04-15` to a date, we would use `as.Date("2015-04-15", "%Y-%m-%d")`. This tells R that:

- The four-digit year comes first
- Then a dash (`-`)
- Then the month
- Another dash
- Then the day

How does it know this?

- `%Y` means 4-digit year
- `%m` means numeric month
- `%d` means numeric day

We call these conversion specifications. They indicate how to convert a string into a date. There are other conversion specifications. For example, `%y` means two-digit year. You can find a full list in the [R documentation](https://stat.ethz.ch/R-manual/R-devel/library/base/html/strptime.html).

For now, let's convert `date.sighted` and `date.reported`.

### Instructions

- Convert the `date.reported` column in `ufos` to a date type, and assign the result to `dateReported`.

```R
dateSighted <- as.character(ufos$date.sighted)
dateSighted <- as.Date(dateSighted, "%Y%m%d")
dateReported <- as.character(ufos$date.reported)
dateReported <- as.Date(dateReported, "%Y%m%d")
```

## 11: Subtracting Vectors

Because R works with vectors by default, mathematical operations automatically work on vectors.

If we have two vectors, `a <- c(3,4,5)`and `b <- c(3,2,1)`, writing `a-b` will subtract the first element in `b` from the first element in `a`, the second element in `b` from the second element in `a`, and so on. We'll end up with a vector that looks like this:

`c(0,2,4)`

Any mathematical operation on a vector will behave the same way. It will perform the operation on the vector elements at each position, and create a new vector with the result.

One major caveat is that it only works this way with vectors of the same length. If the vectors have different lengths, R does something called *recycling*.

Let's say we have these two vectors: `a <- c(3,4,5)` and `b <- 1`. If we write `a-b`, R will recycle the elements in `b`, because `a` is longer. So R will subtract the first element in `b` from the first element in `a`, the first element in `b` from the second element in `a`, and so on. We'll end up with `c(2,3,4)`, because R will subtract one from every element in `a`.

Recycling gets really tricky when one of the vectors has multiple elements, but fewer elements than the other vector has. Let's say we have these two vectors: `a <- c(3,4,5,6,7)` and `b <- c(1,2)`. If we write `a-b`:

1. R will subtract the first element in `b`from the first element in `a`.
2. Then, it will subtract the second element in `b` from the second element in `a`.
3. We run out of elements in `b`, so we go back to the beginning. R will subtract the first element in `b` from the third element in `a`, and the second element in `b` from the fourth element in `a`.
4. We run out of elements in `b` again, so we go back to the beginning again. R subtracts the first element in `b`from the fifth element in `a`.
5. We end up with `c(2,2,4,4,6)`.

R will recycle the elements in the shorter vector as many times as necessary to perform the operation on all of the elements in the longer vector. This can be extremely confusing and lead to unexpected behavior, so be careful about vector lengths when doing operations on them.

### Instructions

- Subtract `dateSighted` from `dateReported`, and assign the result to the variable `delay`.

```R
delay <- dateReported - dateSighted
```

## 12: Making A Table Of Reporting Delays

Subtracting two date vectors gives us the difference between the dates in days. We can generate a table summarizing the results.

### Instructions

- Create a summary table of the `delay` vector.
- Be sure to `print` the table.

```R
print(table(delay))
```

## 13: Cleaning And Combining The Data

The table we just made is extremely messy; there are some values that are greater than `10` years. There are also some negative values.

We'll need to perform a few different steps to clean up the data and generate a good table. First, let's get rid of the negative values. These occur when `dateReported` is less than `dateSighted`. This obviously makes no sense, because you can't tell someone about something you haven't seen yet.

Before we do this, we'll combine `dateReported` and `dateSighted` into one dataframe to make them easier to work with.

We can do this with the `data.frame`function. We just need to pass in all of the vectors we want to have in our dataframe, and the function will create a new dataframe in which **each of those vectors is a column**.

### Instructions

- Use the `data.frame` function to create a dataframe with `dateReported` and `dateSighted` as columns (in that order), and assign the result to `dates`.

```R
dates <- data.frame(dateReported, dateSighted)
```

## 14: Introduction To Boolean Vectors

One of the nice things about R is that we can easily filter dataframes and vectors. This filtering is done with Booleans.

Booleans are a special data type that can only have the value `TRUE` or `FALSE`. We generate Booleans with "truth statements" that assert whether something is TRUE or FALSE. We can generate Booleans by using truth statements that compare equality. For example, the statement `5 == 10` will return `FALSE`, because `5` and `10` aren't equal.

There are a few ways to compare equality in R, but the double equals sign (`==`) is the first one we'll look at. This operator checks whether two values are identical. It returns `TRUE` when they are, and `FALSE`when they aren't. There are some caveats regarding equality that we'll get into later. You can also read about them in the [R documentation](https://stat.ethz.ch/R-manual/R-devel/library/base/html/identical.html) if you'd like to learn more about them now.

If you compare equality across a vector, `==` will return a vector of Boolean values. For example, we can do this:

```R
a <- c(1,2,3)
b <- c(5,2,5)
a == b
```

This will return the vector `c(FALSE, TRUE, FALSE)`. It will compare the first elements in each array, then the second elements, then the third. The same rules about vector recycling we mentioned before also apply to the double equals statement.

In addition to equality, we can make other kinds of comparisons. For instance, we can use the less than sign (`<`) to check whether one value is less than another. Here's an example:

```R
a <- c(1,2,3)
b <- c(5,2,5)
a < b
```

This code will compare the first element in `a` to the first element in `b`, and check whether the element in `a` is smaller. It will then do the same comparison for the other elements in `a` and `b`. The result will be `c(TRUE, FALSE, TRUE)`.

We can do the same thing with the greater than sign (`>`).

### Instructions

- Generate a Boolean vector that indicated when `delay` is greater than `0`, and assign the result to `positiveDelays`.

```R
a <- c(1,2,3)
b <- c(5,2,5)

# Find when each element in a is greater than the corresponding element in b
print(a > b)
positiveDelays <- delay > 0
```

## 15: Filtering With Boolean Vectors

Now that we have a Boolean vector, we can use it to filter a dataframe or "regular" vector. Filtering will keep a row element if the Boolean vector at the same position is `TRUE`. It will discard a row element if the Boolean vector at the same position is `FALSE`. Here's an example:

```R
filter <- c(TRUE, FALSE, TRUE, FALSE)
bestPlanets <- c("Earth", "Mars", "Jupiter", "Venus")
bestPlanets[filter]
```

This filter will select only those elements in `bestPlanets` where `filter` is `TRUE`. We'll end up with `c("Earth", "Jupiter")`.

Here's another example:

```R
filter <- c(FALSE, FALSE, TRUE, TRUE)
bestIceCreamFlavors <- c("Peanut Butter Oreo", "Cookie Dough", "Mint Chocolate Chip", "Peanut Butter Cup")
bestIceCreamFlavors[filter]
```

This code will result in `c("Mint Chocolate Chip", "Peanut Butter Cup")`.

To do the same thing with a dataframe, we also have to specify which columns we want:

```R
filter <- c(FALSE, FALSE, TRUE, TRUE)
bestIceCreamFlavors <- data.frame(c("Peanut Butter Oreo", "Cookie Dough", "Mint Chocolate Chip", "Peanut Butter Cup"))
bestIceCreamFlavors[filter,]
```

This code will select all of the columns in `bestIceCreamFlavors`, but only the last two rows.

### Instructions

- Use the `positiveDelays` vector to filter `dates`, and assign the result to `positiveDates`.

```R
positiveDates <- dates[positiveDelays,]
```

## 16: Removing Null Values

Some of the rows in our original data didn't have a `date.reported` or `date.sighted`. In cases like this, R assigns the symbol `NA` to indicate that the value isn't available. We call this *missing data*. One way to deal with missing data is to remove rows that have empty columns.

We can do this with the `na.omit`function. This function will remove a row from a dataframe if it contains an `NA`value. This will make our results more useful.

`na.omit` requires a single argument, which is the dataframe we want to remove the `NA` values from. It returns a new dataframe without the rows that have missing data.

### Instructions

- Use the `na.omit` function to remove the missing data from `positiveDates`, and assign the result to `cleanDates`.

```R
cleanDates <- na.omit(positiveDates)
```

## 17: Remaking Our Table

Now we can redo our table so that it only contains accurate data. We'll just have to subtract the two columns of `cleanDates`.

### Instructions

- Subtract the `dateSighted` column of `cleanDates` from the `dateReported` column and assign the result to `delay`.
- Make a table of the resulting vector and use the `print`function to display `delay`.

```R
delay <- cleanDates$dateReported - cleanDates$dateSighted
print(table(delay))
```

