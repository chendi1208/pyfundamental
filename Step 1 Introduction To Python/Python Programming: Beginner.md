#  Python Basics

## 1: Programming And Data Science

![img](https://s3.amazonaws.com/dsserver-prod-resources-1/public/1/public/Mission1Comic1.jpg)

As data science techniques shifted from academic research to industry, many organizations started to contribute back to the data science ecosystem by building new tools for working with data. Many of these tools have been released as [open source software](https://en.wikipedia.org/wiki/Open-source_software) and are free for anyone to work with. Before we can work with these tools, we need to learn the basics of **programming**, which is what this course is dedicated to.

**Why learn programming?**

To work with data effectively, you need to learn how to program.

![img](https://s3.amazonaws.com/dq-content/Mission1Comic2.jpg)

At Dataquest, we're strong believers in learning by doing. As you work through our content, you'll work with real data sets and receive immediate feedback.

**Variables**

**Variables** are a fundamental building block of most programming languages. We use variables to store values that we want to use later. Creating a variable involves deciding on a name, entering that name, and then assigning a value to that variable. We then can refer to that value using the variable name in our code. For example, if we know we're going to be using the value `365` often, we can store that value in a variable named `days`.

To assign the value `365` to the variable `days`, we enter the variable name, add an equals sign (`=`), and then enter the value you want that variable to store. This is known as an **assignment statement**:

```python
days = 365
```

```R
days <- 365
```

The equals sign (`=`) is called the **assignment operator** because it's used to assign the value on the right to the variable name on the left. An **operator** is a symbol that we use to express mathematical or logical operations between variables or values. In this case, the assignment operator assigns the value to the right of the equals sign to the variable name on the left.

Variable names in Python can't contain any spaces or special characters like `*` or `|`. The one special character that is allowed is the underscore (`_`), and it's recommended use is connecting multiple words in a single variable name:

```
number_of_days = 365
```

While you can also use numbers in the variable name, you can't start a variable name with a number. Here's the recommended way of using numbers in your variable names:

```
num_days_1 = 365
num_days_2 = 366
```

Lastly, Python contains a set of reserved words that can't be used as variable names like `True`, `False`, and `is`). You can find the full list of reserved words [here](https://docs.python.org/3/reference/lexical_analysis.html#keywords).

In addition to assigning a value to a new variable, you can also use the assignment operator to assign a new value to an existing variable:

```
number_of_days = 365
number_of_days = 366
```

After the last line of code is executed, the variable `number_of_days` will refer to `366`.

**The Dataquest Interface**

In most steps in a mission, you'll be asked to write code that accomplishes a specific task based on what you just learned. We then run your code, perform answer checking on the results, and give you feedback on your answer. This kind of deliberate practice at the time of learning helps you confirm your understanding and spot any weaknesses before moving on.

There are two ways to write and run code:

- the code editor interface
- the console interface

We'll focus on the code editor workflow in this step and discuss the console workflow later this mission. In the code editor workflow, you write your code in a single text file (named `script.py`), run the code to see the results, and receive feedback on your code. You write code in the section titled `script.py` and run code using the **Check** button:

![DQ Interface Check Button](https://s3.amazonaws.com/dq-content/dq_interface_check.png)

You can also run code using the relevant keyboard shortcut for your operating system. You can hover over each button to display the keyboard shortcut. For example, the shortcut to run code is **Option + Enter** on Mac OS X:

![DQ Interface Keyboard Shortcuts](https://s3.amazonaws.com/dq-content/dq_interface_keyboard_shortcuts.png)

The [Python interpreter](https://docs.python.org/3/tutorial/interpreter.html) is the program that reads and runs your code line by line. We walk through how to install the interpreter on your own computer in a later course. As you're learning the basics of Python on Dataquest, we'll handle running and displaying the results of code you write.

Once your code is finished running on our server, any variables you created will be displayed in the **Variables** inspector below the code editor interface along with feedback on your work. In the following screenshot, we receive feedback when we assigned `365` to `number_of_days` when the exercise actually asked us to assign `366` to `number_of_days`.

![DQ Interface](https://s3.amazonaws.com/dq-content/dq_interface.png)

Now it's your turn! In this step's exercise, we work with the hottest recorded temperatures, in Fahrenheit, for the three most populated countries in the world (China, India, and United States). These values are rounded to the nearest whole number.

## 2: Display Values Using The Print Function

In the last step, we assigned values to variables but didn't see any sort of visual confirmation. After we assign a value to a variable, we can confirm using the `print()` function. A **function** is a segment of reusable code that accepts an input value and produces an output value of some kind. While some functions modify the values associated with variables, the `print()` function only displays them. We'll learn more about functions later in this course.

To use the `print()` function, you pass in a value into the parentheses:

```python
print(365)
```

If you pass in a variable name instead of a value, the `print()` function will look up the associated value and display it. The following code assigns the value `365` to the variable `num_days` and then displays `365`:

```
num_days = 365
print(num_days)
```

Anything you display using the `print()`function will appear in the output box below the code editor:

![DQ Interface Print Output](https://s3.amazonaws.com/dq-content/dq_interface_print.png)

## 3: Data Types

While programming languages come in many different flavors, the most common one is **object-oriented programming**, or OOP for short. An object is a value or variable that belongs to a specific class. A class contains a shared blueprint for all objects that are instances of that class. For now, you can think of objects as being variables and classes as being types. In later missions, we'll learn how to create our own classes and dive more into how to organize our code well using object-oriented programming.

We've been working with whole numbers like `123` and `134`, which are known as **integers**. When we assign an integer value to a variable, we say that the variable is an **instance** of the integer class. The two most common numerical types in Python are integer and **float**, which is used to represent fractional values. `3.5` and `4.1111` are both examples of float values.

The most common non-numerical type is a **string**, which is used to represent text. To represent a piece of text as a string value, surround the text with either single quotes (`'`) or double quotes (`"`).`'Hello'` and `"Hello World!"` are both examples of string values. Unlike variable names, strings can contain special characters and spaces.

You can assign a string to a variable in the same way you'd assign it a numeric value:

```
hello = 'Hello'
hello_world = "Hello World!"
```

When you use the `print()` function to display a string value (or a variable associated with a string value), the quotation marks won't be displayed.

You may have noticed a pattern here. Numerical values like integers and floats don't require quotation marks, but strings do. The way in which you enter a value tells Python what data type it is. Python will use the data type to determine how the value should be handled. For example, Python allows integer variables to be divided, but not string variables. We'll learn more about that soon, but first let's practice some of the concepts you've learned so far.

## 4: The Type Function

We can look up the data type of a variable's value using the `type()`function. Similar to the `print()` function, you pass a value (or variable) into the parantheses. Unlike the `print()`function, however, the `type()` function won't display anything. Instead, it will return the data type as a value, which can be assigned to another variable or displayed using the `print()` function:

```python
hello = 'Hello'
hello_type = type(hello)
print(hello_type)
```

```R
hello_type <- typeof(hello)
hello_type <- class(hello)
hello_type <- mode(hello)
```

This will return the string `class 'str'`, which means that the value associated with `hello` is a string (`str` is short for string). To avoid having to create a variable each time, you can chain the `print()` and `type()` functions:

```
hello = 'Hello'
print(type(hello))
```

## 5: Converting Types

So far, we've represented numeric values with the integer and float data types. You can also represent them as strings, which will allow you to take advantage of the features unique to that data type. Python contains functions that will convert a value to a different data type.

The `str()` function converts numeric variables and values into strings. Specifically, it returns a string representation of the value that's passed in:

```python
str_eight = str(8)
eight = 8
str_eight_two = str(eight)
```

```R
str_eight <- as.character(8)
```

The `int()` function does the reverse; it will attempt to convert a string into an integer, but will result in an error if the string isn't actually an integer (e.g., `"January"`).

```python
str_eight = "8"
int_eight = int(str_eight)
```

```R
int_eight <- as.integer(str_eight)
```

You'll learn more about errors in a later mission, but the key idea is that an error stops your code from completing all the way through and displays a message describing the mistake you made.

![Type Casting Error](https://s3.amazonaws.com/dq-content/typecasting_error.png)

## 6: Comments

You can organize your code by inserting **comments**. Comments are notes that help people - including yourself - understand the code. The Python interpreter recognizes comments and treats them as plain text and won't attempt to execute them along with the rest of the code. These are the two main types of comments you can add to your code:

- inline comment
- single-line comment

An inline comment is useful whenever you want to annotate, or add more detail to, a specific statement. To add an inline comment at the end of a statement, start with the hash character (`#`) and then add your comment:

```
china = 123 # Number one in population.
india = 124 # Number two in population.
united_states = 134 # Number three in population.
```

While you don't need to add a space after the hash character (`#`), this is considered good style and makes your comments cleaner and easier to read.

A single-line comment spans the full line and is useful when you want to separate your code into sections. To specify that you want a line of text to be treated as a comment, start the line with the hash character (`#`):

```
# Assigning values to variables.
china = 123
india = 124
united_states = 134

# Displaying values associated with variables.
print(china)
print(india)
print(united_states)
```

## 7: Arithmetic Operators

A key part of data science is performing calculations using numerical values. Python has multiple **arithmetic operators** that allow you to express calculations between values. Here are the main arithmetic operators:

- Addition: `+`.
- Subtraction: `-`.
- Multiplication: `*`.
- Division: `/`.
- Exponent: `**`.

The operator goes between the values that need to be in the calculation. For example, the following code adds `123` and `124`using the addition operator:

```
123 + 124
```

Unfortunately, just performing a calculation like this isn't useful. This is because after the Python interpreter carries out the calculation, the resulting value disappears because it wasn't assigned to any variable. In the following code, we assign the result of the sum to a variable instead:

```
sum_top_two = 123 + 124
```

You can also perform calculations using variables. Before carrying out the calculation, Python will look up the associated values:

```
china = 123
india = 124
sum_top_two = china + india
```

Lastly, you can use arithmetic operators to perform calculations that involve both variables and values:

```
india_squared = india ** 2
double_china = china * 2
```

## 8: Order Of Operations

In the last step, we used a single operator to combine two values (e.g. `123` + `124`). When using multiple operators (e.g. `3 + 5 * 2`), we need rules that determine which order the calculations will be performed in. Take a look at the following example:

```
result = 5 + 5 + 5 / 10
```

If we add the three `5`'s first, we'll get a different answer than if we divided `5` by `10` first then performed the addition:

![Differences in OOO](https://s3.amazonaws.com/dq-content/ooo_differences.png)

Python, and many programming languages, use the **order of operations** rules from mathematics to determine the specific priority that calculations have. An easy way to remember the order of operations is [PEMDAS](https://en.wikipedia.org/wiki/Order_of_operations#Mnemonics). Here's a breakdown of what each letter means, along with the relevant Python operators:

- `P`arentheses: `(` and `)`.
- `E`xponents: `**`.
- `M`ultiplication: `*`.
- `D`ivision: `/`.
- `A`ddition: `+`.
- `S`ubtraction: `-`.

The Python interpreter processes calculations in the following order:

- Calculations in parentheses.
- Calculations using exponents.
- Division or multiplication (these rank equally and are processed left to right in the order they appear).
- Addition or subtraction (these also rank equally and are processed left to right in the order they appear).

Here's a diagram that walks through each step of applying the order of operations:

![OOO Without Parentheses](https://s3.amazonaws.com/dq-content/ooo_without_parentheses.png)

Because the interpreter didn't find any parentheses or exponents, it looked for any multiple or division calculations. When it found `5 / 10`, it performed that calculation first. Finally, it looked for any addition or subtraction calculations and performed the summation of the three values. If we instead use parentheses to group the summation first, that calculation is performed first:

![OOO With Parentheses](https://s3.amazonaws.com/dq-content/ooo_with_parentheses.png)

Let's use what we've learned to convert the three Fahrenheit values we've been working with to Celsius.

## 9: Console

So far, we've been working in the code editor interface by writing multiple lines of code in a text file and running the code in the text file all at once. The console is a programming environment that's helpful whenever we want to rapidly prototype and iterate on our code. In the console, every line of code we write is run using the Python [read-eval-print loop](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop), or REPL for short.

Every line of code is *read* by the Python interpreter, *evaluated*, then any output is *printed* out below our code. Here's a preview of the console in action:

![DQ Console](https://s3.amazonaws.com/dq-content/dq_console.png)

As you can tell from the screenshot, we don't need to explicitly use a `print()`statement to display the result of a calculation. We can even display the value associated with a variable by typing the variable name:

![DQ Console Variable](https://s3.amazonaws.com/dq-content/dq_console_variable.png)

To switch to the console interface, click on the **Console** button:

![DQ Switch To Console](https://s3.amazonaws.com/dq-content/dq_switch_to_console.png)

After you type a line of code, press the **Enter** key to have the interpreter evaluate your code. In this step, we don't perform any answer checking on your code and encourage you to experiment working in the console.

## 10: Using A List To Store Multiple Values

So far, we've been storing individual values in variables. Often in data science, we're working with thousands of data points that are grouped together in a certain way and have an order to them. We need a container that can hold multiple values that we can use to perform operations on.

We can use a **list**, which is an object that represents a sequence of values. For example, the months in a year can be represented as a list as a sequence of strings (`"January"`, `"February"`, and so on). The most basic way to make a list is to create an empty one first, and then adding values to it. To create an empty list, assign a pair of empty brackets `[]` to a variable:

```python
# months is an empty list (contains no values).
months = []
```

To add values to a list object, use the `list.append()` **method**. This method accepts a value and adds it to a list object. Unlike functions, methods are called using dot notation (`.`) on a specific object.`months` is a list object and the Python interpreter knows that `list.append()` can be used. You can see the methods available to list objects [here](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists).

In the following code snippet, we use the `list.append()` method to add the string "`January"` then the string `"February"` to the list `months`. The `list.append()`method is called on an **instance** of the list class (`months`) and modifies that specific object:

```python
months = []
months.append("January")
months.append("February")
```

Lastly, list objects can store values of multiple types:

```
months = []
months.append(1)
months.append("January")
months.append(2)
months.append("February")
```

## 11: Creating Lists With Values

In the last step, we learned how to populate lists by:

- creating an empty list: `months = []`.
- then adding values to the list using the `list.append()` method: `months.append(1)`.

This can become tedious when working with multiple lists, because you have to write many lines of code (one for each value in the list). You can instead create a list and populate a values all in one line using the following syntax:

```python
months = [1, "January", 2, "February"]
type(months[0]) # int
type(months[1]) # str
```

You may be wondering why we would *ever* want to create an empty list and add values manually. That technique is useful when you only want to add values that meet specific criteria. In that case, you need an empty placeholder list that you can append items to individually. As you work with lists, you'll start to learn the best technique for a given situation.

## 12: Accessing Elements In A List

Now that we know how to create a list and add values to it, let's learn how to access and work with the values in a list we've created. Each value in the list has an **index**, or position, associated with it. A list starts at index `0` and goes all the way to one less than the total number of values, or elements, in the list. If you have a list with `5` values, for example, the indexes will range from `0` to `4`.

The main quirk of list indexes is that to access the first element in a list, we actually use the index value `0` - not `1`. The second element is accessed with index value `1`, the third element with index value `2`, and so on. This is known as **zero indexing**. While many programming languages use zero indexing, some, like MATLAB, do not.

index01234values20102011201220132014

To return the value that has a given index, pass the integer for the index into bracket notation. In the following code, we create a list named `years` with five elements, and access the first, second, and fifth elements in the list. We assign each of the accessed values to new variables:

```
years = [2010, 2011, 2012, 2013, 2014]
first_value = years[0] # 2010
second_value = years[1] # 2011
fifth_value = years[4] # 2014
```

The Python interpreter expects that the bracketed integer value will be within the list's range of indexes. Passing in a non-integer value or an integer value outside of the range of indexes (e.g. index `7` for a list only containing `5` elements) will result in an error.

## 13: Retrieving The Length Of A List

We mentioned earlier that trying to lookup a value at an index that's not in the list will return an error and cause your code to halt. You may be wondering how we avoid accidentally looking up a value that's outside the index of a list. Python's `len()` function returns the **length** of a list, or the number of elements in that list. The function returns this value as an integer:

```python
int_months = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
twelve = len(int_months) # Contains the integer value 12.
```

```R
twelve <- length(int_months)
```

If we're ever unsure about the number of elements in a list, we can pass the list into the `len()` function. Because the `len()`function returns an integer, we can subtract `1` from this number to retrieve the index of the last element.

```python
int_months = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
eleven = len(int_months) - 1
last_value = int_months[eleven] # Contains the value at index 11.
last_value = int_months[len(int_months) - 1]
```

```R
last_value <- int_months[length(int_months)]
```

## 14: Slicing Lists

If we have a list containing thousands of values and want to retrieve the ones between index `10` and `500`, this would be a lot of work with what we know so far. Fortunately, lists have a feature called **slicing** that allows you to return all of the values between a starting index and an ending index. When you slice a list, you return a new list containing just the values you're interested in. The value at the starting index and all of the values in between will be returned. The value at the ending index *will not*.

To slice a list, pass the starting and ending index positions into the brackets as integer values, separated by a colon `:`. In the following code, we use the slice `2:4`to return a new list containing the values at indices `2` and `3`:

```
months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul"]
# Values at index 2, 3, but not 4.
two_four = months[2:4]
```

Here's a diagram of the same slice:

monthsindex0123456valuesJanFebMarAprMayJunJulmonths[2:4]MarApr

In the following code, we use the `len()`function to retrieve the total number of elements in the list `months`, and use it as the ending index:

```
# Values at index 3, 4, 5 and 6.
ending_index = len(months)
three_six = months[3:ending_index]
```

We also returned a list `three_six` that contains the last four elements in the list `months` by specifying a slice from the starting index (`3`) to the ending index (`len(months)`).

## 15: Next Steps

In this mission, we learned how to work with variables, use functions, perform math using arithmetic operators, and work with list objects. In the next mission, we'll learn how to interface with files and use loops to easily repeat certain tasks. For now, just click mission end to continue.

# Files and Loops

## 1: Overview

In this mission, we'll learn how to work with files and use loops to iterate through *lists*. We'll be working with crime rate data for 73 cities in the United States. Datasets are often represented in files that you can download and manipulate. Before we get started, we'll first need to learn how to work with files in Python.

## 2: Opening Files

To open a file in Python, we use the `open()` function. This function accepts two different arguments (inputs) in the parentheses, always in the following order:

- the name of the file (as a *string*)
- the mode of working with the file (as a *string*)

We'll learn about the various modes in a later mission. For now, we'll just use `"r"`, the mode for reading in files.

When entering multiple inputs, separate them with commas (`,`). For example, to open a file named `story.txt` in read mode, we write the following:


```
open("story.txt", "r")
```

The `open()` function returns a File object. This object stores the information we passed in, and allows us to call methods specific to the *File* class. We can assign the File object to a variable so we can refer to it later:


```
a = open("story.txt", "r")
```

Note that the File object, `a`, won't contain the actual contents of the file. It's instead an object that acts as an interface to the file and contains methods for reading in and modifying the file's contents (which we'll cover in the next screen).

### Instructions

- Use the `open()` function to create a File object.The name of the file to open is `"crime_rates.csv"`. Access the file in read mode (`"r"`).Assign this File object to the variable `f`.

```
f = open("crime_rates.csv", "r")
```

## 3: Reading In Files

File objects have a `read()` method that returns a *string* representation of the text in a file. Unlike the `append()` method from the previous mission, the `read()`method returns a value instead of modifying the object that calls the method. In the following code, we use the `read()` function to read the contents of `"test.txt"` into a File object, and assign that object to `g`:


```python
f = open("test.txt", "r")
g = f.read()
g = open("test.txt", "r").read()
```

```R
g <- read.table("test.txt")
```

Since `g` is a *string*, we can use the `print()` function to display the contents of the file:


```
f = open("test.txt", "r")
g = f.read()
print(g)
```

### Instructions

- Run the `read()` method on the File object `f` to return the *string* representation of `crime_rates.csv`.Assign the resulting *string* to a new variable named `data`.

```
f = open("crime_rates.csv", "r")
data = f.read()
```

## 4: Splitting

To make our *string* object `data` more useful, let's convert it into a *list*. Here's a preview of how the dataset looks:


```
Albuquerque,749\nAnaheim,371\nAnchorage,828\n
```

Each line is separated by the *string* `\n`, which is referred to as the new-line character. When we open a text file in a text editor, the editor will automatically split the text and create a new line wherever it sees the *string* `\n`:

![img](https://dq-content.s3.amazonaws.com/H6aiK9z.png)

In Python, we can use the `split()`method to turn a *string* object into a *list* of *strings*, like so:


```
["Albuquerque,749", "Anaheim,371", "Anchorage,828"]
```

The `split()` method takes a *string* input corresponding to the **delimiter**, or separator. This delimiter determines how the *string* is split into elements in a *list*. For example, the delimiter for the crime rate data we just looked at is `\n`. Many other files use commas to separate elements:


```python
sample = "john,plastic,joe"
split_list = sample.split(",")
# split_list is a list of _strings_: ["john", "plastic", "joe"]
```

```R
split_list <- strsplit(sample, ",")
```

### Instructions

- Split the *string* object `data` on the new-line character `"\n"`, and store the result in a variable named `rows`. - Then, use the `print()` function to display the first five elements in `rows`.

```
# We can split a string into a list.
sample = "john,plastic,joe"
split_list = sample.split(",")
print(split_list)

# Here's another example.
string_two = "How much wood\ncan a woodchuck chuck\nif a woodchuck\ncould chuck wood?"
split_string_two = string_two.split('\n')
print(split_string_two)

# Code from previous cells
f = open('crime_rates.csv', 'r')
data = f.read()

rows = data.split('\n')
print(rows[0:5])
```

## 5: Loops

We now have a *list* representation of the dataset (`rows`). Each element in the dataset is a *string* containing a comma (`,`) that separates the city name from the crime rate. Because they're *strings*, we can use the `split()` method on each of them to separate those values. Accomplishing this based on what we know so far would be a cumbersome task. We would need to write 73 lines of code, one for each element in the *list*.

Ideally, we could specify the code we want to execute for each element in the *list* a single time. Python and most other programming languages allow you to do this with a **loop**. For example, here's a loop that prints each element in a *list*:


```python
cities = ["Austin", "Dallas", "Houston"]
for city in cities:
    print(city)
```

```R
for (city in cities) {print(city)}
```

Here are the steps Python takes when it runs this code:

- Assigns the value at index `0` in the `cities` list (`"Austin"`) to `city`.
- Executes the indented code. Because the current value of `city` is `"Austin"`, it runs `print("Austin")`.
- Assigns the value at index `1` in the `cities` list (`"Dallas"`) to `city`.
- Executes the indented code. Because the current value of `city` is `"Dallas"`, it runs `print("Dallas")`.
- Assigns the value at index `2` from the `cities` list (`"Houston"`) to `city`.
- Executes the indented code. Because the current value of `city` is `"Houston"`, it runs `print("Houston")`.
- Sees that there are no more elements in the list `cities`, and stops running.

This code uses a type of loop called a *for* loop. We can break the *for* loop down into two main components: the *for* loop itself, and the loop body that contains the code we want to run during each iteration.

Here are the things you need to include in each of these components:

*for* Loop

- Syntax - the words `for` and `in` need to be included in the statement
- Iterator variable - the variable name you decide to use to refer to each element in the *list* (`city`)
- Sequence - the variable you want to iterate over (`cities`)
- Colon - loop statements must end with a colon (`:`)

Loop Body

- Indentation - every line of code within the loop should be indented four spaces
- Logic - the actual code we want to execute for each element. In the above code block, for example, we update the iterator variable (`city`) in each iteration of the loop.

Here's a diagram displaying the values for `city` and the `print()` statement during each iteration of the loop:

cityprint(city)"Austin"print("Austin")"Dallas"print("Dallas")"Houston"print("Houston")

The iterator variable, `city`, is a temporary variable that's only accessible within the *for* loop.

The very basic loop body we wrote above only contains one line of code. To write a loop body with multiple lines of code, we need to indent that code consistently (using four spaces). For example, the following code will run the `print()` statement three times for each element in `cities`:


```python
for city in cities:
    print(city)
    print(city)
    print(city)
# Austin
# Austin
# Austin
# Dallas
# Dallas
# Dallas
# Houston
# Houston
# Houston
```

Here's an annotated diagram of the code:

iteratorvariablesequenceforloopforcityincities:syntaxprint(city)print(city)loopbodyprint(city)

As you become more familiar with loops, you'll learn how to express repetitive and complex logic efficiently.

## 6: Practice - Loops

Let's practice writing a *for* loop on a subset of the crime rate data.

### Instructions

- The variable `ten_rows` contains the first 10 elements in `rows`.
- Write a *for* loop that:iterates over each element in `ten_rows`uses the `print()` function to display each element

```
ten_rows = rows[0:10]
for row in ten_rows:
    print(row)
```

## 7: List Of Lists

Now that we know how to execute code for each element in a *list*, we can use a loop to split those elements on a delimiter and append the results to a new list. So far, we've appended values like *integers*and *strings*, but we've never appended a *list* to a *list*. This is known as a **list of lists**, since each element is itself a *list*.

The following code:

- splits each element in `three_rows`(which contains the first three elements from `rows`) on the comma delimiter (`,`)
- appends the resulting *list*(`split_list`) to a new *list* we create (`final_list`)
- displays the final *list* using the `print()` function


```python
three_rows = ["Albuquerque,749", "Anaheim,371", "Anchorage,828"]
final_list = []

for row in three_rows:
    split_list = row.split(',')
    final_list.append(split_list)
print(final_list)
```

```R
three_rows <- c("Albuquerque,749", "Anaheim,371", "Anchorage,828")
final_list <- list()

for (row in three_rows) {
  split_list <- unlist(strsplit(row, ','))
  final_list[[which(three_rows == row)]] <- split_list
}
print(final_list)
```

Here's the logic above represented as a diagram:

three_rowssplit_list=final_list.append(split_list)row.split(',')0"Albuquerque,749"["Albuquerque","749"]0["Albuquerque","749"]1"Anaheim,371"["Anaheim","371"]1["Anaheim","371"]2"Anchorage,828"["Anchorage","828"]2["Anchorage","828"]

In the leftmost part of the diagram, you'll notice that the elements in `three_rows`are *strings*. In the right-most part of the diagram, you'll notice that the elements in `final_list` are *lists*.

Here's what the output looks like:

`[['Albuquerque', '749'], ['Anaheim', '371'], ['Anchorage', '828']]`

Recall that since only the indented code is executed as part of the loop, the unindented `print()` statement on the last line will execute only after the loop finishes. In the following code cell, we execute the code we visualized in the diagram, along with some additional `print()` statements. These `print()`statements retrieve individual elements from `final_list`. Note that they all display as *list* objects. You'll practice writing your own *for* loop in the next step. For now, explore and run the code we covered in this step in the code cell below.

## 8: Practice - Splitting Elements In A List

Let's now convert the full dataset, `rows`, into a *list* of *lists* using the logic from the previous step.

### Instructions

- Write a *for* loop that splits each element in `rows` on the comma delimiter, and appends the resulting *list* to a new *list* named `final_data`.
- Then, use the `print()` function and *list* slicing to display the first five elements in `final_data`.

```
f = open('crime_rates.csv', 'r')
data = f.read()
rows = data.split('\n')
print(rows[0:5])
final_data = []
for row in rows:
    split_list = row.split(',')
    final_data.append(split_list)
print(final_data[0:5])
```

## 9: Accessing Elements In A List Of Lists: The Manual Way

A *list* of *lists* has some unique interaction mechanisms. Using bracket notation to retrieve an element at a certain index returns a *list* object. However, using bracket notation on the resulting *list* will actually return a data point (such as a *string* or an *integer*).

In the following code, we retrieve the first *list* from `final_data` and assign it to `first_list`. We then retrieve the first element from `first_list` and assign it to `first_list_first_value`:


```
# Returns the first list: ['Albuquerque', '749'].
first_list = final_data[0]

# Returns the first list's first element: 'Albuquerque'.
first_list_first_value = first_list[0]
```

Since using bracket notation once returns a *list*, you can use bracket notation again on the resulting *list* without having to assign it to a variable first. Using double bracket notation is simpler. The following code uses double bracket notation to retrieve the value at index `0` for the first three elements in `final_data`:


```
# Returns the first list's first element, 'Albuquerque'.
first_list_first_value = final_data[0][0]
# Returns the second list's first element, 'Anaheim'.
second_list_first_value = final_data[1][0]
# Returns the third list's first element, 'Anchorage'.
third_list_first_value = final_data[2][0]
```

Here's a visual representation of `final_data[0][0]` to help you grasp double bracket notation:

final_datafinal_data[0]final_data[0][0](listoflists)(list)(string)0['Albuquerque','749']0'Albuquerque''Albuquerque'1['Anaheim','371']1'749'2['Anchorage','828']

### Instructions

- `five_elements` contains the first five elements from `final_data`.
- Create a *list* of *strings* named `cities_list` that contains the city names from each *list* in `five_elements`.

```
print(five_elements)
cities_list = []
cities_list.append(five_elements[0][0])
cities_list.append(five_elements[1][0])
cities_list.append(five_elements[2][0])
cities_list.append(five_elements[3][0])
cities_list.append(five_elements[4][0])
print(cities_list)
```

## 10: Looping Through A List Of Lists

Instead of appending each element from the nested *list* to a new *list* individually, we can use a *for* loop and specify the append logic within the loop body. Let's explore how we can replace the `append()`statements from the previous step with a *for* loop.

In the following code, we create a new *list*called `crime_rates`, and write a *for* loop that iterates through `five_elements`. In each iteration of the *loop*, we retrieve the *string* at index `1` from the current *list*(within `five_elements`), assign that *string*to the variable `crime_rate`, and use the `append()` method to add `crime_rate` to the *list* `crime_rates`.


```
crime_rates = []

for row in five_elements:
    # row is a list variable, not a string.
    crime_rate = row[1]

    # crime_rate is a string, the crime rate of the city
    crime_rates.append(crime_rate)
```

Now it's your turn to apply what you've learned to the full dataset.

### Instructions

- Create a *list* of *strings* named `cities_list` that contains just the city names from `final_data`.Recall that the city name is located at index `0` for each *list* in `final_data`.

```
cities_list = []
for outer_list in final_data:
    city = outer_list[0]
    cities_list.append(city)
```

## 11: Challenge

Let's practice synthesizing the concepts you've learned so far in this mission. Note that this exercise may be more challenging than the previous ones, and may take more time to complete.

### Instructions

Create a *list* of *integers* named `int_crime_rates` that contains just the crime rates - as *integers* - from the *list* `rows`.

- First create an empty *list* and assign it to a new variable `int_crime_rates`.
- Then, write a *for* loop that iterates over `rows` and does the following:uses the `split()` method to convert each *string* in `rows` into a *list*, based on the comma delimiterconverts the value at index `1` from that *list* to an *integer* using the `int()` functionuses the `append()` method to add each *integer* to `int_crime_rates`

```python
f = open('crime_rates.csv', 'r')
data = f.read()
rows = data.split('\n')
print(rows[0:5])
int_crime_rates = []
for row in rows:
    values = row.split(',')
    crime_rate = int(values[1])
    int_crime_rates.append(crime_rate)
```

# Booleans and If Statements

## 1: Booleans

In this mission, we'll learn how to express **conditional logic**. We can use conditional logic to add criteria to the code we write. Some examples of operations that use criteria include:

- Finding all the *integers* in a *list* that are greater than `5`.
- Identifying which elements in a *list*are *strings*, and printing only those values.

We can break down both of these examples into logic we can code:

- For each *integer* in a *list*, if the *integer* is greater than `5`, add to the *list* `greater_than_five`.
- For each element in a *list*, if the value has a *string* data type, use the `print()` function to display it; if it's not a string, ignore it.

Python has a class called **Boolean** that helps express conditional logic. There are only two *Boolean* values: `True` and `False`. Because they're words, *Boolean*values may look like *strings*, but they're an entirely separate class. For example, *string* operations like concatenation won't work with *Booleans*.

The following code example assigns `True`to `t` and `False` to `f`:


```
t = True
f = False
```

If we display the data type for either `t` or `f`, we'll see `class 'bool'`, shorthand for *Boolean*.

### Instructions

- Assign the value `True` to the variable `cat`, and the value `False` to the variable `dog`.
- Then, use the `print()` function and the `type()` function to display the type for `cat`.

```
cat = True
dog = False
print(type(cat))
```

## 2: Boolean Operators

Python has comparison operators that allow us to compare variables:

- `==` returns `True` if both variables are equivalent, and `False` if they're different
- `!=` returns `True` if both variables are different, and `False` if they're equivalent
- `>` returns `True` if the first variable is greater than the second variable, and `False` otherwise
- `<` returns `True` if the first variable is less than the second variable, and `False` otherwise
- `>=` returns `True` if the first variable is greater than or equal to the second variable, and `False` otherwise
- `<=` returns `True` if the first variable is less than or equal to the second variable, and `False` otherwise

For now let's focus on the equality operators (`==` and `!=`). To compare two variables, place the operator between them. We recommend adding a space before and after the operator for better readability:


```
print(8 == 8) # True
print(8 != 8) # False
print(8 == 10) # False
print(8 != 10) # True
```

We can assign the result of a comparison to a *Boolean* variable:


```
# Use parentheses for cleaner code.
t = (8 == 8) # True
u = (8 != 8) # False
```

We can also compare *strings*, *floats*, *Booleans*, and even *lists*:


```
# All of these return True.
"8" == "8"
["January", "February"] == ["January", "February"]
5.0 == 5.0
```

The variable `cities` is a *list* of *strings*containing the city names from the crime rate dataset we used in the previous mission. We've created the *list* for you already.

### Instructions

- Use the *Boolean* operators to determine if the following pairs of values are equivalent:The first element in `cities` and the *string*`"Albuquerque"`. Assign the resulting *Boolean* value to `first_alb`.The second element in `cities` and the *string*`"Albuquerque"`. Assign the resulting *Boolean* value to `second_alb`.The first element in `cities` and the last element in `cities`. Assign the resulting *Boolean* value to `first_last`.

```
print(cities)
first_alb = (cities[0] == "Albuquerque")
second_alb = (cities[1] == "Albuquerque")
last_element_index = len(cities) - 1
first_last = (cities[0] == cities[last_element_index])
```

## 3: Booleans With "Greater Than"

We can use the greater than operator (`>`) to test whether one value is larger than another. Similarly, the greater than or equal to operator (`>=`) determines if one value is larger than or equal to a second value:


```
rates = [10, 15, 20]

rates[0] > rates[1] # False
rates[0] >= rates[0] # True
```

### Instructions

- The variable `crime_rates` is a *list* of *integers* containing the crime rates from the dataset. Perform the following comparisons:Evaluate whether the first element in `crime_rates` is larger than the *integer* `500`, and assign the *Boolean*result to `first_500`.Evaluate whether the first element in `crime_rates` is larger than or equal to `749`, and assign the *Boolean*result to `first_749`.Evaluate whether the first element in `crime_rates` is greater than or equal to the last element in `crime_rates`, and assign the *Boolean* result to `first_last`.

```
print(crime_rates)
first_500 = (crime_rates[0] > 500)
first_749 = (crime_rates[0] >= 749)
last_index = len(crime_rates) - 1
first_last = (crime_rates[0] >= crime_rates[last_index])
```

## 4: Booleans With "Less Than"

We can use the less than operator (`<`) to test whether one value is smaller than another value. Similarly, the less than or equal to operator (`<=`) determines if one value is smaller than or equal to another value:


```
rates = [10, 15, 20]

rates[0] < rates[1] # True
rates[0] <= rates[0] # True
```

### Instructions

- The variable `crime_rates` is a *list* containing the crime rates from the dataset as *integers*. Perform the following comparisons:Determine whether the second element in `crime_rates` is smaller than the *integer* `500`, and assign the *Boolean* result to `second_500`.Determine whether the second element in `crime_rates` is smaller than or equal to `371`, and assign the *Boolean* result to `second_371`.Determine whether the second element in `crime_rates` is smaller than or equal to the last element in `crime_rates`, and assign the *Boolean*result to `second_last`.

```
print(crime_rates)
second_500 = (crime_rates[1] < 500)
second_371 = (crime_rates[1] <= 371)
last_index = len(crime_rates) - 1
second_last = (crime_rates[1] <= crime_rates[last_index])
```

## 5: If Statements

Now that we know how to work with *Boolean* values, let's dive more into how we use *Booleans* to express conditional logic. To complement *Booleans*, Python contains the *if* operator. We can use this operator to write a statement that tests whether certain conditions exist. Our *if*statement will evaluate to either `True` or `False`, and only run the specified code when `True`.

For example, the following code checks whether the *integer* value assigned to `sample_rate` is larger than `5`. This is referred to as a "conditional statement." It assigns the *Boolean* result to `greater`, and uses the `print()` function to display `sample_rate` if `greater` is `True`:


```
sample_rate = 749
greater = (sample_rate > 5)
if greater:                    #This is the conditional statement.
    print(sample_rate)
```

We can also specify the conditional statement inside the *if* statement:


```
if sample_rate > 5:            #This is the conditional statement.
    print(sample_rate)
```

The conditional statement after the *if* has to evaluate to either `True` or `False`.

Similar to `for` loops, we need to format *if* statements in the following way:

- End the conditional statement with a colon (`:`)
- Indent the code (that we want run when `True`) below the conditional statement

Also similar to *for* loops, *if* statements can contain multiple lines in the body, as long as their indentation aligns.


```
t = True
f = False

if t:
    print("Now you see me")
if f:
    print("Now you don't")
```

We'll end with a generalized representation of an *if* statement:

ifsyntaxis(condition):bodyline1onlyevaluatedifindentedbodyline2conditionisTruebodybodyline3

### Instructions

- Determine whether the third element in `cities` is equivalent to the *string* `"Anchorage"`.If it is, change the variable `result` to `1`.

```
result = 0
if cities[2] == "Anchorage":
    result = 1
```

## 6: Nesting If Statements

We can nest *if* statements to specify multiple criteria. In the following code example, we first test whether an *integer*is greater than `500`, and then check whether it's greater than `1000`. Python only evaluates the inner *if* statement if the outer *if* statement evaluates to `True`.


```
value = 1500

if value > 500:
    if value > 1000:
        print("This number is HUGE!")
```

Here's an annotated diagram of the nested *if* statement:

ifvalue>500:outerindented4spacesifvalue>1000:innerindented8spacesprint("ThisnumberisHUGE!")

Be sure to indent your code properly with four spaces for each code block!

### Instructions

- Write a piece of code that nests the following concepts in the order in which they appear:
  - An *if* statement that tests whether the first element in `crime_rates` is larger than `500`
  - A second *if* statement that tests whether the second element in `crime_rates` is larger than `300`
  - If both statements evaluate to `True`, assign the value `True` to the variable `both_conditions`

```
both_conditions = False
if crime_rates[0] > 500:
    if crime_rates[1] > 300:
        both_conditions = True
```

## 7: If Statements And For Loops

We can also nest *if* statements within *for*loops, and vice versa. For example, we can search a *list* for the existence of a specific value by combining a *for* loop with an *if*statement. The *if* statement determines whether the current element is equivalent to the value we're interested in:


```
found = False
for city in cities:
    if city == 'Washington':
        found = True
```

We've set the value `found` set to `False`by default. If one of the elements in `cities` is equivalent to the *string*`"Washington"`, then Python assigns the *Boolean* value `True` to `found`.

We can also use a *for* loop and an *if*statement to determine the index for a specific value in a *list*. To accomplish this, we can create an *integer* variable named `counter` outside the *for* loop and increment it by `1` with each iteration. If the value at the current iteration of the loop matches our desired value, we can set another *integer* variable, `index`, to the current value of `counter`:


```
counter = 0
index = 0

for city in cities:
    if city == "Washington":
        index = counter
    counter += 1
```

Keep in mind that in the above implementation, if there are multiple instances of the desired value (`"Washington"`) in the *list*, the variable `index` will update each time. This means that once the *for* loop finishes, `index` will contain the last index location the loop finds (not the first one).

### Instructions

- Create a new *list*, `five_hundred_list`, that contains only the elements from `crime_rates` that are greater than `500`. To accomplish this, you'll need a *for* loop and an *if*statement:The *for* loop specifies which *list* we want to iterate over and the name of the iterator variable (we use `cr`in our answer).The *if* statement determines whether the current element (`cr`) is larger than `500`.If the current element (`cr`) is larger than `500`, use the `append()` method to add it to `five_hundred_list`.

```
five_hundred_list = []
for cr in crime_rates:
    if cr > 500:
        five_hundred_list.append(cr)
```

## 8: Find The Highest Crime Rate

Now that we know how to combine *if*statements and *for* loops, we can find the highest crime rate in the `crime_rates` *list*.

Here's one way we can approach this task:

- Assign the value at index `0` in `crime_rates` to a new *integer*variable called `highest`
- Use a *for* loop that compares each value in `crime_rates` to `highest`, and assigns that value to `highest` if it's larger

If the first value (at index `0`) is the largest value in the *list*, then the value assigned to `highest` will never change. Otherwise, each time the loop finds a value that's larger than `highest`, it will assign that value to `highest`. The value assigned to `highest` only changes when a new value in the *list* is larger than it. When the loop completes, we're guaranteed to have the largest value.

### Instructions

- Find the largest *integer* in `crime_rates` using the strategy we just outlined, and assign that value to the variable `highest`.

```
print(crime_rates)
highest = crime_rates[0]
for cr in crime_rates:
    if cr > highest:
        highest = cr
```

# Challenge: Files, Loops, and Conditional Logic

## 1: How Challenges Work

Our missions are structured around learning by doing, so you're ready to tackle problems in the real world. We've also designed challenges that will give you an opportunity to practice programming and data science by completing structured problems. Completing challenges will help you solidify concepts and apply what you've learned in the real world. Challenges will feel similar to missions, but with little instructional material and harder exercises. You can check your work as many times as you'd like, and we encourage you to experiment with your code. If you get stuck, write code to explore and review concepts you've learned, or refer back to the missions you've completed.

While you can reveal the solution code, we encourage you to think, make your own attempts, and experiment for a significant amount of time before doing so! If you have questions or run into issues, head over to the [Dataquest forums](https://www.dataquest.io/forum/) or to our [Slack community](https://dscommunity.slack.com/) to get help.

## 2: Unisex Names

For this challenge, you'll be working with the data set behind [this FiveThirtyEight article](http://fivethirtyeight.com/features/there-are-922-unisex-names-in-america-is-yours-one-of-them/) on common unisex (gender-neutral) names in the United States. You'll start by reading in the file and iteratively converting the data to more useful representations. At the end of this challenge, you'll filter the data so that it only includes the names that at least 1,000 people share.

The staff at [FiveThirtyEight](http://fivethirtyeight.com/) compiled this data set from information at the [Social Security Adminstration's website](https://www.ssa.gov/oact/babynames/limits.html). You'll work with a shortened version of the [full data set](https://github.com/fivethirtyeight/data/blob/master/unisex-names/unisex_names_table.csv) to complete this challenge.

Here's a preview of the shortened data set, which is in a CSV file named `dq_unisex_names.csv`:


```
Casey,176544.328149
Riley,154860.66517300002
Jessie,136381.830656
Jackie,132928.78874000002
Avery,121797.41951600001
Jaime,109870.18729000002
Peyton,94896.39521599999
```

Each line contains two values separated by commas. The first value is the unisex name, and the second value is the estimated number of Americans with that name.

## 3: Read The File Into A String

To work with the data, you'll first need to read it into Python. This involves creating a *File* object, then using one of its methods to read the file into a *string*.

### Instructions

Use the `open()` function with the following parameters to return a *File* object:

- `dq_unisex_names.csv` for the file name
- `r` for read mode

Then, use the `read()` method of the *File* object to read the file into a *string*.

- Assign that *string* to a variable named `names`.

```
f = open('dq_unisex_names.csv', 'r')
names = f.read()
```

## 4: Convert The String To A List

Now that you have a *string* representation of the file, convert it to a *list* of *strings*.

namesnames_list(string)(listofstrings)"Casey,1765440"Casey,176544.328149".328149\nRiley,154860.665173001"Riley,154860.66517300002"002\nJessie,.."2"Jessie,136381.830656"

### Instructions

- Use the `split()` method that *strings* have to split on the new-line delimiter (`"\n"`), and assign the resulting *list* to `names_list`.
- Select the first five elements in `names_list`, and assign them to `first_five`.
- Display `first_five` using the `print()` function.

```
names_list = names.split('\n')
first_five = names_list[0:5]
```

## 5: Convert The List Of Strings To A List Of Lists

When you displayed the first five elements, you may have noticed that they contained commas. Let's split each *string*element in `names_list` on the comma, and add the resulting *lists* to a new *list*named `nested_list`.

names_listnested_list(listofstrings)(listoflists)0"Casey,176544.328149"0["Casey","176544.328149"]1"Riley,154860.66517300002"1["Riley","154860.66517300002"]2"Jessie,136381.830656"2["Jessie","136381.830656"]

### Instructions

Split each element in `names_list` on the comma delimiter (`,`) and append the resulting *list* to `nested_list`. To accomplish this:

- Create an empty *list* and assign it to `nested_list`.
- Write a *for* loop that iterates over `names_list`.
- Within the loop body, run the `split()` method on each element to return a *list* (assign that *list* to `comma_list`).
- Within the loop body, run the `append()` method to add each *list* (`comma_list`) to `nested_list`.

Use the `print()` function to display the first five elements in `nested_list`.

```
nested_list = []
for line in names_list:
    comma_list = line.split(',')
    nested_list.append(comma_list)
print(nested_list[0:5])
```

## 6: Convert Numerical Values

You now have a *list* of *lists* assigned to `nested_list`, where each inner *list*contains *string* elements. The second element (the estimated number of people with that name) in each *list* is a decimal value that you should convert to a *float*. By converting these values to *floats*, you'll be able to perform computations on them and analyze the data.

nested_listnumerical_list(listoflists)(listoflists)0["Casey","176544.328149"]0["Casey",176544.328149]1["Riley","154860.66517300002"]1["Riley",154860.66517300002]2["Jessie","136381.830656"]2["Jessie",13681.830656]stringstringstringfloat

### Instructions

Create a new *list* of *lists* called `numerical_list` where:

- The element at index `0` for each *list* is the unisex name (as a *string*)
- The element at index `1` for each *list* is the number of people who share that name (as a *float*)

To accomplish this:

- Create an empty *list* and assign it to `numerical_list`.
- Write a *for* loop that iterates over `nested_list`. In the loop body:Retrieve the element at index `0` and assign it to a variable.Retrieve the element at index `1`, convert it to a *float*, and assign it to a variable.Create a new *list* containing these two elements (in the same order).Use the `append()` method to add this new *list* to `numerical_list`.

Finally, display the first five elements in `numerical_list`.

```
print(nested_list[0:5])
numerical_list = []
for line in nested_list:
    name = line[0]
    count = float(line[1])
    new_list = [name, count]
    numerical_list.append(new_list)
print(numerical_list[0:5])
```

## 7: Filter The List

The data set contains first names shared by at least 100 people. Let's limit it those shared by at least 1,000 people.

### Instructions

Create a new *list* of *strings* called `thousand_or_greater` that only contains the names shared by 1,000 people or more.

To accomplish this:

- Create an empty *list* and assign it to `thousand_or_greater`.
- Write a *for* loop that iterates over `numerical_list`.
- In the loop body, use an *if* statement to determine if the value at index `1` for that element (which is a *list*) is greater than or equal to `1000`.
- If the value is greater than or equal to `1000`, use the `append()` method to add its name to `thousand_or_greater`.

Finally, display the first 10 elements in `thousand_or_greater`.

```
# The last value is ~100 people
numerical_list[len(numerical_list)-1]
thousand_or_greater = []
for line in numerical_list:
    name = line[0]
    population = line[1]
    if population >= 1000:
        thousand_or_greater.append(name)
print(thousand_or_greater[0:10])
```

# List Operations

## 1: Introduction To The Data Set

In this mission, we'll look at daily weather data for Los Angeles (L.A.) during 2014. Here's a look at the beginning of `la_weather.csv`, the data set we'll be working:


```
Day,Type of Weather
1,Sunny
2,Sunny
3,Sunny
4,Sunny
5,Sunny
6,Rain
7,Sunny
8,Sunny
9,Fog
10,Rain
```

In an earlier mission, we split a *CSV* file into *rows*. We'll be doing the same thing here, so it's useful to think of each line in the data file as a separate row. Each row contains multiple values separated by a comma (`,`).

The first row in our data is the *header row*, which contains labels for the values beneath them. As the *header row* indicates, each row has two values:

- `Day` - A number from `1` to `365` indicating the day of the year. January 1st is `1`, and December 31st is `365`.
- `Type of Weather` - The type of weather experienced on that day. The values that may appear here include `Rain`, `Sunny`, `Fog`, `Fog-Rain`, or `Thunderstorm`.

Our ultimate goal is to count how many times each type of weather occurred over the course of the year. During this mission, we'll learn how to manipulate the data with lists, and make good progress towards that goal. In the next mission, we'll fit all the pieces together and tally up the data.

[](https://www.dataquest.io/home)[Dashboard](https://www.dataquest.io/dashboard)GET HELP2CChen 

[Data Analyst](https://www.dataquest.io/path/data-analyst-track) / [Introduction to Python](https://www.dataquest.io/path-step/introduction-python) / [Python Programming: Beginner](https://www.dataquest.io/course/python-programming-beginner) / List Operations

## 2: Parsing The File

Because our data is in a *CSV* file, we'll need to read the file in before we can work with it. In an earlier mission, we read a *CSV* file into a *list*, and we'll do the same here.

To read the file in, we'll need to:

- Open it with the [open()](https://docs.python.org/3/library/functions.html#open) function. This will return a [File](https://docs.python.org/3.5/library/stdtypes.html#file-objects) object.
- Read the open file into a variable using the [read()](https://docs.python.org/3.5/tutorial/inputoutput.html#reading-and-writing-files) method. This will return in a *string*.
- Split the data into rows on the newline character (`\n`). This will result in a *list*.
- Loop through each row, and split each row into a list on the comma character (`,`). This will result in a *list of lists*.

Here's the code we used to parse the `crime_rates.csv` file in an earlier mission:


```
f = open("crime_rates.csv", 'r')
data = f.read()
rows = data.split('\n')
full_data = []
for row in rows:
    split_row = row.split(",")
    full_data.append(split_row)
```

### Instructions

- Open and read in `la_weather.csv`.
- Split the data on the newline character to convert it to a *list*of rows.
- Split each row on the comma and append each *list* to `weather_data`.

```
weather_data = []
f = open("la_weather.csv", 'r')
data = f.read()
rows = data.split('\n')
for row in rows:
    split_row = row.split(",")
    weather_data.append(split_row)
```

## 3: Getting A Single Column From The Data

We've been thinking of our data in terms of *rows*, where *rows* are horizontal, from left to right:

1Day,TypeofWeather21,Sunny32,Sunny43,Sunny

The diagram above shows the first four *rows* in the data, along with their corresponding row numbers. Each item in a row records different information, but refers to the same day.

Another way to think of the data is in terms of values that are alike, or *columns*. *Columns* are vertical, from top to bottom:

12DayTypeofWeather1Sunny2Sunny3Sunny

In the above diagram, we grouped the data by *column*. Each value in a column records similar information. *Column* `1` shows the `Day` of the year, and *column* `2` shows the `Type of Weather` that occurred on that day.

The first value in each *column* is from the *header row*; it labels the data that appears in the column. While not all data sets include headers, they're helpful to have.

Since we'll be counting the total number of times each type of weather occurred during the year, we don't need the `Day`*column*.

You may recall that in a previous mission, we did the following to extract a *column*:

- Looped over each row.
- Retrieved the second element of the row.
- Appended the second element to a list of items.

Here's an example of what this process looks like:


```
numbers = [[1,2],[3,4],[5,6],[7,8]]
second_column = []
for item in numbers:
    value = item[1]
    second_column.append(value)
```

### Instructions

- Loop over each *row* in `weather_data`.
- Append the second item in each *row* to the `weather` *list*.
- When complete, `weather` should contain each value from the `Type of Weather` *column*.

```
# weather_data has already been read in automatically to make things easier.
weather = []
for row in weather_data:
    weather.append(row[1])
```

## 4: Counting The Items In A List

To verify that we selected the `Type of Weather` *column* properly, let's tally the number of items in `weather`.

### Instructions

Count the number of items in `weather`. You can accomplish this by:

- Looping over each element in `weather`.
- Adding `1` to `count` for each element.

When finished, `count` should equal the number of items in `weather`.

```
count = 0
for item in weather:
    count = count + 1
```

## 5: Removing The Header

When we created the `weather` list, we included the *header*. The first few items in `weather` look like this:

012364365weather=["TypeofWeather","Sunny","Sunny",...,"Fog","Fog"]

We want to tally how many times each type of weather occurred in L.A. To do this, we'll need to remove the *string* `Type of Weather` from the beginning of our list. If we don't do this, our count will include the *header*, which isn't part of the data set. The *header* is just a *string* that tells us what kind of values are in a *column*.

To remove the *header* we can use a list slicing. `weather` contains `366` elements, and the last *list index* value is `365`. We'll want to create a slice that goes from the second element (*index* `1`) to the end of the *list*.

### Instructions

- Slice the `weather` *list* to remove the header.
- The slice should only remove the first element in the list.
- Assign the slice to `new_weather`.

```
new_weather = weather[1:366]
```

## 6: The In Statement

In Python, it's often very useful to check whether a certain element is present in a *list*. One way to perform this check is to use a *for loop*:


```
animals = ["cat", "dog", "rabbit"]
for animal in animals:
    if animal == "cat":
        print("Cat found")
```

This is a lot of code for a common operation, though. Luckily, Python has the *in statement*, which allows us to check whether a *list* contains a specific element much more quickly:


```
animals = ["cat", "dog", "rabbit"]
if "cat" in animals:
    print("Cat found")
```

The *in statement* checks whether certain value occurs within a *list*, and returns `True` if it does. If not, the statement returns `False`.

We can directly assign the result to a variable as well:


```
animals = ["cat", "dog", "rabbit"]
cat_found = "cat" in animals
```

### Instructions

- Use the *in statement* to check whether the value `cat` is in the list `animals`, and assign the result to `cat_found`.
- Use the *in statement* to check whether the value `space_monster` is in the list `animals`, and assign the result to `space_monster_found`.

```python
animals = ["cat", "dog", "rabbit", "horse", "giant_horrible_monster"]
cat_found = "cat" in animals # TRUE
space_monster_found = "space_monster" in animals

```

## 7: Weather Types

We can use the *in statement* to discover which types of weather the `new_weather`*list* contains. At the start of this mission, we specified that the data set includes `Rain`, `Sunny`, `Fog`, `Fog-Rain`, and `Thunderstorm`. Let's verify that each of these types occurs in `new_weather`, and that we properly removed the *header* in a previous step.

### Instructions

- Loop through each item in the `weather_types` list.
- Check whether the item occurs in the `new_weather` list.
- Append the result of the check to `weather_type_found`.
- At the end, `weather_type_found` should be a *list* of Boolean values.

```
weather_types = ["Rain", "Sunny", "Fog", "Fog-Rain", "Thunderstorm", "Type of Weather"]
weather_type_found = []
for item in weather_types:
    found = item in new_weather
    weather_type_found.append(found)
```

## 8: Counting The Weather

In this mission, we covered *list slicing*, *columns*, and the *in statement*. We also formatted the weather data correctly so it will be ready for further analysis. In the next mission, we'll count up how many times each type of weather occurred in our data.

# Dictionaries

## 1: The Data Set

In this mission, we'll work with Los Angeles weather data from `2014`. The dataset is a *list* of *string* elements that represent weather patterns:


```
["Sunny"
 "Sunny"
 "Sunny",
 ...,
 "Fog"]
```

The *list* contains `365` elements. The first element is for the type of weather that occurred on January 1st, and the last element represents the type of weather that occurred on December 31st.

We've loaded the *list* into the `weather`variable.

### Instructions

- Assign the first element of `weather` to `first_element`and display it using the `print()` function.
- Assign the last element of `weather` to `last_element` and display it using the `print()` function.

```
# Weather has been loaded in.
first_element = weather[0]
last_element = weather[364]

print(first_element)
print(last_element)
```

## 2: Dictionaries

Let's say we have a set of students, along with their scores from a recent math test:

StudentScoreTom70Jim80Sue85Ann75

To store the students' names and their scores, we could use two lists:


```
students = ["Tom", "Jim", "Sue", "Ann"]
scores = [70, 80, 85, 75]
```

To figure out what score `Sue` got on the test, we'd first have to write a loop to find the *index* corresponding to the element `Sue` in the `students` *list*. We'd then have to find the value for that *index* in `scores`. Here's how we could do this:


```python
indexes = [0,1,2,3]
name = "Sue"
score = 0
for i in indexes:
    if students[i] == name:
        score = scores[i]
print(score)
```

```R
scores[which(students == "Sue")]
```

This is a complex piece of code for a simple task; we just want to find the value associated with a name.

To accomplish this in an easier way, we can use a *dictionary*. A *dictionary* is like a *list* in that it has *indexes*, but the *indexes* aren't necessarily sequential numbers. We can create our own indexes with values of any data type, including *strings*.

While we initiate a new list with square brackets (`[`), we create a new dictionary with curly braces (`{`). We can make an empty dictionary like this:


```python
scores = {}
```

To add values to an existing dictionary, we specify the *index* to the left of the equals sign, and the value it should have on the right side. We use square brackets (`[`) to specify the index.


```
scores["Tom"] = 70
```

Taken together, we call the index and value *key/value pairs*. In this mission, however, we'll refer to the dictionary values on the right side of the equals sign as *elements*, just like the elements in a list.

The code above will create the index `Tom` in the `scores` dictionary, and associate the element `70` with it. To look up the test score for `Tom`, we would simply write:


```
scores["Tom"]
```

This would return the element `70` because we associated it with the *index* `Tom` in the dictionary `scores`. We use square brackets (`[`) to add values to dictionaries or look up values.

We can add the rest of the students' scores in the same way:


```python
scores["Jim"] = 80
scores["Sue"] = 85
scores["Ann"] = 75
```

## 3: Practice Populating A Dictionary

Recall that to create a dictionary, we first define it with curly braces (`{`), then add values for specific *indexes*. We call the values *elements*, and refer to the *indexes*as *dictionary keys*:


```
students = {}
students["Jerry"] = 60
```

In the example above, we create an empty dictionary called `students`, then specify that the *dictionary key* `Jerry` should have the *value* `60`. To find the *value* (that's now an element) associated with the *dictionary key* `Jerry`, we'd look up `Jerry` in the dictionary `students`:


```
print(students["Jerry"])
```

The code above would display `60`.

A *dictionary key* can be a *string*, *integer*, or *float*:


```
students[10] = 100
```

### Instructions

- Assign the value `1` to the key `Aquaman` in a new dictionary named `superhero_ranks`.
- Assign the value `2` to the key `Superman` in `superhero_ranks`.

```
superhero_ranks = {}
superhero_ranks["Superman"] = 2
superhero_ranks["Aquaman"] = 1
```

## 4: Practice Indexing A Dictionary

We can look up values in dictionaries by using square brackets. When we pass in a *dictionary key*, we retrieve the value associated with that key:


```
students["Tom"]
```

### Instructions

- Look up `FDR` in `president_ranks` and assign the result to a new variable `fdr_rank`.
- Look up `Lincoln` in `president_ranks` and assign the result to a new variable `lincoln_rank`.
- Look up `Aquaman` in `president_ranks` and assign the result to a new variable `aquaman_rank`.

```
president_ranks = {}
president_ranks["FDR"] = 1
president_ranks["Lincoln"] = 2
president_ranks["Aquaman"] = 3
fdr_rank = president_ranks["FDR"]
lincoln_rank = president_ranks["Lincoln"]
aquaman_rank = president_ranks["Aquaman"]
```

## 5: Defining A Dictionary With Values

So far, we've created a dictionary and added elements to it in multiple steps:


```
students = {}
students["Tom"] = 60
students["Jim"] = 70
```

This approach is cumbersome when we want to add multiple dictionary keys. Fortunately, we can create a dictionary and add elements to it in a single step:


```
students = {
    "Tom": 60,
    "Jim": 70
}
```

In the example above, we create a dictionary, then specify that the *key* `Tom`should have the value `60`, and the *key*`Jim` should have the value `70`.

We do this by entering the *dictionary key*, then a colon (`:`), then the *value*. We separate each *key/value pair* with a comma. If we wanted to, we could add more students like this:


```python
students = {
    "Tom": 60,
    "Jim": 70,
    "Sue": 85,
    "Ann": 80
}
```

We can use this technique to specify as many *key/value pairs* as we'd like.

### Instructions

Create a dictionary named `animals` with the following *keys* and *values*:

- The key `7` corresponding to the value `raven`.
- The key `8` corresponding to the value `goose`.
- The key `9` corresponding to the value `duck`.

Create a dictionary named `times` with the following *keys* and *values*:

- The key `morning` corresponding to the value `9`.
- The key `afternoon` corresponding to the value `14`.
- The key `evening` corresponding to the value `19`.
- The key `night` corresponding to the value `23`.

```
random_values = {"key1": 10, "key2": "indubitably", "key3": "dataquest", 3: 5.6}
print(random_values)
animals = {7: "raven", 8: "goose", 9: "duck"}
times = {"morning": 9, "afternoon": 14, "evening": 19, "night": 23}
```

## 6: Modifying Dictionary Values

We can modify the elements in a dictionary, just like we can with a list:


```
students = {
    "Tom": 60,
    "Jim": 70
}
```

For example, we can replace the *element*we've associated with a *key*:


```
students["Tom"] = 65
```

The code above would change the *element*for the *key* `Tom` to `65`.

We can also modify an existing *element*:


```
students["Tom"] = students["Tom"] + 5
```

The code above would add `5` to the *value*for the *key* `Tom`.

### Instructions

- Add the *key* `Ann` and *value* `85` to the dictionary `students`.
- Replace the *value* for the *key* `Tom` with `80`.
- Add `5` to the *value* for the *key* `Jim`.

```
students = {
    "Tom": 60,
    "Jim": 70
}
students["Ann"] = 85
students["Tom"] = 80
students["Jim"] = students["Jim"] + 5
```

## 7: The In Statement And Dictionaries

In the last mission, we used the *in statement* to check whether an element occurred in a *list*:


```
animals = ["Cat", "Dog"]
found = "Cat" in animals
```

We can also use the *in statement* to check whether a *key* occurs in a dictionary:


```
students = {
    "Tom": 60,
    "Jim": 70
}
```

`"Tom" in students` would return `True`, and `"Sue" in students` would return in `False`. `60 in students`  is also `False` .

### Instructions

- Check whether `jupiter` is a key in `planet_numbers`, and assign the resulting Boolean value to `jupiter_found`.
- Check whether `earth` is a key in `planet_numbers`, and assign the resulting Boolean value to `earth_found`.

```
planet_numbers = {"mercury": 1, "venus": 2, "earth": 3, "mars": 4}
jupiter_found = "jupiter" in planet_numbers
earth_found = "earth" in planet_numbers
```

## 8: The Else Statement

We learned about the *if statement* in a previous mission. The *if statement* runs a segment of code if a condition is `True`:

```
if temperature > 50:
    print("It's hot!")
```

In the code above, the *if statement* checks whether the variable `temperature` is greater than `50`, and prints out `It's hot!` if it is.

We can also print a different message if the temperature is less than or equal to `50`:


```
if temperature > 50:
    print("It's hot!")
if temperature <= 50:
    print("It's cold!")
```

If `temperature` is greater than `50`, we'll see `It's hot!`, and if it's less than or equal to `50`, we'll see `It's cold!`. In other words, we print one statement when `temperature > 50` equals `True`, and another statement when `temperature > 50` equals `False`.

Performing different actions depending on whether a condition is true or false is a common scenario in programming. The *else statement* offers a simpler way to do this:


```
if temperature > 50:
    print("It's hot!")
else:
    print("It's cold!")
```

The code above is much simpler than the previous example, but results in the same outcome. If `temperature > 50` is `True`, then it executes the code in the if statement block. If `temperature > 50` is `False`, then it executes the code in the else statement block.

When using if/else statements, only one of the blocks will execute. That means the code above will only print `It's hot!` or `It's cold!` - never both.

## 9: Practicing With The Else Statement

Else statements allow us to simplify our code. Here's an example:


```
scores = [80, 100, 60, 30]
high_scores = []
low_scores = []
for score in scores:
    if score > 70:
        high_scores.append(score)
    else:
        low_scores.append(score)
```

The code above will add a `score` to `high_scores` if `score` is greater than `70`, and to `low_scores` otherwise.

### Instructions

Append any names in `planet_names` that are longer than `5`characters to `long_names`. Otherwise, append the names to `short_names`. To accomplish this:

- Loop through each item in `planet_names`.
- Use the [len()](https://docs.python.org/3/library/functions.html#len) function to find the length of the item.
- If the length is greater than `5`, append the item to `long_names`.
- Otherwise, append it to `short_names`.
- When complete, `short_names` should contain any planet names less than `6` characters long, and `long_names`should contain any planet names `6` characters or longer.

```
planet_names = ["Mercury", "Venus", "Earth", "Mars", "Jupiter", "Saturn", "Neptune", "Uranus"]
short_names = []
long_names = []
for planet in planet_names:
    if len(planet) > 5:
        long_names.append(planet)
    else:
        short_names.append(planet)
```

## 10: Counting With Dictionaries

We now have all the pieces we need to count how many times each element occurs in a *dictionary*. Let's practice our skills in the following exercise.

### Instructions

Count the number of times that each element occurs in the list named `pantry` that appears in the code block below. You'll need to:

- Create an empty dictionary named `pantry_counts`.
- Loop through each item in `pantry`.
- If the item appears in `pantry_counts`, add `1` to the *value*in `pantry_counts` for the item's key.Otherwise, add the item to `pantry_counts` as a *key*, with the *value* `1`.
- When finished, each item in `pantry` will have its own key in `pantry_counts`, and its value will be the number of times the item appears in `pantry`.

```
pantry = ["apple", "orange", "grape", "apple", "orange", "apple", "tomato", "potato", "grape"]
pantry_counts = {}

for item in pantry:
    if item in pantry_counts:
        pantry_counts[item] = pantry_counts[item] + 1
    else:
        pantry_counts[item] = 1
```

## 11: Counting The Weather

Now that we have some practice counting with dictionaries, it's time to count how often each type of weather occurs in the `weather` *list*.

### Instructions

- Count how many times each type of weather occurs in the `weather` list, and store the results in a new dictionary called `weather_counts`.
- When finished, `weather_counts` should contain a key for each different type of weather in the `weather` list, along with its associated frequency. Here's a preview of how the result should format the `weather_counts` dictionary (note that you'll be using real values, rather than the dummy ones below):

```
{
    'Fog': 0,
    'Fog-Rain: 0,
    ....
}

```

```
weather_counts = {}
for item in weather:
    if item in weather_counts:
        weather_counts[item] = weather_counts[item] + 1
    else:
        weather_counts[item] = 1
```

# Introduction to Functions

## 1: Overview

In this mission, we will work with a data set consisting of the 5000 highest-grossing movies of all time, according to the Internet Movie Database([IMDb](http://www.imdb.com/)). IMDb is an online extensive database for films, television programs and video games. Our end goal is to create a dictionary that stores useful statistics from this data set, named `movie_metadata`. In order to do this, we will:

- Clean data to make the information useful to us more easily accessible
- Practice using dictionaries in more complex functions
- Learn how to write our own functions!

As you can see in the console, `movie_metadata.csv` is a CSV file. You may recall that CSV stands for "Comma-Seperated Values", meaning that the values in the data set are seperated by commas. Similar to previous missions where we worked with CSV files, we need to represent the CSV file in a structure that is Python is familiar with, which would allow us to manipulate it. This time, we ask you to do all the necessary parsing.

Below is a table showing the first 5 movies in the list, but you may realize that there are 6 rows in the diagram. The first element of `movie_metadata` is not a movie, but instead is a list of the attributes that the other elements have. This is called a **header row**, and we will deal with it later in the mission.

| movie_title                              | director_name     | color | duration | actor_1_name    | language | country | title_year |
| ---------------------------------------- | ----------------- | ----- | -------- | --------------- | -------- | ------- | ---------- |
| Avatar                                   | James Cameron     | Color | 178      | CCH Pounder     | English  | USA     | 2009       |
| Pirates of the Caribbean: At the World's End | Gore Verbinski    | Color | 169      | Johnny Depp     | English  | USA     | 2007       |
| Spectre                                  | Sam Mendes        | Color | 148      | Christoph Waltz | English  | UK      | 2015       |
| The Dark Knight Rises                    | Christopher Nolan | Color | 164      | Tom Hardy       | English  | USA     | 2012       |
| Star Wars VII: The Force Awakens         | JJ Abrams         | Color | 136      | Harrison Ford   | English  | USA     | 2015       |

### Instructions

- Read `movie_metadata.csv` into a list of lists and assign to `movie_data`.Open and read the file `movie_metadata.csv` into a **string** variable.Split the data into rows on the newline character ("\n").Create an empty list, `movie_data`.Loop through each row, and split each row into a list on the comma character (","), and append it to `movie_data`.
- Display the first 6 lists in `movie_data` using the `print()`function.

```
f = open("movie_metadata.csv", "r")
movies = f.read()
split_movies = movies.split("\n")
movie_data = []
for each in split_movies:
    movie_data.append(each.split(","))

print(movie_data[0:5])
```

## 2: Motivating Functions

You may realize that parsing this file was not different than parsing any other file from the previous missions. You may also realize that we rarely rewrite the same code twice; instead, we use a method that allows us to reuse code, **functions** such as `print()`, `type()`, `open()`, etc. Now, we formally define functions.

As we have seen before, a function is a packaged body of code that we can reuse by **calling** with the relevant parameters. The parameters that a function takes are called the **inputs** of the function, and the result that it returns is called the **output**. For example, we called the `open()` function with two **inputs** (the strings "movie_metadata.csv" and "r"), and received the **output** which is a wrapper for the file `movie_metadata.csv`. All functions follow the same road map as `open()`: They take in input(s), execute the code that they surround, and return an output.

Because functions are reusable, we can package all the parsing we just did into one function. Then, we can call the function whenever we need to parse a file instead of having to rewrite the necessary code every time. We have been making extensive use of functions, so this should not be unfamiliar. If we had a `parser()` function, the code for the last instructions would be as consice as this:


```
>>> movie_data = parser(movie_metadata)
>>> print(movie_data[0:5])
[['movie_title', 'director_name', 'color', 'duration', 'actor_1_name', 'language', 'country', 'title_year'], ['Avatar\xa0', 'James Cameron', 'Color', '178', 'CCH Pounder', 'English', 'USA', '2009'], ["Pirates of the Caribbean: At World's End\xa0", 'Gore Verbinski', 'Color', '169', 'Johnny Depp', 'English', 'USA', '2007'], ['Spectre\xa0', 'Sam Mendes', 'Color', '148', 'Christoph Waltz', 'English', 'UK', '2015'], ['The Dark Knight Rises\xa0', 'Christopher Nolan', 'Color', '164', 'Tom Hardy', 'English', 'USA', '2012']]
```

Other than reusability, there are 3 main advantages of using functions:

- They allow us to use other people's code without the necessity to have a deep understanding of how it was written (e.g., we use the `print()` function without reading the code inside it). We call this **information hiding**.
- They break down complex logic into smaller components or modules. Instead of writing very lengthy and complicated code, we can progress function by function. For example, if we were writing a larger piece of code, `parser()` as a function would be easier to manage rather than the code that executes the same behavior. This would make testing easier as well. We refer to this as **modularity**, which is especially important when working on teams. Modularity makes it easier for someone else to read, understand, use, and build upon our code.
- They streamline our code and make it easier to maintain. Programmers reuse the same functions in multiple situations across a project. This means that they generalize the function as much as possible to maximize its usefulness. we call this process **abstraction**, which is an important part of reducing our code's complexity, especially for larger projects.

Knowing the usefulness of functions, let's see how we can write our own functions.

## 3: Writing Our Own Functions

Up until now, we used **built-in** functions: functions that Python has defined for us. However, our toolbox is not limited to this; we can write our own functions. The syntax for defining a function consists of 5 parts:

- `def` keyword - For Python to interpret the following code as a function
- Name - To refer to when we need to call the function later
- Arguments - Input value(s) that the function takes in
- Body - The code that the function executes
- Return value - The value that the function returns to the user when the function terminates

Let us examine the syntax further, using an example function that returns the first element of a list:


```python
def first_elt(input_lst):
    first = input_lst[0]
    return first
```

```R
first_elt <- function(input_lst) {
  first <- input_lst[1]
  return(first)
}
```

We start the function definition with the keyword `def`. We give the function a name that explains its use, in this case: `first_elt()`. Then, we name the single argument that the function takes. Here, we name the argument `input_lst`, suggesting to the user that the function must take in a list as input. In the next line, we define the body of the function, which consists of only one line in our case. This is the actual code that will be executed when the function is called. Finally, we use the keyword "return" to signify the end of the function, and type the variable that we want returned to the user, in this case, `first`.

Here, we should take note of the **indentation** of the function. Realize that after the semicolon, we indent the remainder of the function by one `tab`, which is the equivalent of 4 `space bar`strokes. This is to clarify to Python what part of the code belongs to the function. Therefore, indentation makes functional differences in Python's interpretation of the code. You may notice that if you format the first line of the function corretly, so that Python recognizes that the next line will be the beginning of the function body, it indents the next line by 1 `tab` automatically. To make this behavior clear, you can see the diagram below and try for yourself. Notice that we do not need to press `tab` or `space bar`; Python automatically puts the indentation when we press `enter`.

![img](https://s3.amazonaws.com/dq-content/4/second+auto+indent.gif)

One other thing to note is that `first` and `input_lst` are **temporary variables**, which means that they are only accessible inside the function. If you attempt to use `first` somewhere else in the code outside the function, you will get an error saying that `first` is undefined. For example, observe the example below:

![img](https://s3.amazonaws.com/dq-content/4/scope+error.png)

This eror occurs because `first` does not have a defined value outside of the `first_elt()` function. Then, in order to access the return value of this function, we set it equal to a variable, so the variable gets the function's return value. Here is the fix:

![img](https://s3.amazonaws.com/dq-content/4/scope+fix.png)

### Instructions

- Write a function, with a definition, name, argument(s), body and return value, that returns a list containing the names of the movies in `movie_data`. This function is expected to behave similar to `first_elt()`, but for multiple lists.
  - Give the function a name that describes what it does; `first_elts()` is a good example, but feel free to be creative.
  - Declare an empty list.
  - Use a `for` loop to extract the first element of each list, and append these elements to the empty list.
  - Return the list.
- Assign the returned list to `movie_names`.
- Display the first 5 elements of `movie_names` using the `print()` function.

```
def first_elts(input_lst):
    elts = []
    for each in input_lst:
        elts.append(each[0])
    return elts

movie_names = first_elts(movie_data)
print(movie_names[0:5])
```

## 4: Functions With Multiple Return Paths

Even though we suggested `return`signifies the end of a function, a function can have multiple return statements. We can take advantage of this to add an **if statement** that returns a value if a certain criteria is met, and another value otherwise. For example, let's take a look at a function that checks whether or not the first element of a list is "blah":


```
def is_blah(input_lst):
    if input_lst[0] == "blah":
        return True
    else:
        return False
```

Here, the indentation gives us the necessary intuition to decipher how the function works. If the list's first element is the string "blah", then the first return will execute, and the function will end there. However, if the first element is not "blah", then the function will continue without executing `return True`, because it doesn't enter that path. Instead, it will continue to the `else` path, and end by executing `return False`.

Notice that there is a further layer of indentation after the `if` and `else`statements. Python can do this indentation automatically as well. Here is an example of auto indentation with a `for` loop:

![img](https://s3.amazonaws.com/dq-content/4/second+auto+indent+second.gif)

### Instructions

- Write a function named `is_usa()` that checks whether or not a movie was made in the United States.Check the `movie_metadata.CSV` file to see which column corresponds to the nationality of the movie. Don't forget to subtract one to find the true index of the column in the list.Use an **if statement** to check the right column of the list with the word "USA". The equality operation is case sensitive, so make sure to get the capitilization right.Return `True` if the condition is met, and `False`otherwise.
- Try it with a few movies in `movie_data`.
- Call it on `wonder_woman` and store the result in `wonder_woman_usa`.

```
wonder_woman = ['Wonder Woman','Patty Jenkins','Color',141,'Gal Gadot','English','USA',2017]
def is_usa(input_lst):
    if input_lst[6] == "USA":
        return True
    else:
        return False
wonder_woman_usa = is_usa(wonder_woman)
```

## 5: Functions With Multiple Arguments

This function works, but its use is quite narrow. If we wanted to check if the first value of the 7th column is "UK" instead, we would have to write a completely seperate function:


```
def is_uk(input_lst):
    if input_lst[6] == "UK":
        return True
    else:
        return False
```

However, you can see that this function is almost the same with `is_usa()`, except for the string they check for. This can give us the intution that there is another layer of abstraction we can perform. If we could write a function that takes in two inputs, namely, the list and the string to check for, we could eliminate the inefficieny of writing the same code twice. Fortunately, we can exactly do that:


```
def equals_str(input_lst,input_str):
    if input_lst[0] == x:
        return True
    else:
        return False
```

Now, `is_usa(input_lst)` behaves the same way as `equals_str(input_lst,"USA")`, and `is_uk(input_lst)` behaves the same way as `equals_str(input_lst,"UK")`.

Because there is more than one argument in this function, the order with which we call the arguments becomes important. for example, `equals_str(movie_data[4], "UK")` would be correct; however, `equals_str("UK",movie_data[4])` would not, because the function expects to get the list first and the string second. If we want to override this, we have to used **named arguments** instead of the default, **positional arguments**. If we explicitly write the names of the arguments as we provide them, their positions become unimportant. This means that `equals_str(input_str="UK",input_lst=movie_data[4])` does not result in an error. Naming arguments does not add any functionality, but it may embellish the readability of the code, which is important if you are working on a team.

Finally, we can abstract out another layer by adding a third argument that will determine which column of the list the checked attribute is.

### Instructions

- Write a function `index_equals_str()` that takes in three arguments: a list, an index and a string, and checks whether that index of the list is equal to that string.
- Call the function with a different order of the inputs, using named arguments.
- Call the function on `wonder_woman` to check whether or not it is a movie in color, store it in `wonder_woman_in_color`, and print the value.

```
wonder_woman = ['Wonder Woman','Patty Jenkins','Color',141,'Gal Gadot','English','USA',2017]

def is_usa(input_lst):
    if input_lst[6] == "USA":
        return True
    else:
        return False

def index_equals_str(input_lst,index,input_str):
    if input_lst[index] == input_str:
        return True
    else:
        return False

wonder_woman_in_color = index_equals_str(wonder_woman,2,"Color")
print(wonder_woman_in_color)
```

## 6: Optional Arguments

In the beginning of the mission, we observed that the first row of `movie_metadata` is not an element of the data itself, but is a list of the attributes that defines that data. Although this is useful, there is no need for this row to be a part of our `movie_data`, and can actually cause misinformation. Let's say we want to count the number of movies in the list. Our intuition might be to do the following:


```
def naive_counter(input_lst):
    num_elt = 0
    for each in input_lst:
        num_elt == num_elt + 1
    return num_elt
```

However, if we attempt to call this function with `movie_data` as its argument, we get a wrong answer:


```
>>> print(naive_counter(movie_data))
5001
```

This is because the first item in the list is also counted by the counter. Of course, we can get around this by subtracting one from the result, but manipulating the function that way would cause it to be unusuble in cases where there is no header row. This is not generalizable, so it is not a neat solution. Instead, we can use an argument that has a default value that can be manipulated, we call this an **optional argument**. Optional arguments have default values that they take on unless a different value is provided by the user.

In this case, the default value for an argument that determines whether or not there is a header row would be `False`, because most datasets do not have header rows. However, when we encounter a dataset like this one, we can call the counter by explicit telling it that there is a header row. Let's modify the function to have this behavior by adding an optional parameter:


```
def counter(input_lst,header_row = False):
    num_elt = 0
    if header_row == True:
        input_lst = input_lst[1:len(input_lst)]
    for each in input_lst:
        num_elt = num_elt + 1
    return num_elt
```

Now, the function will behave as we expected:


```
>>> print(counter(movie_data))
5001
>>> print(counter(movie_data, True))
5000
```

If we are concerned about the readibility of our code by co-workers, we can name the optional argument as well:


```
>>> print(counter(movie_data, header_row = True))
5000
```

This way, if there are multiple optional arguments, and you want to provide a latter optional argument, then you can name the arguments and guarantee that Python understands which input corresponds to which argument.

### Instructions

- Write a function `feature_counter()`, combining `index_equals_str()` and `counter()`, which counts the number of elements that fulfill the requirement in `index_equals_str()`.
- Use this to find out how many of the movies were made in USA, and store the value in `num_of_us_movies`.

```
def index_equals_str(input_lst,index,input_str):
    if input_lst[index] == input_str:
        return True
    else:
        return False
def counter(input_lst,header_row = False):
    num_elt = 0
    if header_row == True:
        input_lst = input_lst[1:len(input_lst)]
    for each in input_lst:
        num_elt = num_elt + 1
    return num_elt

def feature_counter(input_lst,index, input_str, header_row = False):
    num_elt = 0
    if header_row == True:
        input_lst = input_lst[1:len(input_lst)]
    for each in input_lst:
        if each[index] == input_str:
            num_elt = num_elt + 1
    return num_elt

num_of_us_movies = feature_counter(movie_data,6,"USA",True)
print(num_of_us_movies)
```

## 7: Calling A Function Inside Another Function

Now, we have all the tools we need to create the statistics summary function we explained in the beginning of the mission. However, we would like `summary_statistics()` to be a function itself, and re-writing all of the code inside `feature_counter()` in `summary_statistics()` defies the purpose of using a function. You may remember that one of the big advantages of using a function is **abstraction**: the fact that it saves us from having to write the same code twice. In this vein, the last feature of functions that we will use is the ability to call a function inside another function.

The body of one function can include a call to another function. We have already seen this, because comparison operators such as the equality operator (`==`) and arithmetic operations such as sum (`+`) and minus (`-`) are functions as well. Similarly, we can call built-in or user-created functions by making their return values equal to a variable in the outer function, so that we can use that variable in the function.

Let's say we want to build a function `list_counter()` that will count the elements in multiple lists, and make a seperate list holding these values. This is how we want the function to operate:


```
>>> lists = [["dog","cat","rabbit"],[1,2,3,4],[True]]
>>> list_count = (list_counter(lists))
>>> print(list_count)
[3,4,1]
```

Even though this seems like a complicated problem, because we have a counter function, it will not take more than 6 lines:


```
def list_counter(input_lst):
    final_list = []
    for each in input_lst:
        num_elt = counter(each)
        final_list.append(num_elt)
    return final_list
```

As you can see, we called the user-defined function `counter()` and assigned its return value to `num_elt`. Each time the for loop starts, the counter will be called with a different argument (the current value assigned to `each`), and return a different value. Whenever we define a new function, we can call it inside another function using this syntax.

Similarly, we can make use of `feature_counter()` when we are building our final function. Now, you are ready to build `summary_statistics()`!

### Instructions

- Write a `summary_statistics()` function that will take `movie_data` a input, and output a dictionary that will give useful numbers from the data.Define `summar_statistics()` with one argument, an input list.Use the `feature_counter()` with the relevant arguments to count the following properties and make them equal to the corresponding variables.number of movies made in Japan --> `num_japan_films`number of movies in black and white --> `num_color_films`number of movies in English --> `num_films_in_english`Create a dictionary that associates the keys (`japan_films`,`color_films`,`films_in_english`) with the corresponding variables.Return the dictionary.
- Call the function with `movie_data` as its input, and store its value in `summary`.

```
def feature_counter(input_lst,index, input_str, header_row = False):
    num_elt = 0
    if header_row == True:
        input_lst = input_lst[1:len(input_lst)]
    for each in input_lst:
        if each[index] == input_str:
            num_elt = num_elt + 1
    return num_elt
def summary_statistics(input_lst):
    num_japan_films = feature_counter(input_lst,6,"Japan",True)
    num_color_films = feature_counter(input_lst,2,"Color",True)
    num_films_in_english = feature_counter(input_lst,5,"English",True)
    summary_dict = {"japan_films" : num_japan_films, "color_films" : num_color_films, "films_in_english" : num_films_in_english}
    return summary_dict

summary = summary_statistics(movie_data)
```

## 8: Next Steps

Congratulations! In this mission, we explored how we can write our own functions, how we can use multiple paths, multiple and optional arguments, and call functions inside one another. Meanwhile, we built a counter, and ultimately a dictionary that uses this counter to store useful statistics from IMDb's 5000 top grossing movies data set. Next, we will see what can go wrong when we are writing a function and learn the meanings of the most common errors.

# Customizing Functions and Debugging Errors

## 1: Overview

In this mission, we'll explore how to customize the functions we write, and apply what we've learned to improve our spell checker from the previous mission. For example, as code becomes increasingly modular (separated into functions), it can also become harder to debug. We'll delve into how to use the errors Python returns to debug our code.

Recall that our spell checker works by:

- Reading in a file of correctly spelled words, tokenizing it into a list, and assigning it to the variable `vocabulary`
- Reading in, cleaning, and tokenizing the text we want to spell check
- Comparing each word (token) in the text with each word in `vocabulary`, and returning the ones it doesn't find

The file `dictionary.txt` contains a sequence of correctly spelled words, which we'll use to seed the vocabulary. The file `story.txt` contains a piece of text with some misspelled words. We added the current version of the spell checker as we wrote it in the previous mission to the following code cell.

## 2: Multiple Return Paths

So far we've explored with functions that have one `return` statement, and therefore one flow of execution. However, we've also learned how to use `if` statements to express conditional logic and create multiple return paths. Adding multiple ways to return a value can stop the flow of execution earlier and make our code run faster. Let's examine what this looks like.

The `flip_coin()` function below expects a *float* input value, and will either return `"Heads"` or `"Tails"`, depending on the value we pass in:


```
def flip_coin(heads_probability):
    if heads_probability >= 0.5:
        return("Heads")
    else:
        return("Tails")
```

Multiple return paths make our functions more robust and more general by allowing us to write code that captures a wider range of outcomes. If each function could only contain one return path, then we'd have to write the following two functions to replicate the above behavior:


```
def is_heads(heads_probability):
    if heads_probability >= 0.5:
        return("Heads")

def is_tails(heads_probability):
    if heads_probability < 0.5:
        return("Tails")
```

In the next step, we'll learn how to combine multiple return paths with optional arguments to make the `tokenize()` function more flexible.

## 3: Optional Arguments

After we clean up the story text, we'll need to tokenize both it and the vocabulary. The process for both is actually the same (splitting on the whitespace character). To make our code more reusable, we can technically use the `tokenize()` function on both files. The vocabulary list doesn't need cleaning, however, so we need a way to specify when we want the input *string*to clean, and only run the cleaning logic in those cases.

We can accomplish this by modifying the `tokenize()` function in a few ways. First, we can wrap the code that cleans the input *string* in an `if` statement, so that it only runs if the Boolean value that's associated with an argument named `clean` evaluates to `True`. Then, we can add `clean` to the definition for the `tokenize()` function. This way, whenever we want to clean the input *string*, we just set `clean` to the Boolean value `True`. When we don't, we set `clean` to the Boolean value `False`.

One downside to adding more arguments is that anyone using our function would need to spend more time deciding on the values for them. In contrast, all of the arguments in the current version of the function are currently required - we have to pass in a value for each one to avoid returning an error. To add an optional argument *and* save time, we could specify a default value for it. When we call that function, we won't be required to pass in that parameter, and the Python interpreter will resort to the default value instead.

In the following code block, we modify the definition for the `tokenize()` function by adding a new parameter named `clean`, and specifying a default value for it:


```
def tokenize(text_string, special_characters, clean=False):
```

We don't need to pass in a third parameter; instead, Python will set `clean`to `False` by default. We can then use the same `tokenize()` function for both the vocabulary and the story text:


```
# We want story_string cleaned, so we set the third parameter to `True`.
tokenized_story = tokenize(story_string, clean_chars, True)
# Since we didn't pass in a value for the third parameter, `clean` will be set to `False` by default.
tokenized_vocabulary = tokenize(vocabulary, clean_chars)
```

We're still not done, though. Even though we made the `clean` parameter optional, we need to still update the code in the body of our function to incorporate the `clean` parameter. By default, we don't want to clean `text_string`. We therefore need to check whether `clean` is `True`, and only clean `text_string` if it is.

### Instructions

Modify the `tokenize()` function:

- Use an `if` statement to check whether `clean` is `True`. If so:Clean `text_string` using `clean_text`, and assign the returned *string* to a variable.Tokenize the new *string* variable using the `split()`method, and assign the returned *list* to another new variable.Return this *list*.
- Outside the `if` statement, write the code we want executed if `clean` is `False`:Tokenize `text_string` using the `split()` method, and assign the returned *list* to a new variable.Return this *list*.

Outside the `tokenize()` function:

- Use the `tokenize()` function to clean and tokenize `story_string`, and assign the result to `tokenized_story`.
- Use the `tokenize()` function to tokenize `vocabulary`, and assign the result to `tokenized_vocabulary`.
- Finally, loop over each element in `tokenized_story` and check whether it exists in `tokenized_vocabulary`. If it doesn't, add it to `misspelled_words`.

```
# Default code
def tokenize(text_string, special_characters, clean=False):
    cleaned_story = clean_text(text_string, special_characters)
    story_tokens = cleaned_story.split(" ")
    return(story_tokens)

clean_chars = [",", ".", "'", ";", "\n"]
tokenized_story = []
tokenized_vocabulary = []
misspelled_words = []

# Answer code
def tokenize(text_string, special_characters, clean=False):
    # If `clean` is `True`.
    if clean:
        cleaned_story = clean_text(text_string, special_characters)
        story_tokens = cleaned_story.split(" ")
        return(story_tokens)
    # If `clean` not equal to `True`, no cleaning.
    story_tokens = text_string.split(" ")
    return(story_tokens)

clean_chars = [",", ".", "'", ";", "\n"]
tokenized_story = tokenize(story_string, clean_chars, True)
tokenized_vocabulary = tokenize(vocabulary, clean_chars)

for ts in tokenized_story:
    if ts not in tokenized_vocabulary:
        misspelled_words.append(ts)
```

## 4: Named Arguments

So far, we've been passing in arguments to the functions we've written in the same order in which we originally specified them in the function definition. The Python interpreter uses the positions of the arguments passed in to assign each one to a specific variable inside the function. They're known as positional arguments for this reason. However, remembering the exact order of the arguments can become difficult as we increase the number of them a function can take. This is especially true with optional arguments.

To help alleviate this problem, the Python interpreter allows us to pass in **named arguments** in any order. This allows us to be more explicit in assigning values to the variables inside the function. When we call a function, we assign each value to the variable from the function definition.

Under the hood, Python uses a dictionary to map the argument names to the values we want when we call the function. Then, it uses that dictionary to make the values we passed in available within the function. Let's see what this looks like in code:


```
# All three of these statements assign the same values to the function arguments.
tokenized_story = tokenize(clean=False, text_string = story_string, special_characters = clean_chars)
tokenized_story = tokenize(text_string = story_string, clean=False, special_characters = clean_chars)
tokenized_story = tokenize(special_characters = clean_chars, text_string = story_string, clean=False)
```

Notice that the parentheses in the code above now include equals signs (=) that assign values to named arguments.

Because optional arguments have defaults, we can leave out the `clean` parameter altogether.


```
tokenized_story = tokenize(text_string=story_string, special_characters=clean_chars)
```

In later courses, we'll explore external libraries that have functions containing 10 or more optional arguments. The programmers who made them took advantage of named and optional arguments to create flexible functions that can work for many different use cases.

The one caveat is that we can't have named parameters before positional arguments. When calling functions, we should specify the positional arguments first, and then the named arguments.

### Instructions

- Explore named arguments by uncommenting the starter code and running the different function calls.
- Use the `print()` function to verify the returned values are the same across the different function calls.
- Click **Next** once you're done exploring.

## 5: Practice: Creating A More Compact Spell Checker

Let's practice what we've learned about functions so far by creating a function containing all the logic for our spell checker. We'll be able to use this function in other projects by simply remembering the arguments it takes, rather than the specific logic behind how it works. This is especially critical when we're working with many team members, and on a much larger code base that could contain thousands of functions.

### Instructions

Create a function called `spell_check()`:

- Include the following arguments:
  - `vocabulary_file` - the location of the vocabulary text file
  - `text_file` - the location of the text we want to spell check
  - `special_characters` - with the default value of the *list* `[",",".","'",";","\n"]`.
- In the function body:
  - Create an empty *list* and assign it to `misspelled_words`.
  - Read both files into *strings* using the `open()` and `read()` functions: `open(vocabulary_file).read()`
  - Call the `tokenize()` function to tokenize the *string*containing the vocabulary. Assign the result to `tokenized_vocabulary`.
  - Call the `tokenize()` function to clean and tokenize the *string* containing the text we want to spell check. Assign the result to `tokenized_text`.
  - Write a `for` loop that iterates over `tokenized_text`. For each token in `tokenized_text`:Write an `if` statement that checks whether the token *isn't* in `tokenized_vocabulary`, *and* if it's not equal to `''`.If it meets both criteria, append the token to `misspelled_words`.
  - Outside the `for` loop, return `misspelled_words`.

After we've written the function, call `spell_check`:

- Pass in `story.txt` as the `text_file` parameter.
- Pass in `dictionary.txt` as the `vocabulary_file`parameter.
- Use the default value for `special_characters`.
- Assign the result to `final_misspelled_words`.
- Use the `print()` function to display `final_misspelled_words`.

```
def clean_text(text_string, special_characters):
    cleaned_string = text_string
    for string in special_characters:
        cleaned_string = cleaned_string.replace(string, "")
    cleaned_string = cleaned_string.lower()
    return(cleaned_string)

def tokenize(text_string, special_characters, clean=False):
    cleaned_text = text_string
    if clean:
        cleaned_text = clean_text(text_string, special_characters)
    tokens = cleaned_text.split(" ")
    return(tokens)

final_misspelled_words = []
def spell_check(vocabulary_file, text_file, special_characters=[",",".","'",";","\n"]):
    misspelled_words = []
    vocabulary = open(vocabulary_file).read()
    text = open(text_file).read()
    tokenized_vocabulary = tokenize(vocabulary, special_characters)
    tokenized_text = tokenize(text, special_characters, True)
    for ts in tokenized_text:
        if ts not in tokenized_vocabulary and ts != '':
            misspelled_words.append(ts)
    return(misspelled_words)

final_misspelled_words = spell_check(vocabulary_file="dictionary.txt", text_file="story.txt")
print(final_misspelled_words)
```

## 6: Types Of Errors

As we begin to take greater advantage of functions to organize our code, it can become more complex. We need to better understand the kinds of mistakes we can make when writing it. We've talked briefly about errors, or mistakes that prevent our code from working as we expect, in previous missions. Now it's time to learn more about them.

The two main types of errors are:

- Syntax errors
- Runtime errors

Before code can be run, it must be parsed by the Python interpreter and organized into a data structure that represents the flow and complexity of the code we wrote. If the interpreter encounters any code that doesn't adhere to Python's language rules, it halts the parsing and returns an error. Rich coding environments like [Atom](https://atom.io/) and [IPython Notebook](http://ipython.org/notebook.html) help us prevent these types of errors through syntax highlighting - a feature that displays different parts of our code (such as brackets) in different colors. Syntax highlighting makes it easier to read our code and spot errors. Some examples of syntax errors include:

- Missing ending quotes or starting quotes
- Using improper indentation
- Using improper keywords

Runtime errors only occur when the code is actually running, which makes them harder to catch and prevent beforehand. Some examples of runtime errors include:

- Calling a function before it's defined
- Calling a method or attribute that the object doesn't contain
- Attempting to convert a value to an incompatible data type

Let's explore some more specific examples of these types of errors.

## 7: Syntax Errors

Here's a simple example of a syntax error:


```
# Missing ending quotes.
the_answer = "42
```

When we run the above line of code, we'll get back an error message describing the mistake we made, as well as the Python interpreter's best guess as to where it occurred:

![Imgur](https://dq-content.s3.amazonaws.com/zzgLM9r.png)

Python uses the `SyntaxError` class to represent syntax errors, and displays the error message after the colon. The interpreter may sometimes struggle to pinpoint the problematic code that caused the error. In the following code block, for example, we attempt to define the `find()`function, but misspell the `def` keyword as `de`:


```
# `def` keyword misspelled as `de`.
de find():
    print("42")
```

The error message suggests that the mistake was in the function name, rather than the `def` keyword:

![Imgur](https://dq-content.s3.amazonaws.com/BYgFbrP.png)

Sometimes the Python interpreter will return an `IndentationError` instead of a `SyntaxError`. This object represents a more specific syntax error that makes it easier to debug our code. We'll see an `IndentationError` when the indentation in our code is inconsistent. Here's an example:


```
def find():
    print("42")
     print("what, really?")
```

Notice that the second print statement is indented differently than the first one (with one extra space). It also doesn't follow the indentation rules for code blocks such as `if` statements and `for`loops. It will return the following error:

![Imgur](https://dq-content.s3.amazonaws.com/tBdYOom.png)

Let's practice debugging and fixing syntax errors in the spell checker code from the previous step.

### Instructions

- The starter code contains multiple syntax errors. Scan and edit the code to resolve these errors.
- When you're ready, click **Check** to see if any errors are returned.

```
def spell_check(vocabulary_file, text_file, special_characters=[",",".","'",";","\n"):
    misspelled_words = []
    vocabulary = open(vocabulary_file).read()
    text = open(text_file.read()
    
     tokenized_vocabulary = tokenize(vocabulary, special_characters)
    tokenized_text = tokenize(text, special_characters, True)
    
    for ts in tokenized_text:
        if ts not in tokenized_vocabulary and ts != '':
```

```
            misspelled_words.append(ts)
```

```
    return(misspelled_words)
```

```
final_misspelled_words = spell_check(vocabulary_file="dictionary.txt", text_file="story.txt")
```

```
print(final_misspelled_words)
```

```
# Add an ending bracket for the `special_characters` default value.
```

```
def spell_check(vocabulary_file, text_file, special_characters=[",",".","'",";","\n"]):
```

18

```
    misspelled_words = []
```

19

```
    vocabulary = open(vocabulary_file).read()
```

20

```
    # Add ending parentheses.
```

21

```
    text = open(text_file).read()
```

22

```
    
```

23

```
    # Fix indentation.
```

24

```
    tokenized_vocabulary = tokenize(vocabulary, special_characters)
```

25

```
    tokenized_text = tokenize(text, special_characters, True)
```

26

```
    
```

27

```
    for ts in tokenized_text:
```

28

```
        if ts not in tokenized_vocabulary and ts != '':
```

29

```
            misspelled_words.append(ts)
```

30

```
    return(misspelled_words)
```

31


32

```
final_misspelled_words = spell_check(vocabulary_file="dictionary.txt", text_file="story.txt")
```

33

```
print(final_misspelled_words)
```

## 8: Runtime Errors

Runtime errors are very common. While code often works in predictable situations, runtime errors occur when it fails at handling a case the programmer didn't account for.

Because runtime errors occur when our code is running and can't be detected during parsing, they're more difficult to prevent than syntax errors. As you become more proficient in programming, however, you'll learn to identify potential runtime errors beforehand and prevent them from occurring. Python and most other programming languages include tools like error handling and automated tests that help you manage and reduce runtime errors. As your code becomes more complex, you'll learn how to incorporate errors into the functions you write to so that they fail gracefully, and prevent certain negative behavior from occuring.

[The documentation for Python 3.5](https://docs.python.org/3/library/exceptions.html#IndentationError) includes a full list of possible errors. If you glance at the hierarchy of errors [here](https://docs.python.org/3/library/exceptions.html#exception-hierarchy), the `SyntaxError` class is a small fraction of the entire tree of possibilities; the rest are runtime errors. Let's look at some examples of runtime errors.

## 9: TypeError And ValueError

In the following code, we try to concatenate a *string* and an *integer*. This returns a `TypeError` because the Python interpreter expects values being added to be the same type.


```
forty_two = 42
forty_two + "42"
```

Specifically, it returns the following error:

![Imgur](https://dq-content.s3.amazonaws.com/jlnvKvl.png)

You may have noticed that runtime errors look a little different. For example, the error appears twice (`TypeError` is in the top left and bottom left corners). In addition, the text `Traceback (most recent call last)` appears at the top right corner. The traceback displays all the code that was executed, ending with the most recent call that actually caused the error. While the code in this example is very simple, we'll explore some errors where the traceback is more useful later in this mission.

Another common runtime error is the `ValueError`, which is generated when the type is correct but the value is still improper. A `ValueError` is returned when we try to convert a *string* representing a non-numeric value into a numeric type, such as a *float*. Recall that we use the `float()` function to cast, or convert, a value to a *float*:


```
float("guardians")
```

The example above will return this error:

![Imgur](https://dq-content.s3.amazonaws.com/YXGq8A0.png)

While trying to cast a *string* to a *float* isn't automatically an issue (which is why there was no `TypeError`), the specific value that we tried to cast was problematic. The `float()` function didn't know how to cast `"guardians"` into a *float*, and returned an instance of `ValueError` instead.

### Instructions

The code cell contains the sample code from this step. Experiment with it to explore other runtime errors.

You could try:

- Concatenating an *integer* to a *string*, instead of the other way around
- Casting `"guardians"` into an *integer* instead using the `int()` function

When you're done exploring, click **Next** to move onto the next step.

## 10: IndexError And AttributeError

The `IndexError` is a common error that's returned when we try to access an element that's not in a *list's*index. Trying to access the fifth element in a *list* containing only two elements would return this error, for example. Here's what this looks like:


```
lives = [1,2,3]
lives[4]
```

The example above will return the following error:

![Imgur](https://dq-content.s3.amazonaws.com/J0PUtGA.png)

Since there's no value at index four, an `IndexError` is returned, along with an arrow pointing to the problematic line of code. If we're working with a *list* and don't know its length, use the `len()` function to look up the number of elements before attempting to access them.

The final runtime error we'll explore is the `AttributeError`. This occurs when we try to call a method or attribute on an object that doesn't contain it. In the following code, we try to call the `split()` method on the File handler instance, instead of using the `read()` method to read it into a *string* first:


```
f = open("story.txt")
f.split(" ")
```

Here's the error that's returned:

![Imgur](https://dq-content.s3.amazonaws.com/bRkRTWS.png)

`TextIOWrapper` is a built-in Python object that represents the File handler. It does not contain the `split()`method. Since the Python interpreter couldn't find the `split()` method within the `TextIOWrapper` class, it returned an instance of `AttributeError`.

## 11: Traceback

When calling a function that uses other functions, our function calls become **nested**. This can make it harder to debug, since the code that triggered the error is usually inside the function, and different than the code we called. In the following example, we try to pass an integer in to the `special_characters` parameter. This is problematic because the code in the `spell_check()` body was written under the assumption that `special_characters`would always be a *list* of *strings*:

![Imgur](https://dq-content.s3.amazonaws.com/vwMkXAa.png)

The traceback shows the series of function calls that occurred. The topmost function call is the highest level of code we wrote, and oftentimes the section we need to fix. The last function call is where the error actually occurred.

### Instructions

- Edit the default code and remove the error.
- Don't set a value for the `special_characters` argument. Instead, let the `spell_check()` function use the default value for it.


```
def spell_check(vocabulary_file, text_file, special_characters=[",",".","'",";","\n"]):
    misspelled_words = []
    vocabulary = open(vocabulary_file).read()
    # Add ending parentheses.
    text = open(text_file).read()
    
    # Fix indentation.
    tokenized_vocabulary = tokenize(vocabulary, special_characters)
    tokenized_text = tokenize(text, special_characters, True)
    for ts in tokenized_text:
        if ts not in tokenized_vocabulary and ts != '':
            misspelled_words.append(ts)
    return(misspelled_words)
    
final_misspelled_words = spell_check(vocabulary_file="dictionary.txt", text_file="story.txt", special_characters=1)
print(final_misspelled_words)
def spell_check(vocabulary_file, text_file, special_characters=[",",".","'",";","\n"]):
    misspelled_words = []
    vocabulary = open(vocabulary_file).read()
    # Add ending parentheses.
    text = open(text_file).read()
    
    # Fix indentation.
    tokenized_vocabulary = tokenize(vocabulary, special_characters)
    tokenized_text = tokenize(text, special_characters, True)
    
    for ts in tokenized_text:
        if ts not in tokenized_vocabulary and ts != '':
            misspelled_words.append(ts)
    return(misspelled_words)
    
final_misspelled_words = spell_check(vocabulary_file="dictionary.txt", text_file="story.txt")
print(final_misspelled_words)
```

## 12: Next Steps

In this mission, we customized functions in new ways, practiced using these techniques to create a compact spell checker function, and learned how to identify and debug common errors. These strategies will help you account for potential errors and write code that's easier to maintain. Next in this course is your first guided project, where we'll explore how to use the Jupyter notebook environment for writing code, exploring data, and more.
