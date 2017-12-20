# Introduction to Statistics

## 1: Introduction To Scales

At its core, statistics is about counting and measuring.

In order to do both effectively, we have to define scales on which to base our counts. A scale represents the possible values that a variable can have.

### Instructions

- Compute the mean of `car_speeds`, and assign the result to `mean_car_speed`.
- Compute the mean of `earthquake_intensities`, and assign the result to `mean_earthquake_intensities`. Note that this value will not be meaningful, because we shouldn't average values on a logarithmic scale this way.

```
car_speeds = [10,20,30,50,20]
earthquake_intensities = [2,7,4,5,8]
mean_car_speed = sum(car_speeds) / len(car_speeds)
mean_earthquake_intensities = sum(earthquake_intensities) / len(earthquake_intensities)
```

## 2: Discrete And Continuous Scales

Scales can be either *discrete* or *continuous*.

Think of someone marking down the number of inches a snail crawls every day. The snail could crawl 1 inch, 2 inches, 1.5 inches, 1.51 inches, or any other number, and it would be a valid observation. This is because inches are on a continuous scale, and even fractions of an inch are possible.

Now think of someone counting the number of cars in a parking lot each day. 1 car, 2 cars, and 10 cars are valid measurements, but 1.5 cars isn't valid.

Half of a car isn't a meaningful quantity, because cars are discrete. You can't have 52% of a car - you either have a car, or you don't.

You can still average items on discrete scales, though. You could say "1.75 cars use this parking lot each day, on average." Any daily value for number of cars, however, would need to be a whole number.

### Instructions

- Make a line plot with `day_numbers` on the x axis and `snail_crawl_length` on the y axis.
- Make a line plot with `day_numbers` on the x axis and `cars_in_parking_lot` on the y axis.

```
day_numbers = [1,2,3,4,5,6,7]
snail_crawl_length = [.5,2,5,10,1,.25,4]
cars_in_parking_lot = [5,6,4,2,1,7,8]

import matplotlib.pyplot as plt
plt.plot(day_numbers, snail_crawl_length)
plt.show()

plt.plot(day_numbers, cars_in_parking_lot)
plt.show()
```

## 3: Understanding Scale Starting Points

Some scales use the zero value in different ways. Think of the number of cars in a parking lot.

Zero cars in the lot means that there are absolutely no cars at all, so absolute zero is at 0 cars. You can't have negative cars.

Now, think of degrees Fahrenheit.

Zero degrees doesn't mean that there isn't any warmth; the degree scale can also be negative, and absolute zero (when there is no warmth at all) is at -459.67 degrees.

Scales with absolute zero points that aren't at 0 don't enable us to take meaningful ratios. For example, if four cars parked in the lot yesterday and eight park today, I can safely say that twice as many cars are in the lot today.

However, if it was 32 degrees Fahrenheit yesterday, and it's 64 degrees today, I can't say that it's twice as warm today as yesterday.

### Instructions

- Convert the values in `fahrenheit_degrees` so that absolute zero is at the value 0. If you think this is already the case, don't change anything. Assign the result to `degrees_zero`.
- Convert the values in `yearly_town_population` so that absolute zero is at the value 0. If you think this is already the case, don't change anything. Assign the result to `population_zero`.

```
fahrenheit_degrees = [32, 64, 78, 102]
yearly_town_population = [100,102,103,110,105,120]
population_zero = yearly_town_population
degrees_zero = [f + 459.67 for f in fahrenheit_degrees]
```

## 4: Working With Ordinal Scales

So far, we've looked at equal interval and discrete scales, where all of the values are numbers. We can also have *ordinal* scales, where items are ordered by rank.

For example, we could ask people how many cigarettes they smoke per day, and the answers could be "none," "a few," "some," or "a lot." These answers don't map exactly to numbers of cigarettes, but we know that "a few" is more than "none."

This is an ordinal rating scale. We can assign numbers to the answers in a logical order to make them easier to work with.

For example, we could map 0 to "none," 1 to "a few," 2 to "some," and so on.

### Instructions

- In the following code block, assign a number to each survey response that corresponds with its position on the scale (`"none"` is `0`, and so on).
- Compute the average value of all the survey responses, and assign it to `average_smoking`.

```
# Results from our survey on how many cigarettes people smoke per day
survey_responses = ["none", "some", "a lot", "none", "a few", "none", "none"]
survey_scale = ["none", "a few", "some", "a lot"]
survey_numbers = [survey_scale.index(response) for response in survey_responses]
average_smoking = sum(survey_numbers) / len(survey_numbers)
```

## 5: Grouping Values With Categorical Scales

We can also have categorical scales, which group values into general categories.

One example is *gender*, which can be *male* or *female*.

Unlike ordinal scales, categorical scales don't have an order. In our gender example, for instance, one category isn't greater than or less than the other.

Categories are common in data science. You'll typically use them to split data into groups.

### Instructions

- Compute the average savings for everyone who is `"male"`. Assign the result to `male_savings`.
- Compute the average savings for everyone who is `"female"`. Assign the result to `female_savings`.

```
# Let's say that these lists are both columns in a matrix.  Index 0 is the first row in both, and so on.
gender = ["male", "female", "female", "male", "male", "female"]
savings = [1200, 5000, 3400, 2400, 2800, 4100]
male_savings_list = [savings[i] for i in range(0, len(gender)) if gender[i] == "male"]
female_savings_list = [savings[i] for i in range(0, len(gender)) if gender[i] == "female"]

male_savings = sum(male_savings_list) / len(male_savings_list)
female_savings = sum(female_savings_list) / len(female_savings_list)
```

## 6: Visualizing Counts With Frequency Histograms

Remember how statistics is all about counting? A **frequency histogram** is a type of plot that helps us visualize counts of data.

These plots tally how many times each value occurs in a list, then graph the values on the x-axis and the counts on the y-axis.

**Frequency histograms** give us a better understanding of where values fall within a data set.

### Instructions

- Plot a histogram of `student_scores`.

```
# Let's say that we watch cars drive by and calculate average speed in miles per hour
average_speed = [10, 20, 25, 27, 28, 22, 15, 18, 17]
import matplotlib.pyplot as plt
plt.hist(average_speed)
plt.show()

# Let's say we measure student test scores from 0-100
student_scores = [15, 80, 95, 100, 45, 75, 65]
plt.hist(student_scores)
plt.show()
```

## 7: Aggregating Values With Histogram Bins

You may have noticed that the code on the last screen plotted all of the values.

In contrast, histograms use *bins* to count values. *Bins* aggregate values into predefined "buckets."

Here's how they work. If the x-axis ranges from 0 to 10 and we have 10 bins, the first bin would be for values between 0-1, the second would be for values between 1-2, and so on.

If we have five bins, the first bin would be for values between 0-2, the second would be for values between 2-4, and so on.

Each value in the list that falls within the bin would increase the bin's count by one. The result looks like a bar chart. Bins give us a better understanding of the shape and distribution of the data than graphing each count individually.

Now that you know about bins, we'd like to point something out about what you saw on the previous screen. *matplotlib*'s default number of *bins* for a plot is 10. We had fewer values than that, so *matplotlib* displayed all of the values.

Let's experiment a bit with using different numbers of bins to gain a better understanding of how they work.

### Instructions

- Plot a histogram of `average_speed` with only `2` bins.

```
average_speed = [10, 20, 25, 27, 28, 22, 15, 18, 17]
import matplotlib.pyplot as plt
plt.hist(average_speed, bins=6)
plt.show()

# As you can see, matplotlib groups the values in the list into the nearest bins.
# If we have fewer bins, each bin will have a higher count (because there will be fewer bins to group all of the values into).
# If there are more bins, the total for each one will decrease, because each one will contain fewer values.
plt.hist(average_speed, bins=4)
plt.show()
plt.hist(average_speed, bins=2)
plt.show()
```

## 8: Measuring Data Skew

Now that you know how to make histograms, did you notice how the plots have "shapes?"

These shapes are important because they can show us the distributional characteristics of the data. The first characteristic we'll look at is `skew`.

Skew refers to asymmetry in the data. When data is concentrated on the right side of the histogram, for example, we say it has a negative skew. When the data is concentrated on the left, we say it has a positive skew.

We can measure the level of skew with the `skew` function. A positive value indicates a positive skew, a negative value indicates a negative skew, and a value close to zero indicates no skew.

### Instructions

- Assign the skew of `test_scores_positive` to `positive_skew`.
- Assign the skew of `test_scores_negative` to `negative_skew`.
- Assign the skew of `test_scores_normal` to `no_skew`.

```
# We've already loaded in some numpy arrays. We'll make some plots with them.
# The arrays contain student test scores that are on a 0-100 scale.
import matplotlib.pyplot as plt

# See how there's a long slope to the left?
# The data is concentrated in the right part of the distribution, but some people also scored poorly.
# This plot has a negative skew.
plt.hist(test_scores_negative)
plt.show()

# This plot has a long slope to the right.
# Most students did poorly, but a few did really well.
# This plot has a positive skew.
plt.hist(test_scores_positive)
plt.show()

# This plot has no skew either way. Most of the values are in the center, and there is no long slope either way.
# It is an unskewed distribution.
plt.hist(test_scores_normal)
plt.show()

# We can test how skewed a distribution is using the skew function.
# A positive value means positive skew, a negative value means negative skew, and close to zero means no skew.
from scipy.stats import skew
positive_skew = skew(test_scores_positive)
negative_skew = skew(test_scores_negative)
no_skew = skew(test_scores_normal)
```

## 9: Checking For Outliers With Kurtosis

*Kurtosis* is another characteristic of distributions. Kurtosis measures whether the distribution is short and flat, or tall and skinny. In other words, it assesses the shape of the peak.

"Shorter" distributions have a lower maximum frequency, but higher subsequent frequencies. A high kurtosis may indicate problems with outliers (very large or very small values that skew the data).

### Instructions

- Assign the kurtosis of `test_scores_platy` to `kurt_platy`.
- Assign the kurtosis of `test_scores_lepto` to `kurt_lepto`.
- Assign the kurtosis of `test_scores_meso` to `kurt_meso`.

```
import matplotlib.pyplot as plt

# This plot is short. It is platykurtic.
# Notice how the values are distributed fairly evenly, and there isn't a large cluster in the middle.
# Student performance varied widely.
plt.hist(test_scores_platy)
plt.show()

# This plot is tall. It is leptokurtic.
# Most students performed similarly.
plt.hist(test_scores_lepto)
plt.show()

# The height of this plot neither short nor tall. It is mesokurtic.
plt.hist(test_scores_meso)
plt.show()

# We can measure kurtosis with the kurtosis function.
# Negative values indicate platykurtic distributions, positive values indicate leptokurtic distributions, and values near 0 are mesokurtic.
from scipy.stats import kurtosis
kurt_platy = kurtosis(test_scores_platy)
kurt_lepto = kurtosis(test_scores_lepto)
kurt_meso = kurtosis(test_scores_meso)
```

## 10: Modality

*Modality* is another characteristic of distributions. Modality refers to the number of modes, or peaks, in a distribution.

