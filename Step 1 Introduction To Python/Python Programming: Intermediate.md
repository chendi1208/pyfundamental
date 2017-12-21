# Classes

## 1: Introduction To Classes And Objects

In this mission, we are going to be introducing the concepts behind **classes** and **objects**. In your path to becoming proficient in Python, you have been introduced to classes and objects everywhere without knowing it. Python is known as an **object-oriented programming language** which means that **everything in Python is an object**! This means that integers, floats, strings, and anything else you can imagine is an object which is created from a class.

Anytime you assign an integer, string, or another type to a variable you have been creating an object from their respective classes.

```
count = 8
foo = "bar"
```

![Instance assignment](https://s3.amazonaws.com/dq-content/66/integer_assignment.svg)

Think of a class as the **blueprint** used to construct objects with. These blueprints share similar functions (called **methods**) that we can use with any object. For example, when we create a list object, every list will have the `append` method defined by the list class.

```
my_list = [1, 2, 3]
my_list.append(4)
# my_list object uses the append method which is defined by the list class.
```

A class bundles up logically grouped functions and variables (called **attributes**) that we can use anywhere in our code. The reason to use classes is similar to modules but instead of a requiring multiple files for different groupings, we can add multiple classes to a single file. This promotes code **abstraction** which helps us by not having to repeatedly write the same code over and over again.

Recall from the previous mission that we used the `csv` module to parse a csv file. As you may remember, all we can do with those csv files is read from a file and then load it in a list. Unfortunately, within the `csv`module, there are no helpful functions to do some actual analysis.

Additional functionality would have to be written on our own. One way is to write a function that can take in the csv data as a parameter and then run some analysis. But then where do we keep this function? What if we want to run multiple functions on the same dataset? Using a class, we can bundle up the csv data with common methods and share it across our code.

For this mission, we'll be working on creating a dataset object that can take in any csv file and expose methods to query it. The example data set we will be working with is a list of National Football League (NFL) games. Here are the first three rows:

| Year | Week | Winner              | Loser               |
| ---- | ---- | ------------------- | ------------------- |
| 2009 | 1    | Pittsburgh Steelers | Tennessee Titans    |
| 2009 | 1    | Minnesota Vikings   | Cleveland Browns    |
| 2009 | 1    | New York Giants     | Washington Redskins |

Each row in our data set represents a game. Here's a description of the columns:

- `year` the game took place
- `week` of the season (out of 17 total weeks)
- `winner` the winning team
- `loser` the losing team

## 2: Defining The Dataset Class

To create a class in Python, we use the keyword `class` followed by the desired class name. The class name, by convention, is in [`PascalCase`](https://en.wikipedia.org/wiki/PascalCase) where the first letter of every word is capitalized. Here is how you would define the dataset class:

```python
class Dataset:
    def __init__(self):
        self.type = "csv"
```

Let's break this down piece by piece. Within the declaration of the `class` we have written what looks like the function syntax for the method `__init__`. Remember from before that internal functions of a class are called methods. Before we explain what `__init__` does, let's see the syntax on how to create a dataset object.


```
new_dataset = Dataset()
```

This `new_dataset` variable refers to an instance of the Dataset class. When creating this object, the Python interpreter uses the special `__init__` method we defined to instantiate the object. This *creates* the new object and then sets those attributes to the instance.

![new_dataset assignment](https://s3.amazonaws.com/dq-content/66/new_dataset_variable_assignment.svg)

To access the attributes of a class we use **dot notation** like the following:

```
print(new_dataset.type)   # prints out "csv"
```

Let's take a look back at where we defined the `__init__` method. You can see that we had to pass in an argument to the `__init__` method, `self`.

Python uses this `self` variable to refer to the created object so you can interact with the instance data. If you didn't have `self`, then the class wouldn't know where to store the internal data you wanted to keep. By convention, `self` is used to define the instance even though it's possible to name it whatever you want. It is highly recommended to use `self` because any project that is built using Python will also give it that name.

When creating an object, you never have to worry about passing in a `self` object on instantiation since this is done automatically by the Python interpreter. If this were not the case then it would look like something like the following:

```
new_dataset = Dataset(new_dataset)
```

This looks odd and it would be tedious to do every time you create an object so thankfully Python will do it for you.

### Instructions

- Create a class called `Dataset`.
  - Inside the class, create a `type` *attribute*. Assign the value `"csv"` to it.
- Create an *instance* of the `Dataset` class, and assign it to the variable `dataset`.
- Print the `type` attribute of the `dataset` instance.

```
class Dataset:
    def __init__(self):
        self.type = "csv"

dataset = Dataset()
print(dataset.type)
```

## 3: Passing Additional Arguments To The Initializer

Before, we saw that we can initialize objects with attributes that we explicitly set on the `self` variable. Setting these attributes means that the values will be shared across every created object. The problem with this is that we might want to be able to set a unique attribute for the new object (like rows from a csv file).

To dynamically add data to our dataset object on instantiation, we have to add an additional argument to the `__init__`method (remembering to keep `self`first!).

```
class Dataset:
    def __init__(self, data):
        self.data = data
```

Then, to create an object with the data, we pass it in when instantiating the class. A reminder from last mission is that we can use the `csv` module to read a data set from a csv file:

```
f = open("somefile.csv", 'r')
csvreader = csv.reader(f)
csv_data = list(csvreader)

csv_dataset = Dataset(csv_data)
```

Recall that `__init__` is a method is just another way of saying a function defined in a class. Therefore, we can treat it as any function you've seen before. As a result, we can pass any amount of arguments when instantiating an object. You can also access the new attribute you set by using the dot notation described previously.

```
# prints the first 10 rows of the csv data.
print(csv_dataset.data[:10])
```

### Instructions

- Add a `data` parameter to the `__init__` method, and set the value to the `self.data` attribute.
- Read the data from `nfl.csv` and set it to the variable `nfl_data`
- Make an instance of the class, passing in `nfl_data` to the `__init__` function (when you call `Dataset(...)`).
  - Assign the result to the variable `nfl_dataset`
- Assign `nfl_dataset.data` to the variable `dataset_data`

```
class Dataset:
    def __init__(self):
        self.type = "csv"
class Dataset:
    def __init__(self, data):
        self.data = data

f = open("nfl.csv", 'r')
csvreader = csv.reader(f)
nfl_data = list(csvreader)

nfl_dataset = Dataset(nfl_data)
dataset_data = nfl_dataset.data
```

## 4: Adding Additional Behavior

Instantiating our objects with attributes is great but we can do even more by having additional instance methods. From a previous example in the code, we printed the first 10 rows of the data by calling the `print()` function outside of the class. Let's make a new method that will always print out the first 10 rows.

```python
class Dataset:
    def __init__(self, data):
        self.data = data
    def print_data(self):
        # New method **remember to add self**.
        print(self.data[:10])
```

Like `__init__`, we need to add `self` to the first parameter of this instance method. As you can see from this example, the benefit of having the `self`variable is that we can reference it from any instance method we define. Also, using `self`, you can call `print_data()`method within other instance methods by calling `self.print_data()`.

Let's create an instance of the dataset again and print the first 10 rows.

```python
nfl_dataset = Dataset(nfl_data)
nfl_dataset.print_data()  # Prints the first 10 rows.
```

To practice what we've learned, let's create a method on the dataset class that can print any amount of rows.

### Instructions

- Add an instance method `print_data` that takes in a `num_rows` argument.This method should print out data up to the given amount of rows.
- Create an instance of the `Dataset` class and initialize with the `nfl_data`. `nfl_data` is already loaded for you.Assign it to the variable `nfl_dataset`
- Call the `print_data` method with `num_rows` as 5

```
class Dataset:
    def __init__(self, data):
        self.data = data
        
    # Your method goes here
class Dataset:
    def __init__(self, data):
        self.data = data
    
    def print_data(self, num_rows):
        print(self.data[:num_rows-1])


nfl_dataset = Dataset(nfl_data)
nfl_dataset.print_data(5)
```

## 5: Enhancing The Initializer

You may have noticed when printing the data that the first element in the list of rows contains some header information. Using the `csv` module, we don't have a way of extracting this header unless we grab the first element. However, with our dataset class, we could add an instance method that would grab the first result of `self.data`, set it as a `header` attribute, and then remove it from the `data`attribute. Let's try that now:

```
...
def extract_header(self):
    self.header = self.data[0]
    self.data = self.data[1:]  # set data 
...
```

This works well but there is a problem. Let's say the user keeps calling the `extract_header` method continuously. Well, then the second time this is called the correct header will be overwritten and the next row will be set as the header. What we want is to extract the header once and only once in our class.

The best place to do that is to set it in the initializer! Because this method will only get called once on instantiation we know that the header will also only be set once. By setting the header within the initializer, the user doesn't have to worry about calling the method after creating the object and this promotes a better user experience.

Let's add the header extraction to the initializer now.

### Instructions

- Add the `extract_header` code to the initializer and set the header data to `self.header`.
- Create a variable called `nfl_header` and set it to the header attribute

```python
class Dataset:
    def __init__(self, data):
        self.data = data

nfl_dataset = Dataset(nfl_data)
class Dataset:
    def __init__(self, data):
        self.header = data[0]
        self.data = data[1:]

nfl_dataset = Dataset(nfl_data)
nfl_header = nfl_dataset.header
```

## 6: Grabbing Column Data

In the previous screen we were able to parse the headers from a csv file. With these headers, a helpul function for analyzing a dataset is to grab all the column data for a given header label. This is helpful since you might want to extract data from a specific column of a dataset and then process it.

Looking at the `nfl_data`, you may notice that the header's index lines up with the rest of the rows. To grab the column data, all we need to do is search through the headers, find the index of the given label, and then loop through the rest of the data returning the value of the index every iteration.

A great function to help us search the header and extract both the index and label to check is called `enumerate`. Here's an example on how it works:

```python
for idx, value in enumerate(['foo', 'bar']):
    print(idx, value)
```

This will print `0 foo` and `1 bar` which represents the index and value, respectively.

### Instructions

- Add a method named `column` that takes in a `label`argument, finds the index of the header, and returns a list of the column data
  - If the `label` is not in the header, you should return `None`
- Create a variable called `year_column` and set it to the return value of `column('year')`
- Create a variable called `player_column` and set it to the return value of `column('player')`

```
class Dataset:
    def __init__(self, data):
        self.header = data[0]
        self.data = data[1:]
        
    # Add your method here.

nfl_dataset = Dataset(nfl_data)
class Dataset:
    def __init__(self, data):
        self.header = data[0]
        self.data = data[1:]
    
    def column(self, label):
        if label not in self.header:
            return None
        
        index = 0
        for idx, element in enumerate(self.header):
            if label == element:
                index = idx
        
        column = []
        for row in self.data:
            column.append(row[index])
        return column

nfl_dataset = Dataset(nfl_data)
year_column = nfl_dataset.column('year')
player_column = nfl_dataset.column('player')
```

## 7: Count Unique Method

Let's add a `count_unique` method to our class so that a user can choose a label and then get the total amount of unique results in the column. This is not too tricky since we have already done all the hard lifting in the `column` method but it returns *all* the elements in a column. Keep this in mind when writing the `count_unique` method.

Recall that we can use the `self`parameter to access instance methods in the same way that we accessed an attribute. Here's an example of how you can access a method within the declaration of another instance method.

```
...
def other_instance_method(self):
    results = self.get_results()
    ...
...
```

### Instructions

- Add a method to the `Dataset` class called `count_unique`that takes in a `label` arguments.
- Get the unique set of items from the `column` method and return the total count.
- Use the instance method to assign the number of unique term values of `years` to `total_years`.

```python
nfl_dataset = Dataset(nfl_data)
class Dataset:
    def __init__(self, data):
        self.header = data[0]
        self.data = data[1:]
    
    def column(self, label):
        if label not in self.header:
            return None
        
        index = 0
        for idx, element in enumerate(self.header):
            if label == element:
                index = idx
        
        column = []
        for row in self.data:
            column.append(row[index])
        return column
    
    def count_unique(self, label):
        count = 0
        for item in set(self.column(label)):
            count += 1
        return count

nfl_dataset = Dataset(nfl_data)
total_years = nfl_dataset.count_unique('year')
```

## 8: Make Objects Human Readable

With all the features of the `Dataset` class, we want to make it easier for the user to take a snapshot look at the data. Multiple screens ago, we wrote a `print_data`method that took in a number of rows and then printed out those rows. Now, we are going to write another method that will do something similar but using a python special function instead.

Across all python classes, there are a [list of special methods](https://docs.python.org/3/reference/datamodel.html#basic-customization) that we can implement. Each one of provides additional customization that the python interpreter will use to enhance your object. When we implemented `__init__`, it told the python interpreter that anything within that method is what we want to initialize when we create our object.

One special method is `__str__` which tells the python interpreter how to *represent your object as a string*. Whenever we try to convert the object into a string or when we want to print out that object, we can use `__str__` method to customize the way it looks when we display the object using the `print()`function. Use the console and check what the `nfl_dataset` object looks like by doing the following:

```
>> nfl_dataset = Dataset(nfl_data)
>> nfl_dataset
<__main__.Dataset instance at 0x10abc23b0>
```

This doesn't really tell the user what the dataset looks like internally so let's fix that.

### Instructions

- Add a method to the `Dataset` class called `__str__`
  - Convert the first 10 rows of `self.data` to a string and set it as the return value.
- Create an instance of the class called `nfl_dataset` and call print on it.

```
class Dataset:
    def __init__(self, data):
        self.header = data[0]
        self.data = data[1:]
    
    def __str__(self):
        data_string = self.data[:10]
        return str(data_string)
    
    def column(self, label):
        if label not in self.header:
            return None
        
        index = 0
        for idx, element in enumerate(self.header):
            if label == element:
                index = idx
        
        column = []
        for row in self.data:
            column.append(row[index])
        return column
    
    def count_unique(self, label):
        count = 0
        for item in set(self.column(label)):
            count += 1
        return count

nfl_dataset = Dataset(nfl_data)
print(nfl_dataset)
```

## 9: Next Steps

If we wanted to extract a header, grab all the columns, and do a count of a dataset, we would have to have written all these functions in a module and imported them for every new dataset we used. This can get tedious over time because we use datasets all the time and we would want a consistent set of behaviors and attributes to use with them.

With classes, we bundle all of that data and behavior together in one location. An instance of the `Dataset`class is all we need to count unique terms in a dataset or get a file's header. Once we add behavior to a class, every instance of the class will be able to perform that behavior. As we develop our application, we can add more properties to classes to extend their functionality. Using classes and instances helps organize our code, and allows us to represent real-world concepts in well-defined code constructs.

When you start to write classes in the real world, you will need to write robust classes so your programs don't crash. In the next mission we are going to learn to handle errors when dealing with empty or malformed data.

# Error Handling

## 1: The Dataset

In this mission, we'll be working with `legislators.csv`, which records information on every historical member of the U.S. Congress. Here's a preview of the dataset:

```
last_name,first_name,birthday,gender,type,state,party
Bassett,Richard,1745-04-02,M,sen,DE,Anti-Administration
Bland,Theodorick,1742-03-21,,rep,VA,
Burke,Aedanus,1743-06-16,,rep,SC,
Carroll,Daniel,1730-07-22,M,rep,MD,
```

The file includes these columns:

- `last_name` -- the legislator's last name
- `first_name` -- the legislator's first name
- `birthday` -- the legislator's birthday
- `gender` -- the legislator's gender
- `type` -- the chamber in which the legislator served - either Senate (`sen`) or House of Representatives (`rep`)
- `state` -- the state the legislator represents
- `party` -- the legislator's party affiliation

As you can see from the data extract, some rows contain missing values for some columns. For example, the `gender` and `party` columns are missing in the second row after the header row. Missing data can cause errors, so it needs to be dealt with. In this mission, we'll explore some of the errors that occur when we ignore missing values and how to handle them.

As we learn, we'll work on finding the most common names for U.S. legislators. We'll lay the groundwork in this mission, and bring it all together in the next one.

## 2: Sets

When exploring data, it's often useful to extract the unique elements in a *list*. For example, this list has duplicate values:

```
["Dog", "Cat", "Hippo", "Dog", "Cat", "Dog", "Dog", "Cat"]
```

Extracting the unique elements will give us:

```
["Dog", "Cat", "Hippo"]
```

Simplifying *lists* in this way can help you find unexpected values.

The result of this conversion is a *set* - a data type where each element is unique. A *set* behaves very similarly to a *list*. However, if you add an element to a *set* that it already contains, the *set* will ignore it. Also, the items in a *set* are unordered, while each item in a list has an *index*.

You can create a *set* with the [set()](https://docs.python.org/3.5/library/stdtypes.html?highlight=set#set) function. Simply pass a *list* into the function, and the function will convert it:

```python
unique_animals = set(["Dog", "Cat", "Hippo", "Dog", "Cat", "Dog", "Dog", "Cat"])

print(unique_animals)
```

```R
unique_animals <- unique(c("Dog", "Cat", "Hippo", "Dog", "Cat", "Dog", "Dog", "Cat"))
```

We'll get `{'Hippo', 'Cat', 'Dog'}` as a result. Note that the interpreter encloses *Sets* in curly braces when it prints them. Because *Sets* don't have indexes, the items in a set may display in a different order each time you print it.

You can add items to a *set* using the [add()](https://docs.python.org/3.5/library/stdtypes.html?highlight=set#set.add) method:

```python
unique_animals.add("Tiger")
```

Finally, you can remove items from a *set* with the [remove()](https://docs.python.org/3.5/library/stdtypes.html?highlight=set#set.remove) method:

```python
unique_animals.remove("Dog")
```

If we want to convert a *set* to a *list*, we can use the [list()](https://docs.python.org/3.5/tutorial/datastructures.html) method:

```python
list(unique_animals)
```

### Instructions

- We've read `legislators.csv` into the variable `legislators`, which is a *list of lists*. Extract the `gender`column from `legislators` and assign it to the variable `gender`.
  - Create an empty *list* named `gender`.
  - Loop over each item in `legislators`.
  - Append the fourth element in the item to `gender`.
- Convert `gender` to a *set*.
- Print out `gender` and see what the unique values are.

```
gender = []
for item in legislators:
    gender.append(item[3])

gender = set(gender)
print(gender)
```

## 3: Exploring The Dataset

When you have a fresh dataset, it's always a good idea to look for any patterns, such as:

- Missing data
  - Some files contain empty fields. Others may use a string like `N/A`to indicate missing values.
- Values that don't make intuitive sense
  - A legislator with a `birthday` in `2050`, for example, would indicate a problem with the data.
- Recurring themes
  - One theme in this dataset is that the overwhelming majority of legislators are male.

Let's take a closer look at a few columns to see if we can identify any patterns.

### Instructions

- Extract the `party` column from `legislators` and convert it to a *set*. Assign the result to `party`.
- Print out `party` and inspect the values.
- Print out `legislators` and inspect the data.

```
party = []
for item in legislators:
    party.append(item[6])

party = set(party)
print(party)

print(legislators)
```

## 4: Missing Values

You may have noticed that the *set* representations of the `gender` and `party`columns on the previous two screens contained an empty string (`''`). This indicates that one or more of the rows in the data have missing values in those columns. Missing values are very common in real world data analysis, since the people compiling the datasets often don't have full information.

You can use one of the following strategies to address missing data:

- Remove any rows that contain missing data.
- Populate the empty fields with a specified value.
- Populate the empty fields with a calculated value.
- Use analysis techniques that work with missing data.

We'll work with missing data in more depth later on, but for now, we'll focus on populating empty fields with a specified value.

Here's how we could replace any missing values in the `party` column with the *string* `No Party`:

```
rows = [
    ["Bassett", "Richard", "1745-04-02", "M", "sen", "DE", "Anti-Administration"],
    ["Bland", "Theodorick", "1742-03-21", "", "rep", "VA", ""]
]

for row in rows:
    if row[6] == "":
        row[6] = "No Party"
```

Step by step, we:

- Loop through each row in `rows`.
- Check whether the `party` column (index `6`) has a missing value.
- If so, replace it with the *string* `No Party`.

Next, we'll populate the empty fields in the `gender` column. Most U.S. legislators have historically been men (although this is changing), so we'll replace any missing values with the *string* `M`.

### Instructions

- Replace any missing values in the `gender` column of `legislators` with the *string* `M`.
- When you're done, the `gender` column of `legislators`should only contain the values `F` and `M`.

```
for row in legislators:
    if row[3] == "":
        row[3] = "M"
```

## 5: Parsing Birth Years

While we're looking for the most common names of U.S. legislators, the year of birth could also be of interest. For example, we could use that field to identify historical naming trends, and explore how popular names have changed from `1820` to today.

As you may have noticed, the `birthday`column has the format `1820-01-02`, which is hard to work with. However, it's common to reformat values to simplify them. In this case, we can split the date into its component parts:

```
date = "1820-01-02"
parts = date.split("-")
print(parts)
```

This will create a list `["1820", "01", "02"]`. The first item in the list is the year the legislator was born, the second is the month, and the last is the day.

### Instructions

- Create an empty list named `birth_years`.
- Loop through each item in `legislators`.
  - Split the value in the `birthday` column on the `-`character.
  - Assign the result to `parts`.
  - Extract the first item in `parts` and append it to `birth_years`.
- At the end, `birth_years` will be a *list* containing the birth years of all the legislators in `legislators`.

```
birth_years = []
for row in legislators:
    birthday = row[2]
    parts = birthday.split("-")
    birth_years.append(parts[0])
```

## 6: Try/Except Blocks

Converting a column to a different data type is a common operation in data analysis. For example, we just extracted the `year` on the previous screen, but it's in *string* form. To find the average year in which legislators were born, we'll need to convert the data to *integers* first. We can perform this conversion with the [int()](https://docs.python.org/3/library/functions.html#int) function. The only challenge is that the `year` column has missing values. If we try to convert a missing value, we'll get an error:

```
int('')
```

The code above will cause a `ValueError`, because an empty *string* can't be converted to an *integer*.

Not all errors should halt execution, though. Sometimes we expect a certain type of error, and want to handle it in a specific way that allows the code to complete. We can manage errors with something known as a *try/except block*. If you surround the code that causes an error with a *try/except block*, the error will be handled, and the code will continue to run:

```python
try:
    int('')
except Exception:
    print("There was an error")
```

In the example above, the Python interpreter will try to run `int('')`, and generate a `ValueError`. Instead of stopping the code from executing, it will be handled by the `except` statement, which will print the message `There was an error`. The Python interpreter will continue to run any lines of code that come after the `except` statement.

### Instructions

- Convert the *string* `hello` to a *float* with the [float()](https://docs.python.org/3/library/functions.html#float) function.
- Wrap the code that does the conversion in a *try/except block*.
- In the `except` statement, print out `Error converting to float.`.

```
try:
    float("hello")
except Exception:
    print("Error converting to float.")
```

## 7: Exception Instances

When the Python interpreter generates an [exception](https://docs.python.org/3/tutorial/errors.html), it actually creates an instance of the `Exception` class. This class has certain properties that help us debug the error. We can access the instance of the `Exception` class in the `except` statement body:

```python
try:
    int('')
except Exception as exc:
    print(type(exc)) # <class 'ValueError'>
```

In the example above, we use the `as`statement to assign the instance of the `Exception` class to the variable `exc`. We can then access the variable in the `except` statement body. Printing `type(exc)` will display the type of `Exception` that occured in the `try`statement body.

We can also convert the `Exception` class to a *string* and print out the error message:

```
try:
    int('')
except Exception as exc:
    print(str(exc)) # invalid literal for int() with base 10: ''
```

This will print a message that will help us debug the error.

### Instructions

- Write a statement that attempts to convert an empty string to an *integer*, and wrap it in a *try/except block*.
- Capture the `Exception` instance.
- Print the `type` of the `Exception` instance.
- Convert the `Exception` instance to a *string* and print it out.

```
try:
    int('')
except Exception as exc:
    print(type(exc))
    print(str(exc))
```

## 8: The Pass Keyword

On the previous screen, we printed a message each time an error occurred:

```
try:
    int('')
except Exception:
    print("There was an error")
```

However, there are times when we don't want to do anything specific to handle errors; we just want the code to keep running. This is common when looping over a long *list*, and performing the same operation multiple times. In cases like this, printing lots of errors messages would be fairly annoying. For example, running the following code results in many errors:

```
numbers = [1,2,3,4,5,6,7,8,9,10]
for i in numbers:
    try:
        int('')
    except Exception:
        print("There was an error")
```

Unfortunately, we can't just leave out the print statement to avoid this, since that would cause an error:

```
numbers = [1,2,3,4,5,6,7,8,9,10]
for i in numbers:
    try:
        int('')
    except Exception:
```

That's because any Python statement that ends in a colon (`:`) needs to have an indented body below it. Instead, we can use the `pass` keyword to avoid generating an error:

```
try:
    int('')
except Exception:
    pass
```

While the `pass` keyword doesn't actually do anything, it's a valid statement body. It offers a solution when we don't want an error to stop code execution, but also don't want to do anything in the `except`statement body.

### Instructions

- Loop through each element in `birth_years`.Assign the element to `year`.Try to convert `year` to an *integer* using the [int()](https://docs.python.org/3/library/functions.html#int) function.Wrap the conversion in a *try/except block*.Use the `pass` keyword in the `except` statement body.Append `year` to `converted_years`.

```
converted_years = []
for year in birth_years:
    try:
        year = int(year)
    except Exception:
        pass
    converted_years.append(year)
```

## 9: Convert Birth Years To Integers

Let's convert all of the birth years in `legislators` to *integers*. To change the items in a *list of lists*, we need to loop over the top-level list (`items`):

```
items = [          
    [1, "1", 2],    
    [2, "", 3],
    [5, "5", 3]
]

for item in items:
    item[1] = int(item[1])
```

The above code will modify the second element in each `item` (embedded *list*). In other words, it will convert all of the values in the second column of `items` to *integers*.

### Instructions

- Loop through each row in `legislators`.Parse the birth year from the `birthday` column.Convert the birth year to an *integer*, and assign it to `birth_year`.Wrap this code in a *try/except block*.If there's an exception, assign `0` to `birth_year`.Append `birth_year` to the row with the [append()](https://docs.python.org/3/tutorial/datastructures.html) method.
- When finished, `legislators` should have an extra column for birth year.

```
for row in legislators:
    birthday = row[2]
    birth_year = birthday.split("-")[0]
    try:
        birth_year = int(birth_year)
    except Exception:
        birth_year = 0
    row.append(birth_year)
```

## 10: Fill In Years Without A Value

We finished parsing the birth years to *integers*, but now we have several birth years with the value `0`. Here are the first few items in the `birth_year` column of `legislators`:

```
[1745,
 1742,
 1743,
 1730,
 1739,
 0,
 1738,
 1745,
 1748,
 ...
]
```

While exploring the dataset, you may have noticed that the legislators appear in roughly chronological order. We can use this knowledge to populate the missing values intelligently.

Earlier, we replaced missing values with a fixed value `M`. This time, because the values generally appear in chronological order, we can loop through each year and replace any `0` values with the values from the previous rows.

By doing this, we'll make sure each legislator without a birth year is assigned one that's relatively close to the actual date.

### Instructions

- Create a variable called `last_value`, and set it to `1`.
- Loop through each row in `legislators`.If the year column (index `7`) equals `0`, replace it with `last_value`.Assign the value of the year column (index `7`) to `last_value`.
- After the code runs, each row previously containing `0` for `birth_year` column will now instead have the previous row's value for the same column.

```
last_value = 1
for row in legislators:
    if row[7] == 0:
        row[7] = last_value
    last_value = row[7]
```

## 11: Next Steps

In this mission, we did some basic exploration and manipulation with `legislators.csv`, and laid the groundwork for our names project. In the next mission, we'll learn some advanced list concepts, then find the most common names for U.S. legislators who served after `1940`.

#  List Comprehensions

## 1: The Data Set

In the previous mission, we worked with `legislators.csv`, which contains information on every person who has served in the U.S. Congress. We cleaned up some missing data and added a column for birth year.

We'll continue to work with the same data set in this mission. Here's a preview of it in *CSV* format:


```
last_name,first_name,birthday,gender,type,state,party,birth_year
Bassett,Richard,1745-04-02,M,sen,DE,Anti-Administration,1745
Bland,Theodorick,1742-03-21,M,rep,VA,1742
Burke,Aedanus,1743-06-16,M,rep,SC,1743
Carroll,Daniel,1730-07-22,M,rep,MD,1730
```

The data set includes the following columns:

- `last_name` -- The legislator's last name
- `first_name` -- The legislator's first name
- `birthday` -- the legislator's birthday
- `gender` -- The legislator's gender
- `type` -- The chamber in which the legislator served - either Senate (`sen`) or House of Representatives (`rep`)
- `state` -- The state the legislator represents
- `party` -- The legislator's party affiliation
- `birth_year` -- *integer* values for the year the legislator was born

In this mission, we'll use the data to find the most common names among U.S. legislators of each gender. Before diving into this, we'll explore some critical concepts, such as enumeration.

## 2: Enumerate

There are many situations where we'll need to iterate over multiple *lists* in tandem, such as this one:


```
animals = ["Dog", "Tiger", "SuperLion", "Cow", "Panda"]
viciousness = [1, 5, 10, 10, 1]

for animal in animals:
    print("Animal")
    print(animal)
    print("Viciousness")
```

In the example above, we have two *lists*. The second *list* describes the viciousness of the animals in the first *list*. A `Dog` has a viciousness level of `1`, and a `SuperLion` has a viciousness level of `10`. We want to retrieve the position of the item in `animals` the loop is currently on, so we can use it to look up the corresponding value in the `viciousness`list.

Unfortunately, we can't just loop through `animals`, and then tap into the second *list*. Python has an [enumerate()](https://docs.python.org/3/library/functions.html#enumerate) function that can help us with this, though. The [enumerate()](https://docs.python.org/3/library/functions.html#enumerate) function allows us to have two variables in the body of a for loop -- an index, and the value.


```
for i, animal in enumerate(animals):
    print("Animal Index")  ## label
    print(i)
    print("Animal") ## label
    print(animal)
```

Here's a diagram of the Python logic that takes place when the code runs:

Iteration1i=0animal="Dog"Iteration2i=1animal="Tiger"Iteration3i=2animal="SuperLionIteration4i=3animal="Cow"Iteration5i=4animal="Panda"

On every iteration of the loop, the value for `i` will become the value of the index in `animals` that corresponds to that iteration. `animal` will take on the value in `animals` that corresponds to the index `i`.

Here's another example of how we can use the [enumerate()](https://docs.python.org/3/library/functions.html#enumerate) function to iterate over multiple *lists* in tandem:


```
animals = ["Dog", "Tiger", "SuperLion", "Cow", "Panda"]
viciousness = [1, 5, 10, 10, 1]

for i, animal in enumerate(animals):
    print("Animal")
    print(animal)
    print("Viciousness")
    print(viciousness[i])
```

In this example, we use the index variable `i` to index the `viciousness` *list*, and print the viciousness value that corresponds to the same index in `animals`.

### Instructions

- Enumerate the `ships` *list* using a for loop and the [enumerate()](https://docs.python.org/3/library/functions.html#enumerate) function.
- For each iteration of the loop:
  - Print the item from `ships` at the current index.
  - Print the item from `cars` at the current index.

```
ships = ["Andrea Doria", "Titanic", "Lusitania"]
cars = ["Ford Edsel", "Ford Pinto", "Yugo"]
for i, ship in enumerate(ships):
    print(ship)
    print(cars[i])
```

## 3: Adding Columns

We can even use the [enumerate()](https://docs.python.org/3/library/functions.html#enumerate) function to add columns to *lists of lists*. For example, here's some starter code:


```
door_count = [4, 4]
cars = [
        ["black", "honda", "accord"],
        ["red", "toyota", "corolla"]
       ]
```

We can add a column to `cars` by appending a value to each inner *list*:


```
for i, car in enumerate(cars):
    car.append(door_count[i])
```

In the code above, we:

- Use the [enumerate()](https://docs.python.org/3/library/functions.html#enumerate) function to loop across each item in `cars`.
- Find the corresponding value in `door_count` that has the index `i`(the same index as the current item in `cars`).
- Add the value in `door_count` with index `i` to `car`.
- After the code runs, each row in `cars` will have a `door_count`column.

Let's reinforce what we've learned by completing an exercise.

### Instructions

- Loop through each row in `things` using the [enumerate()](https://docs.python.org/3/library/functions.html#enumerate) function.
- Append the item in `trees` that has the same index (as the current `thing`) to the end of each row in `things`.
- After the code runs, `things` should have an extra column.

```python
things = [["apple", "monkey"], ["orange", "dog"], ["banana", "cat"]]
trees = ["cedar", "maple", "fig"]
for i, row in enumerate(things):
    row.append(trees[i])
```

## 4: List Comprehensions

We've written many short *for loops* to manipulate *lists*. Here's an example:

```python
animals = ["Dog", "Tiger", "SuperLion", "Cow", "Panda"]

animal_lengths = []
for animal in animals:
    animal_lengths.append(len(animal)) # [3, 5, 9, 3, 5]
```

It takes three lines to calculate the length of each *string* `animals` this way. However, we can condense this down to one line with a list comprehension:

```python
animal_lengths = [len(animal) for animal in animals]
```

This comprehension consists of the list operation `len(animal)`, the loop variable `animal`, and the list that we're iterating over, `animals`.

The diagram below visualizes how a list comprehension condenses a for loop:

animal_lengths=[]foranimalinanimals:animal_lengths.append(len(animal))animal_lengths=[len(animal)foranimalinanimals]

Logically, the list comprehension:

- Loops through each element in the `animals` *list* and assigns the current element to `animal`
- Finds the length of each `animal`*string*
- Generates a new *list* that contains all of the lengths as elements
- Assigns the new *list* to `animal_lengths`

List comprehensions are much more compact notation, and can save space when you need to write multiple for loops.

### Instructions

- Use list comprehension to create a new *list* called `apple_prices_doubled`, where you multiply each item in `apple_prices` by `2`.
- Use list comprehension to create a new *list* called `apple_prices_lowered`, where you subtract `100` from each item in `apple_prices`.

```
apple_prices = [100, 101, 102, 105]
apple_prices_doubled = [price*2 for price in apple_prices]
apple_prices_lowered = [price-100 for price in apple_prices]
```

## 5: Counting Female Names

Let's count how many times each female first name occurs in `legislators`. To limit our count to names from the modern era, we'll only look at those that appear after `1940`. While names like `Theodorick`were common prior to `1940`, they're rare today.

Here's a preview of what this dictionary will look like:

```
{
    'Nancy': 1, 
    'Sandy': 1, 
    'Carolyn': 1, 
    'Melissa': 2, 
    'Jo Ann': 2,
    ...
}
```

Now, let's work on creating it!

### Instructions

- Create an empty dictionary called `name_counts`.
- Loop through each row in `legislators`.
- If the `gender` column of the row equals `F` and the `year`column is greater than `1940`:Assign the `first_name` column of the row to the variable `name`.If `name` is in `name_counts`:Add `1` to the value associated with `name` in `name_counts`.If `name` isn't in `name_counts`:Set the value associated with `name` in `name_counts` to `1`.
- When the loop finishes, `name_counts` should contain each unique name in the `first_name` column of `legislators`as a key, and the corresponding number of times it appeared as the value.

```
name_counts = {}
for row in legislators:
    gender = row[3]
    year = row[7]
    if gender == "F" and year > 1940:
        name = row[1]
        if name in name_counts:
            name_counts[name] = name_counts[name] + 1
        else:
            name_counts[name] = 1
```

## 6: None

Let's say we're trying to find the maximum value in a list. We might write some code that looks like this:


```
values = [50, 80, 100]
max_value = 0
for i in values:
    if i > max_value:
        max_value = i
```

We set `max_value` to a low value so that everything's greater than it. But what if we changed the values list slightly?


```
values = [-50, -80, -100]
max_value = 0
for i in values:
    if i > max_value:
        max_value = i
```

In the above scenario, `max_value` is `0` when the loop finishes. This is wrong, because `0` isn't in `values`; it's just a placeholder we used to initialize `max_value`.

We can resolve this kind of issue using the [None](https://docs.python.org/3.5/library/constants.html#None) object, which has a special data type called *NoneType*.

The `None` object indicates that the variable has no value. Rather than using the normal double equals sign (`==`) to check whether a value equals `None`, we use the `variable is None` syntax.

The [is](https://docs.python.org/3/reference/expressions.html#is) comparison operator checks for object equality. Using `is` instead of `==` prevents some custom classes from resolving to `True` when compared with `None`. We'll explore how to use operators with the `None` object in greater depth during a later mission. For now, let's see what the `variable is None` syntax looks like:


```python
values = [-50, -80, -100]
max_value = None
for i in values:
    if max_value is None or i > max_value:
        max_value = i
```

```R
max_value <- NULL
```

In the example above, we:

- Initialize `max_value` to `None`.
- Loop through each item in `values`.
- Check whether `max_value` equals `None` using the `max_value is None` syntax.
- If `max_value` equals None, or if `i > max_value`, then we assign the value of `i` to `max_value`.
- At the end of the loop, `max_value` will equal `-50`, which is the largest value in `values`.

## 7: Comparing With None

Comparing a value to [None](https://docs.python.org/3.5/library/constants.html#None) will usually generate an error. This is actually helpful when we're writing code, because it prevents unexpected variables from being [None](https://docs.python.org/3.5/library/constants.html#None). For example, this code will cause an error:


```
a = None
a > 10
```

Therefore, when a value could potentially be `None`, and we want to compare it to another value, we should always include code that checks whether it actually is `None` first.

We can use two Boolean statements joined by `or` to do this. Here's an example:

`max_value is None or i > max_value`

The Python interpreter will evaluate the two statements in order. If the first statement is `True`, it won't evaluate the second one. This saves time, since when one statement is `True`, the whole `or`conditional is `True`.

The following code will assign `True` to `b` if `a` is `None`, or if `a` is greater than `10`:


```
a = None
b = a is None or a > 10 # True
```

The same logic applies to an `and`statement. Because both conditions have to be `True`, if the first one is `False`, the Python interpreter won't evaluate the second one. The example below shows how to write an `and` statement involving `None` that won't return an error. It will assign `True` to `b` if `a` does not equal `None` and `a` is greater than `10`:


```
a = None
b = a is not None and a > 10
```

Let's give this a try in our next exercise!

### Instructions

- Loop through each value in `values`.
- Check whether the value is not `None`, and if it's greater than `30`.
- Append the result of the check to `checks`.
- When finished, `checks` should be a *list* of Booleans indicating whether or not the corresponding items in `values` are not None and greater than `30`.

```
values = [None, 10, 20, 30, None, 50]
checks = []
checks = [x is not None and x > 30 for x in values]
# [False, False, False, False, False, True]
```

## 8: Highest Female Name Count

`name_counts` is a dictionary where the keys are female first names from `legislators`, and the values are the number of times the names occured after `1940`.

In order to extract the most common names from this dictionary, we need to determine the highest totals in `name_counts`. Once we know the totals, we can find the keys for them.

We can iterate through all of the keys in a dictionary like this:


```
fruits = {
        "apple": 2,
        "orange": 5,
        "melon": 10
    }

for fruit in fruits:
    rating = fruits[fruit]
```

In the loop above, we iterate through each key in `fruits`. We can access the corresponding value using `fruits[fruit]`.

Let's identify the highest totals in the next exercise.

### Instructions

- Set `max_value` to `None`.
- Loop through the keys in `name_counts`.
- Assign the value associated with the key to `count`.
- If `max_value` is `None`, or `count` is greater than `max_value`:Assign `count` to `max_value`.
- At the end of the loop, `max_value` will contain the largest value in `name_counts`.

```python
max_value = None
for k in name_counts:
    count = name_counts[k]
    if max_value is None or count > max_value:
        max_value = count
```

## 9: The Items Method

The code we used on the previous screen to access the keys and values in a dictionary was slightly awkward. We can simplify this process with the [items()](https://docs.python.org/3.5/library/stdtypes.html#dict.items) method, which allows us to iterate through keys and values at the same time.


```
fruits = {
    "apple": 2,
    "orange": 5,
    "melon": 10
}

for fruit, rating in fruits.items():
    print(rating) # 2 5 10
```

The [items()](https://docs.python.org/3.5/library/stdtypes.html#dict.items) method makes our code clearer and more compact.

### Instructions

- Use the [items()](https://docs.python.org/3.5/library/stdtypes.html#dict.items) method to iterate through the keys and values in `plant_types`.
- Print each key in `plant_types`.
- Print each value in `plant_types`.

```python
plant_types = {"orchid": "flower", "cedar": "tree", "maple": "tree"}
for k,v in plant_types.items():
    print(k)
    print(v)
# orchid
# flower
# cedar
# tree
# maple
# tree
```

## 10: Finding The Most Common Female Names

As we learned on a previous screen, the most common female names occur two times in `name_counts`. Therefore, we want to extract any keys in `name_counts` that have the value `2`.

### Instructions

- Loop through the keys in `name_counts`.
- If any value in `name_counts` equals `2`, append its key to `top_female_names`.
- When you're finished, `top_female_names` will be a list of the most common names of female legislators.

```
top_female_names = []
top_female_names = [k for k, v in name_counts.items() if v == 2]
```

## 11: Finding The Most Common Male Names

Now that we know how to find the most common female names, we can repeat the same process for male names.

### Instructions

- Create a dictionary called `male_name_counts`.
- Loop through `legislators`.Count how many times each name with `"M"` in the gender column and a birth year after `1940` occurs.Store the results in `male_name_counts`.
- Find the highest value in `male_name_counts` and assign it to `highest_male_count`.
- Append any keys from `male_name_counts` with a value equal to `highest_male_count` to `top_male_names`.

```python
top_male_names = []
male_name_counts = {}
for row in legislators:
    if row[3] == "M" and row[7] > 1940:
        name = row[1]
        if name in male_name_counts:
            male_name_counts[name] = male_name_counts[name] + 1
        else:
            male_name_counts[name] = 1

highest_value = None
for name,count in male_name_counts.items():
    if highest_value is None or count > highest_value:
        highest_value = count

for name,count in male_name_counts.items():
    if count == highest_value:
        top_male_names.append(name)
```

# Challenge: Modules, Classes, Error Handling, and List Comprehensions

## 1: How Challenges Work

At Dataquest, we're huge believers in learning through doing, and we hope this shows in your learning experiences. While missions focus on introducing concepts, challenges give you the opportunity to engage in deliberate practice by completing structured problems. You can read more about deliberate practice [on Wikipedia](http://bit.ly/2cJ2Qt7) and [on Nautilus](http://nautil.us/issue/35/boundaries/not-all-practice-makes-perfect).

If you have questions or run into issues, head over to the [Dataquest forums](https://www.dataquest.io/forum/) or our [Slack community](https://www.dataquest.io/chat).

## 2: Introduction To The Data

In this challenge, you'll practice using modules, classes, and list comprehensions to process and represent a data set in Python. You'll be working with data on NFL player suspensions. The [FiveThirtyEight team](https://www.dataquest.io/m/113/challenge-modules-classes-error-handling-and-list-comprehensions/2/www.fivethirtyeight.com) compiled the data set for a [piece on domestic violence](http://fivethirtyeight.com/features/nfl-domestic-violence-policy-suspensions/). You can download it [here](https://github.com/fivethirtyeight/data/blob/master/nfl-suspensions/nfl-suspensions-data.csv). The data set contains all domestic violence-related suspensions issued before 2014.

Here's a preview of what the file, `nfl_suspensions_data.csv`, looks like:

| name        | team | games  | category                          | desc.             | year | source                                   |
| ----------- | ---- | ------ | --------------------------------- | ----------------- | ---- | ---------------------------------------- |
| F. Davis    | WAS  | Indef. | Substance abuse, repeated offense | Marijuana-related | 2014 | http://www.cbssports.com/nfl/eye-on-football/24448694/redskins-te-fred-davis-suspended-Indefiniteinitely-by-nfl |
| J. Blackmon | JAX  | Indef. | Substance abuse, repeated offense |                   | 2014 | http://espn.go.com/nfl/story/_/id/11257934/justin-blackmon-jacksonville-jaguars-arrested-marijuana-possession |
| L. Brazill  | IND  | Indef. | Substance abuse, repeated offense |                   | 2014 | http://www.nfl.com/news/story/0ap2000000364622/article/lavon-brazill-released-by-colts-in-wake-of-suspension |

Let's read the file into Python and explore the data to become more familiar with it.

### Instructions

- Read the dataset into a *list* of *lists*.
  - Import the `csv` module.
  - Create a File handler for `nfl_suspensions_data.csv`.
  - Use the [`csv.reader()`](https://docs.python.org/3/library/csv.html#csv.reader) and `list()` methods to read the file into a *list* named `nfl_suspensions`.
- Remove the first *list* in `nfl_suspensions`, which contains the header row of the CSV file.
  - Select all *lists* in `nfl_suspensions`, except the for the one at index `0`.
  - Assign the resulting *list* of *lists* back to the variable `nfl_suspensions`.
- Count the number of times each value in the `year` column occurs.
  - Create an empty *dictionary* called `years`.
  - Use a `for` loop to iterate over the *list* in `nfl_suspensions` representing the `year` column:Extract that row's value for the `year` column and assign it to `row_year`.If `row_year` is already a key in `years`, add `1` to the value for that key.If `row_year` isn't already a key in `years`, set the value for the key to `1`.
- Use the `print()` function to display the *dictionary* `years`.

```python
import csv
f = open("nfl_suspensions_data.csv") # file handler
nfl_suspensions = list(csv.reader(f))
nfl_suspensions = nfl_suspensions[1:]

years = {}
for suspension in nfl_suspensions:
  row_year = suspension[5]
  if row_year in years:
    years[row_year] += 1
  else:
    years[row_year] = 1

print(years)
```

## 3: Unique Values

Let's explore the values in these columns by using sets and list comprehensions.

### Instructions

- Retrieve the unique values in the `team` column and assign the list to `unique_teams`.Use a list comprehension to create a new list containined just the values in the `team` column.Use the `set()` function to return a list containing only the unique values and assign to `unique_teams`.
- Retrieve the unique values in the `games` column and assign the list to `unique_games`.Use a list comprehension to create a new list containined just the values in the `games` column.Use the `set()` function to return a list containing only the unique values and assign to `unique_games`.
- Display `unique_teams` and `unique_games`.

```
teams = [row[1] for row in nfl_suspensions]
```

3

```
unique_teams = set(teams)
```

4

```
print(unique_teams)
```

5

```

```

6

```
games = [row[2] for row in nfl_suspensions]
```

7

```
unique_games = set(games)
```

8

```
print(unique_games)
```

## 4: Suspension Class

Next, let's create a `Suspension` class that we can use to represent each NFL suspension in the data set.

### Instructions

- Create the `Suspension` class.Define the `__init__()` method with the following criteria:The sole required parameter is a *list* representing a row from the dataset.To create a `Suspension` instance, we want to be able to pass in a *list* from `nfl_suspensions`.Currently, we're only interested in storing the `name`, `team`, `games` and `year` columns. Upon instantiation:Set the name value for that row to the `name` property.Set the team value for that row to the `team` property.Set the games value for that row to the `games` property.Set the year value for that row to the `year` property.
- Create a `Suspension` instance using the third row in `nfl_suspensions`, and assign it to the variable `third_suspension`.

```
class Suspension():
```

3

```
    def __init__(self,row):
```

4

```
        self.name = row[0]
```

5

```
        self.team = row[1]
```

6

```
        self.games = row[2] 
```

7

```
        self.year = row[5]
```

8

```
third_suspension = Suspension(nfl_suspensions[2])
```

## 5: Tweaking The Suspension Class

Let's tweak the `Suspension` class a bit to extend its functionality. Right now, the value for `year` is a *string*, rather than an *integer*. Let's modify the `Suspension`class so that it stores the values as *integers*.

### Instructions

- Instead of assigning the value at index `5` to the `year`property directly, use a *try except block* that:Tries to cast the value at index `5` to an *integer*If an exception is thrown, assign the value `0` to the `year` property instead
- Create a method called `get_year()` that returns the `year`value for that `Suspension` instance.
- Create a `Suspension` instance using the 23rd row, and assign it to `missing_year`.
- Use the `get_year()` method to assign the year of the `missing_year` suspension instance to `twenty_third_year`.

```
class Suspension():
```

2

```
    def __init__(self,row):
```

3

```
        self.name = row[0]
```

4

```
        self.team = row[1]
```

5

```
        self.games = row[2]
```

6

```
class Suspension():
```

7

```
    def __init__(self,row):
```

8

```
        self.name = row[0]
```

9

```
        self.team = row[1]
```

10

```
        self.games = row[2] 
```

11

```
        try:
```

12

```
            self.year = int(row[5])
```

13

```
        except Exception:
```

14

```
             self.year = 0
```

15

```
    def get_year(self):
```

16

```
        return(self.year)
```

17

```
                
```

18

```
missing_year = Suspension(nfl_suspensions[22])
```

19

```
twenty_third_year = missing_year.get_year()
```

## 6: Next Steps

In this challenge, you honed your knowledge of list comprehensions and class creation. You also practiced using the `csv` module, as well as handling exceptions with a try catch block. Next, you'll learn about variable scopes.

# Variable Scopes

## 1: The Data Set

As we work through this mission, we'll be using a data set on student loan defaults in the United States. It's very common for American students to borrow money to pay for college. Because tuition costs are high, many students are unable to repay their loans. When a student cannot pay off his or her loan, it goes into a status known as default.

Each row of our dataset represents a single university, and contains information about the number of its students who later defaulted on their loans. While the data contains twelve columns, we'll be looking at a few in particular:

- `institution` -- The name of the university
- `state` -- The state in which the university is located
- `city` -- The city in which the university is located
- `borrower_default_count_240` -- The total number of students who have defaulted on their loans
- `principal_outstanding_240` -- The total dollar amount of the loans in default

To make the data easier to work with, we've read each of these columns into its own list. For example, you can access the entire `city` column by using the variable `city`.

## 2: Built-In Functions

Some Python functions are available by default, without having to import them. We call these *built-in* functions. The [sum()](https://docs.python.org/3/library/functions.html#sum) function, which works on lists, is one such built-in function.

Here are a few others:

- [len()](https://docs.python.org/3/library/functions.html#len)
- [float()](https://docs.python.org/3/library/functions.html#float)
- [min()](https://docs.python.org/3/library/functions.html#min)
- [max()](https://docs.python.org/3/library/functions.html#max)

Developers use these functions so often that it made sense to make them a part of the language itself. You can find a full list of built-in Python functions [here](https://docs.python.org/3/library/functions.html).

### Instructions

- Use the `sum()` function to add `6` and `11` and assign the result to `total`.

```
total = sum([6, 11])
```

## 3: Overwriting A Built-In Function

You're probably used to assigning values to variables, then accessing those values, like this:

```
b = 10
print(b)
```

The value `10` is assigned to variable `b`, which is why running the code displays `10`. Here's a slightly more complex example:

```
b = [1,2]
sum = sum(b)
sum(20)
```

This code will actually generate an error because `sum` no longer points to the built-in Python function but to the expression `sum(b)` instead. Once we overwrite the `sum` variable with a value, we can't access the function anymore. Calling `sum(20)` won't make any sense, because `sum` evalutes to a single integer value (the result of `sum(b)`). If we called `print(sum)`, it would print out the value `3`.

On the next few screens, we'll delve into why this behavior occurs.

### Instructions

- Experiment with the code to see what happens before and after we overwrite the [sum()](https://docs.python.org/3/library/functions.html#sum) function.
- Click "Next Step" when you're done.

## 4: Scopes

When we write functions, we're writing reusable blocks of code. This means that no matter what's happening with the rest of the code we write, the function should operate in exactly the same way each time. This allows us to create programs that run in predictable ways. We wouldn't want a function to behave differently at random if we had a variable called `total` in our code. Let's say we wrote a function that adds two numbers:

```

```

```
def add(a,b):
```

```
    total = a + b
```

```
    return total
```

Inside the function, we're defining a variable named `total`. We could call the function like this:

```

```

```
total = 15
```

```
print(add(10, 20))
```

```
print(total)
```

Since functions are designed to be reusable, they have to be isolated from the rest of the program. Even though there's a variable called `total` inside the `add`function, that variable is not connected to the `total` variable in our script. For example, the script above would print out two different totals: first `30`, then `15`. That's because the variable `total` we defined in our script is in the *global scope*, whereas the `total` variable inside `add` is in a *local scope*.

Here's a diagram of how the variables look as the code runs:

Globalscopetotal=15Localscope(add)total=30

The idea of variable scoping is extremely important in programming, and allows us to isolate what happens in functions from what happens in the rest of our program.

The *global* scope is whatever happens outside of a function. Anything that happens inside a function is in a *local scope*. There's only one *global* scope, but each function creates its own *local scope*.

### Instructions

- Use the `find_average` function to find the average of `principal_outstanding_240`, and assign the result to the variable `average`.
- Afterwards, print the variable `total` to verify that it's unchanged in the *global scope*.

```
def find_average(column):
```

2

```
    length = len(column)
```

3

```
    total = sum(column)
```

4

```
    return total / length
```

5

```

```

6

```
total = sum(borrower_default_count_240)
```

7

```
average = find_average(principal_outstanding_240)
```

## 5: Scope Isolation

*Local scopes* aren't just isolated from the *global scope* - they're also isolated from every other *local scope*. Our code creates a local scope when it calls a function, and destroys it when the function finishes running. Calling the same function twice will create two separate *local scopes*. This ensures that any variables our code creates inside the function disappear when the function finishes running, and don't affect the rest of the program.

Here's an example:

```

```

```
def add(a,b):
```

```
    total = a + b
```

```
    return total
```

```

```

```
def subtract(a,b):
```

```
    total = a - b
```

```
    return total
```

```

```

```
print(add(1,5))
```

```
print(subtract(1,5))
```

Even though both of the functions in the code above define a variable called `total`within them, each one has its own *local scope*. After each function is called, the values for both `total` variables disappear, because all the variables defined inside the *local scope* are removed. The code snippet above doesn't define any variables in the *global scope*.

Here's how the variables look when the code is run:

GlobalscopeLocalscope(add)Localscope(subtract)total=6total=-4

### Instructions

- Calculate the average of `principal_outstanding_240`with the `find_average()` function, and assign the result to the variable `average`.
- Calculate the length of `principal_outstanding_240` with the `find_length()` function, and assign the result to the variable `principal_length`.
- Afterwards, verify that the variable `length` is unchanged in the *global scope*.
- Also verify that changing the order in which you call `find_average` and `find_length` doesn't alter the results.

```
def find_average(column):
```

2

```
    length = len(column)
```

3

```
    total = sum(column)
```

4

```
    return total / length
```

5

```

```

6

```
def find_length(column):
```

7

```
    length = len(column)
```

8

```
    return length
```

9

```

```

10

```
length = len(borrower_default_count_240)
```

11

```
average = find_average(principal_outstanding_240)
```

12

```
principal_length = find_length(principal_outstanding_240)
```

## 6: Scope Inheritance

When our code uses a variable name in a *local scope* that it hasn't defined there yet, the Python interpreter will check whether the variable exists in the *global scope*.

Here's an example:

```
total = 50
```

```
def find_average(column):
```

```
    length = len(column)
```

```
    return total / length
```

In the code above, we use the `total`variable inside `find_average()` without having first defined it. In this case, the Python interpreter will check whether `total` exists in the *global scope*. Because it does, the Python interpreter will return `50 / length` from the `find_average()`function.

Here's a diagram:

Globalscopetotal=50Localscope(find_average)length=1845total

### Instructions

- Find the average of `principal_outstanding_240` with the `find_average()` function, and assign the result to the variable `average`.
- Verify that the `find_average()` function used the value `length` from the *global scope*.

```
def find_average(column):
```

2

```
    total = sum(column)
```

3

```
    # In this function, we are going to pretend that we forgot to calculate the length
```

4

```
    return total / length
```

5

```

```

6

```
length = 10
```

7

```
average = find_average(principal_outstanding_240)
```

## 7: Inheritance Limits

There are limits to how much we can work with *global scope* variables inside a *local scope*. These limits allow functions to be reusable, and prevent them from changing how your script behaves.

Here's an example of what won't work:

```
a = 2
```

```
def alter_a():
```

```
    a = a * 2
```

```
    return a
```

The function above will cause an error. That's because while we can access the value of a *global scope* variable inside a *local scope*, we can't change the value of that variable.

### Instructions

Experiment with the code to see what happens before and after we call the `find_total()` function. Click "Next Step" when you're done.

## 8: Built-In Inheritance

As we recently learned, if we use a variable in a *local scope* that isn't defined there, the Python interpreter will look for it in the *global scope*. If it doesn't find the variable there, it will check whether the variable is a built-in function name.

Here's an example of the type of code that would generate this behavior:

```
def total(a):
```

```
    return sum(a)
```

We use the `sum` variable in the `total()` function, but don't define in the *local scope* or the *global scope*. This variable is actually a built-in function called [sum()](https://docs.python.org/3/library/functions.html#sum). So the Python interpreter calls the function, and uses it to add the values in the list `a`.

If other code in the *global scope* overwrites the built-in function, then the interpreter uses the value in the *global scope*:

```
sum = 10
```

```

```

```
def total(a):
```

```
    return sum(a)
```

```

print(total([1,2]))
```

The code above will cause an error, because the interpreter will use the *global scope* value for `sum` in the `total()` function. That's because the *global scope* value for `sum` is an integer, and won't work as a function.

## 9: Global Variables

*Global variables* are variables that are available across all scopes. We can access *and* change the value of a *global variable* inside any *global scope* or *local scope*.

While *Global variables* can sometimes be handy, the developer community generally doesn't recommend using them. That's because they make functions dependent on the value of variables in the *global scope*, and prevent them from being reusable in any situation.

Still, let's take a look at how we would use them. We define global variables with the `global` keyword.

```
total = 10
```

```

```

```
def add_to_total(a):
```

```
    global total
```

```
    total = total + a
```

```

```

```
add_to_total(20)
```

```
print(total)
```

The code above will add `20` to `total`, then print out `30`.

When we create a *global variable*, we can't create it and assign a value to it on the same line. We first define the variable as global using the `global` keyword, then assign a value to it on a separate line.

We can also define *global variables* inside *local scopes*:

```

```

```
def test_function():
```

```
    global a
```

```
    a = 10
```

```

```

```
test_function()
```

```
print(a)
```

Because we defined `a` with the `global`keyword, this code will print out `10`.

### Instructions

- Create a new function:
  - Make a global variable `b` inside the function.
  - Assign the value `20` to `b` inside the function.
- Call the function.
- Print out `b`.

```
def new_function():
```

3

```
    global b
```

4

```
    b = 20
```

5

```
    
```

6

```
new_function()
```

7

```

```

8

```
print(b)
```

## 10: Inheritance Rules

When we use a variable anywhere in a Python script, the Python interpreter will look for its value according to some simple rules. It will:

- Start with the *local scope*, if any. If the variable is defined here, it will use that value.
- Look at any *enclosing scopes*, starting with the innermost. These are "outside" *local scopes*. If the variable is defined in any of them, it will use the value.
- Look in the *global scope*. If the variable is there, it uses the value.
- Look in the built-in functions.
- Throw an error if it doesn't find the variable.

A simple way to remember this is *LEGBE*, which stands for "Local, Enclosing, Global, Built-ins, Error".

# Regular Expressions


## 1: Introduction

Data scientists often need to parse strings to extract important information. Suppose we have manually-entered data that includes dates, and need to extract the years from those dates. The dates may look something like this:

```
- "Jan 17, 2012"
- "9/22/2005"
- "Spring 2007"
- "New Year's Eve 1999"
```

All of these strings contain the information we need, but in very different formats. If we try to split the strings, what character would we split them on? In the resulting lists, which element would contain the year? We can handle a problem like this with **regular expressions**.

A regular expression (regex) is a sequence of characters that describes a search pattern. We can use regular expressions to search for and extract data.

In practice, we say that strings *match* a regular expression if the pattern exists anywhere within those strings (as substrings). The simplest example of a regular expression is an ordinary sequence of characters that we specify. We say that any string containing that sequence of characters, adjacent and in the same exact order, *matches* the regular expression. Here are a few examples:

RegularExampleExamplesthatExpressionMatchesdonotMatch"123""12345","abc123def","1a2b3c","321""abc123def","123""b""b","abc","bread""xyz","c","B"

This is the simplest form of a regex. We'll soon see that regular expressions can also contain special characters that denote particular patterns.

### Instructions

- In the code cell, assign to the variable `regex` a regular expression that's four characters long and matches every string in the list `strings`.


```
strings = ["data science", "big data", "metadata"]
regex = "data"
```

## 2: Wildcards In Regular Expressions

We've seen that we can use regular expressions to find strings containing a simple pattern, but they can match much more complex patterns.

There are a number of special characters we can use with regular expressions to change the way a pattern is interpreted. In Python, we use the [re](https://docs.python.org/3/library/re.html) module to work with regular expressions. The module's documentation provides a list of these special characters.

For instance, we use the special character `"."` to indicate that *any* character can be put in its place. Here are a few examples of how you might use this placeholder:

RegularExampleExamplesthatExpressionMatchesdonotMatch"a.c""aac","abc","acc","add","crash""alchemy","branch""..t""bat","habit","oat""at","it","to"

Let's create a regular expression in the exercise on the next screen.

### Instructions

- Assign a regular expression that is three characters long and matches every string in `strings` to the variable `regex`.


```
strings = ["bat", "robotics", "megabyte"]
regex = "b.t"
```


## 3: Searching The Beginnings And Endings Of Strings

We can use the caret symbol (`"^"`) to match the beginning of a string, and the dollar sign (`"$"`) to match the end of a string.

`"^a"` will match all strings that start with `"a"`.

`"a$"` will match all strings that end with `"a"`.

We can use any combination of special characters in a regex. Let's combine what we've learned so far to create some more advanced expressions.

### Instructions

- Assign a regular expression that's seven characters long and matches every string in `strings` (except for `bad_string`) to the variable `regex`.

```
strings = ["better not put too much", "butter in the", "batter"]
bad_string = "We also wouldn't want it to be bitter"
regex = "^b.tter"
```

## 4: Introduction To The AskReddit Data Set

[Reddit](https://www.reddit.com/) is a content and community website where users can submit links, text posts, and other types of content to groups of people with similar interests. These groups are called **subreddits**, and each one specializes in a particular topic.

For example, [AskReddit](https://www.reddit.com/r/AskReddit/) is a popular **subreddit** where you can pose questions to the entire Reddit community. Users answer the questions by commenting on them.

In this mission, we'll be working with a data set containing the top 1,000 questions users posted to AskReddit in 2015. Reddit user [P_S_Laplace](https://www.reddit.com/user/P_S_Laplace) created the data set, which has five columns that appear in the following order:

1. Title -- The title of the post
2. Score -- The number of upvotes the post received
3. Time -- When the post was posted
4. Gold -- How much [Reddit Gold](https://www.reddit.com/gilding) users gave the post
5. NumComs -- The number of comments the post received


## 5: Reading And Printing The Data Set

Let's use the `csv` module to read and print our data file, `"askreddit_2015.csv"`. Recall that we can use the `csv` module by performing the following steps:

1. Import `csv`.
2. Open the file that contains our CSV data in `'r'` mode.
3. Call the `csv.reader()` function with the file object as input.
4. Convert the result to a list.

### Instructions

- Use the `csv` module to read our data set and assign it to `posts_with_header`.
- Use list slicing to exclude the first row, which represents the column names. Assign this sliced data set to `posts`.
- Use a *for* loop and string slicing to print the first 10 rows. See if you notice any patterns in this sample of the data set.

```
import csv
f = open("askreddit_2015.csv", 'r')
csvreader = csv.reader(f)
posts_with_header = list(csvreader)
posts = posts_with_header[1:]
for post in posts[:10]:
    print(post)
```


## 6: Counting Simple Matches In The Data Set With Re()

We mentioned the [re](https://docs.python.org/3/library/re.html) module earlier, and now we'll begin to use it in our code. One useful function the module provides is [re.search](https://docs.python.org/3/library/re.html#re.search).

With `re.search(regex, string)`, we can check whether `string` is a match for `regex`. If it is, the expression will return a **match** object. If it isn't, it will return `None`. For now, we won't worry about returning the actual matches - we'll just compare the result to `None` to see whether we have a match or not.

```
if re.search("needle", "haystack") is not None:
    print("We found it!")
else:
    print("Not a match")
```

The code above will print `Not a match`, because `"haystack"` is not a match for the regex `"needle"`.

You may have noticed that many of the posts in our AskReddit database are directed towards particular groups of people, using phrases like `"Soldiers of Reddit"`. These types of posts are common, and always follow a similar format. We can use regular expressions to count how many of them are in the top 1,000.

Let's do this in our next exercise. We've already read the data set into the variable `posts`.

### Instructions

- Count the number of posts in our data set that match the regex `"of Reddit"`. Assign the count to `of_reddit_count`.

```
import re

of_reddit_count = 0
for row in posts:
    if re.search("of Reddit", row[0]) is not None:
        of_reddit_count += 1
```


## 7: Using Square Brackets To Match Multiple Characters

If you look at the data set closely, you may notice that some posts use `"of Reddit"`, and others use `"of reddit"`. While both versions have the same format, the capitalization of `"Reddit"` is different. We can account for this inconsistency with square brackets. We use square brackets in a regex to indicate that any character within them can fill the space.

For example, the regex `"[bcr]at"` would match the substrings `"bat"`, `"cat"`, and `"rat"`, but nothing else. We indicate that the first character in the regex can be either `"b"`, `"c"` or `"r"`.

### Instructions

- Use square bracket notation to make the code account for both capitalizations of `"Reddit"`, and count how many posts contain `"of Reddit"` or `"of reddit"` in the title.
- Assign the resulting count to `of_reddit_count`.

```
import re
of_reddit_count_old = 0
for row in posts:
    if re.search("of Reddit", row[0]) is not None:
        of_reddit_count_old += 1
of_reddit_count = 0
for row in posts:
    if re.search("of [Rr]eddit", row[0]) is not None:
        of_reddit_count += 1
```


## 8: Escaping Special Characters

Our data set contains a lot of posts that use the **[Serious]** tag. AskReddit members use this tag to indicate that they're not looking for humorous responses, and that their question should be taken seriously. We'd like to search through our data set to see how many posts have this tag, but the regex `"[Serious]"` doesn't do what we need. Since square brackets serve a special function within regular expressions, `"[Serious]"` will match any string that contains `"S"`, `"e"`, `"r"`, etc.

To deal with this sort of problem, we need to **escape** special characters. In regular expressions, escaping a character means indicating that you don't want the character to do anything special, and that the interpreter should treat it just like any other character. We use the backslash (`"\"`) to escape characters in a regex.

Suppose we wanted to match all of the strings that end with a period. If we used `".$"`, it would match all strings that contain *any* character, because `"."` has that special meaning. Instead, we need to escape the `"."` with a backslash, so our regex becomes `"\.$"`.

### Instructions

- Escape the square bracket characters to count the number of posts in our data set that contain the `"[Serious]"` tag.
- Assign the count to `serious_count`.

```
import re
serious_count = 0
for row in posts:
    if re.search("\[Serious\]", row[0]) is not None:
        serious_count += 1
```

## 9: Combining Escaped Characters And Multiple Matches

Some people tag serious posts as `"[Serious]"`, and others as `"[serious]"`. We should account for both capitalizations.

### Instructions

- Refine the code to count how many posts have either `"[Serious]"` or `"[serious]"` in the title.
- Assign the count to `serious_count`.

```
import re
serious_count_old = 0
for row in posts:
    if re.search("\[Serious\]", row[0]) is not None:
        serious_count_old += 1
serious_count = 0
for row in posts:
    if re.search("\[[Ss]erious\]", row[0]) is not None:
        serious_count += 1
```


## 10: Adding More Complexity To Your Regular Expression

On the previous screen, you saw that we can use square brackets as both special notation and escaped characters, all in the same regex. We'll be using more brackets to refine our search.

In our data set, some users have tagged their posts with `"(Serious)"` or `"(serious)"`, including the parentheses. Therefore, we should account for both square brackets and parentheses. We can do this by using square bracket notation, and escaping the `"["`, `"]"`, `"("`, and `")"` characters with the backslash.

### Instructions

- Refine the code so that it counts how many posts have the serious tag enclosed in either square brackets or parentheses.
- Assign the count to `serious_count`.

```
import re
serious_count = 0
for row in posts:
    if re.search("[\[\(][Ss]erious[\]\)]", row[0]) is not None:
        serious_count += 1
```


## 11: Combining Multiple Regular Expressions

We should consider a post serious only if the tag occurs at the beginning or end of the title. To match titles with the tag at the beginning, we can use the `"^"`character in a regex. To match titles with the tag at the end, we can use `"$"`. These characters produce two different regular expressions, and we'd like to identify all titles that match **either** of them.

To combine regular expressions, we use the `"|"` character. For example, `"cat|dog"` would match `"catfish"` and `"hotdog"`, because both of these strings match either `"cat"` or `"dog"`. Similarly, we can combine our regular expressions for the serious tags with the `"|"` operator to match all titles that either begin or end with the tag.

### Instructions

- Use the `"^"` character to count how many posts include the serious tag at the beginning of the title. Assign this count to `serious_start_count`.
- Use the `"$"` character to count how many posts include the serious tag at the end of the title. Assign this count to `serious_end_count`.
- Use the `"|"` character to count how many posts include the serious tag at either the beginning or end of the title. Assign this count to `serious_count_final`.

```
import re
serious_start_count = 0
serious_end_count = 0
serious_count_final = 0
for row in posts:
    if re.search("^[\[\(][Ss]erious[\]\)]", row[0]) is not None:
        serious_start_count += 1
    if re.search("[\[\(][Ss]erious[\]\)]$", row[0]) is not None:
        serious_end_count += 1
    if re.search("^[\[\(][Ss]erious[\]\)]|[\[\(][Ss]erious[\]\)]$", row[0]) is not None:
        serious_count_final += 1
```

## 12: Using Regular Expressions To Substitute Strings

We've looked at one way we can account for inconsistencies in data; now let's examine another approach. The `re`module provides a [sub()](https://docs.python.org/3/library/re.html#re.sub) function that takes the following parameters (in order):

```
1. pattern - The regex to match
2. repl    - The string that should replace the substring matches
3. string  - The string containing the pattern we want to search
```

If we were to call `re.sub("yo", "hello", "yo world")`, the function will replace the `"yo"` in `"yo world"` with `"hello"`, producing the result `"hello world"`. If it doesn't find a pattern, the `re.sub()`function simply returns the original `string`.

Let's use `re.sub()` to convert all **serious** tags to the format `"[Serious]"`.

### Instructions

- Replace `"[serious]"`, `"(Serious)"`, and `"(serious)"`with `"[Serious]"` for all of the titles in `posts`.You should only need to use one call to `sub()`, and one regex.Recall that the `repl` argument is an ordinary string. It's not a regex, so you don't need to escape characters like `"["`.Append each formatted row to `posts_new`.

```
import re
posts_new = []
for row in posts:
    row[0] = re.sub("[\[\(][Ss]erious[\]\)]", "[Serious]", row[0])
    posts_new.append(row)
```

## 13: Matching Years With Regular Expressions

Let's return to the example from the beginning of our mission. Suppose we need to extract years from strings. An intuitive way to do this would be to match any string that contains four consecutive integers. We can indicate that we're looking for integers in a pattern by using square brackets (`"["` and `"]"`), along with a dash (`"-"`). For example, `"[0-9]"`will match any character that falls between `0` and `9` (all of which will be one-digit integers). Similarly, `"[a-z]"`would match any lowercase letter. We can also specify smaller ranges like `"[3-5]"`or `"[d-g]"`.

Since we want to match four consecutive integers, our regex could be `"[0-9][0-9][0-9][0-9]"`. This would work, but let's also add the condition that we only want to match years after year 999 and before year 3000 (any other four-digit numbers in a string are probably not years).

### Instructions

- We've loaded a number of strings into the `strings`variable for you.
- Loop through `strings` and use `re.search()` to determine whether each string contains a year between 1000 and 2999.
- Store every string that contains a year in `year_strings`. The `.append()` function will help here.

```
import re
year_strings = []
for string in strings:
    if re.search("[1-2][0-9][0-9][0-9]", string) is not None:
        year_strings.append(string)
```

## 14: Repeating Characters In Regular Expressions

On the previous screen, we used the regex `"[1-2][0-9][0-9][0-9]"`, which looks a bit repetitive. Luckily, there's a better way to do it!

We can use curly brackets (`"{"` and `"}"`) to indicate that a pattern should repeat. To match any four-digit number, for example, we could repeat the pattern `"[0-9]"` four times by writing `"[0-9]{4}"`.

### Instructions

- We've loaded a number of strings into the `strings`variable for you.
- Loop through `strings` and use `re.search()` to determine whether each string contains a year between 1000 and 2999. Use a regex that takes advantage of curly brackets.
- Store every string that contains a year in `year_strings`. The `.append()` function will help here.

```
import re
year_strings = []
for string in strings:
    if re.search("[1-2][0-9]{3}", string) is not None:
        year_strings.append(string)
```

## 15: Challenge: Extracting All Years

Finally, let's extract years from a string. The `re` module contains a [findall()](https://docs.python.org/3/library/re.html#re.findall) function that returns a list of substrings matching the regex. `re.findall("[a-z]", "abc123")` would return `["a", "b", "c"]`, because those are the substrings that match the regex.

### Instructions

- Use `re.findall()` to generate a list of all years between 1000 and 2999 in the string `years_string`.
- Assign the result to `years`.

```
import re
years = re.findall("[1-2][0-9]{3}", years_string)
```

# Dates in Python


## 1: The Time Module

There are a few modules in Python's [Standard Library](https://docs.python.org/3/library/) that deal with dates and times. One is the [time](https://docs.python.org/3/library/time.html) module, which deals primarily with **Unix timestamps**.

A Unix timestamp is a floating point value with no explicit mention of day, month, or year. This value represents the *number of seconds that have passed since the "epoch"*, or the first second of the year 1970. So, a timestamp of `0.0` would represent the epoch, and a timestamp of `60.0` would represent one minute after the epoch. We can represent any date after 1970 this way.

To retrieve the current Unix timestamp, we use the [time.time()](https://docs.python.org/3/library/time.html#time.time) function.

### Instructions

- Return the current timestamp and assign it to `current_time`.
- Display `current_time` using the `print()` function.

```
import time
current_time = time.time()
print(current_time)
```

## 2: Converting Timestamps

We can convert a timestamp to a more human-readable form using the [time.gmtime()](https://docs.python.org/3/library/time.html#time.gmtime) function. This function takes a timestamp as an argument, and returns an instance of the `struct_time`class. `struct_time` instances have attributes that represent the current time in other ways.

Here are some of the attributes:

- `tm_year`: The year of the timestamp
- `tm_mon`: The month of the timestamp (1-12)
- `tm_mday`: The day in the month of the timestamp (1-31)
- `tm_hour`: The hour of the timestamp (0-23)
- `tm_min`: The minute of the timestamp (0-59)

For example, we can retrieve the year value as an *integer* using the `tm_year`property:


```
current_time = time.time()
current_struct_time = time.gmtime(current_time)
current_year = current_struct_time.tm_year
```

### Instructions

- Use the `time.time()` function assign the current Unix timestamp to a new variable `current_time`.
- Convert `current_time` to a `struct_time` object and assign the resulting object to `current_struct_time`.
- Assign the current hour to `current_hour` and display the value.

```
import time
current_time = time.time()
current_struct_time = time.gmtime(current_time)
current_hour = current_struct_time.tm_hour
print(current_hour)
```


## 3: UTC

Note the value for the hour from the last screen. The `time` module always results in a **UTC** time. **UTC** stands for **Coordinated Universal Time**. This is the accepted time standard within the programming community. It corresponds to the mean solar time at 0° longitude, or Greenwich Mean Time, except that it doesn't follow daylight saving time. While we can convert UTC to other time zones, we'll use UTC in this mission for simplicity.

The [datetime](https://docs.python.org/3/library/datetime.html) module offers better support for working extensively with dates. For example, it's easier to perform arithmetic on them (such as adding days), and to work with different time zones.

The `datetime` module has a `datetime`class that represents points in time. `datetime` instances appear similar to `struct_time` instances, and have the following attributes:

- `year`
- `month`
- `day`
- `hour`
- `minute`
- `second`
- `microsecond`

To get the current `datetime`, we use the [datetime.now()](https://docs.python.org/3/library/datetime.html#datetime.datetime.now) function, which returns a `datetime.datetime` instance.

### Instructions

- Import the `datetime` module.
- Assign the `datetime` object representation of the current time to a new variable `current_datetime`.
- Assign the current year to `current_year`.
- Assign the current month to `current_month`.

```
import datetime
current_datetime = datetime.datetime.now()
current_year = current_datetime.year
current_month = current_datetime.month
print(current_datetime)
```


## 4: Timedelta

We know how to represent dates, but we'd also like to perform arithmetic on them. Since adding a day, week, month, etc. to a date can be tedious to do from scratch, the `datetime` module provides the [timedelta](https://docs.python.org/3/library/datetime.html#timedelta-objects) class. We can create an instance of this class that represents a span of time, then add or subtract it from instances of the `datetime` class.

When we create instances of the `timedelta` class, we can specify the following parameters:

- `weeks`
- `days`
- `hours`
- `minutes`
- `seconds`
- `milliseconds`
- `microseconds`

Suppose we want to calculate the date for three weeks and two days from now. We would first create an instance of the `datetime` class that represents today:

```
today = datetime.datetime.now()
```

Then, we could get an instance of the `timedelta` class that represents the span of time we're working with:


```
diff = datetime.timedelta(weeks = 3, days = 2)
```

Finally, we would add these two instances:
```
result = today + diff
```

If we wanted to, we could also subtract a `timedelta` instance from a `datetime`instance. Let's use the `timedelta` class to retrieve `datetime` instances for both tomorrow and yesterday (at the current time).

### Instructions

- Create an instance of the `datetime` class that represents the current time and date. Assign this to a new variable `today`.
- Create an instance of the `timedelta` class that represents one day. Assign this to a new variable `diff`.
- Assign the `datetime` instance for tomorrow to a new variable `tomorrow`, and the `datetime` instance for yesterday to a new variable `yesterday`.

```
import datetime
today = datetime.datetime.now()
diff = datetime.timedelta(days = 1)
tomorrow = today + diff
yesterday = today - diff
```

## 5: Formatting Dates

Suppose we'd like to output dates in human-readable formats. If we use the `print()` function to display a `datetime`object, the output will look something like `2016-01-06 13:51:25.849719`. Instead of displaying the full timestamp down to the microsecond, we can use the [datetime.strftime()](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior) method to specify how we'd like the *string* output to be formatted.

The `datetime.datetime.strftime()`method takes a format string as its input. A format string contains special indicators, usually preceded by percent characters (`"%"`), that indicate where certain values should go. For example, suppose we stored a timestamp from March 3, 2010 in the object `march3`. If we want to format it nicely into the string `"Mar 03, 2010"`, we can write the following code:

```
march3 = datetime.datetime(year = 2010, month = 3, day = 3)
pretty_march3 = march3.strftime("%b %d, %Y")
print(pretty_march3)
```

The format string argument in `strftime()` indicates that we want:

- the abbreviated month name (`"%b"`) followed by a space
- the day of the month (`"%d"`) followed by a comma and space
- the full year (`"%Y"`).

Thankfully, we don't have to memorize the string arguments and can refer to the [documentation](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior) for the `strftime()`method, which provides a useful summary table of the different options.

### Instructions

Using the `datetime.datetime.strftime()` method, display `mystery_date`, a `datetime` instance we've created for you, in the following format:

- `[12-hour time with minutes][AM/PM] on [Day of week] [Month full name] [Day of month], [Full year]`

Here's an example in that format:

- `"11:00AM on Wednesday March 03, 2010"`

Store this *string* in the new variable `mystery_date_formatted_string` and display using the `print()` function.


```
import datetime
mystery_date_formatted_string = mystery_date.strftime("%I:%M%p on %A %B %d, %Y")
print(mystery_date_formatted_string)
```

## 6: Parsing Dates

Just as we can convert a `datetime` object into a formatted string, we can also do the reverse. The `datetime.datetime.strptime()` function allows us to convert a *string* to a `datetime` instance:

1. The date string (e.g. `"Mar 03, 2010"`)
2. The format string (e.g. `"%b %d, %Y"`)

With just these two arguments, `strptime()` will return a `datetime`instance for March 3, 2010. The one thing to remember is that `datetime.datetime.strptime()` is a function, not a method that's called on a specific object.

```
march3 = datetime.datetime.strptime("Mar 03, 2010", "%b %d, %Y")
```

This is useful if we have a date in a string format, and need to convert it to a `datetime` instance. If we inspect the data and determine the format of every date, we can save ourselves a lot of manual string manipulation by using the `datetime.datetime.strptime()` function instead. We could even use `datetime.strptime()` and `datetime.strftime()` together to convert a date string to a `datetime` object, and then convert it to a date string of a different format.

### Instructions

- Use the `datetime.datetime.strptime()` function to return a `datetime` instance that represents the timestamp associated with the *string*`mystery_date_formatted_string`:
  - `mystery_date_formatted_string` has the format: `[Time][AM/PM] on [Day of week] [Month full name] [Day of month], [Full year]`.
  - March 3, 2010 at 11:00AM would look like `"11:00AM on Wednesday March 03, 2010"` in this format.
- Assign the resulting `datetime` instance in `mystery_date`and display it using the `print()` function.

```
import datetime
mystery_date = datetime.datetime.strptime(mystery_date_formatted_string, "%I:%M%p on %A %B %d, %Y")
print(mystery_date)
```

## 7: AskReddit Data

[Reddit](https://www.reddit.com/) is a content and community website where users can submit links, text posts, and other types of content to groups of people with similar interests. These groups are called **subreddits**, and each one specializes in a particular topic. One popular subreddit, [AskReddit](https://www.reddit.com/r/AskReddit/), is a place where users pose questions to the entire Reddit population. Other users answer those questions in the comments section.

We'll be working with a data set of the top 1,000 posts on AskReddit from 2015. Reddit user [P_S_Laplace](https://www.reddit.com/user/P_S_Laplace) created the data set, which contains the following columns:

- `Title`: The title of the post
- `Score`: The number of upvotes the post received
- `Time`: When the post appeared (timestamp)
- `Gold`: How much [Reddit Gold](https://www.reddit.com/gilding) users gave the post
- `NumComs`: The number of comments the post received

## 8: Reformatting Our Data

In the following code cell, we've read the AskReddit data into the `posts` variable for you as a list of lists. Each nested list represents an AskReddit post. Here's what the first few rows of the data set looks like:

| Title                                    | Score | Time         | Gold | NumComs |
| ---------------------------------------- | ----- | ------------ | ---- | ------- |
| "What's your internet ""white whale"", something you've been searching for years to find with no luck?" | 11510 | 1433213314.0 | 1    | 26195   |
| What's your favorite video that is 10 seconds or less? | 8656  | 1434205517   | 4    | 8479    |
| What are some interesting tests you can take to find out about yourself? | 8480  | 1443409636.0 | 1    | 4055    |

Here's what the first three lists in `posts`looks like:


```
posts = [
            ['What\'s your internet "white whale", something you\'ve been searching for years to find with no luck?', '11510', '1433213314.0', '1', '26195'],
            ["What's your favorite video that is 10 seconds or less?", '8656', '1434205517.0', '4', '8479'],
            ['What are some interesting tests you can take to find out about yourself?', '8480', '1443409636.0', '1', '4055'],
            ...
        ]
```

The values in the `Time` column are formatted as Unix timestamps, not human-readable *strings*. We can convert each Unix time stamp into `datetime`object using the [datetime.datetime.fromtimestamp()](https://docs.python.org/3/library/datetime.html#datetime.datetime.fromtimestamp) function:

```
datetime_object = datetime.datetime.fromtimestamp(1433213314.0)
```

We'll convert the Unix timestamp for each row to a `datetime` object using [datetime.datetime.fromtimestamp()](https://docs.python.org/3/library/datetime.html#datetime.datetime.fromtimestamp), format the date with `strftime()`, and store the result back in the row, replacing the Unix timestamp.

### Instructions

Loop through `posts`, and for each row:

- Convert the value in the `Time` column (index `2` of each row) to a floating point number.
- Convert the floating point number to a `datetime` instance using `datetime.datetime.fromtimestamp()`.
- Store the resulting `datetime` instance back in index `2` of the row, overwriting the original Unix timestamp value.

```
import datetime
for row in posts:
    float_stamp = float(row[2])
    day = datetime.datetime.fromtimestamp(float_stamp)
    row[2] = day
```

## 9: Counting Posts From March

Now that we've converted our `posts` data set to contain `datetime` instances, we can count how many of the top 1,000 posts users submitted in the month of March.

### Instructions

- Loop through `posts`, and for each row:Use the `datetime.month` attribute to check if the `datetime` instance at index `2` equals `3`.If so, increment `march_count`.

```
march_count = 0
for row in posts:
    if row[2].month == 3:
        march_count += 1
```


## 10: Counting Posts From Any Month

Let's write a function that generalizes our counting logic and makes it works for any month.

### Instructions

- Write a function that takes in an integer value representing a month, and returns the number of posts users submitted during that month.
- Use the function to return the number of posts users submitted in February (`month` value of `2`), and assign the count to a new variable `feb_count`.
- Use the function to return the number of posts users submitted in August (`month` value of `8`), and assign the count to a new variable `aug_count`.

```
march_count = 0
for row in posts:
    if row[2].month == 3:
        march_count += 1
def count_posts_in_month(month):
    count = 0
    for row in posts:
        if row[2].month == month:
            count += 1
    return count

feb_count = count_posts_in_month(2)
aug_count = count_posts_in_month(8)
```

# Guided Project: Exploring Gun Deaths in the US

## 1: Introducing US Gun Deaths Data

In this project, you'll be working with Jupyter notebook, and analyzing data on gun deaths in the US. By the end, you'll have a notebook that you can add to your portfolio or build on top of on your own. If you need help at any point, you can consult our solution notebook [here](https://github.com/dataquestio/solutions/blob/master/Mission218Solution.ipynb).

The dataset came from [FiveThirtyEight](https://www.fivethirtyeight.com/), and can be found [here](https://github.com/fivethirtyeight/guns-data). The dataset is stored in the `guns.csv` file. It contains information on gun deaths in the US from `2012` to `2014`. Each row in the dataset represents a single fatality. The columns contain demographic and other information about the victim. Here are the first few rows of the dataset:

|      |      | year | month | intent  | police | sex  | age  | race                   | hispanic | place           | education |
| ---- | ---- | ---- | ----- | ------- | ------ | ---- | ---- | ---------------------- | -------- | --------------- | --------- |
| 0    | 1    | 2012 | 1     | Suicide | 0      | M    | 34.0 | Asian/Pacific Islander | 100      | Home            | 4.0       |
| 1    | 2    | 2012 | 1     | Suicide | 0      | F    | 21.0 | White                  | 100      | Street          | 3.0       |
| 2    | 3    | 2012 | 1     | Suicide | 0      | M    | 60.0 | White                  | 100      | Other specified | 4.0       |
| 3    | 4    | 2012 | 2     | Suicide | 0      | M    | 64.0 | White                  | 100      | Home            | 4.0       |
| 4    | 5    | 2012 | 2     | Suicide | 0      | M    | 31.0 | White                  | 100      | Other specified | 2.0       |

As you can see above, the first row of the data is a header row, which tells you what kind of data is in each column of the CSV file. Each row contains information about the fatality, and the victim. Here's an explanation of each column:

- ``-- this is an identifier column, which contains the row number. It's common in CSV files to include a unique identifier for each row, but we can ignore it in this analysis.
- `year` -- the year in which the fatality occurred.
- `month` -- the month in which the fatality occurred.
- `intent` -- the intent of the perpetrator of the crime. This can be `Suicide`, `Accidental`, `NA`, `Homicide`, or `Undetermined`.
- `police` -- whether a police officer was involved with the shooting. Either `0` (false) or `1` (true).
- `sex` -- the gender of the victim. Either `M` or `F`.
- `age` -- the age of the victim.
- `race` -- the race of the victim. Either `Asian/Pacific Islander`, `Native American/Native Alaskan`, `Black`, `Hispanic`, or `White`.
- `hispanic` -- a code indicating the Hispanic origin of the victim.
- `place` -- where the shooting occurred. Has several categories, which you're encouraged to explore on your own.
- `education` -- educational status of the victim. Can be one of the following:`1` -- Less than High School`2` -- Graduated from High School or equivalent`3` -- Some College`4` -- At least graduated from College`5` -- Not available

In this project, we'll explore the dataset, and try to find patterns in the demographics of the victims. Our first step is to read the data in and take a look at it.

### Instructions

- Read the dataset in as a list using the [csv](https://docs.python.org/3/library/csv.html) module.Import the `csv` module.Open the file using the [open()](https://docs.python.org/3/library/functions.html#open) function.Use the [csv.reader()](https://docs.python.org/3/library/csv.html#csv.reader) function to load the opened file.
- Call [list()](https://docs.python.org/3/library/functions.html#func-list) on the result to get a list of all the data in the file.Assign the result to the variable `data`.
- Display the first `5` rows of `data` to verify everything.

## 2: Removing Headers From A List Of Lists

In the last screen, we read our data into the list of lists `data`. Each inner list in `data` represents a single row. Each item in the inner lists represents a single column for that row. Here's how the first `5` rows should have looked:

```

```

```
[
```

```
    ['', 'year', 'month', 'intent', 'police', 'sex', 'age', 'race', 'hispanic', 'place', 'education'], 
```

```
    ['1', '2012', '01', 'Suicide', '0', 'M', '34', 'Asian/Pacific Islander', '100', 'Home', '4'], 
```

```
    ['2', '2012', '01', 'Suicide', '0', 'F', '21', 'White', '100', 'Street', '3'], 
```

```
    ['3', '2012', '01', 'Suicide', '0', 'M', '60', 'White', '100', 'Other specified', '4'], 
```

```
    ['4', '2012', '02', 'Suicide', '0', 'M', '64', 'White', '100', 'Home', '4']
```

```
]
```

You hopefully noticed that the first item in the `data` list is a header row. In order to analyze the data properly, we'll have to remove the header row, which contains the names of each column. We can remove this using list slicing. You can read more about lists [here](https://docs.python.org/3/tutorial/datastructures.html).

### Instructions

- Extract the first row of `data`, and assign it to the variable `headers`.
- Remove the first row from `data`.
- Display `headers`.
- Display the first `5` rows of `data` to verify that you removed the header row properly.

## 3: Counting Gun Deaths By Year

The `year` column contains information on the year in which gun deaths occurred. We can use this column to calculate how many gun deaths happened in each year.

We can perform this operation by creating a dictionary, then keeping count in the dictionary of how many times each element occurs in the year column.

### Instructions

- Use a list comprehension to extract the `year` column from `data`.Because the `year` column is the second column in the data, you'll need to get the element at index `1` in each row.Assign the result to the variable `years`.
- Create an empty dictionary called `year_counts`.
- Loop through each element in `years`.If the element isn't a key in `year_counts`, create it, and set the value to `1`.If the element is a key in `year_counts`, increment the value by one.
- Display `year_counts` to see how many gun deaths occur in each year.

## 4: Exploring Gun Deaths By Month And Year

It looks like gun deaths didn't change much by year from `2012` to `2014`. Let's see if gun deaths in the US change by month and year. In order to do this, we'll have to create a [datetime.datetime](https://docs.python.org/3/library/datetime.html#datetime-objects) object using the `year` and `month` columns. We'll then be about to count up gun deaths by date, like we did by `year` in the last screen.

As you may recall from an earlier mission, you can create a `datetime` object by specifying the `year`, `month`, and `day`keyword arguments:

```

```

```
date = datetime(year=2016, month=12, day=1)
```

We can use the `month` and `year` column of `data` to create a datetime. We'll specify a fixed `day` because we're missing that column in our data.

If we create a `datetime.datetime` object for each row, we can then count up how many gun deaths occurred in each month and year using a similar procedure to what we did in the last screen.

### Instructions

- Use a list comprehension to create a `datetime.datetime`object for each row. Assign the result to `dates`.The `year` column in the second element in each row.The `month` column is the third element in each row.Make sure to convert `year` and `month` to integers using [int()](https://docs.python.org/3/library/functions.html#int).Pass `year`, `month`, and `day=1` into the `datetime.datetime()` function.
- Display the first `5` rows in `dates` to verify everything worked.
- Count up how many times each unique date occurs in `dates`. Assign the result to `date_counts`.This follows a similar procedure to what we did in the last screen with `year_counts`.
- Display `date_counts`.

## 5: Exploring Gun Deaths By Race And Sex

The `sex` and `race` columns contain potentially interesting information on how gun deaths in the US vary by gender and race. Exploring both of these columns can be done with a similar dictionary counting technique to what we did earlier.

### Instructions

- Count up how many times each item in the `sex` column occurs.Assign the result to `sex_counts`.
- Count up how many times each item in the `race` column occurs.Assign the result to `race_counts`.
- Display `race_counts` and `sex_counts` to verify your work, and see if you can spot any patterns.
- Write a markdown cell detailing what you've learned so far, and what you think might need further examination.

## 6: Reading In A Second Dataset

We explored gun deaths by race in the past screen. However, our analysis only gives us the total number of gun deaths by race in the US. Unless we know the proportion of each race in the US, we won't be able to meaningfully compare those numbers. What we really want to get is a rate of gun deaths per `100000` people of each race. In order to do this, we'll need to read in data about what percentage of the US population falls into each racial category. Luckily, we can import some census data to help us out.

The data contains information on the total population of the US, as well as the total population of each racial group in the US. The data is stored in the `census.csv` file, and only consists of two rows:

|      | Id       | Year                 | Id.1   | Sex        | Id.2    | Hispanic Origin | Id.3      | Id2  | Geography     | Total     | Race Alone - White | Race Alone - Hispanic | Race Alone - Black or African American | Race Alone - American Indian and Alaska Native | Race Alone - Asian | Race Alone - Native Hawaiian and Other Pacific Islander | Two or More Races |
| ---- | -------- | -------------------- | ------ | ---------- | ------- | --------------- | --------- | ---- | ------------- | --------- | ------------------ | --------------------- | -------------------------------------- | ---------------------------------------- | ------------------ | ---------------------------------------- | ----------------- |
| 0    | cen42010 | April 1, 2010 Census | totsex | Both Sexes | tothisp | Total           | 0100000US | NaN  | United States | 308745538 | 197318956          | 44618105              | 40250635                               | 3739506                                  | 15159516           | 674625                                   | 6984195           |

As you can see, the first row is a header row, and the second row consists of population counts. We'll need to read this file in using the `csv.reader()` function.

### Instructions

- Read in `census.csv`, and convert to a list of lists. Assign the result to the `census` variable.
- Display `census` to verify your work.

## 7: Computing Rates Of Gun Deaths Per Race

Earlier, we computed the number of gun deaths per race, and created a dictionary, `race_counts`, that looked like this:

```

```

```
{
```

```
     'Asian/Pacific Islander': 1326,
```

```
     'Black': 23296,
```

```
     'Hispanic': 9022,
```

```
     'Native American/Native Alaskan': 917,
```

```
     'White': 66237
```

```
}
```

In order to get from the raw counts of gun deaths by race to a rate of gun deaths per `100000` people in each race, we'll need to divide the total number of gun deaths by the population of each race. From the census dataset, we know that the number of people in the `White` racial category is `197318956`. We'd divide `66237` by `197318956`:

```

```

```
white_gun_death_rate = 66237 / 197318956
```

This gives us the percentage chance that a given person in the `White` census race category would have been killed by a gun in the US from `2012` to `2014`. If you do this computation, you'll see that the rate is a very small number, `0.0003356849303419181`. It's for this reason that it's typical to express crime statistics as the "rate per 100000". This tells you the number of people in a given group out of every `100000` that were killed by guns in the US. To get this, we just multiply by `100000`:

```

```

```
rate_per_hundredk = 0.0003356849303419181 * 100000
```

This gives us `33.56`, which we can interpret as "`33.56` out of every `100000`people in the `White` census race category in the US were killed by guns between `2012` and `2014`".

We'll need to calculate these same rates for each racial category. The only stumbling block is that the racial categories are named slightly differently in `census` and in `data`. We'll need to manually construct a dictionary that allows us to map between them, and perform the division.

Here's a list of the race name in `data`, and the corresponding race name in `census`:

- `Asian/Pacific Islander` -- `Race Alone - Asian` plus `Race Alone - Native Hawaiian and Other Pacific Islander`.
- `Black` -- `Race Alone - Black or African American`.
- `Hispanic` -- `Race Alone - Hispanic`
- `Native American/Native Alaskan` -- `Race Alone - American Indian and Alaska Native`
- `White` -- `Race Alone - White`

We'll need to create a dictionary that has each race name from `data` as a key, and has the population count for the races from `census` as the values.

### Instructions

- Manually create a dictionary, `mapping` that maps each key from `race_counts` to the population count of the race from `census`.The keys in the dictionary should be `Asian/Pacific Islander`, `Black`, `Native American/Native Alaskan`, `Hispanic`, and `White`.In the case of `Asian/Pacific Islander`, you'll need to add the counts from `census` for `Race Alone - Asian`, and `Race Alone - Native Hawaiian and Other Pacific Islander`.
- Create an empty dictionary, `race_per_hundredk`.
- Loop through each key in `race_counts`.Divide the value associated with the key in `race_counts` by the value associated with the key in `mapping`.Multiply by `100000`.Assign the result to the same key in `race_per_hundredk`.
- When you're done, `race_per_hundredk` should contain the rate of gun deaths per `100000` people for each racial category.
- Print `race_per_hundredk` to verify your work.

## 8: Filtering By Intent

We can filter our results, and restrict them to the `Homicide` intent. This will tell us what the gun-related murder rate per `100000` people in each racial category is. In order to do this, we'll need to redo our work in generating `race_counts`, but only count rows where the `intent` was `Homicide`.

We can do this by first extracting the `intent` column, then using the [enumerate()](https://docs.python.org/3/library/functions.html#enumerate) function to loop through each index and value in the race column. If the value in the same position in `intents` is `Homicide`, we'll count the value in the race column.

Finally, we'll use the `mapping` dictionary to convert from raw counts to rates.

### Instructions

- Extract the `intent` column using a list comprehension. The `intent` column is the fourth column in `data`.Assign the result to `intents`.
- Extract the `race` column using a list comprehension. The `race` column is the eighth column in `data`.Assign the result to `races`.
- Create an empty dictionary called `homicide_race_counts`
- Use the `enumerate()` function to loop through each item in `races`. The position should be assigned to the loop variable `i`, and the value to the loop variable `race`.Check the value at position `i` in `intents`.If the value at position `i` in `intents` is `Homicide`:If the key `race` doesn't exist in `homicide_race_counts`, create it.Add `1` to the value associated with `race` in `homicide_race_counts`.
- When you're done, `homicide_race_counts` should have one key for each of the racial categories in `data`. The associated value should be the number of gun deaths by homicide for that race.
- Perform the same procedure we did in the last screen using `mapping` on `homicide_race_counts` to get from raw numbers to rates per `100000`.
- Display `homicide_race_counts` to verify your work.
- Write up your findings in a markdown cell.
- Write up any next steps you want to pursue with the data in a markdown cell.

## 9: Next Steps

That's it for the guided steps! We recommend exploring the data more on your own.

Here are some potential next steps:

- Figure out the link, if any, between month and homicide rate.
- Explore the homicide rate by gender.
- Explore the rates of other intents, like `Accidental`, by gender and race.
- Find out if gun death rates correlate to location and education.

We recommend creating a [Github](https://github.com/) repository and placing this project there. It will help other people, including employers, see your work. As you start to put multiple projects on Github, you'll have the beginnings of a strong portfolio. You're welcome to keep working on the project here, but we recommend downloading it to your computer using the download icon above and working on it there.