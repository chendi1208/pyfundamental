# Line Charts

## 1: Representation Of Data

So far, we've mostly been manipulating and working with data that are represented as tables. Microsoft Excel, the pandas library in Python, and the CSV file format for datasets were all developed around this representation. Because a table neatly organizes values into rows and columns, we can easily look up specific values at the intersection of a row value and a colum value. Unfortunately, it's very difficult to explore a dataset to uncover patterns when it's represented as a table, especially when that dataset contains many values. We need a different representation of data that can help us identify patterns more easily.

In this mission, we'll learn the basics of **data visualization**, a discipline that focuses on the visual representation of data. As humans, our brains have evolved to develop powerful visual processing capabilities. We can quickly find patterns in the visual information we encounter, which was incredibly important from a survivability standpoint. Unfortunately, when data is represented as tables of values, we can't really take advantage of our visual pattern matching capabilities. This is because our ability to quickly process symbolic values (like numbers and words) is very poor. Data visualization focuses on transforming data from table representations visual ones.

In this course, named **Exploratory Data Visualization**, we'll focus on data visualization techniques to explore datasets and help us uncover patterns. In this mission, we'll use a specific type of data visualization to understand U.S. unemployment data.

## 2: Introduction To The Data

The [United States Bureau of Labor Statistics](https://www.dataquest.io/m/142/line-charts/2/www.bls.gov) (BLS) surveys and calculates the monthly unemployment rate. The unemployment rate is the percentage of individuals in the labor force without a job. While unemployment rate [isn't perfect](https://en.wikipedia.org/wiki/Unemployment#Limitations_of_the_unemployment_definition), it's a commonly used proxy for the health of the economy. You may have heard politicians and reporters state the unemployment rate when commenting on the economy. You can read more about how the BLS calculates the unemployment rate [here](http://www.bls.gov/cps/cps_htgm.htm).

The BLS releases monthly unemployment data available for download as an Excel file, with the `.xlsx` file extension. While the pandas library can read in XLSX files, it relies on an external library for actually parsing the format. Let's instead download the same dataset as a CSV file from the website of the [Federal Reserve Bank of St. Louis](https://www.stlouisfed.org/). We've downloaded the monthly unemployment rate as a CSV from January 1948 to August 2016, saved it as `unrate.csv`, and made it available in this mission.

To download this dataset on your own, head to the [Federal Reserve Bank of St. Louis's website](https://fred.stlouisfed.org/series/UNRATE/downloaddata), select `Text, Comma Separated` as the **File Format**, make sure the **Date Range** field starts at `1948-01-01` and ends at `2016-08-01`. If these options are no longer present and the website has been changed because this mission was written, please let us know and we'll update the instructions!

Before we get into visual representations of data, let's first read this CSV file into pandas to explore the table representation of this data. The dataset we'll be working with is a [time series](https://en.wikipedia.org/wiki/Time_series) dataset, which means the data points (monthly unemployment rates) are ordered by time. Here's a preview of the dataset:

| DATE       | VALUE |
| ---------- | ----- |
| 1948-01-01 | 3.4   |
| 1948-02-01 | 3.8   |
| 1948-03-01 | 4.0   |
| 1948-04-01 | 3.9   |
| 1948-05-01 | 3.5   |

When we read the dataset into a DataFrame, pandas will set the data type of the `DATE` column as a text column. Because of how pandas reads in strings internally, this column is given a data type of `object`. We need to convert this column to the `datetime` type using the [pandas.to_datetime()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.to_datetime.html) function, which returns a Series object with the `datetime` data type that we can assign back to the DataFrame:

```python
import pandas as pd
df['col'] = pd.to_datetime(df['col'])
```

## 3: Introduction To The Data

### Instructions

- Read `unrate.csv` into a DataFrame and assign to `unrate`.
- Use the [pandas.to_datetime](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.to_datetime.html) function to convert the `DATE`column into a series of `datetime` values.
- Display the first 12 rows in `unrate`.

```
import pandas as pd
unrate = pd.read_csv('unrate.csv')
unrate['DATE'] = pd.to_datetime(unrate['DATE'])
print(unrate.head(12))
```

## 4: Table Representation

The dataset contains 2 columns:

- `DATE`: date, always the first of the month. Here are some examples:`1948-01-01`: January 1, 1948.`1948-02-01`: February 1, 1948.`1948-03-01`: March 1, 1948.`1948-12-01`: December 1, 1948.
- `VALUE`: the corresponding unemployment rate, in percent.

The first 12 rows reflect the unemployment rate from January 1948 to December 1948:

| DATE       | VALUE |
| ---------- | ----- |
| 1948-01-01 | 3.4   |
| 1948-02-01 | 3.8   |
| 1948-03-01 | 4.0   |
| 1948-04-01 | 3.9   |
| 1948-05-01 | 3.5   |
| 1948-06-01 | 3.6   |
| 1948-07-01 | 3.6   |
| 1948-08-01 | 3.9   |
| 1948-09-01 | 3.8   |
| 1948-10-01 | 3.7   |
| 1948-11-01 | 3.8   |
| 1948-12-01 | 4.0   |

Take a minute to visually scan the table and observe how the monthly unemployment rate has changed over time. When you're finished, head to the next step in this mission.

## 5: Observations From The Table Representation

We can make the following observations from the table:

- In 1948:
  - monthly unemployment rate ranged between `3.4` and `4.0`.
  - highest unemployment rate was reached in both March and December.
  - lowest unemployment rate was reached in January.
- From January to March, unemployment rate trended up.
- From March to May, unemployment rate trended down.
- From May to August, unemployment rate trended up.
- From August to October, unemployment rate trended down.
- From October to December, unemployment rate trended up.

Because the table only contained the data from 1948, it didn't take *too* much time to identify these observations. If we scale up the table to include all 824 rows, it would be very time-consuming and painful to understand. Tables shine at presenting information precisely at the intersection of rows and columns and allow us to perform quick lookups when we know the row and column we're interested in. In addition, problems that involve comparing values between adjacent rows or columns are well suited for tables. Unfortunately, many problems you'll encounter in data science require comparisons that aren't possible with just tables.

For example, one thing we learned from looking at the monthly unemployment rates for 1948 is that every few months, the unemployment rate switches between trending up and trending down. It's not switching direction every month, however, and this could mean that there's a seasonal effect. **Seasonality** is when a pattern is observed on a regular, predictable basis for a specific reason. A simple example of seasonality would be a large increase textbook purchases every August every year. Many schools start their terms in August and this spike in textbook sales is directly linked.

We need to first understand if there's any seasonality by comparing the unemployment trends across many years so we can decide if we should investigate it further. The faster we're able to assess our data, the faster we can perform high-level analysis quickly. If we're reliant on just the table to help us figure this out, then we won't be able to perform a high level test quickly. Let's see how a visual representation of the same information can be more helpful than the table representation.

## 6: Visual Representation

Instead of representing data using text like tables do, visual representations use visual objects like dots, shapes, and lines on a grid. [Plots](https://en.wikipedia.org/wiki/Plot_%28graphics%29) are a category of visual representations that allow us to easily understand the relationships between variables. There are many types of plots and selecting the right one is an important skill that you'll hone as you create data visualizations. Because we want to compare the unemployment trends across time, we should use **line charts**. Here's an overview of line charts using 4 sample data points:

![Line Plot Basics](https://s3.amazonaws.com/dq-content/line_plot_basics.png)

Line charts work best when there is a logical connection between adjacent points. In our case, that connection is the flow of time. Between 2 reported monthly unemployment values, the unemployment rate is fluctuating and time is passing. To emphasize how the visual representation of the line chart helps us observe trends easily, let's look at the same 12 data points from 1948 as a line chart.

![Unemployment Line Chart](https://s3.amazonaws.com/dq-content/unemp_line_plot.png)

We can reach the same observations about the data from the line chart as we did from the table representation:

![Line Plot Observations](https://s3.amazonaws.com/dq-content/unemp_plot_observations.png)

In the rest of this mission, we'll explore how to recreate this line chart in Python. In the next mission, we'll explore how to create multiple line charts to help us compare unemployment trends.

## 7: Introduction To Matplotlib

To create the line chart, we'll use the [matplotlib](http://matplotlib.org/) library, which allows us to:

- quickly create common plots using high-level functions
- extensively tweak plots
- create new kinds of plots from the ground up

To help you become familiar with matplotlib, we'll focus on the first 2 use cases. When working with commonly used plots in matplotlib, the general workflow is:

- create a plot using data
- customize the appearance of the plot
- display the plot
- edit and repeat until satisfied

This interactive style aligns well with the exploratory workflow of data visualization because we're asking questions and creating data visualizations to help us get answers. The [pyplot](http://matplotlib.org/api/pyplot_api.html) module provides a high-level interface for matplotlib that allows us to quickly create common data plots and perform common tweaks to them.

The pyplot module is commonly imported as `plt` from `matplotlib`:

```
import matplotlib.pyplot as plt
```

Using the different pyplot functions, we can create, customize, and display a plot. For example, we can use 2 functions to :

```
plt.plot()
plt.show()
```

Because we didn't pass in any arguments, the [plot()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.plot) function would generate an empty plot with just the axes and ticks and the [show()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.show) function would display that plot. You'll notice that we didn't assign the plot to a variable and then call a method on the variable to display it. We instead called 2 functions on the pyplot module directly.

This is because every time we call a pyplot function, the module maintains and updates the plot internally (also known as state). When we call `show()`, the plot is displayed and the internal state is destroyed. While this workflow isn't ideal when we're writing functions that create plots on a repeated basis as part of a larger application, it's useful when exploring data.

Let's run this code to see the default properties matplotlib uses. If you'd like to follow along on your own computer, we recommend installing matplotlib using Anaconda: `conda install matplotlib`. We recommend working with matplotlib using Jupyter Notebook because it can render the plots in the notebook itself. You will need to run the following Jupyter magic in a code cell each time you open your notebook: `%matplotlib inline`. Whenever you call `show()`, the plots will be displayed in the output cell. You can read more [here](http://ipython.readthedocs.io/en/stable/interactive/plotting.html).

### Instructions

- Generate an empty plot using `plt.plot()` and display it using `plt.show()`.

```
import matplotlib.pyplot as plt
plt.plot()
plt.show()
```

## 8: Adding Data

By default, Matplotlib displayed a coordinate grid with:

- the x-axis and y-axis values ranging from `-0.06` to `0.06`
- no grid lines
- no data

Even though no data was plotted, the x-axis and y-axis **ticks** corresponding to the `-0.06` to `0.06` value range. The axis ticks consist of tick marks and tick labels. Here's a focused view of the x-axis tick marks and x-axis tick labels:

![Axis Ticks](https://s3.amazonaws.com/dq-content/axis+ticks.png)

To create a line chart of the unemployment data from 1948, we need:

- the x-axis to range from `01-01-1948`to `12-01-1948` (which corresponds to the first and last months in 1948)
- the y-axis to range from `3.4` to `4.0` (which correspond to the minimum and maximum unemployment values)

Instead of manually updating the ticks, drawing each marker, and connecting the markers with lines, we can just specify the data we want plotted and let matplotlib handle the rest. To generate the line chart we're interested in, we pass in the list of x-values as the first parameter and the list of y-values as the second parameter to [plot()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.plot):

```
plt.plot(x_values, y_values)
```

Matplotlib will accept any iterable object, like NumPy arrays and `pandas.Series`instances.

### Instructions

- Generate a line chart that visualizes the unemployment rates from 1948:
  - x-values should be the first 12 values in the `DATE`column
  - y-values should be the first 12 values in the `VALUE`column
- Display the plot.

```python
# Assigned first 12 rows to a variable just for easy reference.
first_twelve = unrate[0:12]
plt.plot(first_twelve['DATE'], first_twelve['VALUE']) # line
plt.show()
```
## 9: Fixing Axis Ticks

While the y-axis looks fine, the x-axis **tick labels** are too close together and are unreadable. The line charts from earlier in the mission suggest a better way to display the x-axis tick labels:

![Unemployment Line Plot](https://s3.amazonaws.com/dq-content/unemp_line_plot.png)

We can rotate the x-axis tick labels by 90 degrees so they don't overlap. The `xticks()` function within pyplot lets you customize the behavior of the x-axis ticks. If you head over to the [documentation for that function](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.xticks), it's not immediately obvious the arguments it takes:

```
matplotlib.pyplot.xticks(*args, **kwargs)
```

In the documentation for the function, you'll see a link to the matplotlib [Text](http://matplotlib.org/api/text_api.html#matplotlib.text.Text) class, which is what pyplot uses to represent the x-axis tick labels. You'll notice that there's a `rotation` parameter that accepts degrees of rotation as a parameter. We can specify degrees of rotation using a float or integer value.

As a side note, if you read the [documentation for pyplot](http://matplotlib.org/api/pyplot_api.html), you'll notice that many functions for tweaking the x-axis have matching functions for the y-axis. For example, the y-axis counterpart to the [xticks()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.xticks) function is the yticks() function.

Use what we've discussed so far to rotate the x-axis tick labels by 90 degrees.

### Instructions

- Generate the same line chart from the last screen that visualizes the unemployment rates from 1948:
  - x-values should be the first 12 values in the `DATE`column
  - y-values should be the first 12 values in the `VALUE`column
- Use `pyplot.xticks()` to rotate the x-axis tick labels by `90` degrees.
- Display the plot.
```python
plt.plot(first_twelve['DATE'], first_twelve['VALUE'])
plt.xticks(rotation=90)
plt.show()
```

## 10: Adding Axis Labels And A Title

Let's now finish tweaking this plot by adding axis labels and a title. Always adding axis labels and a title to your plot is a good habit to have, and is especially useful when we're trying to keep track of multiple plots down the road.

Here's an overview of the pyplot functions we need to tweak the axis labels and the plot title:

- [xlabel()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.xlabel): accepts a string value, which gets set as the x-axis label.
- [ylabel()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.ylabel): accepts a string value, which is set as the y-axis label.
- [title()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.title): accepts a string value, which is set as the plot title.

### Instructions

- Generate the same line chart from the last screen that visualizes the unemployment rates from 1948:
  - x-values should be the first 12 values in the `DATE`column
  - y-values should be the first 12 values in the `VALUE`column
  - Rotate the x-axis tick labels by `90` degrees.
- Set the x-axis label to `"Month"`.
- Set the y-axis label to `"Unemployment Rate"`.
- Set the plot title to `"Monthly Unemployment Trends, 1948"`.
- Display the plot.

```python
plt.plot(first_twelve['DATE'], first_twelve['VALUE'])
plt.xticks(rotation=90)
plt.xlabel('Month')
plt.ylabel('Unemployment Rate')
plt.title('Monthly Unemployment Trends, 1948')
plt.show()
```

## 11: Next Steps

In this mission, we looked at a sample of a dataset on unemployment as a table and noted some observations. We introduced line charts and witnessed how a visual representation of the same data sample allowed us to surface the same observations quickly. Finally, we explored the basics of the pyplot module and learned how to create and customize a line chart that visualized the sample.

In the next mission, we'll dive deeper into matplotlib and learn how to create multiple plots to compare unemployment trends across multiple years.

# Multiple plots

## 1: Recap

In the previous mission, we explored how a visual representation of data can help us reach observations about data more quickly than a table representation of the same data. We learned how to work with the pyplot module, which provides a high-level interface to the matplotlib library, to create and customize a line chart of unemployment data. To look for potential seasonality, we started by creating a line chart of unemployment rates from 1948.

In this mission, we'll dive a bit deeper into matplotlib to learn how to create multiple line charts to help us compare monthly unemployment trends across time. The unemployment dataset contains 2 columns:

- `DATE`: date, always the first of the month. Examples:`1948-01-01`: January 1, 1948.`1948-02-01`: February 1, 1948.`1948-03-01`: March 1, 1948.`1948-12-01`: December 1, 1948.
- `VALUE`: the corresponding unemployment rate, in percent.

Here's what the first 12 rows look like, which reflect the unemployment rate from January 1948 to December 1948:

| DATE       | VALUE |
| ---------- | ----- |
| 1948-01-01 | 3.4   |
| 1948-02-01 | 3.8   |
| 1948-03-01 | 4.0   |
| 1948-04-01 | 3.9   |
| 1948-05-01 | 3.5   |
| 1948-06-01 | 3.6   |
| 1948-07-01 | 3.6   |
| 1948-08-01 | 3.9   |
| 1948-09-01 | 3.8   |
| 1948-10-01 | 3.7   |
| 1948-11-01 | 3.8   |
| 1948-12-01 | 4.0   |

Let's practice what you learned in the previous mission.

### Instructions

- Read `unrate.csv` into a DataFrame and assign to `unrate`.
- Use [Pandas.to_datetime](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.to_datetime.html) to convert the `DATE` column into a Series of `datetime` values.
- Generate a line chart that visualizes the unemployment rates from 1948:
  - x-values should be the the first 12 values in the `DATE`column
  - y-values should be the first 12 values in the `VALUE`column
- Use `pyplot.xticks()` to rotate the x-axis tick labels by `90` degrees.
- Use `pyplot.xlabel()` to set the x-axis label to `"Month"`.
- Use `pyplot.ylabel()` to set the y-axis label to `"Unemployment Rate"`.
- Use `pyplot.title()` to set the plot title to `"Monthly Unemployment Trends, 1948"`.
- Display the plot.

```
import pandas as pd
import matplotlib.pyplot as plt

unrate = pd.read_csv('unrate.csv')
unrate['DATE'] = pd.to_datetime(unrate['DATE'])
first_twelve = unrate[0:12]
plt.plot(first_twelve['DATE'], first_twelve['VALUE'])
plt.xticks(rotation=90)
plt.xlabel('Month')
plt.ylabel('Unemployment Rate')
plt.title('Monthly Unemployment Trends, 1948')
plt.show()
```

## 2: Matplotlib Classes

When we were working with a single plot, pyplot was storing and updating the state of that single plot. We could tweak the plot just using the functions in the pyplot module. When we want to work with multiple plots, however, we need to be more explicit about which plot we're making changes to. This means we need to understand the matplotlib classes that pyplot uses internally to maintain state so we can interact with them directly. Let's first start by understanding what pyplot was automatically storing under the hood when we create a single plot:

- a container for all plots was created (returned as a [Figure object](http://matplotlib.org/api/figure_api.html#matplotlib.figure.Figure))
- a container for the plot was positioned on a grid (the plot returned as an [Axes object](http://matplotlib.org/api/axes_api.html#matplotlib-axes))
- visual symbols were added to the plot (using the Axes methods)

A figure acts as a container for all of our plots and has methods for customizing the appearance and behavior for the plots within that container. Some examples include changing the overall width and height of the plotting area and the spacing between plots.

We can manually create a figure by calling [pyplot.figure()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.figure):

```
fig = plt.figure()
```

Instead of only calling the pyplot function, we assigned its return value to a variable (`fig`). After a figure is created, an axes for a single plot containing no data is created within the context of the figure. When rendered without data, the plot will resemble the empty plot from the previous mission. The Axes object acts as its own container for the various components of the plot, such as:

- values on the x-axis and y-axis
- ticks on the x-axis and y-axis
- all visual symbols, such as:
  - markers
  - lines
  - gridlines

While plots are represented using instances of the Axes class, they're also often referred to as subplots in matplotlib. To add a new subplot to an existing figure, use [Figure.add_subplot](http://matplotlib.org/api/figure_api.html#matplotlib.figure.Figure.add_subplot). This will return a new Axes object, which needs to be assigned to a variable:

```
axes_obj = fig.add_subplot(nrows, ncols, plot_number)
```

If we want the figure to contain 2 plots, one above the other, we need to write:

```
ax1 = fig.add_subplot(2,1,1)
ax2 = fig.add_subplot(2,1,2)
```

This will create a grid, 2 rows by 1 column, of plots. Once we're done adding subplots to the figure, we display everything using `plt.show()`:

```python
import matplotlib.pyplot as plt
fig = plt.figure()
ax1 = fig.add_subplot(2,1,1)
ax2 = fig.add_subplot(2,1,2)
plt.show()
```

Let's create a figure, add subplots to it, and display it.

## 3: Matplotlib Classes

### Instructions

- Use `plt.figure()` to create a figure and assign to `fig`.
- Use `Figure.add_subplot()` to create two subplots above and below each other
- Assign the top Axes object to `ax1`.
- Assign the top Axes object to `ax2`.
- Use `plt.show()` to display the resulting plot.

```
import matplotlib.pyplot as plt
fig = plt.figure()
ax1 = fig.add_subplot(2,1,1)
ax2 = fig.add_subplot(2,1,2)
plt.show()
```

## 4: Grid Positioning

For each subplot, matplotlib generated a coordinate grid that was similar to the one we generated in the last mission using the `plot()` function:

- the x-axis and y-axis values ranging from `0.0` to `1.0`
- no gridlines
- no data

The main difference is that this plot ranged from `0.0` to `1.0` instead of from `-0.06` to `0.06`, which is a quirk suggested by a difference in default properties.

Now that we have a basic understanding of the important matplotlib classes, we can create multiple plots to compare monthly unemployment trends. If you recall, we need to specify the position of each subplot on a grid. Here's a diagram that demonstrates how a 2 by 2 subplot layout would look like:

![Multiple Subplots](https://s3.amazonaws.com/dq-content/multiple_subplots.png)

When the first subplot is created, matplotlib knows to create a grid with 2 rows and 2 columns. As we add each subplot, we specify the plot number we want returned and the corresponding Axes object is created and returned. In matplotlib, the plot number starts at the top left position in the grid (left-most plot on the top row), moves through the remaining positions in that row, then jumps to the left-most plot in the second row, and so forth.

![Subplot Grid](https://s3.amazonaws.com/dq-content/subplot_grid.png)

If we created a grid of 4 subplots but don't create a subplot for each position in the grid, areas without axes are left blank:

![Missing One Plot](https://s3.amazonaws.com/dq-content/multiple_subplots_missing_one_plot.png)

## 5: Adding Data

To generate a line chart within an Axes object, we need to call [Axes.plot()](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.plot) and pass in the data you want plotted:

```
x_values = [0.0, 0.5, 1.0]
y_values = [10, 20, 40]
ax1.plot(x_values, y_values)
```

Like `pyplot.plot()`, the `Axes.plot()` will accept any iterable object for these parameters, including NumPy arrays and pandas Series objects. It will also use generate a line chart by default from the values passed in. Each time we want to generate a line chart, we need to call `Axes.plot()` and pass in the data we want to use in that plot.

### Instructions

Create 2 line subplots in a 2 row by 1 column layout:

- In the top subplot, plot the data from 1948.
  - For the x-axis, use the first 12 values in the `DATE`column.
  - For the y-axis, use the first 12 values in the `VALUE`column.
- In the bottom subplot, plot the data from 1949.
  - For the x-axis, use the values from index 12 to 24 in the `DATE` column.
  - For the y-axis, use the values from index 12 to 24 in the `VALUE` column.

Use `plt.show()` to display all the plots.

```python
fig = plt.figure()
ax1 = fig.add_subplot(2,1,1)
ax2 = fig.add_subplot(2,1,2)

ax1.plot(unrate[0:12]['DATE'], unrate[0:12]['VALUE'])
ax2.plot(unrate[12:24]['DATE'], unrate[12:24]['VALUE'])

plt.show()
```

## 6: Formatting And Spacing

One issue with the 2 plots is that the x-axis ticks labels are unreadable. The other issue is that the plots are squeezed together vertically and hard to interpret. Even though now we generated 2 line charts, the total plotting area for the figure remained the same:

![Two Plots](https://s3.amazonaws.com/dq-content/plotting_area_stays_same.png)

This is because matplotlib used the default dimensions for the total plotting area instead of resizing it to accommodate the plots. If we want to expand the plotting area, we have to specify this ourselves when we create the figure. To tweak the dimensions of the plotting area, we need to use the `figsize` parameter when we call `plt.figure()`:

This parameter takes in a tuple of floats:

```python
fig = plt.figure(figsize=(width, height))
```

The unit for both width and height values is inches. The `dpi` parameter, or dots per inch, and the `figsize` parameter determine how much space on your display a plot takes up. By increasing the width and the height of the plotting area, we can address both issues.

### Instructions

For the plot we generated in the last screen, set the width of the plotting area to `12` inches and the height to `6` inches.

```
fig = plt.figure(figsize=(12,5))
ax1 = fig.add_subplot(2,1,1)
ax2 = fig.add_subplot(2,1,2)
ax1.plot(unrate[0:12]['DATE'], unrate[0:12]['VALUE'])
ax1.set_title('Monthly Unemployment Rate, 1948')
ax2.plot(unrate[12:24]['DATE'], unrate[12:24]['VALUE'])
ax2.set_title('Monthly Unemployment Rate, 1949')
plt.show()
```

## 7: Comparing Across More Years

Instead of having to rotate the x-axis tick labels, we were able to horizontally extend the entire plotting area to make the labels more readable. Because the goal is to be able to look for any visual similarities between the lines in the plots, we want the space between the 2 plots to be as small as possible. If we had rotated the labels by 90 degrees instead, like we did in the last mission, we'd need to increase the spacing between the plots to keep them from overlapping. Expanding the plotting area horizontally improved the readability of the x-axis tick labels and minimized the amount of space between the 2 line charts.

If you recall, we generated these 2 line charts because we were interested in looking for any seasonality in the monthly unemployment trends. If you spend some time visually analyzing both line charts, you'll discover that there's no changes in unemployment trends that are occurring in the same month in both years.

Let's visualize data from a few more years to see if we find any evidence for seasonality between those years.

### Instructions

- Set the width of the plotting area to `12` inches and the height to `12` inches.
- Generate a grid with 5 rows and 1 column and plot data from the individual years. Start with 1948 in the top subplot and end with 1952 in the bottom subplot.
- Use `plt.show()` to display the plots.

```
fig = plt.figure(figsize=(12,12))

for i in range(5):
    ax = fig.add_subplot(5,1,i+1)
    start_index = i*12
    end_index = (i+1)*12
    subset = unrate[start_index:end_index]
    ax.plot(subset['DATE'], subset['VALUE'])

plt.show()
```

## 8: Overlaying Line Charts

By adding more line charts, we can look across more years for seasonal trends. This comes at a cost, unfortunately. We now have to visually scan over more space, which is a limitation that we experienced when scanning the table representation of the same data. If you recall, one of the limitations of the table representation we discussed in the previous mission was the amount of time we'd have to spend scanning the table as the number of rows increased significantly.

We can handle the visual overhead each additional plot adds by overlaying the line charts in a single subplot. If we remove the year from the x-axis and just keep the month values, we can use the same x-axis values to plot all of the lines. First, we'll explore how to extract just the month values from the `DATE` column, then we'll dive into generating multipe plots on the same coordinate grid.

To extract the month values from the `DATE` column and assign them to a new column, we can use the [pandas.Series.dt](http://pandas.pydata.org/pandas-docs/stable/basics.html#basics-dt-accessors) accessor:

```
unrate['MONTH'] = unrate['DATE'].dt.month
```

Calling `pandas.Series.dt.month` returns a Series containing the integer values for each month (e.g. `1` for January, `2` for February, etc.). Under the hood, pandas applies the [datetime.date](https://docs.python.org/3/library/datetime.html#datetime.date.month) function over each datetime value in the `DATE` column, which returns the integer month value. Let's now move onto generating multiple line charts in the same subplot.

In the last mission, we called `pyplot.plot()` to generate a single line chart. Under the hood, matplotlib created a figure and a single subplot for this line chart. If we call `pyplot.plot()` multiple times, matplotlib will generate the line charts on the single subplot.

```
plt.plot(unrate[0:12]['MONTH'], unrate[0:12]['VALUE'])
plt.plot(unrate[12:24]['MONTH'], unrate[12:24]['VALUE'])
```

If we want to set the dimensions for the plotting area, we can create the figure ourselves first then plot the data. This is because matplotlib first checks if a figure already exists before plotting data. It will only create one if we didn't create a figure.

```
fig = plt.figure(figsize=(6,6))
plt.plot(unrate[0:12]['MONTH'], unrate[0:12]['VALUE'])
plt.plot(unrate[12:24]['MONTH'], unrate[12:24]['VALUE'])
```

By default, matplotlib will select a different color for each line. To specify the color ourselves, use the `c` parameter when calling `plot()`: 


```
plt.plot(unrate[0:12]['MONTH'], unrate[0:12]['VALUE'], c='red')
```

You can read about the different ways we can specify colors in matplotlib [here](http://matplotlib.org/api/colors_api.html).

### Instructions

- Set the plotting area to a width of `6` inches and a height of `3` inches.
- Generate 2 line charts in the base subplot, using the `MONTH`column for the x-axis instead of the `DATE` column:
  - One line chart using data from 1948, with the line color set to `"red"`.
  - One line chart using data from 1949, with the line color set to `"blue"`.
- Use `plt.show()` to display the plots.

```python
unrate['MONTH'] = unrate['DATE'].dt.month
fig = plt.figure(figsize=(6,3))

plt.plot(unrate[0:12]['MONTH'], unrate[0:12]['VALUE'], c='red')
plt.plot(unrate[12:24]['MONTH'], unrate[12:24]['VALUE'], c='blue')

plt.show()
```

## 9: Adding More Lines

Let's visualize 5 years worth of unemployment rates on the same subplot.

### Instructions

- Set the plotting area to a width of `10` inches and a height of `6` inches.
- Generate the following plots in the base subplot:
  - 1948: set the line color to `"red"`
  - 1949: set the line color to `"blue"`
  - 1950: set the line color to `"green"`
  - 1951: set the line color to `"orange"`
  - 1952: set the line color to `"black"`
- Use `plt.show()` to display the plots.

```
fig = plt.figure(figsize=(10,6))
colors = ['red', 'blue', 'green', 'orange', 'black']
for i in range(5):
    start_index = i*12
    end_index = (i+1)*12
    subset = unrate[start_index:end_index]
    plt.plot(subset['MONTH'], subset['VALUE'], c=colors[i])
    
plt.show()
```

## 10: Adding A Legend

How colorful! By plotting all of the lines in one coordinate grid, we got a different perspective on the data. The main thing that sticks out is how the blue and green lines span a larger range of y values (4% to 8% for blue and 4% to 7% for green) while the 3 plots below them mostly range only between 3% and 4%. You can tell from the last sentence that we don't know which line corresponds to which year, because the x-axis now only reflects the month values.

To help remind us which year each line corresponds to, we can add a **legend** that links each color to the year the line is representing. Here's what a legend for the lines in the last screen could look like:

![Legend](https://s3.amazonaws.com/dq-content/legend.png)

When we generate each line chart, we need to specify the text label we want each color linked to. The `pyplot.plot()`function contains a `label` parameter, which we use to set the year value:

```python
plt.plot(unrate[0:12]['MONTH'], unrate[0:12]['VALUE'], c='red', label='1948')
plt.plot(unrate[12:24]['MONTH'], unrate[12:24]['VALUE'], c='blue', label='1949')
```

We can create the legend using [pyplot.legend](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.legend) and specify its location using the `loc` parameter:

```python
plt.legend(loc='upper left')
```

If we're instead working with multiple subplots, we can create a legend for each subplot by mirroring the steps for each subplot. When we use `plt.plot()` and `plt.legend()`, the `Axes.plot()` and [Axes.legend()](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.legend) methods are called under the hood and parameters passed to the calls. When we need to create a legend for each subplot, we can use `Axes.legend()`instead.

Let's now add a legend for the plot we generated in the last screen.

### Instructions

- Modify the code from the last screen that overlaid 5 plots to include a legend. Use the year value for each line chart as the label.
  - E.g. the plot of 1948 data that uses `"red"` for the line color should be labeled `"1948"` in the legend.
- Place the legend in the `"upper left"` corner of the plot.
- Display the plot using `plt.show()`.

```
fig = plt.figure(figsize=(10,6))
colors = ['red', 'blue', 'green', 'orange', 'black']
for i in range(5):
    start_index = i*12
    end_index = (i+1)*12
    subset = unrate[start_index:end_index]
    label = str(1948 + i)
    plt.plot(subset['MONTH'], subset['VALUE'], c=colors[i], label=label)
plt.legend(loc='upper left')

plt.show()
```

## 11: Final Tweaks

Instead of referring back to the code each time we want to confirm what subset each line corresponds to, we can focus our gaze on the plotting area and use the legend. At the moment, the legend unfortunately covers part of the green line (which represents data from 1950). Since the legend isn't critical to the plot, we should move this outside of the coordinate grid. We'll explore how to do so in a later course because it requires a better understanding of some design principles as well as matplotlib.

Before we wrap up this mission, let's enhance the visualization by adding a title and labels for both axes. To set the title, we use [pyplot.title()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.title) and pass in a string value:

```
plt.title("Monthly Unemployment Trends, 1948-1952")
```

To set the x-axis and y-axis labels, we use [pyplot.xlabel()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.xlabel) and [pyplot.ylabel()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.ylabel). Both of these methods accept string values.

### Instructions

- Modify the code from the last screen:
  - Set the title to `"Monthly Unemployment Trends, 1948-1952"`.
  - Set the x-axis label to `"Month, Integer"`.
  - Set the y-axis label to `"Unemployment Rate, Percent"`.

```
fig = plt.figure(figsize=(10,6))
colors = ['red', 'blue', 'green', 'orange', 'black']
for i in range(5):
    start_index = i*12
    end_index = (i+1)*12
    subset = unrate[start_index:end_index]
    label = str(1948 + i)
    plt.plot(subset['MONTH'], subset['VALUE'], c=colors[i], label=label)
plt.legend(loc='upper left')
plt.xlabel('Month, Integer')
plt.ylabel('Unemployment Rate, Percent')
plt.title('Monthly Unemployment Trends, 1948-1952')

plt.show()
```

## 12: Next Steps

In this mission, we learned about the important matplotlib building blocks and used them to experiment with creating multiple line charts. In the next mission, we'll explore plots that allow us to visualize discrete data.

# Bar Plots And Scatter Plots

## 1: Recap

In the previous missions in this course, we explored trends in unemployment data using line charts. The unemployment data we worked with had 2 columns:

- `DATE` - monthly time stamp
- `VALUE` - unemployment rate (in percent)

Line charts were an appropriate choice for visualizing this dataset because the rows had a natural ordering to it. Each row reflected information about an event that occurred after the previous row. Changing the order of the rows would make the line chart inaccurate. The lines from one marker to the next helped emphasize the logical connection between the data points.

In this mission, we'll be working with a dataset that has no particular order. Before we explore other plots we can use, let's get familiar with the dataset we'll be working with.

## 2: Introduction To The Data

To investigate how different movie review sites the potential bias that movie reviews site has, [FiveThirtyEight](https://www.dataquest.io/m/144/bar-plots-and-scatter-plots/2/www.fivethirtyeight.com) compiled data for 147 films from 2015 that have substantive reviews from both critics and consumers. Every time Hollywood releases a movie, critics from [Metacritic](https://www.dataquest.io/m/144/bar-plots-and-scatter-plots/2/www.metacritic.com), [Fandango](https://www.dataquest.io/m/144/bar-plots-and-scatter-plots/2/www.fandango.com), [Rotten Tomatoes](https://www.dataquest.io/m/144/bar-plots-and-scatter-plots/2/www.rottentomatoes.com), and [IMDB](https://www.dataquest.io/m/144/bar-plots-and-scatter-plots/2/www.imdb.com) review and rate the film. They also ask the users in their respective communities to review and rate the film. Then, they calculate the average rating from both critics and users and display them on their site. Here are screenshots from each site:

![Imgur](https://s3.amazonaws.com/dq-content/review_sites_screenshots.png)

FiveThirtyEight compiled this dataset to investigate if there was any bias to Fandango's ratings. In addition to aggregating ratings for films, Fandango is unique in that it also sells movie tickets, and so it has a direct commercial interest in showing higher ratings. After discovering that a few films that weren't good were still rated highly on Fandango, the team investigated and published [an article about bias in movie ratings](http://fivethirtyeight.com/features/fandango-movies-ratings/).

We'll be working with the `fandango_scores.csv` file, which can be downloaded from the [FiveThirtEight Github repo](https://github.com/fivethirtyeight/data/tree/master/fandango). Here are the columns we'll be working with in this mission:

- `FILM` - film name
- `RT_user_norm` - average user rating from Rotten Tomatoes, normalized to a 1 to 5 point scale
- `Metacritic_user_nom` - average user rating from Metacritc, normalized to a 1 to 5 point scale
- `IMDB_norm` - average user rating from IMDB, normalized to a 1 to 5 point scale
- `Fandango_Ratingvalue` - average user rating from Fandango, normalized to a 1 to 5 point scale
- `Fandango_Stars` - the rating displayed on the Fandango website (rounded to nearest star, 1 to 5 point scale)

Instead of displaying the raw rating, the writer discovered that Fandango usually rounded the average rating to the next highest half star (next highest `0.5`value). The `Fandango_Ratingvalue` column reflects the true average rating while the `Fandango_Stars` column reflects the displayed, rounded rating.

Let's read in this dataset, which allows us to compare how a movie fared across all 4 review sites.

### Instructions

- Read `fandango_scores.csv` into a Dataframe named `reviews`.
- Select the following columns and assign the resulting Dataframe to `norm_reviews`:`FILM``RT_user_norm``Metacritic_user_nom` (note the misspelling of `norm`)`IMDB_norm``Fandango_Ratingvalue``Fandango_Stars`
- Display the first row in `norm_reviews`

```
import pandas as pd
reviews = pd.read_csv('fandango_scores.csv')
cols = ['FILM', 'RT_user_norm', 'Metacritic_user_nom', 'IMDB_norm', 'Fandango_Ratingvalue', 'Fandango_Stars']
norm_reviews = reviews[cols]
print(norm_reviews[:1])
```

## 3: Bar Plot

These sites use different scales for ratings. Some use a 5 star scale while others use a 100 point scale. In addition, Metacritic and Rotten Tomatoes aggregate scores from both users and film critics while IMDB and Fandango aggregate only from their users. We'll focus on just the average scores from users, because not all of the sites have scores from critics.

The `RT_user_norm`, `Metacritic_user_nom`, `IMDB_norm`, and `Fandango_Ratingvalue` columns contain the average user rating for each movie, normalized to a 0 to 5 point scale. This allows us to compare how the users on each site rated a movie. While using averages isn't perfect because films with a few reviews can skew the average rating, FiveThirtyEight only selected movies with a non-trivial number of ratings to ensure films with only a handful of reviews aren't included.

If you look at the first row, which lists the average user ratings for **Avengers: Age of Ultron (2015)**, you'll notice that the Fandango ratings, both the actual and the displayed rating, are higher than those from the other sites for a given movie. While calculating and comparing summary statistics give us hard numbers for quantifying the bias, visualizing the data using plots can help us gain a more intuitive understanding. We need a visualization that scales graphical objects to the quantitative values we're interested in comparing. One of these visualizations is a **bar plot**.

![Vertical Bar Plot](https://s3.amazonaws.com/dq-content/vertical_bar_plot.png)

In the bar plot above, the x-axis represented the different ratings and the y-axis represented the actual ratings. An effective bar plot uses categorical values on one axis and numerical values on the other axis. Because bar plots can help us find the category corresponding to the smallest or largest values, it's important that we restrict the number of bars in a single plot. Using a bar plot to visualize hundreds of values makes it difficult to trace the category with the smallest or largest value.

If the x-axis contains the categorical values and the rectangular bars are scaled vertically, this is known as a vertical bar plot. A horizontal bar plot flips the axes, which is useful for quickly spotting the largest value.

![Horizontal Bar Plot](https://s3.amazonaws.com/dq-content/horizontal_bar_plot.png)

An effective bar plot uses a consistent width for each bar. This helps keep the visual focus on the heights of the bars when comparing. Let's now learn how to create a vertical bar plot in matplotlib that represents the different user scores for **Avengers: Age of Ultron (2015)**.

## 4: Creating Bars

When we generated line charts, we passed in the data to `pyplot.plot()` and matplotlib took care of the rest. Because the markers and lines in a line chart correspond directly with x-axis and y-axis coordinates, all matplotlib needed was the data we wanted plotted. To create a useful bar plot, however, we need to specify the positions of the bars, the widths of the bars, and the positions of the axis labels. Here's a diagram that shows the various values we need to specify:

![Matplotlib Barplot Positioning](https://s3.amazonaws.com/dq-content/matplotlib_barplot_positioning.png)

We'll focus on positioning the bars on the x-axis in this step and on positioning the x-axis labels in the next step. We can generate a vertical bar plot using either [pyplot.bar()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.bar) or [Axes.bar()](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.bar). We'll use `Axes.bar()` so we can extensively customize the bar plot more easily. **We can use [pyplot.subplots()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.subplots) to first generate a single subplot and return both the Figure and Axes object.** This is a shortcut from the technique we used in the previous mission:

```
fig, ax = plt.subplots()
```

The `Axes.bar()` method has 2 required parameters, `left` and `height`. We use the `left` parameter to specify the x coordinates of the left sides of the bar. We use the `height` parameter to specify the height of each bar. Both of these parameters accept a list-like object:

```
# Positions of the left sides of the bars. [0.75, 1.75, 2.75, 3.75, 4.75]
from numpy import arange
bar_positions = arange(5) + 0.75

# Heights of the bars.  In our case, the average rating for the first movie in the dataset.
num_cols = ['RT_user_norm', 'Metacritic_user_nom', 'IMDB_norm', 'Fandango_Ratingvalue', 'Fandango_Stars']
bar_heights = norm_reviews.ix[0, num_cols].values

ax.bar(bar_positions, bar_heights)
```

We can also use the `width` parameter to specify the width of each bar. This is an optional parameter and the width of each bar is set to `0.8` by default. The following code sets the `width` parameter to `1.5`:

```
ax.bar(bar_positions, bar_heights, 1.5)
```

### Instructions

- Create a single subplot and assign the returned Figure object to `fig` and the returned Axes object to `ax`.
- Generate a bar plot with:
  - `left` set to `bar_positions`
  - `height` set to `bar_heights`
  - `width` set to `0.5`
- Use `plt.show()` to display the bar plot.

```
import matplotlib.pyplot as plt
from numpy import arange
num_cols = ['RT_user_norm', 'Metacritic_user_nom', 'IMDB_norm', 'Fandango_Ratingvalue', 'Fandango_Stars']

bar_heights = norm_reviews.ix[0, num_cols].values
bar_positions = arange(5) + 0.75
fig, ax = plt.subplots()
ax.bar(bar_positions, bar_heights, 0.5)
plt.show()
```

## 5: Aligning Axis Ticks And Labels

By default, matplotlib sets the x-axis tick labels to the integer values the bars spanned on the x-axis (from `0` to `6`). We only need tick labels on the x-axis where the bars are positioned. We can use [Axes.set_xticks()](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.set_xticks) to change the positions of the ticks to `[1, 2, 3, 4, 5]`:

```
tick_positions = range(1,6)
```

```
ax.set_xticks(tick_positions)
```

Then, we can use [Axes.set_xticklabels()](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.set_xticklabels) to specify the tick labels:

```
num_cols = ['RT_user_norm', 'Metacritic_user_nom', 'IMDB_norm', 'Fandango_Ratingvalue', 'Fandango_Stars']
```

```
ax.set_xticklabels(num_cols)
```

If you look at the [documentation](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.set_xticklabels) for the method, you'll notice that we can specify the orientation for the labels using the `rotation` parameter:



```
ax.set_xticklabels(num_cols, rotation=90)
```

Rotating the labels by 90 degrees keeps them readable. In addition to modifying the x-axis tick positions and labels, let's also set the x-axis label, y-axis label, and the plot title.

### Instructions

- Create a single subplot and assign the returned Figure object to `fig` and the returned Axes object to `ax`.
- Generate a bar plot with:
  - `left` set to `bar_positions`
  - `height` set to `bar_heights`
  - `width` set to `0.5`
- Set the x-axis tick positions to `tick_positions`.
- Set the x-axis tick labels to `num_cols` and rotate by `90`degrees.
- Set the x-axis label to `"Rating Source"`.
- Set the y-axis label to `"Average Rating"`.
- Set the plot title to `"Average User Rating For Avengers: Age of Ultron (2015)"`.
- Use `plt.show()` to display the bar plot.

```
num_cols = ['RT_user_norm', 'Metacritic_user_nom', 'IMDB_norm', 'Fandango_Ratingvalue', 'Fandango_Stars']
bar_heights = norm_reviews.ix[0, num_cols].values
bar_positions = arange(5) + 0.75
tick_positions = range(1,6)
fig, ax = plt.subplots()

ax.bar(bar_positions, bar_heights, 0.5)
ax.set_xticks(tick_positions)
ax.set_xticklabels(num_cols, rotation=90)

ax.set_xlabel('Rating Source')
ax.set_ylabel('Average Rating')
ax.set_title('Average User Rating For Avengers: Age of Ultron (2015)')
plt.show()
```

## 6: Horizontal Bar Plot

We can create a horizontal bar plot in matplotlib in a similar fashion. Instead of using `Axes.bar()`, we use [Axes.barh()](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.barh). This method has 2 required parameters, `bottom` and `width`. We use the `bottom`parameter to specify the y coordinate for the bottom sides for the bars and the `width` parameter to specify the lengths of the bars:

```
bar_widths = norm_reviews.ix[0, num_cols].values
bar_positions = arange(5) + 0.75
ax.barh(bar_positions, bar_widths, 0.5)
```

To recreate the bar plot from the last step as horizontal bar plot, we essentially need to map the properties we set for the y-axis instead of the x-axis. We use `Axes.set_yticks()` to set the y-axis tick positions to `[1, 2, 3, 4, 5]` and `Axes.set_yticklabels()` to set the tick labels to the column names:

```
tick_positions = range(5) + 1
num_cols = ['RT_user_norm', 'Metacritic_user_nom', 'IMDB_norm', 'Fandango_Ratingvalue', 'Fandango_Stars']
ax.set_yticks(tick_positions)
ax.set_yticklabels(num_cols)
```

### Instructions

- Create a single subplot and assign the returned Figure object to `fig` and the returned Axes object to `ax`.
- Generate a bar plot with:
  - `bottom` set to `bar_positions`
  - `width` set to `bar_widths`
  - `height` set to `0.5`
- Set the y-axis tick positions to `tick_positions`.
- Set the y-axis tick labels to `num_cols`.
- Set the y-axis label to `"Rating Source"`.
- Set the x-axis label to `"Average Rating"`.
- Set the plot title to `"Average User Rating For Avengers: Age of Ultron (2015)"`.
- Use `plt.show()` to display the bar plot.

```
import matplotlib.pyplot as plt
from numpy import arange
num_cols = ['RT_user_norm', 'Metacritic_user_nom', 'IMDB_norm', 'Fandango_Ratingvalue', 'Fandango_Stars']

bar_widths = norm_reviews.ix[0, num_cols].values
bar_positions = arange(5) + 0.75
tick_positions = range(1,6)
fig, ax = plt.subplots()
ax.barh(bar_positions, bar_widths, 0.5)

ax.set_yticks(tick_positions)
ax.set_yticklabels(num_cols)
ax.set_ylabel('Rating Source')
ax.set_xlabel('Average Rating')
ax.set_title('Average User Rating For Avengers: Age of Ultron (2015)')
plt.show()
```

## 7: Scatter Plot

From the horizontal bar plot, we can more easily determine that the 2 average scores from Fandango users are higher than those from the other sites. While bar plots help us visualize a few data points to quickly compare them, they aren't good at helping us visualize many data points. Let's look at a plot that can help us visualize many points.

In the previous mission, the line charts we generated always connected points from left to right. This helped us show the trend, up or down, between each point as we scanned visually from left to right. Instead, we can avoid using lines to connect markers and just use the underlying markers. A plot containing just the markers is known as a **scatter plot**.

![Imgur](https://s3.amazonaws.com/dq-content/scatter_plot_intro.png)

A scatter plot helps us determine if 2 columns are weakly or strongly correlated. While calculating the [correlation coefficient](https://en.wikipedia.org/wiki/Correlation_coefficient) will give us a precise number, a scatter plot helps us find outliers, gain a more intuitive sense of how spread out the data is, and compare more easily.

To generate a scatter plot, we use [Axes.scatter()](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.scatter). The `scatter()` method has 2 required parameters, `x` and `y`, which matches the parameters of the `plot()`method. The values for these parameters need to be iterable objects of matching lengths (lists, NumPy arrays, or pandas series).

Let's start by creating a scatter plot that visualizes the relationship between the `Fandango_RatingValue` and `RT_user_norm`columns. We're looking for at least a weak correlation between the columns.

### Instructions

- Create a single subplot and assign the returned Figure object to `fig` and the returned Axes object to `ax`.
- Generate a scatter plot with the `Fandango_Ratingvalue`column on the x-axis and the `RT_user_norm` column on the y-axis.
- Set the x-axis label to `"Fandango"` and the y-axis label to `"Rotten Tomatoes"`.
- Use `plt.show()` to display the resulting plot.

```
fig, ax = plt.subplots()
ax.scatter(norm_reviews['Fandango_Ratingvalue'], norm_reviews['RT_user_norm'])
ax.set_xlabel('Fandango')
ax.set_ylabel('Rotten Tomatoes')
plt.show()
```

## 8: Switching Axes

The scatter plot suggests that there's a weak, positive correlation between the user ratings on Fandango and the user ratings on Rotten Tomatoes. The correlation is weak because for many x values, there are multiple corresponding y values. The correlation is positive because, in general, as x increases, y also increases.

When using scatter plots to understand how 2 variables are correlated, it's usually not important which one is on the x-axis and which one is on the y-axis. This is because the relationship is still captured either way, even if the plots look a little different. If you want to instead understand how an independent variable affects a dependent variables, you want to put the independent one on the x-axis and the dependent one on the y-axis. Doing so helps emphasize the potential cause and effect relation.

In our case, we're not exploring if the ratings from Fandango influence those on Rotten Tomatoes and we're instead looking to understand how much they agree. Let's see what happens when we flip the columns.

### Instructions

- For the subplot associated with `ax1`:Generate a scatter plot with the `Fandango_Ratingvalue` column on the x-axis and the `RT_user_norm` column on the y-axis.Set the x-axis label to `"Fandango"` and the y-axis label to `"Rotten Tomatoes"`.
- For the subplot associated with `ax2`:
  - Generate a scatter plot with the `RT_user_norm`column on the x-axis and the `Fandango_Ratingvalue` column on the y-axis.
  - Set the x-axis label to `"Rotten Tomatoes"` and the y-axis label to `"Fandango"`.
- Use `plt.show()` to display the resulting plot.

```
fig = plt.figure(figsize=(5,10))
ax1 = fig.add_subplot(2,1,1)
ax2 = fig.add_subplot(2,1,2)
ax1.scatter(norm_reviews['Fandango_Ratingvalue'], norm_reviews['RT_user_norm'])
ax1.set_xlabel('Fandango')
ax1.set_ylabel('Rotten Tomatoes')
ax2.scatter(norm_reviews['RT_user_norm'], norm_reviews['Fandango_Ratingvalue'])
ax2.set_xlabel('Rotten Tomatoes')
ax2.set_ylabel('Fandango')
plt.show()
```

## 9: Benchmarking Correlation

The second scatter plot is a mirror reflection of the first second scatter plot. The nature of the correlation is still reflected, however, which is the important thing. Let's now generate scatter plots to see how Fandango ratings correlate with all 3 of the other review sites.

When generating multiple scatter plots for the purpose of comparison, it's important that all plots share the same ranges in the x-axis and y-axis. In the 2 plots we generated in the last step, the ranges for both axes didn't match. We can use [Axes.set_xlim()](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.set_xlim) and [Axes.set_ylim()](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.set_ylim) to set the data limits for both axes:

```
ax.set_xlim(0, 5)
ax.set_ylim(0, 5)
```

By default, matplotlib uses the minimal ranges for the data limits necessary to display all of the data we specify. By manually setting the data limits ranges to specific ranges for all plots, we're ensuring that we can accurately compare. We can even use the methods we just mentioned to zoom in on a part of the plots. For example, the following code will constrained the axes to the `4` to `5`range:

```
ax.set_xlim(4, 5)
ax.set_ylim(4, 5)
```

This makes small changes in the actual values in the data appear larger in the plot. A difference of `0.1` in a plot that ranges from `0` to `5` is hard to visually observe. A difference of `0.1` in a plot that only ranges from `4` to `5` is easily visible since the difference is 1/10th of the range.

### Instructions

- For the Subplot associated with `ax1`:Generate a scatter plot with the `Fandango_Ratingvalue` column on the x-axis and the `RT_user_norm` column on the y-axis.Set the x-axis label to `"Fandango"` and the y-axis label to `"Rotten Tomatoes"`.Set the x-axis and y-axis data limits to range from `0`and `5`.
- For the Subplot associated with `ax2`:Generate a scatter plot with the `Fandango_Ratingvalue` column on the x-axis and the `Metacritic_user_nom` column on the y-axis.Set the x-axis label to `"Fandango"` and the y-axis label to `"Metacritic"`.Set the x-axis and y-axis data limits to range from `0`and `5`.
- For the Subplot associated with `ax3`:Generate a scatter plot with the `Fandango_Ratingvalue` column on the x-axis and the `IMDB_norm` column on the y-axis.Set the x-axis label to `"Fandango"` and the y-axis label to `"IMDB"`.Set the x-axis and y-axis data limits to range from `0`and `5`.
- Use `plt.show()` to display the plots.

```
import matplotlib.pyplot as plt

fig = plt.figure(figsize=(5,10))
ax1 = fig.add_subplot(3,1,1)
ax2 = fig.add_subplot(3,1,2)
ax3 = fig.add_subplot(3,1,3)
ax1.scatter(norm_reviews['Fandango_Ratingvalue'], norm_reviews['RT_user_norm'])
ax1.set_xlabel('Fandango')
ax1.set_ylabel('Rotten Tomatoes')
ax1.set_xlim(0, 5)
ax1.set_ylim(0, 5)

ax2.scatter(norm_reviews['Fandango_Ratingvalue'], norm_reviews['Metacritic_user_nom'])
ax2.set_xlabel('Fandango')
ax2.set_ylabel('Metacritic')
ax2.set_xlim(0, 5)
ax2.set_ylim(0, 5)

ax3.scatter(norm_reviews['Fandango_Ratingvalue'], norm_reviews['IMDB_norm'])
ax3.set_xlabel('Fandango')
ax3.set_ylabel('IMDB')
ax3.set_xlim(0, 5)
ax3.set_ylim(0, 5)

plt.show()
```

## 10: Next Steps

From the scatter plots, we can conclude that user ratings from IMDB and Fandango are the most similar. In addition, user ratings from Metacritic and Rotten Tomatoes have positive but weak correlations with user ratings from Fandango. We can also notice that user ratings from Metacritic and Rotten Tomatoes span a larger range of values than those from IMDB or Fandango. User ratings from Metacritic and Rotten Tomatoes range from 1 to 5. User ratings from Fandango range approximately from 2.5 to 5 while those from IMDB range approximately from 2 to 4.5.

The scatter plots unfortunately only give us a cursory understanding of the distributions of user ratings from each review site. For example, if a hundred movies had the same average user rating from IMDB and Fandango in the dataset, we would only see a single marker in the scatter plot. In the next mission, we'll learn about two types of plots that help us understand distributions of values.

# Histograms And Box Plots

## 1: Introduction

In the last mission, we learned how to create bar plots to compare the average user rating a movie received from four movie review sites. We also learned how to create scatter plots to explore how ratings on one site compare with ratings on another site. We ended the mission with the observations that user ratings from Metacritic and Rotten Tomatoes spanned a larger range (1.0 to 5.0) while those from Fandango and IMDB spanned a smaller range (2.5 to 5 and 2 to 5 respectively).

In this mission, we'll learn how to visualize the distributions of user ratings using **histograms** and **box plots**. We'll continue working with the same dataset from the last mission. Recall that you can download the dataset `fandango_scores.csv` from the [FiveThirtEight Github repo](https://github.com/fivethirtyeight/data/tree/master/fandango). We've read the dataset into pandas, selected the columns we're going to work with, and assigned the new Dataframe to `norm_reviews`.

## 2: Frequency Distribution

Let's first compare the **frequency distributions** of user ratings from Fandango with those from IMDB using tables. A column's [frequency distribution](https://en.wikipedia.org/wiki/Frequency_distribution) consists of the unique values in that column along with the count for each of those values (or their frequency). We can use [Series.value_counts()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.value_counts.html) to return the frequency distribution as Series object:

```
freq_counts = df['Fandango_Ratingvalue'].value_counts()
```

The resulting Series object will be sorted by frequency in descending order:

![Fandango Frequency Distribution](https://s3.amazonaws.com/dq-content/fandango_frequency_distribution.png)

While this ordering is helpful when we're looking to quickly find the most common values in a given column, it's not helpful when trying to understand the range that the values in the column span. We can use [Series.sort_index()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.sort_index.html) to sort the frequency distribution in ascending order by the values in the column (which make up the index for the Series object):

```
freq_counts = df['Fandango_Ratingvalue'].value_counts()
sorted_freq_counts = freq_counts.sort_index()
```

Here's what both frequency distributions look like side-by-side:

![Both Fandango Frequency Distributions](https://s3.amazonaws.com/dq-content/both_fandango_distributions.png)

### Instructions

- Use the `value_counts()` method to return the frequency counts for the `Fandango_Ratingvalue` column. Sort the resulting Series object by the index and assign to `fandango_distribution`.
- Use the `value_counts()` method to return frequency counts the `IMDB_norm` column. Sort the resulting Series object by the index and assign to `imdb_distribution`.
- Use the `print()` function to display `fandango_distribution` and `imdb_distribution`.

```
fandango_distribution = norm_reviews['Fandango_Ratingvalue'].value_counts()
fandango_distribution = fandango_distribution.sort_index()

imdb_distribution = norm_reviews['IMDB_norm'].value_counts()
imdb_distribution = imdb_distribution.sort_index()

print(fandango_distribution)
print(imdb_distribution)
```

## 3: Binning

Because there are only a few unique values, we can quickly scan the frequency counts and confirm that the `Fandango_Ratingvalue` column ranges from 2.7 to 4.8 while the `IMDB_norm` column ranges from 2 to 4.3. While we can quickly determine the minimum and maximum values, we struggle to answer the following questions about a column:

- What percent of the ratings are contained in the 2.0 to 4.0 range?
  - How does this compare with other sites?
- Which values represent the top 25% of the ratings? The bottom 25%?
  - How does this compare with other sites?

Comparing frequency distributions is also challenging because the `Fandango_Ratingvalue` column contains 21 unique values while `IMDB_norm` contains 41 unique values. We need a way to compare frequencies across a shared set of values. Because all ratings have been normalized to a range of 0 to 5, we can start by dividing the range of possible values into a series of fixed length intervals, called **bins**. We can then sum the frequencies for the values that fall into each bin. Here's a diagram that makes binning easier to understand:

![Binning Introduction](https://s3.amazonaws.com/dq-content/histogram_binning.png)

The distributions for both of these columns are now easier to compare because of the shared x-axis (the bins). We can now plot the bins along with the frequency sums as a bar plot. This type of plot is called a [histogram](https://en.wikipedia.org/wiki/Histogram). Let's dive right into creating a histogram in matplotlib.

## 4: Histogram In Matplotlib

We can generate a histogram using [Axes.hist()](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.hist). This method has only 1 required parameter, an iterable object containing the values we want a histogram for. By default, matplotlib will:

- calculate the minimum and maximum value from the sequence of values we passed in
- create 10 bins of equal length that span the range from the minimum to the maximum value
- group unique values into the bins
- sum up the associated unique values
- generate a bar for the frequency sum for each bin

The default behavior of `Axes.hist()` is problematic for the use case of comparing distributions for multiple columns using the same binning strategy. This is because the binning strategy for each column would depend on the minimum and maximum values, instead of a shared binning strategy. We can use the `range`parameter to specify the range we want matplotlib to use as a tuple:

```

```

```
ax.hist(norm_reviews['Fandango_Ratingvalue'], range=(0, 5))
```

While histograms use bars whose lengths are scaled to the values they're representing, they differ from bar plots in a few ways. Histograms help us visualize continuous values using bins while bar plots help us visualize discrete values. The locations of the bars on the x-axis matter in a histogram but they don't in a simple bar plot. Lastly, bar plots also have gaps between the bars, to emphasize that the values are discrete.

### Instructions

- Create a single subplot and assign the returned Figure object to `fig` and the returned Axes object to `ax`.
- Generate a histogram from the values in the `Fandango_Ratingvalue` column using a range of `0` to `5`.
- Use `plt.show()` to display the plot.

```

fig, ax = plt.subplots()
ax.hist(norm_reviews['Fandango_Ratingvalue'], range=(0, 5))
plt.show()
```

## 5: Comparing Histograms

If you recall, one of the questions we were looking to answer was:

- What percent of the ratings are contained in the 2.0 to 4.0 range?

We can visually examine the proportional area that the bars in the 2.0 to 4.0 range take up and determine that more than 50% of the movies on Fandango fall in this range. We can increase the number of bins from 10 to 20 for improved resolution of the distribution. The length of each bin will be 0.25 (5 / 20) instead of 0.5 (5 / 10). The `bins` parameter for `Axes.hist()` is the 2nd positional parameter, but can also be specified as a named parameter:

```

```

```
# Either of these will work.
```

```
ax.hist(norm_reviews['Fandango_Ratingvalue'], 20, range=(0, 5))
```

```
ax.hist(norm_reviews['Fandango_Ratingvalue'], bins=20, range=(0, 5))
```

Let's now generate histograms using 20 bins for all four columns. To ensure that the scales for the y-axis are the same for all histograms, let's set them manually using `Axes.set_ylim()`.

### Instructions

- For the subplot associated with `ax1`:Generate a histogram of the values in the `Fandango_Ratingvalue` column using 20 bins and a range of `0` to `5`.Set the title to `Distribution of Fandango Ratings`.
- For the subplot associated with `ax2`:Generate a histogram of the values in the `RT_user_norm` column using 20 bins and a range of `0` to `5`.Set the title to `Distribution of Rotten Tomatoes Ratings`.
- For the subplot associated with `ax3`:Generate a histogram of the values in the `Metacritic_user_nom` column using 20 bins and a range of `0` to `5`.Set the title to `Distribution of Metacritic Ratings`.
- For the subplot associated with `ax4`:Generate a histogram of the values in the `IMDB_norm`column using 20 bins and a range of `0` to `5`.Set the title to `Distribution of IMDB Ratings`.
- For all subplots:
  - Set the y-axis range to `0` to `50` using `Axes.set_ylim()`.
  - Set the y-axis label to `Frequency` using `Axes.set_ylabel()`.
- Use `plt.show()` to display the plots.

```
fig = plt.figure(figsize=(5,20))
ax1 = fig.add_subplot(4,1,1)
ax2 = fig.add_subplot(4,1,2)
ax3 = fig.add_subplot(4,1,3)
ax4 = fig.add_subplot(4,1,4)
ax1.hist(norm_reviews['Fandango_Ratingvalue'], bins=20, range=(0, 5))
ax1.set_title('Distribution of Fandango Ratings')
ax1.set_ylim(0, 50)

ax2.hist(norm_reviews['RT_user_norm'], 20, range=(0, 5))
ax2.set_title('Distribution of Rotten Tomatoes Ratings')
ax2.set_ylim(0, 50)

ax3.hist(norm_reviews['Metacritic_user_nom'], 20, range=(0, 5))
ax3.set_title('Distribution of Metacritic Ratings')
ax3.set_ylim(0, 50)

ax4.hist(norm_reviews['IMDB_norm'], 20, range=(0, 5))
ax4.set_title('Distribution of IMDB Ratings')
ax4.set_ylim(0, 50)

plt.show()
```

## 6: Quartiles

From the histograms, we can make the following observations:

- Around 50% of user ratings from Fandango fall in the 2 to 4 score range
- Around 50% of user ratings from Rotten Tomatoes fall in the 2 to 4 score range
- Around 75% of the user ratings from Metacritic fall in the 2 to 4 score range
- Around 90% of the user ratings from IMDB fall in the 2 to 4 score range

While histograms allow us to visually estimate the percentage of ratings that fall into a range of bins, they don't allow us to easily understand how the top 25% or the bottom 25% of the ratings differ across the sites. The bottom 25% of values and top 25% of values both represent [quartiles](https://en.wikipedia.org/wiki/Quartile). The four quartiles divide the range of values into four regions where each region contains 1/4th of the total values.

While these regions may sound similar to bins, they differ in how values are grouped into each region. Each bin covers an equal proportion of the values in the range. On the other hand, each quantile covers an equal number of values (1/4th of the total values). To visualize quartiles, we need to use a **box plot**, also referred to as a [box-and-whisker plot](https://en.wikipedia.org/wiki/Box_plot).

## 7: Box Plot

A box plot consists of box-and-whisker diagrams, which represents the different quartiles in a visual way. Here's a box plot of the values in the `RT_user_norm` column:

![Boxplot](https://s3.amazonaws.com/dq-content/boxplot_intro.png)

The two regions contained within the box in the middle make up the **interquartile range**, or IQR. The [IQR](https://en.wikipedia.org/wiki/Interquartile_range) is used to measure dispersion of the values. The ratio of the length of the box to the whiskers around the box helps us understand how values in the distribution are spread out.

We can generate a boxplot using [Axes.boxplot()](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.boxplot).

```

```

```
ax.boxplot(norm_reviews['RT_user_norm'])
```

Matplotlib will sort the values, calculate the quartiles that divide the values into four equal regions, and generate the box and whisker diagram.

### Instructions

- Create a single subplot and assign the returned Figure object to `fig` and the returned Axes object to `ax`.
- Generate a box plot from the values in the `RT_user_norm`column.Set the y-axis limit to range from `0` to `5`.Set the x-axis tick label to `Rotten Tomatoes`.
- Use `plt.show()` to display the plot.

```

fig, ax = plt.subplots()
ax.boxplot(norm_reviews['RT_user_norm'])
ax.set_xticklabels(['Rotten Tomatoes'])
ax.set_ylim(0, 5)
plt.show()
```

## 8: Multiple Box Plots

From the box plot we generated using Rotten Tomatoes ratings, we can conclude that:

- the bottom 25% of user ratings range from around 1 to 2.5
- the top 25% of of user ratings range from around 4 to 4.6

To compare the lower and upper ranges with those for the other columns, we need to generate multiple box-and-whisker diagrams in the same box plot. When selecting multiple columns to pass in to `Axes.boxplot()`, we need to use the `values` accessor to return a multi-dimensional numpy array:

```

```

```
num_cols = ['RT_user_norm', 'Metacritic_user_nom', 'IMDB_norm', 'Fandango_Ratingvalue', 'Fandango_Stars']
```

```
ax.boxplot(norm_reviews[num_cols].values)
```

### Instructions

- Create a single subplot and assign the returned Figure object to `fig` and the returned Axes object to `ax`.
- Generate a box plot containing a box-and-whisker diagram for each column in `num_cols`.
- Set the x-axis tick labels to the column names in `num_cols` and rotate the ticks by `90` degrees.
- Set the y-axis limit to range from `0` to `5`.
- Use `plt.show()` to display the plot.

```
num_cols = ['RT_user_norm', 'Metacritic_user_nom', 'IMDB_norm', 'Fandango_Ratingvalue']
fig, ax = plt.subplots()
ax.boxplot(norm_reviews[num_cols].values)
ax.set_xticklabels(num_cols, rotation=90)
ax.set_ylim(0,5)
plt.show()
```

## 9: Next Steps

From the boxplot, we can reach the following conclusions:

- user ratings from Rotten Tomatoes and Metacritic span a larger range of values
- user ratings from IMDB and Fandango are both skewed in the positive direction and span a more constrained range of values

In this mission, we learned how to use histograms and box plots to visualize and compare the distributions of the ratings from the four movie review sites. Next in this course is a guided project, where we'll explore how to use pandas with matplotlib effectively and create more complex plots.

# Guided Project: Visualizing Earnings Based On College Majors

## 1: Introduction

In this course, we've been creating plots using pyplot and matplotlib directly. When we want to explore a new dataset by quickly creating visualizations, using these tools directly can be cumbersome. Thankfully, pandas has many methods for quickly generating common plots from data in DataFrames. Like pyplot, the plotting functionality in pandas is a wrapper for matplotlib. This means we can customize the plots when necessary by accessing the underlying Figure, Axes, and other matplotlib objects.

In this guided project, we'll explore how using the pandas plotting functionality along with the Jupyter notebook interface allows us to explore data quickly using visualizations. If you're new to either our guided projects or Jupyter notebook in general, you can learn more [here](https://www.dataquest.io/mission/162/guided-project-using-jupyter-notebook). You can find the solutions to this guided project [here](https://github.com/dataquestio/solutions/blob/master/Mission146Solutions.ipynb).

We'll be working with a dataset on the job outcomes of students who graduated from college between 2010 and 2012. The original data on job outcomes was released by [American Community Survey](https://www.census.gov/programs-surveys/acs/), which conducts surveys and aggregates the data. FiveThirtyEight cleaned the dataset and released it on their [Github repo](https://github.com/fivethirtyeight/data/tree/master/college-majors).

Each row in the dataset represents a different major in college and contains information on gender diversity, employment rates, median salaries, and more. Here are some of the columns in the dataset:

- `Rank` - Rank by median earnings (the dataset is ordered by this column).
- `Major_code` - Major code.
- `Major` - Major description.
- `Major_category` - Category of major.
- `Total` - Total number of people with major.
- `Sample_size` - Sample size (unweighted) of full-time.
- `Men` - Male graduates.
- `Women` - Female graduates.
- `ShareWomen` - Women as share of total.
- `Employed` - Number employed.
- `Median` - Median salary of full-time, year-round workers.
- `Low_wage_jobs` - Number in low-wage service jobs.
- `Full_time` - Number employed 35 hours or more.
- `Part_time` - Number employed less than 36 hours.

Using visualizations, we can start to explore questions from the dataset like:

- Do students in more popular majors make more money?
  - Using scatter plots
- How many majors are predominantly male? Predominantly female?
  - Using histograms
- Which category of majors have the most students?
  - Using bar plots

We'll explore how to do these and more while primarily working in pandas. Before we start creating data visualizations, let's import the libraries we need and remove rows containing null values.

### Instructions

- Let's setup the environment by importing the libraries we need and running the necessary Jupyter magic so that plots are displayed inline.
  - Import pandas and matplotlib into the environment.
  - Run the Jupyter magic `%matplotlib inline` so that plots are displayed inline.
- Read the dataset into a DataFrame and start exploring the data.
  - Read `recent-grads.csv` into pandas and assign the resulting DataFrame to `recent_grads`.
  - Use `DataFrame.iloc[]` to return the first row formatted as a table.
  - Use `DataFrame.head()` and `DataFrame.tail()` to become familiar with how the data is structured.
  - Use [DataFrame.describe()](https://www.dataquest.io/m/146/(http://pandas.pydata.org/pandas-docs/stable/basics.html#summarizing-data-describe)) to generate summary statistics for all of the numeric columns.
- Drop rows with missing values. Matplotlib expects that columns of values we pass in have matching lengths and missing values will cause matplotlib to throw errors.
  - Look up the number of rows in `recent_grads` and assign the value to `raw_data_count`.
  - Use `DataFrame.dropna()` to drop rows containing missing values and assign the resulting DataFrame back to `recent_grads`.
  - Look up the number of rows in `recent_grads` now and assign the value to `cleaned_data_count`. If you compare `cleaned_data_count` and `raw_data_count`, you'll notice that only one row contained missing values and was dropped.

## 2: Pandas, Scatter Plots

Most of the plotting functionality in pandas is contained within the [DataFrame.plot()](http://pandas.pydata.org/pandas-docs/version/0.19.0/generated/pandas.DataFrame.plot.html) method. When we call this method, we specify the data we want plotted as well as the type of plot. We use the `kind` parameter to specify the type of plot we want. We use `x` and `y` to specify the data we want on each axis. You can read about the different parameters in the [documentation](http://pandas.pydata.org/pandas-docs/version/0.19.0/generated/pandas.DataFrame.plot.html).

```

```

```
recent_grads.plot(x='Sample_size', y='Employed', kind='scatter')
```

If you create a new cell in jupyter notebook and run the above code, the scatter plot will be displayed immediately. This functionality is a byproduct of running the jupyter magic `%matplotlib inline`. This means we can write one line of code to generate a scatter plot, run the cell using a keyboard shortcut, inspect the plot, and repeat. The `DataFrame.plot()`method has a few parameters we can use for tweaking the scatter plot:

```

```

```
recent_grads.plot(x='Sample_size', y='Employed', kind='scatter', title='Employed vs. Sample_size', figsize=(5,10))
```

We can access the underlying matplotlib Axes object by assigning the return value to a variable:

```

```

```
ax = recent_grads.plot(x='Sample_size', y='Employed', kind='scatter')
```

```
ax.set_title('Employed vs. Sample_size')
```

When you run the code above in a jupyter notebook cell, the plot will be returned inline just like before.

### Instructions

- Generate scatter plots in separate jupyter notebook cells to explore the following relations:
  - `Sample_size` and `Median`
  - `Sample_size` and `Unemployment_rate`
  - `Full_time` and `Median`
  - `ShareWomen` and `Unemployment_rate`
  - `Men` and `Median`
  - `Women` and `Median`
- Use the plots to explore the following questions:
  - Do students in more popular majors make more money?
  - Do students that majored in subjects that were majority female make more money?
  - Is there any link between the number of full-time employees and median salary?

## 3: Pandas, Histograms

To explore the distribution of values in a column, we can select it from the DataFrame, call [Series.plot()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.plot.html), and set the `kind` parameter to `hist`:

```

```

```
recent_grads['Sample_size'].plot(kind='hist')
```

The `DataFrame.plot()` and `Series.plot()`methods have many of the same parameters but are used for different use cases. We use `Series.plot()` to plot a specific column and `DataFrame.plot()` to generate plots that use values from multiple columns. For example, because scatter plots are generated using 2 sets of values (one for each axis), we can't create a scatter plot using `Series.plot()`.

Unfortunately, `Series.plot()` doesn't contain parameters for tweaking a histogram because it was implemented to handle generating standard plots with default settings quickly. If we want to control the binning strategy of a histogram, we should use [Series.hist()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.hist.html) instead, which contains parameters specific to customizing histograms:

```

```

```
recent_grads['Sample_size'].hist(bins=25, range=(0,5000))
```

### Instructions

- Generate histograms in separate jupyter notebook cells to explore the distributions of the following columns:
  - `Sample_size`
  - `Median`
  - `Employed`
  - `Full_time`
  - `ShareWomen`
  - `Unemployment_rate`
  - `Men`
  - `Women`
- We encourage you to experiment with different bin sizes and ranges when generating these histograms.
- Use the plots to explore the following questions:
  - What percent of majors are predominantly male? Predominantly female?
  - What's the most common median salary range?

## 4: Pandas, Scatter Matrix Plot

In the last 2 steps, we created individual scatter plots to visualize potential relationships between columns and histograms to visualize the distributions of individual columns. A **scatter matrix plot** combines both scatter plots and histograms into one grid of plots and allows us to explore potential relationships and distributions simultaneously. A scatter matrix plot consists of `n` by `n` plots on a grid, where `n` is the number of columns, the plots on the diagonal are histograms, and the non-diagonal plots are scatter plots.

![Scatter Matrix Plot](https://s3.amazonaws.com/dq-content/scatterplot_matrix_intro.png)

Because scatter matrix plots are frequently used in the exploratory data analysis, pandas contains a function named `scatter_matrix()` that generates the plots for us. This function is part of the `pandas.tools.plotting` module and needs to be imported separately. To generate a scatter matrix plot for 2 columns, select just those 2 columns and pass the resulting DataFrame into the `scatter_matrix()` function.

```

```

```
scatter_matrix(recent_grads[['Women', 'Men']], figsize=(10,10))
```

While passing in a DataFrame with 2 columns returns a 2 by 2 scatter matrix plot (4 plots total), passing in one with 3 returns a 3 by 3 scatter matrix plot (9 plots total). This means that the number of plots generated scales exponentially by a factor of 2, not linearly. If you increase the number of columns to 4 or more, the resulting grid of plots becomes unreadable and difficult to interpret (even if you increase the plotting area using the `figsize` parameter).

Unfortunately, the documentation for `scatter_matrix()` is missing from the pandas website. If you want to read more about the parameters the function accepts, read the comments in the [source code for the function](https://github.com/pandas-dev/pandas/blob/master/pandas/tools/plotting.py).

### Instructions

- Import `scatter_matrix` from `pandas.tools.plotting`
- Create a 2 by 2 scatter matrix plot using the `Sample_size`and `Median` columns.
- Create a 3 by 3 scatter matrix plot using the `Sample_size`, `Median`, and `Unemployment_rate` columns.
- Explore the questions from the last few steps using these scatter matrix plots. You may need to create more scatter matrix plots.

## 5: Pandas, Bar Plots

To create bar plots in matplotlib, we had to specify many aspects of the bar plot ourselves. We had to specify the locations, labels, lengths, and widths of the bars. When creating bar plots using pandas, we only need specify the data we want the bars to represent and the labels for each bar. The following code returns a bar plot of the first 5 values in the `Women` column:

```

```

```
recent_grads[:5]['Women'].plot(kind='bar')
```

By default, pandas will use the default labels on the x-axis for each bar (`1` to `n`) from matplotlib. If we instead use the [DataFrame.plot.bar()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.plot.bar.html) method, we can use the `x` parameter to specify the labels and the `y` parameter to specify the data for the bars:

```

```

```
recent_grads[:5].plot.bar(x='Major', y='Women')
```

### Instructions

- Use bar plots to compare the percentages of women (`ShareWomen`) from the 10 highest paying majors and from the 10 lowest paying majors.
- Use bar plots to compare the unemployment rate (`Unemployment_rate`) from the 10 highest paying majors and from the 10 lowest paying majors.

## 6: Next Steps

In this guided project, we learned how to use the plotting tools built into pandas to explore data on job outcomes. If you head over to the documentation on [plotting in pandas](http://pandas.pydata.org/pandas-docs/version/0.18.1/visualization.html), you'll notice that there's built in support for many more plots.

We encourage you to keep exploring these other visualizations on your own. Here are some ideas:

- Use a grouped bar plot to compare the number of men with the number of women in each category of majors.
- Use a box plot to explore the distributions of median salaries and unemployment rate.
- Use a hexagonal bin plot to visualize the columns that had dense scatter plots from earlier in the project.