Real-world data is often unimodal (it has only one mode).

### Instructions

- Plot `test_scores_multi`, which has four peaks.

```
import matplotlib.pyplot as plt

# This plot has one mode. It is unimodal.
plt.hist(test_scores_uni)
plt.show()

# This plot has two peaks. It is bimodal.
# This could happen if one group of students learned the material and another learned something else, for example.
plt.hist(test_scores_bi)
plt.show()

# More than one peak means that the plot is multimodal.
# We can't easily measure the modality of a plot, like we can with kurtosis or skew.
# Often, the best way to detect multimodality is to examine the plot visually.
plt.hist(test_scores_multi)
plt.show()
```

## 11: Measures Of Central Tendency

Now that we know how to measure the characteristics of a distribution, let's look at central tendency measures.

Central tendency measures assess how likely the data points are to cluster around a central value.

The first one we'll look at is the **mean**. We've calculated mean before, but let's explore it further.

The mean is just the sum of all of the elements in an array divided by the number of elements.

### Instructions

- Compute the mean of `test_scores_normal`, and assign it to `mean_normal`.
- Compute the mean of `test_scores_negative`, and assign it to `mean_negative`.
- Compute the mean of `test_scores_positive`, and assign it to `mean_positive`.

```
import matplotlib.pyplot as plt
# Let's put a line over our plot that shows the mean.
# This is the same histogram we plotted for skew a few screens ago.
plt.hist(test_scores_normal)
# We can use the .mean() method of a numpy array to compute the mean.
mean_test_score = test_scores_normal.mean()
# The axvline function will plot a vertical line over an existing plot.
plt.axvline(mean_test_score)

# Now we can show the plot and clear the figure.
plt.show()

# When we plot test_scores_negative, which is a very negatively skewed distribution, we see that the small values on the left pull the mean in that direction.
# Very large and very small values can easily skew the mean.
# Very skewed distributions can make the mean misleading.
plt.hist(test_scores_negative)
plt.axvline(test_scores_negative.mean())
plt.show()

# We can do the same with the positive side.
# Notice how the very high values pull the mean to the right more than we would expect.
plt.hist(test_scores_positive)
plt.axvline(test_scores_positive.mean())
plt.show()
mean_normal = test_scores_normal.mean()
mean_negative = test_scores_negative.mean()
mean_positive = test_scores_positive.mean()
```

## 12: Calcultaing The Median

**Median** is another measure of central tendency. This is the midpoint of an array.

To calculate the median, we need to sort the array, then take the value in the middle. If there are two values in the middle (because there are an even number of items in the array), then we take the mean of the two middle values.

The median is less sensitive to very large or very small values (which we call outliers), and is a more realistic *center* of the distribution.

### Instructions

- Plot a histogram for `test_scores_positive`.
- Add a blue line for the median.
- Add a red line for the mean.

```
# Let's plot the mean and median side-by-side in a negatively skewed distribution.
# Unfortunately, arrays don't have a nice median method, so we have to use a numpy function to compute it.
import numpy
import matplotlib.pyplot as plt

# Plot the histogram
plt.hist(test_scores_negative)
# Compute the median
median = numpy.median(test_scores_negative)

# Plot the median in blue (the color argument of "b" means blue)
plt.axvline(median, color="b")

# Plot the mean in red
plt.axvline(test_scores_negative.mean(), color="r")

# Notice how the median is further to the right than the mean.
# It's less sensitive to outliers, and isn't pulled to the left.
plt.show()
plt.hist(test_scores_positive)
plt.axvline(numpy.median(test_scores_positive), color="b")
plt.axvline(test_scores_positive.mean(), color="r")
plt.show()
```

## 13: Overview Of The Titanic Data

On the next few screens, we'll work on cleaning up and analyzing data on passenger survival from the [Titanic](https://en.wikipedia.org/wiki/RMS_Titanic). Each row represents a passenger on the Titanic, complete with personal details and information on their survival.

Here are some of the relevant columns:

- `name` - The passenger's name
- `sex` - The passenger's gender
- `age` - the passenger's age

Several of the columns have missing values, such as the `age` and `sex` columns. Before we can analyze the data, we'll need to deal with these values first.

## 14: Removing Missing Data

Now that we've learned a bit about statistics, let's practice on our Titanic data.

The data is a manifest of all the passengers on the Titanic, a ship that sunk in April 1912. It contains passenger names, ages, and other information (such as whether or not they survived).

Unfortunately, not all of the data is available; details such as age are missing for some passengers. Before we can analyze the data, we have to do something about the missing rows.

The easiest way to address them is to just remove all of the rows with missing data. This isn't necessarily the best solution in all cases, but we'll learn about other ways to handle these situations later on.

### Instructions

- Remove the `NaN` values in the `"age"` and `"sex"` columns.
- Assign the result to `new_titanic_survival`.

```
import pandas
f = "titanic_survival.csv"
titanic_survival = pandas.read_csv(f)

# Luckily, pandas DataFrames have a method that can drop rows that have missing data
# Let's look at how large the DataFrame is first
print(titanic_survival.shape)

# There were 1,310 passengers on the Titanic, according to our data
# Now let's drop any rows that have missing data
# The DataFrame dropna method will do this for us
# It will remove any rows with that contain missing values
new_titanic_survival = titanic_survival.dropna()

# Hmm, it looks like we were too zealous with dropping rows that contained NA values
# We now have no rows in our DataFrame
# This is because some of the later columns, which aren't immediately relevant to our analysis, contain a lot of missing values
print(new_titanic_survival.shape)

# We can use the subset keyword argument to the dropna method so that it only drops rows if there are NA values in certain columns
# This line of code will drop any row where the embarkation port (where people boarded the Titanic) or cabin number is missing
new_titanic_survival = titanic_survival.dropna(subset=["embarked", "cabin"])

# This result is much better. We've only removed the rows we needed to.
print(new_titanic_survival.shape)
new_titanic_survival = titanic_survival.dropna(subset=["age", "sex"])
```

## 15: Plotting Age

Now that we've cleaned up the data, let's analyze it.

### Instructions

- Plot a histogram of the `"age"` column in `new_titanic_survival`.Add a blue line for the median.Add a red line for the mean.

```
# We've loaded the clean version of the data into the variable new_titanic_survival
import matplotlib.pyplot as plt
import numpy
plt.hist(new_titanic_survival["age"])
plt.axvline(numpy.median(new_titanic_survival["age"]), color="b")
plt.axvline(new_titanic_survival["age"].mean(), color="r")
plt.show()
```

## 16: Calculating Indexes For Age

The age distribution is very interesting. It shows that many people in their 20s-40s were travelling without children.

Now that we know what the distribution looks like, let's calculate its characteristics and central tendency measures.

### Instructions

- Assign the *mean* of the `"age"` column of `new_titanic_survival` to `mean_age`.
- Assign the *median* of the `"age"` column of `new_titanic_survival` to `median_age`.
- Assign the *skew* of the `"age"` column of `new_titanic_survival` to `skew_age`.
- Assign the *kurtosis* of the `"age"` column of `new_titanic_survival` to `kurtosis_age`.

```

import numpy
from scipy.stats import skew
from scipy.stats import kurtosis
mean_age = new_titanic_survival["age"].mean()
median_age = numpy.median(new_titanic_survival["age"])
skew_age = skew(new_titanic_survival["age"])
kurtosis_age = kurtosis(new_titanic_survival["age"])
```

# Standard Deviation and Correlation

## 1: Introduction

In this mission, we'll be calculating statistics using data from the National Basketball Association (NBA). Here are the first few rows of the *CSV* file we'll explore:

```
player,pos,age,bref_team_id,g,gs,mp,fg,fga,fg.,x3p,x3pa,x3p.,x2p,x2pa,x2p.,efg.,ft,fta,ft.,orb,drb,trb,ast,stl,blk,tov,pf,pts,season,season_end
Quincy Acy,SF,23,TOT,63,0,847,66,141,0.468,4,15,0.266666666666667,62,126,0.492063492063492,0.482,35,53,0.66,72,144,216,28,23,26,30,122,171,2013-2014,2013
Steven Adams,C,20,OKC,81,20,1197,93,185,0.503,0,0,NA,93,185,0.502702702702703,0.503,79,136,0.581,142,190,332,43,40,57,71,203,265,2013-2014,2013
```

Each row holds data on a single player for a single season. It contains the player's team, the total number of points the player scored, and other information.

Here are some of the interesting columns:

- `player` - The player's name
- `pts` - The total number of points the player scored
- `ast` - The player's total number of assists
- `fg.` - The player's field goal percentage for the season

On the next few screens, we'll explore this data set by calculating a few summary statistics.

## 2: The Mean As The Center

While we've looked at the mean briefly before, it has an interesting property we'd like to point out here.

If we subtract the mean of a set of numbers from each of the numbers within that set, the overall total of all of the differences will always add up to zero.

That's because the mean is the "center" of the data. All of the differences that are negative will always cancel out all of the differences that are positive. Let's look at some examples to verify this.

Let's also become familiar with the mathematical symbol for the mean:

x¯

This symbol means "the average of all of the values in x." The fact that x is lowercase and in bold indicates that it's a vector. The bar over the top indicates "the average of".

### Instructions

- Find the **median** of the `values` list. Assign the result to `values_median`.
- Subtract the median from each element in `values`. Sum up all of the differences, and assign the result to `median_difference_sum`.

```
# Make a list of values
values = [2, 4, 5, -1, 0, 10, 8, 9]
# Compute the mean of the values
values_mean = sum(values) / len(values)
# Find the difference between each of the values and the mean by subtracting the mean from each value.
differences = [i - values_mean for i in values]
# This equals 0.  If you'd like, try changing the values around to verify that it still equals 0.
print(sum(differences))

# We can use the median function from numpy to find the median.
# The median is the "middle" value in a set of values. If we sort the values in order, it's the one in the center (or the average of the two in the center if there are an even number of items in the set).
# You'll see that the differences from the median don't always add up to 0.  You might want to play around with this and think about why that is.
from numpy import median
values_median = median(values)
differences = [i - values_median for i in values]
median_difference_sum = sum(differences)
```

## 3: Finding Variance

Let's look at **variance** in the data. *Variance* tells us how concentrated or "spread out" the data is around the mean.

We looked at *kurtosis* earlier, which measures the shape of a distribution. Variance directly measures how far the average data point is from the mean.

We calculate variance by subtracting every value from the mean, squaring the results, and then averaging them. Mathemically, this looks like this:

σ2=∑i=1n(xi−x¯)2n

σ2 is variance, and ∑i=1n means "the sum from 1 to n", where n is the number of elements in a vector.

This formula goes through the exact same process we just described, and is the most common way to represent it.

Let's complete an exercise to solidify what we've learned.

The data set's `"pf"` column contains the total number of personal fouls each player committed during the season. Let's look at its variance.

### Instructions

- Compute the variance of the data set's `"pts"` column, which holds the total number of points each player scored.
- Assign the result to `point_variance`.

```
import matplotlib.pyplot as plt
import pandas as pd
# We've already loaded the NBA data into the nba_stats variable.
# Find the mean value of the column.
pf_mean = nba_stats["pf"].mean()
# Initialize variance at zero.
variance = 0
# Loop through each item in the "pf" column.
for p in nba_stats["pf"]:
    # Calculate the difference between the mean and the value.
    difference = p - pf_mean
    # Square the difference. This ensures that the result isn't negative.
    # If we didn't square the difference, the total variance would be zero.
    # ** in python means "raise whatever comes before this to the power of whatever number is after this."
    square_difference = difference ** 2
    # Add the difference to the total.
    variance += square_difference
# Average the total to find the final variance.
variance = variance / len(nba_stats["pf"])
point_mean = nba_stats["pts"].mean()
point_variance = 0
for p in nba_stats["pts"]:
    difference = p - point_mean
    square_difference = difference ** 2
    point_variance += square_difference
point_variance = point_variance / len(nba_stats["pts"])
```

## 4: Understanding The Order Of Operations

We've been multiplying and dividing values, but we haven't really discussed the order of operations yet.

The order of operations defines the sequence in which mathematical operations occur. You may recall learning this in your primary school math classes.

Think about the statement `2 * 5 - 1`. The result will be different if we do the multiplication first, rather than the subtraction.

If we multiply first, we get `10 - 1`, which equals `9`.

If we subtract first, we get `2 * 4`, which equals `8`.

We definitely want the results of these operations to be consistent; we don't want to get `8` one time and `9` the next. A default "order of operations" exists for this reason.

Exponents occur first. That means if we raise something to a power (`x ** y`), that operation will execute before anything else. Multiplication (`x * y`) and division (`x / y`) occur next. They are equal to each other in priority. Addition (`x + y`) and subtraction (`x - y`) will occur last. They are also equal to each other in priority.

So raising something to a power will always happen first, then any multiplication/division, and finally any addition/subtraction.

Let's practice with these types of statements to get a better feel for the order of operations.

### Instructions

- Change the mathematical operations around so that `c`equals `25` and `d` equals `.5`.

```
# You may be wondering why multiplication and division are on the same level.
# It doesn't matter whether we do the multiplication or division first; the answer here will always be the same.
# In this case, we need to think of division as multiplication by a fraction. Otherwise, we'll be dividing more than we want to.
# Create a formula
a = 5 * 5 / 2
# Multiply by 1/2 instead of dividing by 2. The result is the same (2/2 == 2 * 1/2).
a_subbed = 5 * 5 * 1/2
a_mul_first = 25 * 1/2
a_div_first = 5 * 2.5
print(a_mul_first == a_div_first)

# The same is true for subtraction and addition.
# In this case, we need to convert subtraction into adding a negative number. If we don't we'll end up subtracting more than we expect.
b = 10 - 8 + 5
# Add -8 instead of subtracting 8.
b_subbed = 10 + -8 + 5
b_sub_first = 2 + 5
b_add_first = 10 + -3
print(b_sub_first == b_add_first)

c = 10 / 2 + 5
d = 3 - 1 / 2 * 2
c = 10 / 2 * 5
d = 3 - 1 / 2 - 2
```

## 5: Using Parentheses To Change The Order Of Operations

We can use parentheses to override the order of operations and make something happen first.

For example, `10 - 2 / 2` will equal `9`, because the division occurs before the subtraction.

If we write `(10 - 2) / 2` instead, the parentheses "force" the operation inside to occur first, so we end up with `8/2`.

### Instructions

- Use parentheses to make `b` equal `1100`.
- Use parentheses to make `c` equal `200`.

```
a = 50 * 50 - 10 / 5
a_paren = 50 * (50 - 10) / 5
# If we put multiple operations inside parentheses, the interpreter will use the order of operations to determine the sequence in which it should execute them.
a_paren = 50 * (50 - 10 / 5)

b = 10 * 10 + 100
c = 8 - 6 * 100
b = 10 * (10 + 100)
c = (8 - 6) * 100
```

## 6: Fractional Powers

Before we explore variance in greater depth, let's take a quick look at exponents.

We wrote `difference ** 2` on a previous screen. This code squared the difference between the mean and the value in the `pf` column. It calculates the equivalent of `difference * difference`.

We could cube the difference by writing `difference ** 3`. This calculation is equal to `difference * difference * difference`.

The same pattern holds true as we raise to higher powers like 4, 5, and so on.

We can also take the roots of numbers using the same syntax.

`difference ** (1/2)` will take the square root. We need to put the fraction in parentheses because raising a value to a power is the operation that would normally occur first.

`difference ** (1/3)` will take the cube root. We can even continue with even smaller fractions.

### Instructions

- Raise `11` to the `fifth` power. Assign the result to `e`.
- Take the `fourth` root of `10000`. Assign the result to `f`.

```
a = 5 ** 2
# Raise to the fourth power
b = 10 ** 4

# Take the square root ( 3 * 3 == 9, so the answer is 3)
c = 9 ** (1/2)

# Take the cube root (4 * 4 * 4 == 64, so 4 is the cube root)
d = 64 ** (1/3)
e = 11 ** 5
f = 10000 ** (1/4)
```

## 7: Calculating Standard Deviation

**Standard deviation** is the most common way to refer to the distance between data points and the mean. It's a very useful concept and a great way to measure the density of a data set.

While it may sound complicated, standard deviation is fairly straightforward; it's the square root of the variance.

Here's the mathematical formula for standard deviation:

σ=√n∑i=1(xi−ˉx)2n

Statisticians and data scientists typically measure the percentage of data that falls within one or two standard deviations of the mean.

### Instructions

- Write a function that calculates the standard deviation of a given column in the `nba_stats` data.
- Use the new function to calculate the standard deviation of the minutes played column (`"mp"`). Assign the results to `mp_dev`.
- Use the function to calculate the standard deviation of the assists column (`"ast"`). Assign the results to `ast_dev`.

```
# We've already loaded the NBA stats into the nba_stats variable.
def calc_column_deviation(column):
    mean = column.mean()
    variance = 0
    for p in column:
        difference = p - mean
        square_difference = difference ** 2
        variance += square_difference
    variance = variance / len(column)
    return variance ** (1/2)

mp_dev = calc_column_deviation(nba_stats["mp"])
ast_dev = calc_column_deviation(nba_stats["ast"])
```

## 8: Finding Standard Deviation Distance

The standard deviation is very useful because it lets us compare the points in a distribution to the mean.

We can say that a certain point is "two standard deviations away from the mean," for example. This gives us a way to compare data density across different charts.

### Instructions

- Find how many standard deviations away from the mean `point_10` is. Assign the result to `point_10_std`.
- Find how many standard deviations away from the mean `point_100` is. Assign the result to `point_100_std`.

```
import matplotlib.pyplot as plt

plt.hist(nba_stats["pf"])
mean = nba_stats["pf"].mean()
plt.axvline(mean, color="r")
# We can calculate standard deviation by using the std() method on a pandas series.
std_dev = nba_stats["pf"].std()
# Plot a line one standard deviation below the mean.
plt.axvline(mean - std_dev, color="g")
# Plot a line one standard deviation above the mean.
plt.axvline(mean + std_dev, color="g")

# We can see how many of the data points fall within one standard deviation of the mean.
# The more that fall into this range, the more dense the data is.
plt.show()

# We can calculate how many standard deviations a data point is from the mean by doing some subtraction and division.
# First, we find the total distance by subtracting the mean.
total_distance = nba_stats["pf"][0] - mean
# Then we divide by standard deviation to find how many standard deviations away the point is.
standard_deviation_distance = total_distance / std_dev

point_10 = nba_stats["pf"][9]
point_100 = nba_stats["pf"][99]
point_10_std = (point_10 - mean) / std_dev
point_100_std = (point_100 - mean) / std_dev
```

## 9: Working With The Normal Distribution

The **normal distribution** is a special kind of distribution. You might recognize it more commonly as a bell curve.

The normal distribution is found in a variety of natural phenomena. If we made a histogram of the heights of everyone on the planet, for example, it would be more or less a normal distribution.

We can generate a normal distribution by using a **probability density function**.

### Instructions

- Make a normal distribution across the range that starts at `-10`, ends at `10`, and has the step `.1`.
- The distribution should have a mean of `0` and standard deviation of `2`.

```
import numpy as np
import matplotlib.pyplot as plt
# The norm module has a pdf function (pdf stands for probability density function)
from scipy.stats import norm

# The arange function generates a numpy vector
# The vector below will start at -1, and go up to, but not including 1
# It will proceed in "steps" of .01.  So the first element will be -1, the second -.99, the third -.98, all the way up to .99.
points = np.arange(-1, 1, 0.01)

# The norm.pdf function will take the points vector and convert it into a probability vector
# Each element in the vector will correspond to the normal distribution (earlier elements and later element smaller, peak in the center)
# The distribution will be centered on 0, and will have a standard devation of .3
probabilities = norm.pdf(points, 0, .3)

# Plot the points values on the x-axis and the corresponding probabilities on the y-axis
# See the bell curve?
plt.plot(points, probabilities)
plt.show()
points = np.arange(-10, 10, 0.1)
probabilities = norm.pdf(points, 0, 2)
plt.plot(points, probabilities)
plt.show()
```

## 10: Normal Distribution Deviation

One cool thing about normal distributions is that for every single one, the same percentage of the data is within one standard deviation of the mean, the same percentage is within two standard deviations of the mean, and so on.

About 68% of the data is within one standard deviation, roughly 95% is within two standard deviations, and about 99% is within three standard deviations.

This helps us quickly understand where values fall within the data set, as well as how typical or unusual they are.

### Instructions

- For each point in `wing_lengths`, calculate the distance from the mean in number of standard deviations.
- Calculate the percentage of the data that's within one standard deviation of the mean. Assign the result to `within_one_percentage`.
- Calculate the percentage of the data that's within two standard deviations of the mean. Assign the result to `within_two_percentage`.
- Calculate the percentage of the data that's within three standard deviations of the mean. Assign the result to`within_three_percentage`.

```
# Housefly wing lengths in millimeters
wing_lengths = [36, 37, 38, 38, 39, 39, 40, 40, 40, 40, 41, 41, 41, 41, 41, 41, 42, 42, 42, 42, 42, 42, 42, 43, 43, 43, 43, 43, 43, 43, 43, 44, 44, 44, 44, 44, 44, 44, 44, 44, 45, 45, 45, 45, 45, 45, 45, 45, 45, 45, 46, 46, 46, 46, 46, 46, 46, 46, 46, 46, 47, 47, 47, 47, 47, 47, 47, 47, 47, 48, 48, 48, 48, 48, 48, 48, 48, 49, 49, 49, 49, 49, 49, 49, 50, 50, 50, 50, 50, 50, 51, 51, 51, 51, 52, 52, 53, 53, 54, 55]
mean = sum(wing_lengths) / len(wing_lengths)
variances = [(i - mean) ** 2 for i in wing_lengths]
variance = sum(variances)/ len(variances)
standard_deviation = variance ** (1/2)

standard_deviations = [(i - mean) / standard_deviation for i in wing_lengths]
def within_percentage(deviations, count):
    within = [i for i in deviations if i <= count and i >= -count]
    count = len(within)
    return count / len(deviations)

within_one_percentage = within_percentage(standard_deviations, 1)
within_two_percentage = within_percentage(standard_deviations, 2)
within_three_percentage = within_percentage(standard_deviations, 3)
```

## 11: Using Scatterplots To Plot Correlations

We've spent a lot of time looking at single variables and how their distributions look. While distributions are interesting on their own, it can also be revealing to look at how two variables correlate with each other.

Much of statistics deals with analyzing how variables impact each other, and the first step is to graph them out with a scatterplot.

While graphing them out, we can look at correlation. If two variables change together (ie, when one goes up, the other goes up), we know that they are correlated.

### Instructions

- Make a scatterplot of the `"fta"` (free throws attempted) column against the `"pts"` column.
- Make a scatterplot of the `"stl"` (steals) column against the `"pf"` column.

```
import matplotlib.pyplot as plt

# Plot field goals attempted (number of shots someone takes in a season) vs. point scored in a season.
# Field goals attempted is on the x-axis, and points is on the y-axis.
# As you can tell, they are very strongly correlated. The plot is close to a straight line.
# The plot also slopes upward, which means that as field goal attempts go up, so do points.
# That means that the plot is positively correlated.
plt.scatter(nba_stats["fga"], nba_stats["pts"])
plt.show()

# If we make points negative (so the people who scored the most points now score the least, because 3000 becomes -3000), we can change the direction of the correlation.
# Field goals are negatively correlated with our new "negative" points column -- the more free throws you attempt, the less negative points you score.
# We can see this because the correlation line slopes downward.
plt.scatter(nba_stats["fga"], -nba_stats["pts"])
plt.show()

# Now, we can plot total rebounds (number of times someone got the ball back for their team after someone shot) vs total assists (number of times someone helped another person score).
# These are uncorrelated, so you don't see the same nice line as you see with the plot above.
plt.scatter(nba_stats["trb"], nba_stats["ast"])
plt.show()
plt.scatter(nba_stats["fta"], nba_stats["pts"])
plt.show()

plt.scatter(nba_stats["stl"], nba_stats["pf"])
plt.show()
```

## 12: Measuring Correlation With Pearson's R

Measuring correlation can be a big help when we need to analyze a lot of variables. This spares us from having to eyeball everything.

The most common way to measure correlation is to use **Pearson's r**, which we also call an **r-value**.

We'll explore how the calculations work later. For now, though, we'll focus on the values.

An *r-value* ranges from -1 to 1, and indicates how strongly two variables are correlated.

A 1 indicates a perfect positive correlation. This would appear as a straight, upward-sloping line on our plots.

A 0 indicates no correlation. We'd see a scatterplot with points that appear random.

A -1 indicates a perfect negative correlation. This would appear as a straight, downward-sloping line.

Any correlation between -1 and 0 will show up as a scattering of points. The same is true of correlations falling between 0 and 1. The closer the value is to 0, the more random the points will appear. The closer it is to -1 or 1, the more "line-like" the points will appear.

We can use a function from *scipy* to calculate Pearson's r.

### Instructions

- Find the correlation between the `"fta"` column and the `"pts"` column. Assign the result to `r_fta_pts`.
- Find the correlation between the `"stl"` column and the `"pf"` column. Assign the result to `r_stl_pf`.

```
from scipy.stats.stats import pearsonr

# The pearsonr function will find the correlation between two columns of data.
# It returns the r value and the p value.  We'll learn more about p values later on.
r, p_value = pearsonr(nba_stats["fga"], nba_stats["pts"])
# As we can see, this is a very high positive r value - it's close to 1.
print(r)

# These two columns are much less correlated.
r, p_value = pearsonr(nba_stats["trb"], nba_stats["ast"])
# We get a much lower, but still positive, r value.
print(r)
r_fta_pts, p_value = pearsonr(nba_stats["fta"], nba_stats["pts"])
r_stl_pf, p_value = pearsonr(nba_stats["stl"], nba_stats["pf"])
```

## 13: Calculate Covariance

We looked at finding the correlation coefficient with a function. Now, let's take a brief look under the hood to see how we can do it ourselves.

Another way to think of correlation is in terms of variance.

Two variables are correlated when they both vary individually, but in similar ways. For example, correlation occurs when if one variable goes up, another variable also goes up.

This is called *covariance*. *Covariance* refers to how different numbers vary jointly.

There's a limit to how much two variables can *co-vary*. This is because each variable has its own variance. These individual variances set a maximum theoretical limit on the covariance between two variables. In other words, a set of variables can't co-vary more from the mean than the two variables individually vary from the mean.

Two variables reach the maximum possible covariance when they vary in an identical way (ie, you see a straight line on the plot).

The *r-value* is a ratio between the actual covariance and the maximum possible positive covariance.

Let's look at actual covariance first. Mathematically speaking, covariance between two variables looks like this:

For each element in the vectors x and y, we:

1. Take the value at each position from 1 to the length of the vectors.
2. Subtract the mean of the vector from those values.
3. Multiply them together at each position, and all of the resulting values together.

### Instructions

- Make a function that calculates covariance.
- Use the function to calculate the covariance of the `"stl"`and `"pf"` columns. Assign the result to `cov_stl_pf`.
- Use the function to calculate the covariance of the `"fta"`and `"pts"` columns. Assign the result to `cov_fta_pts`.

```
# We've already loaded the nba_stats variable.
def covariance(x, y):
    x_mean = sum(x) / len(x)
    y_mean = sum(y) / len(y)
    x_diffs = [i - x_mean for i in x]
    y_diffs = [i - y_mean for i in y]
    codeviates = [x_diffs[i] * y_diffs[i] for i in range(len(x))]
    return sum(codeviates) / len(codeviates)

cov_stl_pf = covariance(nba_stats["stl"], nba_stats["pf"])
cov_fta_pts = covariance(nba_stats["fta"], nba_stats["pts"])
```

## 14: Calculate Correlation With The Std() Method

Now that we know how to calculate covariance, we can get the correlation coefficient using the following formula:

For the denominator, we need to multiple the standard deviations for x and y. This is the maximum possible positive covariance, which is just both of the standard deviation values multiplied. If we divide our actual covariance by this, we get the r-value.

We can use the `std` method on any pandas [DataFrame](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.std.html) or [Series](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.std.html) to calculate the standard deviation. The following code returns the standard deviation for the `pf`column:

```
nba_stats["pf"].std()
```

We can use the `cov` function from NumPy to compute covariance, returning a 2x2 matrix. The following code returns the covariance between the `pf` and `stl`columns:

```
cov(nba_stats["pf"], nba_stats["stl"])[0,1]
```

### Instructions

- Compute the correlation coefficient for the `fta` and `blk`columns, and assign the result to `r_fta_blk`.
- Compute the correlation coefficient for the `ast` and `stl`columns, and assign the result to `r_ast_stl`.

```
from numpy import cov
# We've already loaded the nba_stats variable for you.
r_fta_blk = cov(nba_stats["fta"], nba_stats["blk"])[0,1] / ((nba_stats["fta"].var() * nba_stats["blk"].var())** (1/2))
r_ast_stl = cov(nba_stats["ast"], nba_stats["stl"])[0,1] / ((nba_stats["ast"].var() * nba_stats["stl"].var())** (1/2))
```

# Challenge: Descriptive Statistics

## 1: Introduction

In this challenge, you'll practice the statistical techniques you learned in the previous missions. These techniques are referred to collectively as **descriptive statistics** since they're used to describe and understand a dataset and not directly for prediction.

The FiveThirtyEight team recently released a dataset containing the critics scores for a sample of movies that have substantive (at least 30) user and critic reviews from IMDB, Rotten Tomatoes, Metacritic, IMDB, and Fandango. Whenever a movie is released, movie review sites ask their approved network of critics and their site's user base to rate the movie. These scores are aggregated and the average score from both groups are posted on their site for each movie.

Here are links to the page for **Star Trek Beyond** for each site:

- [Rotten Tomatoes](https://www.rottentomatoes.com/m/star_trek_beyond)
- [Fandango](http://www.fandango.com/startrekbeyond_188462/movieoverview)
- [Metacritic](http://www.metacritic.com/movie/star-trek-beyond)
- [IMDB](http://www.imdb.com/title/tt2660888/)

The dataset contains information on most movies from 2014 and 2015 and was used to help the team at FiveThirtyEight explore Fandango's suspiciously high ratings. You can read their analysis [here](http://fivethirtyeight.com/features/fandango-movies-ratings/).

You'll be working with the file `fandango_score_comparison.csv`, which you can download from [their Github repo](https://github.com/fivethirtyeight/data/tree/master/fandango). Here are some of the columns in the dataset:

- `FILM` - film name.
- `RottenTomatoes` - Rotten Tomatoes critics average score.
- `RottenTomatoes_User` - Rotten Tomatoes user average score.
- `RT_norm` - Rotten Tomatoes critics average score (normalized to a 0 to 5 point scale).
- `RT_user_norm` - Rotten Tomatoes user average score (normalized to a 0 to 5 point scale).
- `Metacritic` - Metacritic critics average score.
- `Metacritic_user_nom` - Metacritic user average score (normalized to a 0 to 5 point scale).
- `Metacritic_norm` - Metacritic critics average score (normalized to a 0 to 5 point scale).
- `Fandango_Ratingvalue` - Fandango user average score (0 to 5 stars).
- `IMDB_norm` - IMDB user average score (normalized to a 0 to 5 point scale).

The full column list and descriptions are available at FiveThirtyEight's [Github repo](https://github.com/fivethirtyeight/data/tree/master/fandango). Let's focus on the normalized user scores for now and generate histograms to better understand each site's distributions.

### Instructions

- Create a matplotlib subplot grid with the following properties:
  - 4 rows by 1 column,
  - `figsize` of 5 (width) by 12 (height).
  - each Axes instance should have an x-value range of `0.0` to `5.0`.
- Generate the following histograms:
  - First plot (top most): Histogram of normalized Rotten Tomatoes scores by users.
  - Second plot: Histogram of normalized Metacritic scores by users.
  - Third plot: Histogram of Fandango scores by users.
  - Fourth plot (bottom most): Histogram of IMDB scores by users.

```
import matplotlib.pyplot as plt
import pandas as pd
movie_reviews = pd.read_csv("fandango_score_comparison.csv")
fig = plt.figure(figsize=(5,12))
ax1 = fig.add_subplot(4,1,1)
ax2 = fig.add_subplot(4,1,2)
ax3 = fig.add_subplot(4,1,3)
ax4 = fig.add_subplot(4,1,4)

ax1.set_xlim(0,5.0)
ax2.set_xlim(0,5.0)
ax3.set_xlim(0,5.0)
ax4.set_xlim(0,5.0)

movie_reviews["RT_user_norm"].hist(ax=ax1)
movie_reviews["Metacritic_user_nom"].hist(ax=ax2)
movie_reviews["Fandango_Ratingvalue"].hist(ax=ax3)
movie_reviews["IMDB_norm"].hist(ax=ax4)
```

## 2: Mean

The most obvious things that stick out is that essentially all of the Fandango average user reviews are greater than 3 on a 5 point scale. The distributions of the Rotten Tomatoes and Metacritic scores, on the other hand, more closely resemble a normal distribution, which is generally what you'd expect if you knew nothing else. This is because the normal distribution is the most common distribution in nature and is used to approximate many phenomenon. Starting with the assumption that a phemonenon is normal is incredibly common, especially when you don't have a clear generative model to understand how the data was generated.

Now that you hopefully have some visual understanding of these scores, let's calculate some statistical measures to see how the properties the histograms suggested are reflected in numerical values. Let's focus on just the normalized user reviews in this mission.

### Instructions

Write a function, named `calc_mean`, that returns the mean for the values in a Series object.

- Recall that you can return the values in a Series using the [values attribute](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.values.html).

Select just the columns containing normalized user reviews and assign to a separate Dataframe named `user_reviews`. Those columns are: `RT_user_norm`, `Metacritic_user_nom`, `Fandango_Ratingvalue`, `IMDB_norm`.

Use the Dataframe method [apply](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.apply.html) to apply the `calc_mean`function over the filtered Dataframe `user_reviews`.

- Assign the mean of `RT_user_norm` to `rt_mean`.
- Assign the mean of `Metacritic_user_nom` to `mc_mean`.
- Assign the mean of `Fandango_Ratingvalue` to `fg_mean`.
- Assign the mean of `IMDB_norm` to `id_mean`.
- Use the variables display below the code output window or `print` statements to observe each value.

```

def calc_mean(series):
    vals = series.values
    mean = sum(vals) / len(vals)
    return mean    

columns = ["RT_user_norm", "Metacritic_user_nom", "Fandango_Ratingvalue", "IMDB_norm"]
user_reviews = movie_reviews[columns]
user_reviews_means = user_reviews.apply(calc_mean)

rt_mean = user_reviews_means["RT_user_norm"]
mc_mean = user_reviews_means["Metacritic_user_nom"]
fg_mean = user_reviews_means["Fandango_Ratingvalue"]
id_mean = user_reviews_means["IMDB_norm"]

print("Rotten Tomatoes (mean):", rt_mean)
print("Metacritic (mean):", mc_mean)
print("Fandango (mean):",fg_mean)
print("IMDB (mean):",id_mean)
```

## 3: Variance And Standard Deviation

It seems like the Fandango user reviews have the highest mean and skew the most towards to the higher end compared to the other review sites. Let's now calculate variance and standard deviation to better understand the spreads.

### Instructions

- To calculate the variance:
  - write a function, named `calc_variance`, that returns the variance for the values in a Series object.
- To calculate the standard deviation:
  - use the output of the `calc_variance` function since standard deviation is a simple calculation away from variance.
- Calculate the variance and standard deviation for the `RT_user_norm` column and assign to `rt_var` and `rt_stdev` respectively.
- Calculate the variance and standard deviation for the `Metacritic_user_nom` column and assign to `mc_var` and `mc_stdev` respectively.
- Calculate the variance and standard deviation for the `Fandango_Ratingvalue` column and assign to `fg_var` and `fg_stdev` respectively.
- Calculate the variance and standard deviation for the `IMDB_norm` column and assign to `id_var` and `id_stdev`respectively.
- Use the variables display below the code output window or `print` statements to observe each value.

```
def calc_variance(series):
    mean = calc_mean(series)
    squared_deviations = (series - mean)**2
    mean_squared_deviations = calc_mean(squared_deviations)
    return mean_squared_deviations

columns = ["RT_user_norm", "Metacritic_user_nom", "Fandango_Ratingvalue", "IMDB_norm"]
user_reviews = movie_reviews[columns]
user_reviews_variances = user_reviews.apply(calc_variance)

rt_var = user_reviews_variances["RT_user_norm"]
mc_var = user_reviews_variances["Metacritic_user_nom"]
fg_var = user_reviews_variances["Fandango_Ratingvalue"]
id_var = user_reviews_variances["IMDB_norm"]

rt_stdev = rt_var ** (1/2)
mc_stdev = mc_var ** (1/2)
fg_stdev = fg_var ** (1/2)
id_stdev = id_var ** (1/2)

print("Rotten Tomatoes (variance):", rt_var)
print("Metacritic (variance):", mc_var)
print("Fandango (variance):", fg_var)
print("IMDB (variance):", id_var)

print("Rotten Tomatoes (standard deviation):", rt_stdev)
print("Metacritic (standard deviation):", mc_stdev)
print("Fandango (standard deviation):", fg_stdev)
print("IMDB (standard deviation):", id_stdev)
```

## 4: Scatter Plots

The mean and variance values you calculated in the last screens should match the visual intuition the histograms gave you. Rotten Tomatoes and Metacritic have more spread out scores (high variance) and the mean is around 3. Fandango, on the other hand, has low spread (low variance) and a much higher mean, which could imply that the site has a strong bias towards higher reviews. IMDB is somewhere in the middle, with a low variance, like Fandango's user reviews, but a much more moderate mean value.

So it seems like something is especially fishy about Fandango's ratings, which was the inspiration behind [the FiveThirtyEight post](http://fivethirtyeight.com/features/fandango-movies-ratings/) to begin with. Since Fandango's main business is selling movie tickets, it's possible their primary incentive may differ from pure review sites like Rotten Tomatoes or Metacritic.

Let's now explore if Fandango's user ratings are at least relatively correct. More precisely, are movies that are highly rated on Rotten Tomatoes, IMDB, and Metacritic also highly rated on Fandango?

We can accomplish that by understanding how Fandango's scores and related to scores from Rotten Tomatoes and Metacritic. First things first, let's get a visual sense by generating scatter plots.

### Instructions

- Create a matplotlib subplot grid with the following properties:
  - 3 rows by 1 column,
  - `figsize` of 4 (width) by 8 (height),
  - each Axes instance should have an x-value range of `0.0` to `5.0`.
- Generate the following scatter plot (y-axis vs x-axis):
  - First plot (top most): Fandango user reviews vs. Rotten Tomatoes user reviews.
  - Second plot: Fandango user reviews vs. Metacritic user reviews.
  - Third plot (bottom most): Fandango user reviews vs. IMDB user reviews.

```
fig = plt.figure(figsize=(4,8))
ax1 = fig.add_subplot(3,1,1)
ax2 = fig.add_subplot(3,1,2)
ax3 = fig.add_subplot(3,1,3)

ax1.set_xlim(0,5.0)
ax2.set_xlim(0,5.0)
ax3.set_xlim(0,5.0)

ax1.scatter(movie_reviews["RT_user_norm"], movie_reviews["Fandango_Ratingvalue"])
ax2.scatter(movie_reviews["Metacritic_user_nom"], movie_reviews["Fandango_Ratingvalue"])
ax3.scatter(movie_reviews["IMDB_norm"], movie_reviews["Fandango_Ratingvalue"])
```

## 5: Covariance

It seems like Rotten Tomatoes and IMDB user reviews correlate the most with Fandango user reviews while Metacritic only weakly correlates. Let's write a function that to calculates the covariance values in this screen and a function to calculate the correlation values in the next screen.

Here's the formula for computing the covariance between 2 variables: cov(x,y)=∑i=1n(xi−x¯)(yi−y¯)n.

### Instructions

- Write a function, named `calc_covariance`, that computes the covariance between the values of 2 Series objects.

Use the `calc_covariance` function you wrote to:

- Compute the covariance between the `RT_user_norm` and `Fandango_Ratingvalue` columns. Assign the result to `rt_fg_covar`.
- Compute the covariance between the `Metacritic_user_nom` and `Fandango_Ratingvalue`columns. Assign the result to `mc_fg_covar`.
- Compute the covariance between the `IMDB_norm` and `Fandango_Ratingvalue` columns. Assign the result to `id_fg_covar`.
- Use the variables display below the code output window or `print` statements to observe each value.

```
def calc_mean(series):
    vals = series.values
    mean = sum(vals) / len(vals)
    return mean
def calc_covariance(series_one, series_two):
    x = series_one.values
    y = series_two.values
    x_mean = calc_mean(series_one)
    y_mean = calc_mean(series_two)
    x_diffs = [i - x_mean for i in x]
    y_diffs = [i - y_mean for i in y]
    codeviates = [x_diffs[i] * y_diffs[i] for i in range(len(x))]
    return sum(codeviates) / len(codeviates)

rt_fg_covar = calc_covariance(movie_reviews["RT_user_norm"], movie_reviews["Fandango_Ratingvalue"])
mc_fg_covar = calc_covariance(movie_reviews["Metacritic_user_nom"], movie_reviews["Fandango_Ratingvalue"])
id_fg_covar = calc_covariance(movie_reviews["IMDB_norm"], movie_reviews["Fandango_Ratingvalue"])

print("Covariance between Rotten Tomatoes and Fandango:", rt_fg_covar)
print("Covariance between Metacritic and Fandango", mc_fg_covar)
print("Covariance between IMDB and Fandango", id_fg_covar)
```

## 6: Correlation

Interestingly, Rotten Tomatoes covaries strongly with Fandango (`0.36`) compared to Metacritic (`0.13`) and IMDB (`0.14`). Finally, let's calculate the correlation values by using the `calc_covariance` a function from the previous step.

Here's the full formula for correlation:

cov(x,y)σxσy.

where `cov` is shorthand for the covariance function, and σ represents the standard deviation.

### Instructions

- Write a function, named `calc_correlation`, that uses the `calc_covariance` and `calc_variance` functions to calculate the correlation for 2 Series objects. The function should have 2 parameters, one for each Series object.
- Compute the correlation between the `RT_user_norm` and `Fandango_Ratingvalue` columns and assign the result to `rt_fg_corr`.
- Compute the correlation between the `Metacritic_user_nom` and `Fandango_Ratingvalue`columns and assign the result to `mc_fg_corr`.
- Compute the correlation between the `IMDB_norm` and `Fandango_Ratingvalue` columns and assign the result to `id_fg_corr`.
- Use the variables display below the code output window or `print` statements to observe each value.

```
def calc_correlation(series_one, series_two):
    numerator = calc_covariance(series_one, series_two)
    series_one_std = calc_variance(series_one) ** (1/2)
    series_two_std = calc_variance(series_two) ** (1/2)
    denominator = series_one_std * series_two_std
    correlation = numerator / denominator
    return correlation

rt_fg_corr = calc_correlation(movie_reviews["RT_user_norm"], movie_reviews["Fandango_Ratingvalue"])
mc_fg_corr = calc_correlation(movie_reviews["Metacritic_user_nom"], movie_reviews["Fandango_Ratingvalue"])
id_fg_corr = calc_correlation(movie_reviews["IMDB_norm"], movie_reviews["Fandango_Ratingvalue"])

print("Correlation between Rotten Tomatoes and Fandango", rt_fg_corr)
print("Correlation between Metacritic and Fandango", mc_fg_corr)
print("Correlation between IMDB and Fandango", id_fg_corr)
```

## 7: Next Steps

As the scatter plots suggested, Rotten Tomatoes and IMDB correlate the strongest with Fandango, with correlation values of `0.72` and `0.60` respectively. Metacritic, on the other hand, only has a correlation value of `0.34` with Fandango. While covariance and correlation values may seem complicated to compute and hard to reason with, their best use case is in comparing relationships like we did in this challenge.

In the next mission, we'll explore the basics of linear regression, which is a powerful machine learning technique.

# Linear regression

## 1: Introduction

In this mission, we'll be looking at how expert wine tasters evaluated different white wines. Here are the first few rows of the data:

```
"fixed acidity","volatile acidity","citric acid","residual sugar","chlorides","free sulfur dioxide","total sulfur dioxide","density","pH","sulphates","alcohol","quality"
7,0.27,0.36,20.7,0.045,45,170,1.001,3,0.45,8.8,6
6.3,0.3,0.34,1.6,0.049,14,132,0.994,3.3,0.49,9.5,6
```

Each row represents a single wine, and each column represents some property of that wine. Here are some of the interesting columns:

- `density` -- shows the amount of material dissolved in the wine.
- `alcohol` -- the alcohol content of the wine.
- `quality` -- the average quality rating (`1-10`) given to the wine.

In the next few screens, we'll learn how to use a technique called linear regression to make predictions about wine quality using existing data.

## 2: Drawing Lines

Before we get started with linear regression, let's take a look at how to draw lines.

A simple line is y=x. This means that the value of a point on the y-axis is the same as the corresponding value on the x-axis.

### Instructions

- Plot the equation y=x−1, using the existing `x` variable.
- Plot the equation y=x+10, using the existing `x`variable.

```
import matplotlib.pyplot as plt
import numpy as np

x = [0, 1, 2, 3, 4, 5]
# Going by our formula, every y value at a position is the same as the x-value in the same position.
# We could write y = x, but let's write them all out to make this more clear.
y = [0, 1, 2, 3, 4, 5]

# As you can see, this is a straight line that passes through the points (0,0), (1,1), (2,2), and so on.
plt.plot(x, y)
plt.show()

# Let's try a slightly more ambitious line.
# What if we did y = x + 1?
# We'll make x an array now, so we can add 1 to every element more easily.
x = np.asarray([0, 1, 2, 3, 4, 5])
y = x + 1

# y is the same as x, but every element has 1 added to it.
print(y)

# This plot passes through (0,1), (1,2), and so on.
# It's the same line as before, but shifted up 1 on the y-axis.
plt.plot(x, y)
plt.show()

# By adding 1 to the line, we moved what's called the y-intercept -- where the line intersects with the y-axis.
# Moving the intercept can shift the whole line up (or down when we subtract).
y = x - 1
plt.plot(x, y)
plt.show()

y = x + 10
plt.plot(x, y)
plt.show()
```

## 3: Working With Slope

Now that we have a way to move the line up and down, what about the steepness of the line?

This was unchanged earlier -- the values on the line always went up 1 on the y-axis every time they went up 1 on the x-axis.

What if we want to make a line that goes up 2 numbers on the y-axis every time it goes up 1 on the x-axis?

This is where **slope** comes in. The slope is multiplied by the x-value to get the new y value.

It looks like this: y=mx. If we set the slope, m, equal to `2`, we'll get what we want.

### Instructions

- Plot the equation y=4x, using the existing `x` variable.
- Plot the equation y=.5x, using the existing `x` variable.
- Plot the equation y=−2x, using the existing `x` variable.

```
import matplotlib.pyplot as plt
import numpy as np

x = np.asarray([0, 1, 2, 3, 4, 5])
# Let's set the slope of the line to 2.
y = 2 * x

# See how this line is "steeper" than before?  The larger the slope is, the steeper the line becomes.
# On the flipside, fractional slopes will create a "shallower" line.
# Negative slopes will create a line where y values decrease as x values increase.
plt.plot(x, y)
plt.show()
y = 4 * x
plt.plot(x, y)
plt.show()

y = .5 * x
plt.plot(x, y)
plt.show()

y = -2 * x
plt.plot(x, y)
plt.show()
```

## 4: Starting Out With Linear Regression

In the last mission, we did some work with the *r-value*. The *r-value* indicates how correlated two variables are. This can range from no correlation to a negative correlation to a positive correlation.

The more correlated two variables are, the easier it becomes to use one to predict the other. For instance, if I know that how much I pay for my steak is highly positively correlated to the size of the steak (in ounces), I can create a formula that helps me predict how much I would be paying for my steak.

The way we do this is with *linear regression*. Linear regression gives us a formula. If we plug in the value for one variable into this formula, we get the value for the other variable.

The equation to create the formula takes the form y=mx+b.

You might recognize pieces of this equation from the past two screens -- we're just adding the intercept and slope into one equation.

This equation is saying "the predicted value of the second variable (y) is equal to the value of the first variable (x) times the slope (m) plus the intercept (b)".

We have to calculate values for m and b before we can use our formula.

We'll calculate slope first -- the formula is cov(x,y)σx2, which is just the covariance of x and y divided by the variance of x.

We can use the `cov` function to calculate covariance, and the `.var()` method on Pandas series to calculate variance.

### Instructions

- Calculate the slope you would need to predict the `"quality"` column (y) using the `"density"` column (x).
- Assign the slope to `slope_density`.

```
# The wine quality data is loaded into wine_quality
from numpy import cov
slope_density = cov(wine_quality["density"], wine_quality["quality"])[0, 1] / wine_quality["density"].var()
```

## 5: Finishing Linear Regression

Now that we can calculate the slope for our linear regression line, we just need to calculate the intercept.

The intercept is just how much higher or lower the average y point is than our predicted value.

We can compute the intercept by taking the slope we calculated and doing this: y¯−mx¯. So we just take the mean of the y values, and then subtract the slope times the mean of the x values from that.

Remember that we can calculate the mean by using the `.mean()` method.

### Instructions

- Calculate the y-intercept that you would need to predict the `"quality"` column (y) using the `"density"` column (x).
- Assign the result to `intercept_density`.

```
from numpy import cov

# This function will take in two columns of data, and return the slope of the linear regression line.
def calc_slope(x, y):
  return cov(x, y)[0, 1] / x.var()
intercept_density = wine_quality["quality"].mean() - (calc_slope(wine_quality["density"], wine_quality["quality"]) * wine_quality["density"].mean())
```

## 6: Making Predictions

Now that we've computed our slope and our intercept, we can make predictions about the y-values from the x-values.

In order to do this, we go back to our original formula: y=mx+b, and just plug in the values for m and b.

We can then compute predicted y-values for any x-value. This lets us make predictions about the quality of x-values that we've never seen. For example, a wine with a density of `.98` isn't in our dataset, but we can make a prediction about what quality a reviewer would assign to a wine with this density.

Depending on how correlated the predictor and the value being predicted are, the predictions may be good or bad.

Let's look at making predictions now, and we'll move on to figuring out how good they are.

### Instructions

- Write a function to compute the predicted y-value from a given x-value.
- Use the `.apply()` method on the `"density"` column to apply the function to each item in the column. This will compute all the predicted y-values.
- Assign the result to `predicted_quality`.

```
from numpy import cov

def calc_slope(x, y):
  return cov(x, y)[0, 1] / x.var()

# Calculate the intercept given the x column, y column, and the slope
def calc_intercept(x, y, slope):
  return y.mean() - (slope * x.mean())
slope = calc_slope(wine_quality["density"], wine_quality["quality"])
intercept = calc_intercept(wine_quality["density"], wine_quality["quality"], slope)

def compute_predicted_y(x):
  return x * slope + intercept

predicted_quality = wine_quality["density"].apply(compute_predicted_y)
```

## 7: Finding Error

Now that we know how to make a regression line manually, let's look at an easier way to do it, using a function from *scipy*.

The `linregress` function makes it simple to do linear regression.

Now that we know a simpler way to do linear regression, let's look at how to figure out if our regression is good or bad.

We can plot out our line and our actual values, and see how far apart they are on the y-axis.

We can also compute the distance between each prediction and the actual value -- these distances are called *residuals*.

If we add up the sum of the squared residuals, we can get a good error estimate for our line.

We have to add the squared residuals, because just like differences from the mean, the residuals add to 0 if they aren't squared.

To put it in math terms, the sum of squared residuals is: ∑i=1n(yi−y^i)2

The variable

y^i

is the predicted y value at position i.

### Instructions

- Using the given `slope` and `intercept`, calculate the predicted y values.
- Subtract each predicted y value from the corresponding actual y value, square the difference, and add all the differences together.
- This will give you the sum of squared residuals. Assign this value to `rss`.

```
from scipy.stats import linregress

# We've seen the r_value before -- we'll get to what p_value and stderr_slope are soon -- for now, don't worry about them.
slope, intercept, r_value, p_value, stderr_slope = linregress(wine_quality["density"], wine_quality["quality"])

# As you can see, these are the same values we calculated (except for slight rounding differences)
print(slope)
print(intercept)
import numpy
predicted_y = numpy.asarray([slope * x + intercept for x in wine_quality["density"]])
residuals = (wine_quality["quality"] - predicted_y) ** 2
rss = sum(residuals)
```

## 8: Standard Error

From the sum of squared residuals, we can find the **standard error**. The standard error is similar to the standard deviation, but it tries to make an estimate for the whole population of y-values -- even the ones we haven't seen yet that we may want to predict in the future.

The standard error lets us quickly determine how good or bad a linear model is at prediction.

The equation for standard error is RSSn−2.

You take the sum of squared residuals, divide by the number of y-points minus two, and then take the square root.

You might be wondering about why 2 is subtracted -- this is due to differences between the whole population and a sample. This will be explained in more depth later on.

### Instructions

- Calculate the standard error using the above formula.
- Calculate what percentage of actual y values are within 1 standard error of the predicted y value. Assign the result to `within_one`.
- Calculate what percentage of actual y values are within 2 standard errors of the predicted y value. Assign the result to `within_two`.
- Calculate what percentage of actual y values are within 3 standard errors of the predicted y value. Assign the result to `within_three`.
- Assume that "within" means "up to and including", so be sure to count values that are exactly 1, 2, or 3 standard errors away.

```
from scipy.stats import linregress
import numpy as np

# We can do our linear regression
# Sadly, the stderr_slope isn't the standard error, but it is the standard error of the slope fitting only
# We'll need to calculate the standard error of the equation ourselves
slope, intercept, r_value, p_value, stderr_slope = linregress(wine_quality["density"], wine_quality["quality"])

predicted_y = np.asarray([slope * x + intercept for x in wine_quality["density"]])
residuals = (wine_quality["quality"] - predicted_y) ** 2
rss = sum(residuals)
stderr = (rss / (len(wine_quality["quality"]) - 2)) ** .5

def within_percentage(y, predicted_y, stderr, error_count):
  within = stderr * error_count

  differences = abs(predicted_y - y)
  lower_differences = [d for d in differences if d <= within]

  within_count = len(lower_differences)
  return within_count / len(y)

within_one = within_percentage(wine_quality["quality"], predicted_y, stderr, 1)
within_two = within_percentage(wine_quality["quality"], predicted_y, stderr, 2)
within_three = within_percentage(wine_quality["quality"], predicted_y, stderr, 3)
```

# Distributions and sampling

## 1: Exploring The Data

In this mission, we'll be looking at US income data. Each row is a single county in the US. For each county, we have the following columns:

- `id` -- the county id.
- `county` -- the name and state of the county.
- `pop_over_25` -- the number of adults over age 25.
- `median_income` -- the median income for residents over age 25 in the county.
- `median_income_no_hs` -- median income for residents without a high school education.
- `median_income_hs` -- median income for high school graduates who didn't go to college.
- `median_income_some_college` -- median income for residents who went to college but didn't graduate.
- `median_income_college` -- median income for college graduates.
- `median_income_graduate_degree` -- median income for those with a masters or other graduate degree.

The figures are the estimated values for 2013, from the [American Community Survey](http://www.census.gov/acs/www/)(ACS). The ACS is conducted yearly by the US Census Bureau, and gives a picture of demographic changes between the larger census, which the US does every 10 years.

### Instructions

- Find the county with the lowest median income in the US (`median_income`). Assign the name of the county (`county`) to `lowest_income_county`.
- Find the county that has more than `500000` residents with the lowest median income. Assign the name of the county to `lowest_income_high_pop_county`.

```
# The first 5 rows of the data.
print(income.head())
lowest_income_county = income["county"][income["median_income"].idxmin()]

high_pop = income[income["pop_over_25"] > 500000]
lowest_income_high_pop_county = high_pop["county"][high_pop["median_income"].idxmin()]
```

## 2: Random Numbers

Sometimes, instead of looking at a whole dataset, you just want to take a sample of it. This usually happens when dealing with the whole set of data is impractical. This can be due to processing power and memory constraints -- it is sometimes much faster to analyze 1/10th of a dataset than the whole thing. It can also be due to cost or other complexities.

The ACS is an example of this. It would be costly and impractical to survey every household in the US every year. Instead, the ACS randomly samples all of the addresses in the US, and picks about 1 in every 40 to participate.

We'll get into how a sample and a full population compare shortly. For now, let's look at how we can generate a random sample.

The first step is to generate random numbers. We can use the `random` package in Python to do this for us.

### Instructions

- Set a random seed of `20` and generate a list of `10` random numbers between the values `0` and `10`.
- Assign the list to `new_sequence`.

```
import random

# Returns a random integer between the numbers 0 and 10, inclusive.
num = random.randint(0, 10)

# Generate a sequence of 10 random numbers between the values of 0 and 10.
random_sequence = [random.randint(0, 10) for _ in range(10)]

# Sometimes, when we generate a random sequence, we want it to be the same sequence whenever the program is run.
# An example is when you use random numbers to select a subset of the data, and you want other people
# looking at the same data to get the same subset.
# We can ensure this by setting a random seed.
# A random seed is an integer that is used to "seed" a random number generator.
# After a random seed is set, the numbers generated after will follow the same sequence.
random.seed(10)
print([random.randint(0,10) for _ in range(5)])
random.seed(10)
# Same sequence as above.
print([random.randint(0,10) for _ in range(5)])
random.seed(11)
# Different seed means different sequence.
print([random.randint(0,10) for _ in range(5)])
random.seed(20)
new_sequence = [random.randint(0, 10) for _ in range(10)]
```

## 3: Selecting Items From A List

When we do sampling, we usually want to select a certain number of items from a list. There are a few ways to do this.

The easiest way is to use the `random.sample` method to select a specified number of items from a list.

## 4: Population Vs Sample

Let's say that you have a normal, six-sided die. You roll it four times, and you get `1, 1, 3, 4`. Based on this, could you conclude that there is a 50% probability of rolling a 1, a 25% probability of rolling a 3, and a 25% probability of rolling a 4?

No, as it's pretty easy to see that a die has a 1/6 chance of rolling any single digit. This means that the probabilities we observe are not necessarily the true probabilities of an event occurring.

As you may be able to guess, the larger the sample size (in this case, the more rolls we perform), the closer to the "true" probabilities we get. Let's explore this more.

### Instructions

- Set the random seed to `1`, then generate a medium sample of `100` die rolls. Plot the result using a histogram with 6 bins.
- Set the random seed to `1`, then generate a large sample of `10000` die rolls. Plot the result using a histogram with 6 bins.

```
import matplotlib.pyplot as plt

# A function that returns the result of a die roll.
def roll():
    return random.randint(1, 6)

random.seed(1)
small_sample = [roll() for _ in range(10)]

# Plot a histogram with 6 bins (1 for each possible outcome of the die roll)
plt.hist(small_sample, 6)
plt.show()
random.seed(1)
medium_sample = [roll() for _ in range(100)]

plt.hist(medium_sample, 6)
plt.show()

random.seed(1)
large_sample = [roll() for _ in range(10000)]

plt.hist(large_sample, 6)
plt.show()
```

## 5: Finding The Right Sample Size

As you can see from the graphs above, the probability of rolling a 1 should be around `.166`. However, we only really noticed the probability reaching this value once we got to `10000` dice rolls. Generally, the lower your sample size, the more variability the probability will have around the "true" probability.

We can graph out this variability by repeatedly rolling the die N times. So we could do 20 trials of rolling the die 10 times, and graph out all the resulting probabilities of rolling a 1. This would tell us how much error we could expect by rolling the die 20 times.

### Instructions

- Set the random seed to `1`, then generate probabilities for `300` trials of `100` die rolls each. Make a histogram with `20`bins.
- Set the random seed to `1`, then generate probabilities for `300` trials of `1000` die rolls each. Make a histogram with `20` bins.

```
def probability_of_one(num_trials, num_rolls):
    """
    This function will take in the number of trials, and the number of rolls per trial.
    Then it will conduct each trial, and record the probability of rolling a one.
    """
    probabilities = []
    for i in range(num_trials):
        die_rolls = [roll() for _ in range(num_rolls)]
        one_prob = len([d for d in die_rolls if d==1]) / num_rolls
        probabilities.append(one_prob)
    return probabilities

random.seed(1)
small_sample = probability_of_one(300, 50)
plt.hist(small_sample, 20)
plt.ylim(0,70)
plt.xlim(0,0.4)
plt.show()
random.seed(1)
medium_sample = probability_of_one(300, 100)
plt.hist(medium_sample, 20)
plt.ylim(0,70)
plt.xlim(0,0.4)
plt.show()

random.seed(1)
large_sample = probability_of_one(300, 1000)
plt.hist(large_sample, 20)
plt.ylim(0,70)
plt.xlim(0,0.4)
plt.show()
```

## 6: What Are The Odds?

See how the graphs in the last screen got "steeper" as we added more rolls? This is because the variability around the mean decreases as we have more samples in each trial.

One interesting thing that we can do given the distributions above is find the odds of getting a certain probability for rolling a one given the number of rolls we make.

So, if we do 100 rolls of the die, and get a `.25` probability of rolling a `1`, we could look up how many trials in our data above got that probability or higher for one.

This lets us find how likely our result is.

### Instructions

- Find how many standard deviations away from the mean of `large_sample` `.18` is. Assign the result to `deviations_from_mean`.
- Find how many probabilities in large sample are greater than or equal to `.18`. Assign the result to `over_18_count`.

```
import numpy

large_sample_std = numpy.std(large_sample)
avg = numpy.mean(large_sample)
deviations_from_mean = (.18 - avg) / large_sample_std

over_18_count = len([p for p in large_sample if p >= .18])
```

## 7: Sampling Counties

Now, let's look at why random sampling is important instead of just picking any rows we want.

### Instructions

- Use the `select_random_sample` function to pick `1000`random samples of `100` counties each from the `income`data. Find the mean of the `median_income` column for each sample.
- Plot a histogram with `20` bins of all the mean median incomes.

```
# This is the mean median income in any US county.
mean_median_income = income["median_income"].mean()
print(mean_median_income)

def get_sample_mean(start, end):
    return income["median_income"][start:end].mean()

def find_mean_incomes(row_step):
    mean_median_sample_incomes = []
    # Iterate over the indices of the income rows
    # Starting at 0, and counting in blocks of row_step (0, row_step, row_step * 2, etc).
    for i in range(0, income.shape[0], row_step):
        # Find the mean median for the row_step counties from i to i+row_step.
        mean_median_sample_incomes.append(get_sample_mean(i, i+row_step))
    return mean_median_sample_incomes

nonrandom_sample = find_mean_incomes(100)
plt.hist(nonrandom_sample, 20)
plt.show()

# What you're seeing above is the result of biased sampling.
# Instead of selecting randomly, we selected counties that were next to each other in the data.
# This picked counties in the same state more often that not, and created means that didn't represent the whole country.
# This is the danger of not using random sampling -- you end up with samples that don't reflect the entire population.
# This gives you a distribution that isn't normal.

import random
def select_random_sample(count):
    random_indices = random.sample(range(0, income.shape[0]), count)
    return income.iloc[random_indices]

random.seed(1)
random_sample = [select_random_sample(100)["median_income"].mean() for _ in range(1000)]
plt.hist(random_sample, 20)
plt.show()
```

## 8: An Experiment

Let's say we're the US government (oh no!). We want to run an experiment to see whether a certain kind of adult education can help high school graduates earn more relative to college graduates than they could otherwise.

We decide to trial our program in `100`counties, and measure the median incomes of both groups in `5` years.

At the end of `5` years, we first need to measure the whole population to determine the typical ratio between high school graduate earnings and college graduate earnings.

### Instructions

- Select `1000` random samples of `100` counties each from the `income` data using the `select_random_sample`method.
- For each sample:
  - Divide the `median_income_hs` column by `median_income_college` to get ratios.
  - Then, find the mean of all the ratios in the sample.
  - Add it to the list, `mean_ratios`.
- Plot a histogram containing `20` bins of the `mean_ratios`list.

```
def select_random_sample(count):
    random_indices = random.sample(range(0, income.shape[0]), count)
    return income.iloc[random_indices]

random.seed(1)
mean_ratios = []
for i in range(1000):
    sample = select_random_sample(100)
    ratios = sample["median_income_hs"] / sample["median_income_college"]
    mean_ratios.append(ratios.mean())
    
plt.hist(mean_ratios, 20)
plt.show()
```

## 9: Statistical Significance

After 5 years, we determine that the mean ratio in our random sample of 100 counties is .675 -- that is, high school graduates on average earn 67.5% of what college graduates do.

Now that we have our result, how do we know if our hypothesis is correct? Remember, our hypothesis was about the whole population, not about the sample.

Statistical significance is used to determine if a result is valid for a population or not. You usually set a significance level beforehand that will determine if your hypothesis is true or not. After conducting the experiment, you check against the significance level to determine.

A common significance level is `.05`. This means: "only 5% or less of the time will the result have been due to chance".

In our case, chance could be that the high school graduates in the county changed income some way other than through our program -- maybe some higher paying factory jobs came to town, or there were some other educational initiatives around.

In order to test for significance, we compare our result ratio with the mean ratios we found in the last section.

### Instructions

- Determine how many values in `mean_ratios` are greater than or equal to `.675`.
- Divide by the total number of items in `mean_ratios` to get the significance level.
- Assign the result to `significance_value`.

```
significance_value = None
mean_higher = len([m for m in mean_ratios if m >= .675])
significance_value = mean_higher / len(mean_ratios)
```

## 10: Final Result

Our significance value was `.014`. Based on the entire population, only 1.4% of the time will the wage results we saw have occurred on their own. So our experiment exceeded our significance level (lower means more significant). Thus, our experiment showed that the program did improve the wages of high school graduates relative to college graduates.

You may have noticed earlier that the more samples in our trials, the "steeper" the histograms of outcomes get (look back on the probability of rolling one with the die if you need a refresher). This "steepness" arose because the more trials we have, the less likely the value is to vary from the "true" value.

This same principle applies to significance testing. You need a larger deviation from the mean to have something be "significant" if your sample size is smaller. The larger the trial, the smaller the deviation needs to be to get a significant result.

You may be asking at this point how we can determine statistical significance without knowing the population values upfront. In a lot of cases, like drug trials, you don't have the capability to measure everyone in the world to compare against your sample.

Statistics gives us tools to deal with this, and we'll learn about them in the next missions.

# Guided Project: Analyzing Movie Reviews

## 1: Movie Reviews

In this project, you'll be working with Jupyter notebook, and analyzing data on movie review scores. By the end, you'll have a notebook that you can add to your portfolio or build on top of on your own. If you need help at any point, you can consult our solution notebook [here](https://github.com/dataquestio/solutions/blob/master/Mission209Solution.ipynb).

The dataset is stored in the `fandango_score_comparison.csv` file. It contains information on how major movie review services rated movies. The data originally came from [FiveThirtyEight](http://fivethirtyeight.com/features/fandango-movies-ratings/).

Here are the first few rows of the data, in CSV format:

|      | FILM                           | RottenTomatoes | Metacritic | IMDB | Fandango_Stars | RT_norm | RT_user_norm | Fandango_votes |
| ---- | ------------------------------ | -------------- | ---------- | ---- | -------------- | ------- | ------------ | -------------- |
| 0    | Avengers: Age of Ultron (2015) | 74             | 86         | 7.1  | 7.8            | 4.5     | 3.70         | 271107         |
| 1    | Cinderella (2015)              | 85             | 80         | 7.5  | 7.1            | 4.5     | 4.25         | 65709          |
| 2    | Ant-Man (2015)                 | 80             | 90         | 8.1  | 7.8            | 4.5     | 4.00         | 103660         |
| 3    | Do You Believe? (2015)         | 18             | 84         | 4.7  | 5.4            | 4.5     | 0.90         | 3136           |
| 4    | Hot Tub Time Machine 2 (2015)  | 14             | 28         | 3.4  | 5.1            | 3.0     | 0.70         | 19560          |

Each row represents a single movie. Each column contains information about how the online moview review services [RottenTomatoes](http://rottentomatoes.com/), [Metacritic](http://metacritic.com/), [IMDB](http://www.imdb.com/), and [Fandango](http://www.fandango.com/) rated the movie. The dataset was put together to help detect bias in the movie review sites. Each of these sites has 2 types of score -- `User` scores, which aggregate user reviews, and `Critic`score, which aggregate professional critical reviews of the movie. Each service puts their ratings on a different scale:

- RottenTomatoes - `0-100`, in increments of `1`.
- Metacritic - `0-100`, in increments of `1`.
- IMDB - `0-10`, in increments of `.1`.
- Fandango - `0-5`, in increments of `.5`.

Typically, the primary score shown by the sites will be the `Critic` score. Here are descriptions of some of the relevant columns in the dataset:

- `FILM` -- the name of the movie.
- `RottenTomatoes` -- the RottenTomatoes (RT) critic score.
- `RottenTomatoes_User` -- the RT user score.
- `Metacritic` -- the Metacritic critic score.
- `Metacritic_User` -- the Metacritic user score.
- `IMDB` -- the IMDB score given to the movie.
- `Fandango_Stars` -- the number of stars Fandango gave the movie.

To make it easier to compare scores across services, the columns were normalized so their scale and rounding matched the Fandango ratings. Any column with the suffix `_norm` is the corresponding column changed to a `0-5` scale. For example, `RT_norm` takes the `RottenTomatoes`column and turns it into a `0-5` scale from a `0-100` scale. Any column with the suffix `_round` is the rounded version of another column. For example, `RT_user_norm_round` rounds the `RT_user_norm` column to the nearest `.5`.

### Instructions

- Read `fandango_score_comparison.csv` into a Dataframe named `movies`.
- Display the `movies` DataFrame by just typing the variable name and running the code cell.
- If you're unfamiliar with [RottenTomatoes](http://rottentomatoes.com/), [Metacritic](http://metacritic.com/), [IMDB](http://www.imdb.com/), or [Fandango](http://www.fandango.com/), visit the websites to get a better handle on their review methodology.

## 2: Histograms

Now that you've read the dataset in, you can do some statistical exploration of the ratings columns. We'll primarily focus on the `Metacritic_norm_round` and the `Fandango_Stars` columns, which will let you see how Fandango and Metacritic differ in terms of review scores.

### Instructions

- Enable plotting in Jupyter notebook with `import matplotlib.pyplot as plt` and run the following magic `%matplotlib inline`.
- Create a histogram of the `Metacritic_norm_round`column.
- Create a histogram of the `Fandango_Stars` column.
- Look critically at both histograms, and write up any differences you see in a markdown cell.

## 3: Mean, Median, And Standard Deviation

In the last screen, you may have noticed some differences between the Fandango and Metacritic scores. Metrics we've covered, including the mean, median, and standard deviation, allow you to quantify these differences. You can apply these metrics to the `Fandango_Stars` and `Metacritic_norm_round` columns to figure out how different they are.

### Instructions

- Calculate the mean of both `Fandango_Stars` and `Metacritic_norm_round`.
- Calculate the median of both `Fandango_Stars` and `Metacritic_norm_round`.
- Calculate the standard deviation of both `Fandango_Stars`and `Metacritic_norm_round`. You can use the [numpy.std](http://docs.scipy.org/doc/numpy/reference/generated/numpy.std) method to find this.
- Print out all the values and look over them.
- Look at the review methodologies for [Metacritic](http://metacritic.com/) and [Fandango](http://www.fandango.com/). You can find the methodologies on their websites, or by using [Google](http://www.google.com/). Do you see any major differences? Write them up in a markdown cell.
- Write up the differences in numbers in a markdown cell, including the following:
  - Why would the median for `Metacritic_norm_round`be lower than the mean, but the median for `Fandango_Stars` is higher than the mean? Recall that the mean is usually larger than the median when there are a few large values in the data, and lower when there are a few small values.
  - Why would the standard deviation for `Fandango_Stars` be much lower than the standard deviation for `Metacritic_norm_round`?
  - Why would the mean for `Fandango_Stars` be much higher than the mean for `Metacritic_norm_round`.

## 4: Scatter Plots

We know the ratings tend to differ, but we don't know which movies tend to be the largest outliers. You can find this by making a scatterplot, then looking at which movies are far away from the others.

You can also subtract the `Fandango_Stars`column from the `Metacritic_norm_round`column, take the absolute value, and sort `movies` based on the difference to find the movies with the largest differences between their Metacritic and Fandango ratings.

### Instructions

- Make a scatterplot that compares the `Fandango_Stars`column to the `Metacritic_norm_round` column.
- Several movies appear to have low ratings in Metacritic and high ratings in Fandango, or vice versa. We can explore this further by finding the differences between the columns.
  - Subtract the `Fandango_Stars` column from the `Metacritic_norm_round` column, and assign to a new column, `fm_diff`, in `movies`.
  - Assign the absolute value of `fm_diff` to `fm_diff`. This will ensure that we don't only look at cases where `Metacritic_norm_round` is greater than `Fandango_Stars`.You can calculate absolute values with the [absolute](http://docs.scipy.org/doc/numpy-1.10.0/reference/generated/numpy.absolute.html) function in NumPy.
  - Sort `movies` based on the `fm_diff` column, in descending order.
  - Print out the top `5` movies with the biggest differences between `Fandango_Stars` and `Metacritic_norm_round`.

## 5: Correlations

Let's see what the correlation coefficient between `Fandango_Stars` and `Metacritic_norm_round` is. This will help you determine if Fandango consistently has higher scores than Metacritic, or if only a few movies were assigned higher ratings.

You can then create a linear regression to see what the predicted Fandango score would be based on the Metacritic score.

### Instructions

- Calculate the r-value measuring the correlation between `Fandango_Stars` and `Metacritic_norm_round` using the [scipy.stats.pearsonr](http://docs.scipy.org/doc/scipy-0.16.0/reference/generated/scipy.stats.pearsonr.html) function.
- The correlation is actually fairly low. Write up a markdown cell that discusses what this might mean.
- Use the [scipy.stats.linregress](http://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.linregress.html) function create a linear regression with `Metacritic_norm_round` as the x-values and `Fandango_Stars` as the y-values.
- Predict what a movie that got a `3.0` in Metacritic would get on Fandango using the formula `pred_3 = 3 * slope + intercept`.

## 6: Finding Residuals

In the last screen, you created a linear regression for relating `Metacritic_norm_round` to `Fandango_Stars`. You can create a residual plot to better visualize how the line relates to the existing datapoints. This can help you see if two variables are linearly related or not.

### Instructions

- Predict what a movie that got a `1.0` in Metacritic would get on Fandango using the line from the last screen.
- Predict what a movie that got a `5.0` in Metacritic would get on Fandango using the line from the last screen.
- Make a scatter plot using the [scatter](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.scatter) function in `matplotlib.pyplot`.
- On top of the scatter plot, use the [plot](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.plot) function in `matplotlib.pyplot` to plot a line using the predicted values for `1.0` and `5.0`.Setup the `x` values as the list `[1.0, 5.0]`.The `y` values should be a list with the corresponding predictions.Pass in both `x` and `y` to [plot](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.plot) to create a line.
- Set the x-limits of the plot to `1` and `5` using the [pyplot.xlim()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.xlim) method.
- Show the plot.

## 7: Next Steps

That's it for the guided steps! We recommend exploring the data more on your own.

Here are some potential next steps:

- Explore the other rating services, IMDB and RottenTomatoes.
  - See how they differ from each other.
  - See how they differ from Fandango.
- See how user scores differ from critic scores.
- Acquire more recent review data, and see if the pattern of Fandango inflating reviews persists.
- Dig more into why certain movies had their scores inflated more than others.

We recommend creating a [Github](https://github.com/) repository and placing this project there. It will help other people, including employers, see your work. As you start to put multiple projects on Github, you'll have the beginnings of a strong portfolio.

You're welcome to keep working on the project here, but we recommend downloading it to your computer using the download icon above and working on it there.

We hope this guided project has been a good experience, and please email us at hello@dataquest.io if you want to share your work!