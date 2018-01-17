# Improving Plot Aesthetics

## 1: Aesthetics

In the **Exploratory Data Visualization** course, we learned how to use visualizations to explore and understand data. Because we were focused on exploring trends and getting familiar with the data, we didn't focus much on tweaking the appearance of the plots to make them more presentable to others. We instead focused on the workflow of quickly creating, tweaking, displaying, and iterating on plots.

In this course, we'll focus on how to use data visualization to communicate insights and tell stories. In this mission, we'll start with a standard matplotlib plot and improve its appearance to better communicate the patterns we want a viewer to understand. Along the way, we'll introduce the principles that informed those changes and provide a framework for you to apply them in the future. Here's a preview that demonstrates some of the improvements we make in this course:

![Storytelling Course Overview](https://s3.amazonaws.com/dq-content/storytelling_course_overview.png)

## 2: Introduction To The Data

[The Department of Education Statistics](http://nces.ed.gov/programs/digest/2013menu_tables.asp) releases a data set annually containing the percentage of bachelor's degrees granted to women from 1970 to 2012. The data set is broken up into 17 categories of degrees, with each column as a separate category.

Randal Olson, a data scientist at University of Pennsylvania, has cleaned the data set and made it available on his personal website. You can download the dataset Randal compiled [here](http://www.randalolson.com/wp-content/uploads/percent-bachelors-degrees-women-usa.csv). Here's a preview of the first few rows:

| Year | Agriculture | Architecture | Art and Performance | Biology   | Business  | Communications and Journalism | Computer Science | Education | Engineering | English   | Foreign Languages | Health Professions | Math and Statistics | Physical Sciences | Psychology | Public Administration | Social Sciences and History |
| ---- | ----------- | ------------ | ------------------- | --------- | --------- | ----------------------------- | ---------------- | --------- | ----------- | --------- | ----------------- | ------------------ | ------------------- | ----------------- | ---------- | --------------------- | --------------------------- |
| 1970 | 4.229798    | 11.921005    | 59.7                | 29.088363 | 9.064439  | 35.3                          | 13.6             | 74.535328 | 0.8         | 65.570923 | 73.8              | 77.1               | 38.0                | 13.8              | 44.4       | 68.4                  | 36.8                        |
| 1971 | 5.452797    | 12.003106    | 59.9                | 29.394403 | 9.503187  | 35.5                          | 13.6             | 74.149204 | 1.0         | 64.556485 | 73.9              | 75.5               | 39.0                | 14.9              | 46.2       | 65.5                  | 36.2                        |
| 1972 | 7.420710    | 13.214594    | 60.4                | 29.810221 | 10.558962 | 36.6                          | 14.9             | 73.554520 | 1.2         | 63.664263 | 74.6              | 76.9               | 40.2                | 14.8              | 47.6       | 62.6                  | 36.1                        |

Randal compiled this data set to explore the gender gap in STEM fields, which stands for science, technology, engineering, and mathematics. This gap is reported on often [in the news](https://www.google.com/search?hl=en&gl=us&tbm=nws&authuser=0&q=gender+gap+stem&oq=gender+gap+stem&gs_l=news) and [not everyone agrees](http://www.pbs.org/newshour/making-sense/truth-women-stem-careers/) that there is a gap.

In this mission and the next few missions, we'll explore how we can communicate the nuanced narrative of gender gap using effective data visualization. Let's first generate a standard matplotlib plot.

## 3: Introduction To The Data

### Instructions

- Generate a line chart that visualizes the historical percentage of Biology degrees awarded to women:
  - Set the x-axis to the `Year` column from `women_degrees`.
  - Set the y-axis to the `Biology` column from `women_degrees`.
- Display the plot.

```
import pandas as pd
import matplotlib.pyplot as plt

women_degrees = pd.read_csv('percent-bachelors-degrees-women-usa.csv')
plt.plot(women_degrees['Year'], women_degrees['Biology'])
plt.show()
```

## 4: Visualizing The Gender Gap

From the plot, we can tell that Biology degrees increased steadily from 1970 and peaked in the early 2000's. We can also tell that the percentage has stayed above 50% since around 1987. While it's helpful to visualize the trend of Biology degrees awarded to women, it only tells half the story. If we want the gender gap to be apparent and emphasized in the plot, we need a visual analogy to the difference in the percentages between the genders.

If we visualize the trend of Biology degrees awarded to men on the same plot, a viewer can observe the space between the lines for each gender. We can calculate the percentages of Biology degrees awarded to men by subtracting each value in the `Biology` column by `100`. Once we have the male percentages, we can generate two line charts as part of the same diagram.

Let's now create a diagram containing both the line charts we just described.

### Instructions

- Generate 2 line charts on the same figure:
  - One that visualizes the percentages of Biology degrees awarded to women over time. Set the line color to `"blue"` and the label to `"Women"`.
  - One that visualizes the percentages of Biology degrees awarded to men over time. Set the line color to `"green"` and the label to `"Men"`.
- Set the title of the chart to `"Percentage of Biology Degrees Awarded By Gender"`.
- Generate a legend and place it in the `"upper right"`location.
- Display the chart.

```
plt.plot(women_degrees['Year'], women_degrees['Biology'], c='blue', label='Women')
plt.plot(women_degrees['Year'], 100-women_degrees['Biology'], c='green', label='Men')
plt.legend(loc='upper right')
plt.title('Percentage of Biology Degrees Awarded By Gender')
plt.show()
```

## 5: Data-Ink Ratio

The chart containing both line charts tells a more complete story than the one containing just the line chart that visualized just the women percentages. This plot instead tells the story of two distinct periods. In the first period, from 1970 to around 1987, women were a minority when it came to majoring in Biology while in the second period, from around 1987 to around 2012, women became a majority. You can see the point where women overtook men where the lines intersect. While a viewer could have reached the same conclusions using the individual line chart of just the women percentages, it would have required more effort and mental processing on their part.

Although our plot is better, it still contains some extra visual elements that aren't necessary to understand the data. We're interested in helping people understand the gender gap in different fields across time. These excess elements, sometimes known as [**chartjunk**](https://en.wikipedia.org/wiki/Chartjunk), increase as we add more plots for visualizing the other degrees, making it harder for anyone trying to interpret our charts. In general, we want to maximize the [**data-ink ratio**](http://www.infovis-wiki.net/index.php/Data-Ink_Ratio), which is the fractional amount of the plotting area dedicated to displaying the data.

The following is an animated GIF by [Darkhorse Analytics](http://blog.darkhorseanalytics.com/data-looks-better-naked) that shows a series of tweaks for boosting the data-ink ratio:

![Darkhorse Analytics GIF](http://cdn2.hubspot.net/hubfs/2020805/Imported_Blog_Media/data-ink.gif?t=1477583494534)

Non-data ink includes any elements in the chart that don't directly display data points. This includes tick markers, tick labels, and legends. Data ink includes any elements that display and depend on the data points underlying the chart. In a line chart, data ink would primarily be the lines and in a scatter plot, the data ink would primarily be in the markers. As we increase the data-ink ratio, we decrease non-data ink that can help a viewer understand certain aspects of the plots. We need to be mindful of this trade-off as we work on tweaking the appearance of plots to tell a story, because plots we create could end up telling the *wrong* story.

This principle was originally set forth by [Edward Tufte](https://en.wikipedia.org/wiki/Edward_Tufte), a pioneer of the field of data visualization. Tufte's first book, [*The Visual Display of Quantitative Information*](https://www.edwardtufte.com/tufte/books_vdqi), is considered a bible among information designers. We cover some of the ideas presented in the book in this course, but we recommend going through the entire book for more depth.

To improve the data-ink ratio, let's make the following changes to the plot we created in the last step:

- Remove all of the axis tick marks.
- Hide the spines, which are the lines that connects the tick marks, on each axis.

## 6: Hiding Tick Marks

To customize the appearance of the ticks, we use the [Axes.tick_params()](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.tick_params) method. Using this method, we can modify which tick marks and tick labels are displayed. By default, matplotlib displays the tick marks on all four sides of the plot. Here are the four sides for a standard line chart:

- The left side is the y-axis.
- The bottom side is the x-axis.
- The top side is across from the x-axis.
- The right side is across from the y-axis.

The parameters for enabling or disabling tick marks are conveniently named after the sides. To hide all of them, we need to pass in the following values for each parameter when we call `Axes.tick_params()`:

- `bottom`: `"off"`
- `top`: `"off"`
- `left`: `"off"`
- `right`: `"off"`

### Instructions

- Generate 2 line chart in the same plotting area:
  - One that visualizes the percentages of Biology degrees awarded to women over time. Set the line color to `"blue"` and the label to `"Women"`.
  - One that visualizes the percentages of Biology degrees awarded to men over time. Set the line color to `"green"` and the label to `"Men"`.
- Remove all of the tick marks.
- Set the title of the plot to `"Percentage of Biology Degrees Awarded By Gender"`.
- Generate a legend and place it in the `"upper right"`location.
- Display the chart.

```
fig, ax = plt.subplots()
ax.plot(women_degrees['Year'], women_degrees['Biology'], label='Women')
ax.plot(women_degrees['Year'], 100-women_degrees['Biology'], label='Men')

ax.tick_params(bottom="off", top="off", left="off", right="off")
ax.set_title('Percentage of Biology Degrees Awarded By Gender')
ax.legend(loc="upper right")

plt.show()
```

## 7: Hiding Spines

With the axis tick marks gone, the data-ink ratio is improved and the chart looks much cleaner. In addition, the spines in the chart now are no longer necessary. When we're exploring data, the spines and the ticks complement each other to help us refer back to specific data points or ranges. When a viewer is viewing our chart and trying to understand the insight we're presenting, the ticks and spines can get in the way. As we mentioned earlier, chartjunk becomes much more noticeable when you have multiple plots in the same chart. By keeping the axis tick labels but not the spines or tick marks, we strike an appropriate balance between hiding chartjunk and making the data visible.

In matplotlib, the spines are represented using the [matplotlib.spines.Spine](http://matplotlib.org/api/spines_api.html) class. When we create an Axes instance, four Spine objects are created for us. If you run `print(ax.spines)`, you'll get back a dictionary of the Spine objects:


```
{'right': <matplotlib.spines.spine object="" at="" 0x111089c18="">, 'bottom': <matplotlib.spines.spine object="" at="" 0x111060898="">, 'top': <matplotlib.spines.spine object="" at="" 0x1110606a0="">, 'left': <matplotlib.spines.spine object="" at="" 0x11107cd30="">}
</matplotlib.spines.spine></matplotlib.spines.spine></matplotlib.spines.spine></matplotlib.spines.spine>
```

To hide all of the spines, we need to:

- access each Spine object in the dictionary
- call the `Spine.set_visible()` method
- pass in the Boolean value `False`

The following line of code removes the spines for the right axis:


```
ax.spines["right"].set_visible(False)
```

### Instructions

- Generate 2 line charts on the same plotting area:
  - One that visualizes the percentages of Biology degrees awarded to women over time. Set the line color to `"blue"` and the label to `"Women"`.
  - One that visualizes the percentages of Biology degrees awarded to men over time. Set the line color to `"green"` and the label to `"Men"`.
- Remove all of the axis tick marks.
- Hide all of the spines.
- Set the title of the plot to `"Percentage of Biology Degrees Awarded By Gender"`.
- Display the chart.

```
fig, ax = plt.subplots()
ax.plot(women_degrees['Year'], women_degrees['Biology'], label='Women')
ax.plot(women_degrees['Year'], 100-women_degrees['Biology'], label='Men')
ax.tick_params(bottom="off", top="off", left="off", right="off")
# Add your code here

ax.legend(loc='upper right')
ax.set_title('Percentage of Biology Degrees Awarded By Gender')
plt.show()
fig, ax = plt.subplots()
ax.plot(women_degrees['Year'], women_degrees['Biology'], c='blue', label='Women')
ax.plot(women_degrees['Year'], 100-women_degrees['Biology'], c='green', label='Men')
ax.tick_params(bottom="off", top="off", left="off", right="off")
# Start solution code.
for key,spine in ax.spines.items():
    spine.set_visible(False)
# End solution code.
ax.legend(loc='upper right')
plt.show()
```

## 8: Comparing Gender Gap Across Degree Categories

So far, matplotlib has set the limits automatically for each axis and this hasn't had any negative effect on communicating our story with data. If we want to generate charts to compare multiple degree categories, the axis ranges need to be consistent. Inconsistent data ranges can distort the story our charts are telling and fool the viewer.

Edward Tufte often preaches that a good chart encourages comparison over just description. A good chart uses a consistent style for the elements that aren't directly conveying the data points. These elements art part of the non-data ink in the chart. By keeping the non-data ink as consistent as possible across multiple plots, differences in those elements stick out easily to the viewer. This is because our visual processing systems are excellent at discerning differences quickly and brings them to the front of our thought process. The similarities naturally fade to the back of our thought process.

Let's generate line charts for four STEM degree categories on a grid to encourage comparison. Our instructions for generating the chart are cumbersome. Here's what the final chart looks like, so you can refer to it as you write your code:

![Four Major Categories](https://s3.amazonaws.com/dq-content/four_major_categories_plots.png)

We've also added some starter code to help you generate the chart. This code uses a for loop to generate the line charts for each subplot in the chart. We can save space this way and easily tweak the code to generate other variations of the chart.

### Instructions

- Generate a line chart using the women and men percentages for `Biology` in the top left subplot.
- Generate a line chart using the women and men percentages for `Computer Science` in the top right subplot.
- Generate a line chart using the women and men percentages for `Engineering` in the bottom left subplot.
- Generate a line chart using the women and men percentages for `Math and Statistics` in the bottom right subplot.
- For all subplots:
  - For the line chart visualizing female percentages, set the line color to `"blue"` and the label to `"Women"`.
  - For the line chart visualizing male percentages, set the line color to `"green"` and the label to `"Men"`.
  - Set the x-axis limit to range from `1968` to `2011`.
  - Set the y-axis limit to range from `0` to `100`.
  - Hide all of the spines and tick marks.
  - Set the title of each subplot to the name of the major category (e.g. `"Biology"`, `"Computer Science"`).
- Place a legend in the `upper right` corner of the bottom right subplot.
- Display the plot.

```
major_cats = ['Biology', 'Computer Science', 'Engineering', 'Math and Statistics']
fig = plt.figure(figsize=(12, 12))

for sp in range(0,4):
    ax = fig.add_subplot(2,2,sp+1)
    ax.plot(women_degrees['Year'], women_degrees[major_cats[sp]], c='blue', label='Women')
    ax.plot(women_degrees['Year'], 100-women_degrees[major_cats[sp]], c='green', label='Men')
    # Add your code here.

# Calling pyplot.legend() here will add the legend to the last subplot that was created.
plt.legend(loc='upper right')
plt.show()
major_cats = ['Biology', 'Computer Science', 'Engineering', 'Math and Statistics']
fig = plt.figure(figsize=(12, 12))

for sp in range(0,4):
    ax = fig.add_subplot(2,2,sp+1)
    ax.plot(women_degrees['Year'], women_degrees[major_cats[sp]], c='blue', label='Women')
    ax.plot(women_degrees['Year'], 100-women_degrees[major_cats[sp]], c='green', label='Men')
    for key,spine in ax.spines.items():
        spine.set_visible(False)
    ax.set_xlim(1968, 2011)
    ax.set_ylim(0,100)
    ax.set_title(major_cats[sp])
    ax.tick_params(bottom="off", top="off", left="off", right="off")

# Calling pyplot.legend() here will add the legend to the last subplot that was created.
plt.legend(loc='upper right')
plt.show()
```

## 9: Next Steps

By spending just a few seconds reading the chart, we can conclude that the gender gap in Computer Science and Engineering have big gender gaps while the gap in Biology and Math and Statistics is quite small. In addition, the first two degree categories are dominated by men while the latter degree categories are much more balanced. This chart can still be improved, however, and we'll explore more techniques in the next mission.

In this mission, we explored how to enhance a chart's storytelling capabilities by minimizing chartjunk and encouraging comparison. In the next mission, we'll explore how to use color, spacing, and weights to further enhance the storytelling capability of the plots.

# Color, Layout, and Annotations

## 1: Introduction

In the previous mission, we learned some basic techniques and principles for making our plots more aesthetic. In this mission, we'll focus more directly on customizing colors, line widths, layout, and annotations to improve the ability for a viewer to extract insights from the charts. We'll continue to use the same data set containing the percentage of bachelor's degrees granted to women from 1970 to 2012

We've gone ahead and read the data set into a DataFrame named `women_degrees`. We've also brought over the code we wrote at the end of the previous mission to generate line charts for four STEM degree categories. If it's been a while since you completed the last mission, spend some time getting familiar with the data set and the charts we generated.

## 2: Color

So far, we've been using the [default matplotlib colors](http://matplotlib.org/api/colors_api.html#module-matplotlib.colors) to color the lines in line charts. When selecting colors, we need to be mindful of people who have some amount of color blindness. People who have [color blindness](https://en.wikipedia.org/wiki/Color_blindness) have a decreased ability to distinguish between certain kinds of colors. The most common form of color blindness is red-green color blindness, where the person can't distinguish between red and green shades. Approximately 8% of men and 0.5% of women of Northern European descent suffer from red-green color blindness.

The [Ishihara test](https://en.wikipedia.org/wiki/Ishihara_test) is a well known test for color blindness, where the person is asked to identify the number in the following image:

![Ishihara color test plate](https://s3.amazonaws.com/dq-content/ishihara_test.png)

People with complete color vision can observe the number **74**. Some with partial color blindness see the number **21** instead and those with full color blindness can't see any number at all.

If we wanted to publish the data visualizations we create, we need to be mindful of color blindness. Thankfully, there are color palettes we can use that are friendly for people with color blindness. One of them is called **Color Blind 10** and was released by Tableau, the company that makes the data visualization platform of the same name. Navigate to [this page](http://tableaufriction.blogspot.ro/2012/11/finally-you-can-use-tableau-data-colors.html) and select just the **Color Blind 10** option from the list of palettes to see the ten colors included in the palette.

## 3: Setting Line Color Using RGB

The Color Blind 10 palette contains ten colors that are colorblind friendly. Let's use the first two colors in the palette for the line colors in our charts. You'll notice that next to each color strip are three integer values, separated by periods (`.`):

![Tableau RGB values](https://s3.amazonaws.com/dq-content/tableau_rgb_values.png)

These numbers represent the **RGB values** for each color. The [RGB color model](https://en.wikipedia.org/wiki/RGB_color_model) describes how the three primary colors (red, green, and blue) can be combined in different proportions to form any secondary color. The RGB color model is very familiar to people who work in photography, filmography, graphic design, and any field that use colors extensively. In computers, each RGB value can range between 0 and 255. This is because 256 integer values can be represented using 8 bits. You can read more about 8-bit color [here](https://en.wikipedia.org/wiki/8-bit_color).

The first color in the palette is a color that resembles dark blue and has the following RGB values:

- Red: `0`
- Green: `107`
- Blue: `164`

To specify a line color using RGB values, we pass in a tuple of the values to the `c`parameter when we generate the line chart. Matplotlib expects each value to be scaled down and to range between 0 and 1 (not 0 and 255). In the following code, we scale the first color, which resembles dark blue, in the Color Blind 10 palette and set it as the line color:


```
cb_dark_blue = (0/255,107/255,164/255)
ax.plot(women_degrees['Year'], women_degrees['Biology'], label='Women', c=cb_dark_blue)
```

### Instructions

- Modify the starter code to:
  - Set the line color for the line charts visualizing women percentages to the dark blue color from the Color Blind 10 palette (RGB value of (0, 107, 164)).
  - Set the line color for the line charts visualizing men percentages to the orange color from the Color Blind 10 palette (RGB value of (255, 128, 14)).
- Display the figure after you've made these changes.

```
fig = plt.figure(figsize=(12, 12))

for sp in range(0,4):
    ax = fig.add_subplot(2,2,sp+1)
    # The color for each line is assigned here.
    ax.plot(women_degrees['Year'], women_degrees[major_cats[sp]], c='blue', label='Women')
    ax.plot(women_degrees['Year'], 100-women_degrees[major_cats[sp]], c='green', label='Men')
    for key,spine in ax.spines.items():
        spine.set_visible(False)
    ax.set_xlim(1968, 2011)
    ax.set_ylim(0,100)
    ax.set_title(major_cats[sp])
    ax.tick_params(bottom="off", top="off", left="off", right="off")

plt.legend(loc='upper right')
plt.show()
cb_dark_blue = (0/255, 107/255, 164/255)
cb_orange = (255/255, 128/255, 14/255)

fig = plt.figure(figsize=(12, 12))

for sp in range(0,4):
    ax = fig.add_subplot(2,2,sp+1)
    # The color for each line is assigned here.
    ax.plot(women_degrees['Year'], women_degrees[major_cats[sp]], c=cb_dark_blue, label='Women')
    ax.plot(women_degrees['Year'], 100-women_degrees[major_cats[sp]], c=cb_orange, label='Men')
    for key,spine in ax.spines.items():
        spine.set_visible(False)
    ax.set_xlim(1968, 2011)
    ax.set_ylim(0,100)
    ax.set_title(major_cats[sp])
    ax.tick_params(bottom="off", top="off", left="off", right="off")

plt.legend(loc='upper right')
plt.show()
```

## 4: Setting Line Width

By default, the actual lines reflecting the underlying data in the line charts we've been generating are quite thin. The white color in the blank area in the line charts is still a dominating color. To emphasize the lines in the plots, we can increase the width of each line. Increasing the line width also improves the data-ink ratio a little bit, because more of the chart area is used to showcase the data.

When we call the `Axes.plot()` method, we can use the `linewidth` parameter to specify the line width. Matplotlib expects a float value for this parameter:


```
ax.plot(women_degrees['Year'], women_degrees['Biology'], label='Women', c=cb_dark_blue, linewidth=2)
```

The higher the line width, the thicker each line will be.

### Instructions

- Modify the starter code to set the line widths for both line charts to `3`.

```
cb_dark_blue = (0/255, 107/255, 164/255)
cb_orange = (255/255, 128/255, 14/255)

fig = plt.figure(figsize=(12, 12))

for sp in range(0,4):
    ax = fig.add_subplot(2,2,sp+1)
    # Set the line width when specifying how each line should look.
    ax.plot(women_degrees['Year'], women_degrees[major_cats[sp]], c=cb_dark_blue, label='Women')
    ax.plot(women_degrees['Year'], 100-women_degrees[major_cats[sp]], c=cb_orange, label='Men')
    for key,spine in ax.spines.items():
        spine.set_visible(False)
    ax.set_xlim(1968, 2011)
    ax.set_ylim(0,100)
    ax.set_title(major_cats[sp])
    ax.tick_params(bottom="off", top="off", left="off", right="off")

plt.legend(loc='upper right')
plt.show()
cb_dark_blue = (0/255, 107/255, 164/255)
cb_orange = (255/255, 128/255, 14/255)

fig = plt.figure(figsize=(12, 12))

for sp in range(0,4):
    ax = fig.add_subplot(2,2,sp+1)
    # Set the line width when specifying how each line should look.
    ax.plot(women_degrees['Year'], women_degrees[major_cats[sp]], c=cb_dark_blue, label='Women', linewidth=3)
    ax.plot(women_degrees['Year'], 100-women_degrees[major_cats[sp]], c=cb_orange, label='Men', linewidth=3)
    for key,spine in ax.spines.items():
        spine.set_visible(False)
    ax.set_xlim(1968, 2011)
    ax.set_ylim(0,100)
    ax.set_title(major_cats[sp])
    ax.tick_params(bottom="off", top="off", left="off", right="off")

plt.legend(loc='upper right')
plt.show()
```

## 5: Improve The Layout And Ordering

So far, we've been generating our line charts on a 2 by 2 subplot grid. If we wanted to visualize all six STEM degrees, we'd need to either add a new column or a new row. Unfortunately, neither solution orders the plots in a beneficial way to the viewer. By scanning horizontally or vertically, a viewer isn't able to learn any new information and this can cause some frustration as the viewer's gaze jumps around the image.

To make the viewing experience more coherent, we can:

- use layout of a single row with multiple columns
- order the plots in decreasing order of initial gender gap

Here's what that would look like:

![Line Charts Ordered By Decreasing Initial Gender Gap](https://s3.amazonaws.com/dq-content/line_charts_dec_initial_gg.png)

The leftmost plot has the largest gender gap in 1968 while the rightmost plot has the smallest gender gap in 1968. If we're instead interested in the recent gender gaps in STEM degrees, we can order the plots from largest to smallest ending gender gaps. Here's what that would look like:

![Line Charts Ordered By Decreasing Ending Gender Gap](https://s3.amazonaws.com/dq-content/line_charts_dec_ending_gg.png)

In this exercise, you'll order the charts by decreasing ending gender gap. We've populated the list `stem_cats` with the six STEM degree categories. In the next step, we'll explore how we can replace the legend, which is currently overlapping with the rightmost line chart.

### Instructions

- Modify the starter code to:
  - Change the width of the figure to a width of `18`inches and a height of `3` inches.
  - In the for loop, change the range to `(0,6)` instead of `(0,4)`.
  - Change the subplot layout from 2 rows by 2 columns to 1 row by 6 columns.
  - Use `stem_cats` instead of `major_cats` when generating and setting the titles for the line charts.

```
stem_cats = ['Engineering', 'Computer Science', 'Psychology', 'Biology', 'Physical Sciences', 'Math and Statistics']

fig = plt.figure(figsize=(12, 12))

for sp in range(0,4):
    ax = fig.add_subplot(2,2,sp+1)
    ax.plot(women_degrees['Year'], women_degrees[major_cats[sp]], c=cb_dark_blue, label='Women', linewidth=3)
    ax.plot(women_degrees['Year'], 100-women_degrees[major_cats[sp]], c=cb_orange, label='Men', linewidth=3)
    for key,spine in ax.spines.items():
        spine.set_visible(False)
    ax.set_xlim(1968, 2011)
    ax.set_ylim(0,100)
    ax.set_title(major_cats[sp])
    ax.tick_params(bottom="off", top="off", left="off", right="off")

plt.legend(loc='upper right')
plt.show()
fig = plt.figure(figsize=(18, 3))

for sp in range(0,6):
    ax = fig.add_subplot(1,6,sp+1)
    ax.plot(women_degrees['Year'], women_degrees[stem_cats[sp]], c=cb_dark_blue, label='Women', linewidth=3)
    ax.plot(women_degrees['Year'], 100-women_degrees[stem_cats[sp]], c=cb_orange, label='Men', linewidth=3)
    for key,spine in ax.spines.items():
        spine.set_visible(False)
    ax.set_xlim(1968, 2011)
    ax.set_ylim(0,100)
    ax.set_title(stem_cats[sp])
    ax.tick_params(bottom="off", top="off", left="off", right="off")

plt.legend(loc='upper right')
plt.show()
```

## 6: Replacing The Legend With Annotations

The purpose of a legend is to ascribe meaning to symbols or colors in a chart. We're using it to inform the viewer of what gender corresponds to each color. Tufte encourages removing legends entirely if the same information can be conveyed in a cleaner way. Legends consist of non-data ink and take up precious space that could be used for the visualizations themselves (data-ink).

Instead of trying to move the legend to a better location, we can replace it entirely by annotating the lines directly with the corresponding genders:

![Annotated Legend](https://s3.amazonaws.com/dq-content/annotated_legend.png)

If you notice, even the position of the text annotations have meaning. In both plots, the annotation for `Men` is positioned above the orange line while the annotation for `Women` is positioned below the dark blue line. This positioning subtly suggests that men are a majority for the degree categories the line charts are representing (`Engineering` and `Math and Statistics`) and women are a minority for those degree categories.

Combined, these two observations suggest that we should stick with annotating just the leftmost and the rightmost line charts, prioritizing the data-ink ratio over the consistency of elements.

## 7: Annotating In Matplotlib

To add text annotations to a matplotlib plot, we use the [Axes.text()](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.text) method. This method has a few required parameters:

- `x`: x-axis coordinate (as a float)
- `y`: y-axis coordinate (as a float)
- `s`: the text we want in the annotation (as a string value)

The values in the coordinate grid match exactly with the data ranges for the x-axis and the y-axis. If we want to add text at the intersection of `1970` from the x-axis and `0` from the y-axis, we would pass in those values:


```
ax.text(1970, 0, "starting point")
```

### Instructions

- Add the following text annotations in the leftmost chart:
  - The string `"Men"` at the x-axis coordinate of `2005`and the y-axis coordinate of `87`.
  - The string `"Women"` at the x-axis coordinate of `2002`and the y-axis coordinate of `8`.
- Add the following text annotations in the rightmost chart:
  - The string `"Men"` at the x-axis coordinate of `2005`and the y-axis coordinate of `62`.
  - The string `"Women"` at the x-axis coordinate of `2001`and the y-axis coordinate of `35`.

```
fig = plt.figure(figsize=(18, 3))

for sp in range(0,6):
    ax = fig.add_subplot(1,6,sp+1)
    ax.plot(women_degrees['Year'], women_degrees[stem_cats[sp]], c=cb_dark_blue, label='Women', linewidth=3)
    ax.plot(women_degrees['Year'], 100-women_degrees[stem_cats[sp]], c=cb_orange, label='Men', linewidth=3)
    for key,spine in ax.spines.items():
        spine.set_visible(False)
    ax.set_xlim(1968, 2011)
    ax.set_ylim(0,100)
    ax.set_title(stem_cats[sp])
    ax.tick_params(bottom="off", top="off", left="off", right="off")
    
    if sp == 0:
        ax.text(2005, 87, 'Men')
        ax.text(2002, 8, 'Women')
    elif sp == 5:
        ax.text(2005, 62, 'Men')
        ax.text(2001, 35, 'Women')
plt.show()
```

## 8: Next Steps

In this mission, we learned how to improve the viewing experience by making our plots more color-blind friendly and thickening the line widths. We then explored how to use the layout and ordering of the plots as well annotations directly onto the plots to enhance the story that's being told to the viewer. Next in this course is a guided project, where we'll extend the work we did in this mission to all of the degree categories.

# Guided Project: Visualizing The Gender Gap In College Degrees

## 1: Introduction

In this guided project, we'll extend the work we did in the last two missions on visualizing the gender gap across college degrees. So far, we mostly focused on the STEM degrees but now we will generate line charts to compare across all degree categories. In the last step of this guided project, we'll explore how to export the final diagram we create as an image file. You can download the solutions for this guided project [here](https://github.com/dataquestio/solutions/blob/master/Mission149Solutions.ipynb).

### Instructions

- Use the starter code in the notebook to get familiar with the data set and the visualization we created at the end of the last mission.

## 2: Comparing Across All Degrees

Because there are seventeen degrees that we need to generate line charts for, we'll use a subplot grid layout of 6 rows by 3 columns. We can then group the degrees into STEM, liberal arts, and other, in the following way:


```
stem_cats = ['Psychology', 'Biology', 'Math and Statistics', 'Physical Sciences', 'Computer Science', 'Engineering', 'Computer Science']
lib_arts_cats = ['Foreign Languages', 'English', 'Communications and Journalism', 'Art and Performance', 'Social Sciences and History']
other_cats = ['Health Professions', 'Public Administration', 'Education', 'Agriculture','Business', 'Architecture']
```

Here's what the diagram will look like:

![Comparing Across Degree Categories](https://s3.amazonaws.com/dq-content/149_comparing_across_categories.png)

While in the last mission, the `stem_cats`list was ordered by ending gender gap, all three of these lists are ordered in descending order by the percentage of degrees awarded to women. You may have also noticed that while `stem_cats` and `other_cats` have six degree categories as elements, `lib_arts_cats` only has five. You'll need to not only modify the for loop to generate the STEM line charts that we wrote in the last mission but also add two new for loops to generate the line charts for liberal arts degrees and for other degrees.

### Instructions

- Generate a 6 row by 3 column grid of subplots.
- In the first column:
  - Generate line charts for both male and female percentages for every degree in `stem_cats`.
  - Add text annotations for `Women` and `Men` in the topmost and bottommost plots.
- In the second column:
  - Generate line charts for both male and female percentages for every degree in `lib_arts_cats`.
  - - Add text annotations for `Women` and `Men` for only the topmost plot (since the lines overlap at the end in the bottommost plot).
- In the third column:
  - Generate line charts for both male and female percentages for every degree in `other_cats`.
  - Add text annotations for `Women` and `Men` in the topmost and bottommost plots.

## 3: Hiding X-Axis Labels

With seventeen line charts in one diagram, the non-data elements quickly clutter the field of view. The most immediate issue that sticks out is the titles of some line charts overlapping with the x-axis labels for the line chart above it. If we remove the titles for each line chart, the viewer won't know what degree each line chart refers to. Let's instead remove the x-axis labels for every line chart in a column except for the bottom most one. We can accomplish this by modifying the call to [`Axes.tick_params()`](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.tick_params) and setting `labelbottom` to `off`:


```
ax.tick_params(bottom="off", top="off", left="off", right="off", labelbottom='off')
```

This will disable the x-axis labels for all of the line charts. You can then enable the x-axis labels for the bottommost line charts in each column:


```
ax.tick_params(labelbottom='on')
```

### Instructions

- Disable the x-axis labels for all line charts except the bottommost line charts in each column.
- Click [here](https://s3.amazonaws.com/dq-content/149_hiding_x_axis_labels.png) to see what the diagram should look like.

## 4: Setting Y-Axis Labels

Removing the x-axis labels for all but the bottommost plots solved the issue we noticed with the overlapping text. In addition, the plots are cleaner and more readable. The trade-off we made is that it's now more difficult for the viewer to discern approximately which years some interesting changes in trends may have happened. This is acceptable because we're primarily interested in enabling the viewer to quickly get a high level understanding of which degrees are prone to gender imbalance and how has that changed over time.

In the vein of reducing cluttering, let's also simplify the y-axis labels. Currently, all seventeen plots have six y-axis labels and even though they are consistent across the plots, they still add to the visual clutter. By keeping just the starting and ending labels (`0` and `100`), we can keep some of the benefits of having the y-axis labels to begin with.

We can use the [Axes.set_yticks()](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.set_yticks) method to specify which labels we want displayed. The following code enables just the `0`and `100` labels to be displayed:


```
ax.set_yticks([0,100])
```

### Instructions

- For all plots:
  - Enable just the y-axis labels at `0` and `100`.
- Click [here](https://s3.amazonaws.com/dq-content/149_setting_y_axis_labels.png) to see what the diagram should look like.

## 5: Adding A Horizontal Line

While removing most of the y-axis labels definitely reduced clutter, it also made it hard to understand which degrees have close to 50-50 gender breakdown. While keeping all of the y-axis labels would have made it easier, we can actually do one better and use a horizontal line across all of the line charts where the y-axis label `50` would have been.

We can generate a horizontal line across an entire subplot using the [Axes.axhline()](http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.axhline) method. The only required parameter is the y-axis location for the start of the line:


```
ax.axhline(50)
```

Let's use the next color in the [Color Blind 10 palette](http://tableaufriction.blogspot.ro/2012/11/finally-you-can-use-tableau-data-colors.html) for this horizontal line, which has an RGB value of (171, 171, 171). Because we don't want this line to clutter the viewing experience, let's increase the transparency of the line. We can set the color using the `c` parameter and the transparency using the `alpha` parameter. The value passed in to the `alpha`parameter must range between `0` and `1`:


```
ax.axhline(50, c=(171/255, 171/255, 171/255), alpha=0.3)
```

### Instructions

- For all plots:
  - Generate a horizontal line with the following properties:
    - Starts at the y-axis position `50`
    - Set to the third color (light gray) in the Color Blind 10 palette
    - Has a transparency of `0.3`
- Click [here](https://s3.amazonaws.com/dq-content/149_adding_horizontal_lines.png) to see what the diagram should look like.

## 6: Exporting To A File

If you recall, matplotlib can be used many different ways. It can be used within a Jupyter Notebook interface (like this one), from the command line, or in an integrated development environment. Many of these ways of using matplotlib vary in workflow and handle the rendering of images differently as well. To help support these different use cases, matplotlib can target different outputs or **backends**. If you import matplotlib and run `matplotlib.get_backend()`, you'll see the specific backend you're currently using.

With the current backend we're using, we can use [Figure.savefig()](http://matplotlib.org/api/figure_api.html#matplotlib.figure.Figure.savefig) or [pyplot.savefig()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.savefig) to export all of the plots contained in the figure as a single image file. Note that these have to be called before we display the figure using `pyplot.show()`.:


```
plt.plot(women_degrees['Year'], women_degrees['Biology'])
plt.savefig('biology_degrees.png')
```

In the above code, we saved a line chart as a PNG file. You can read about the different popular file types for images [here](https://www.sitepoint.com/gif-png-jpg-which-one-to-use/). The image will be exported into the same folder that your Jupyter Notebook server is running.

Exporting plots we create using matplotlib allows us to use them in Word documents, Powerpoint presentations, and even in emails.

### Instructions

- Export the figure containing all of the line charts to `"gender_degrees.png"`.

# Conditional Plots

## 1: Introduction To Seaborn

So far, we've mostly worked with plots that are quick to analyze and make sense of. Line charts, scatter plots, and bar plots allow us to convey a few nuggets of insights to the reader. We've also explored how we can combine those plots in interesting ways to convey deeper insights and continue to extend the storytelling power of data visualization. In this mission, we'll explore how to quickly create multiple plots that are subsetted using one or more conditions.

We'll be working with the [seaborn](http://seaborn.pydata.org/) visualization library, which is built on top of matplotlib. Seaborn has good support for more complex plots, attractive default styles, and integrates well with the pandas library. Here are some examples of some complex plots that can be created using seaborn:

![Seaborn Gallery](https://s3.amazonaws.com/dq-content/seaborn_gallery.png)

Before we dive into seaborn, let's understand the data set we'll be working with in this mission.

## 2: Introduction To The Data Set

We'll be working with a data set of the passengers of the Titanic. The [Titanic shipwreck](https://en.wikipedia.org/wiki/Sinking_of_the_RMS_Titanic) is the most famous shipwreck in history and led to the creation of better safety regulations for ships. One substantial safety issue was that there were not enough lifeboats for every passenger on board, which meant that some passengers were prioritized over others to use the lifeboats.

The data set was compiled by Kaggle for their introductory data science competition, called **Titanic: Machine Learning from Disaster**. The goal of the competition is to build machine learning models that can predict if a passenger survives from their attributes. You can download the data set by navigating to the [data download page](https://www.kaggle.com/c/titanic/data) for the competition and creating a free account.

The data for the passengers is contained in two files:

- `train.csv`: Contains data on 712 passengers
- `test.csv`: Contains data on 418 passengers

Each row in both data sets represents a passenger on the Titanic, and some information about them. We'll be working with the `train.csv` file, because the `Survived` column, which describes if a given passenger survived the crash, is preserved in the file. The column was removed in `test.csv`, to encourage competitors to practice making predictions using the data. Here are descriptions for each of the columns in `train.csv`:

- `PassengerId` -- A numerical id assigned to each passenger.
- `Survived` -- Whether the passenger survived (`1`), or didn't (`0`).
- `Pclass` -- The class the passenger was in.
- `Name` -- the name of the passenger.
- `Sex` -- The gender of the passenger -- male or female.
- `Age` -- The age of the passenger. Fractional.
- `SibSp` -- The number of siblings and spouses the passenger had on board.
- `Parch` -- The number of parents and children the passenger had on board.
- `Ticket` -- The ticket number of the passenger.
- `Fare` -- How much the passenger paid for the ticker.
- `Cabin` -- Which cabin the passenger was in.
- `Embarked` -- Where the passenger boarded the Titanic.

Here's what the first few rows look like:

| PassengerId | Survived | Pclass | Name                                     | Sex    | Age  | SibSp | Parch | Ticket           | Fare    | Cabin | Embarked |
| ----------- | -------- | ------ | ---------------------------------------- | ------ | ---- | ----- | ----- | ---------------- | ------- | ----- | -------- |
| 1           | 0        | 3      | Braund, Mr. Owen Harris                  | male   | 22.0 | 1     | 0     | A/5 21171        | 7.2500  |       | S        |
| 2           | 1        | 1      | Cumings, Mrs. John Bradley (Florence Briggs Thayer) | female | 38.0 | 1     | 0     | PC 17599         | 71.2833 | C85   | C        |
| 3           | 1        | 3      | Heikkinen, Miss. Laina                   | female | 26.0 | 0     | 0     | STON/O2. 3101282 | 7.9250  |       | S        |

Let's remove columns like `Name` and `Ticket` that we don't have a way to visualize. In addition, we need to remove any rows containing missing values, as seaborn will throw errors when we try to plot missing values.

## 3: Introduction To The Data Set

### Instructions

- Read `train.csv` into a DataFrame named `titanic`. Keep only the following columns:
  - `"Survived"`
  - `"Pclass"`
  - `"Sex"`
  - `"Age"`
  - `"SibSp"`
  - `"Parch"`
  - `"Fare"`
  - `"Embarked"`
- Use the [DataFrame.dropna()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.dropna.html) method to remove rows containing missing values.

```
import pandas as pd
titanic = pd.read_csv('train.csv')
cols = ['Survived', 'Pclass', 'Sex', 'Age', 'SibSp', 'Parch', 'Fare', 'Embarked']
titanic = titanic[cols].dropna()
```

## 4: Creating Histograms In Seaborn

Seaborn works similarly to the `pyplot`module from matplotlib. We primarily use seaborn interactively, by calling functions in its top level namespace. Like the `pyplot` module from matplotlib, seaborn creates a matplotlib figure or adds to the current, existing figure each time we generate a plot. When we're ready to display the plots, we call `pyplot.show()`.

To get familiar with seaborn, we'll start by creating the familiar histogram. We can generate a histogram of the `Fare` column using the [`seaborn.distplot()`](http://seaborn.pydata.org/generated/seaborn.distplot.html#seaborn.distplot) function:


```
# seaborn is commonly imported as `sns`.
import seaborn as sns
sns.distplot(titanic["Fare"])
plt.show()
```

Here's the plot that's generated when the above code is run:

![Seaborn Histogram With KDE](https://s3.amazonaws.com/dq-content/seaborn_histogram_with_kde.png)

Under the hood, seaborn creates a histogram using matplotlib, scales the axes values, and styles it. In addition, seaborn uses a technique called kernel density estimation, or KDE for short, to create a smoothed line chart over the histogram. If you're interested in learning about how KDE works, you can read more on [Wikipedia](https://en.wikipedia.org/wiki/Kernel_density_estimation).

What you need to know for now is that the resulting line is a smoother version of the histogram, called a **kernel density plot**. Kernel density plots are especially helpful when we're comparing distributions, which we'll explore later in this mission. When viewing a histogram, our visual processing systems influence us to smooth out the bars into a continuous line.

### Instructions

- Import `seaborn` as `sns` and `matplotlib.pyplot` as `plt`.
- Use the `seaborn.distplot()` function to visualize the distribution of the `"Age"` column.
- Display the plot using `pyplot.show()`.

```
import seaborn as sns
import matplotlib.pyplot as plt
sns.distplot(titanic['Age'])
plt.show()
```

## 5: Generating A Kernel Density Plot

While having both the histogram and the kernel density plot is useful when we want to explore the data, it can be overwhelming for someone who's trying to understand the distribution. To generate just the kernel density plot, we use the [`seaborn.kdeplot()`](http://seaborn.pydata.org/generated/seaborn.kdeplot.html#seaborn.kdeplot) function:


```
sns.kdeplot(titanic["Fare"])
```

Here's what just the kernel density plot looks like:

![Seaborn KDE Plot](https://s3.amazonaws.com/dq-content/seaborn_kdeplot.png)

While the distribution of data is displayed in a smoother fashion, it's now more difficult to visually estimate the area under the curve using just the line chart. When we also had the histogram, the bars provided a way to understand and compare proportions visually.

To bring back some of the ability to easily compare proportions, we can shade the area under the line using a single color. When calling the `seaborn.kdeplot()`function, we can shade the area under the line by setting the `shade` parameter to `True`.

### Instructions

- Generate a kernel density plot:
  - Using the values in the `"Age"` column
  - With the area under the curve shaded
- Set the x-axis label to `"Age"` using `pyplot.xlabel()`.

```
sns.kdeplot(titanic['Age'], shade=True)
plt.xlabel("Age")
plt.show()
```

## 6: Modifying The Appearance Of The Plots

From the plots in the previous step, you'll notice that seaborn:

- Sets the x-axis label automatically based on the column name we passed in
- Sets the background color to a light gray color
- Hides the x-axis and y-axis ticks
- Displays the coordinate grid

In the last few missions, we explored some general aesthetics guidelines for plots. The default seaborn style sheet gets some things right, like hiding axis ticks, and some things wrong, like displaying the coordinate grid and keeping all of the axis spines. We can use the [`seaborn.set_style()`](http://seaborn.pydata.org/generated/seaborn.set_style.html#seaborn.set_style) function to change the default seaborn style sheet. Seaborn comes with a few style sheets:

- `darkgrid`: Coordinate grid displayed, dark background color
- `whitegrid`: Coordinate grid displayed, white background color
- `dark`: Coordinate grid hidden, dark background color
- `white`: Coordinate grid hidden, white background color
- `ticks`: Coordinate grid hidden, white background color, ticks visible

Here's a diagram that compares the same plot across all styles:

![Seaborn All Styles](https://s3.amazonaws.com/dq-content/seaborn_all_styles.png)

By default, the seaborn style is set to `"darkgrid"`:

```
sns.set_style("darkgrid")
```

If we change the style sheet using this method, all future plots will match that style in your current session. This means you need to set the style before generating the plot.

To remove the axis spines for the top and right axes, we use the [`seaborn.despine()`](http://seaborn.pydata.org/generated/seaborn.despine.html#seaborn.despine)function:

```
sns.despine()
```

By default, only the top and right axes will be **despined**, or have their spines removed. To despine the other two axes, we need to set the `left` and `bottom`parameters to `True`.

### Instructions

- Set the style to the style sheet that hides the coordinate grid and sets the background color to white.
- Generate a kernel density plot of the `"Age"` column, with the area under the curve shaded.
- Set the x-axis label to `"Age"`.
- Despine all of the axes.

```
sns.set_style('white')
sns.kdeplot(titanic['Age'], shade=True)
sns.despine(left=True, bottom=True)
plt.xlabel('Age')
```

## 7: Conditional Distributions Using A Single Condition

In the last few missions, we created a [small multiple](https://en.wikipedia.org/wiki/Small_multiple), which is a group of plots that have the same axis scales so the viewer can compare plots effectively. We accomplished this by subsetting the data manually and generating a plot using matplotlib for each one.

In seaborn, we can create a small multiple by specifying the conditioning criteria and the type of data visualization we want. For example, we can visualize the differences in age distributions between passengers who survived and those who didn't by creating a pair of kernel density plots. One kernel density plot would visualize the distribution of values in the `"Age"`column where `Survived` equalled `0` and the other would visualize the distribution of values in the `"Age"` column where `Survived` equalled `1`.

Here's what those plots look like:

![Simple Conditional KDE Plot](https://s3.amazonaws.com/dq-content/seaborn_simple_conditional.png)

The code to generate the pair of plots, is short and sweet:


```
# Condition on unique values of the "Survived" column.
g = sns.FacetGrid(titanic, col="Survived", size=6)
# For each subset of values, generate a kernel density plot of the "Age" columns.
g.map(sns.kdeplot, "Age", shade=True)
```

Seaborn handled:

- subsetting the data into rows where `Survived` is `0` and where `Survived`is `1`
- creating both Axes objects, ensuring the same axis scales
- plotting both kernel density plots

Instead of subsetting the data and generating each plot ourselves, seaborn allows us to express the plots we want as parameter values. The [`seaborn.FacetGrid`](http://seaborn.pydata.org/generated/seaborn.FacetGrid.html#seaborn.FacetGrid)object is used to represent the layout of the plots in the grid and the columns used for subsetting the data. The word "facet" from `FacetGrid` is another word for "subset". Setting the `col` parameter to `"Survived"` specifies a separate plot for each unique value in the `Survived`column. Setting the `size` parameter to `6` specifies a height of 6 inches for each plot.

Once we've created the grid, we use the `FacetGrid.map()` method to specify the plot we want for each unique value of `Survived`. Seaborn generated one kernel density plot for the ages of passengers that survived and one kernel density plot for the ages of passengers that didn't survive.

The function that's passed into `FacetGrid.map()` has to be a valid matplotlib or seaborn function. For example, we can map matplotlib histograms to the grid:

```
g = sns.FacetGrid(titanic, col="Survived", size=6)
g.map(plt.hist, "Age")
```

Let's create a grid of plots that displays the age distributions for each class.

### Instructions

- Use a `FacetGrid` instance to generate three plots on the same row:One for each unique value of `Pclass`.Each plot should be a kernel density plot of the `"Age"` column, with the area under the curve shaded.Each plot should have a height of `6` inches.
- Remove all of the spines using `seaborn.despine()`.
- Display the plots.

```
g = sns.FacetGrid(titanic, col="Pclass", size=6)
g.map(sns.kdeplot, "Age", shade=True)
sns.despine(left=True, bottom=True)
plt.show()
```

## 8: Creating Conditional Plots Using Two Conditions

We can use two conditions to generate a grid of plots, each containing a subset of the data with a unique combination of each condition. When creating a `FacetGrid`, we use the `row` parameter to specify the column in the dataframe we want used to subset across the rows in the grid. The best way to understand this is to see a working example.

The starter code subsets the dataframe on different combinations of unique values in both the `Pclass` and `Survived` columns. Try changing the conditions to see the resulting plots.

## 9: Creating Conditional Plots Using Three Conditions

When subsetting data using two conditions, the rows in the grid represented one condition while the columns represented another. We can express a third condition by generating multiple plots on the same subplot in the grid and color them differently. Thankfully, we can add a condition just by setting the `hue` parameter to the column name from the dataframe.

Let's add a new condition to the grid of plots we generated in the last step and see what this grid of plots would look like.

### Instructions

- Use a `FacetGrid` instance to generate a grid of plots using the following conditions:The `Survived` column across the columns in the grid.The `Pclass` column across the rows in the grid.The `Sex` column using different hues.
- Each plot should be a kernel density plot of the `"Age"`column, with the area under the curve shaded.
- Each plot should have a height of `3` inches.
- Remove all of the spines using `seaborn.despine()`.
- Display the plots.

```
g = sns.FacetGrid(titanic, col="Survived", row="Pclass")
g.map(sns.kdeplot, "Age", shade=True)
sns.despine(left=True, bottom=True)
plt.show()
g = sns.FacetGrid(titanic, col="Survived", row="Pclass", hue="Sex", size=3)
g.map(sns.kdeplot, "Age", shade=True)
sns.despine(left=True, bottom=True)
plt.show()
```

## 10: Adding A Legend

Now that we're coloring plots, we need a legend to keep track of which value each color represents. As a challenge to you, we won't specify how exactly to generate a legend in seaborn. Instead, we encourage you to use the examples from the [page](http://seaborn.pydata.org/generated/seaborn.FacetGrid.html#seaborn.FacetGrid) on plotting using the `FacetGrid` instance.

Here's what we want the final grid to look like:

![Seaborn Three Conditions](https://s3.amazonaws.com/dq-content/seaborn_three_conditions.png)

### Instructions

- Use a `FacetGrid` instance to generate a grid of plots using the following conditions:The `Survived` column across the columns in the grid.The `Pclass` column across the rows in the grid.The `Sex` column using different hues.
- Each plot should be a kernel density plot of the `"Age"`column, with the area under the curve shaded.
- Each plot should have a height of `3` inches.
- Remove all of the spines using `seaborn.despine()`.
- Add a legend for the `Sex` column.
- Display the plots.

```
g = sns.FacetGrid(titanic, col="Survived", row="Pclass", hue="Sex", size=3)
g.map(sns.kdeplot, "Age", shade=True)
g.add_legend()
sns.despine(left=True, bottom=True)
plt.show()
```

## 11: Next Steps

In this mission, we learned how the seaborn library lets us quickly style plots and create small multiples using conditions we specify. In the next mission, we'll explore how to visualize geographic data using matplotlib.

# Visualizing Geographic Data

## 1: Geographic Data

From scientific fields like meteorology and climatology, through to the software on our smartphones like Google Maps and Facebook check-ins, geographic data is always present in our everyday lives. Raw geographic data like latitudes and longitudes are difficult to understand using the data charts and plots we've discussed so far. To explore this kind of data, you'll need to learn how to visualize the data on maps.

In this mission, we'll explore the fundamentals of geographic coordinate systems and how to work with the basemap library to plot geographic data points on maps. We'll be working with flight data from the [openflights website](http://openflights.org/data.html). Here's a breakdown of the files we'll be working with and the most pertinent columns from each dataset:

- `airlines.csv` - data on each airline.
  - `country` - where the airline is headquartered.
  - `active` - if the airline is still active.
- `airports.csv` - data on each airport.
  - `name` - name of the airport.
  - `city` - city the airport is located.
  - `country` - country the airport is located.
  - `code` - unique airport code.
  - `latitude` - latitude value.
  - `longitude` - longitude value.
- `routes.csv` - data on each flight route.
  - `airline` - airline for the route.
  - `source` - starting city for the route.
  - `dest` - destination city for the route.

We can explore a range of interesting questions and ideas using these datasets:

- For each airport, which destination airport is the most common?
- Which cities are the most important hubs for airports and airlines?

Before diving into coordinate systems, explore the datasets in the code cell below.

### Instructions

- Read in the 3 CSV files into 3 separate dataframe objects - `airlines`, `airports`, and `routes`.
- Use the `DataFrame.iloc[]` method to return the first row in each dataframe as a neat table.
- Display the first rows for all dataframes using the `print()`function. Try to answer the following questions:What's the best way to link the data from these 3 different datasets together?What are the formats of the latitude and longitude values?

```
import pandas as pd
airlines = pd.read_csv('airlines.csv')
airports = pd.read_csv('airports.csv')
routes  = pd.read_csv('routes.csv')

print(airports.iloc[0])
print(airlines.iloc[0])
print(routes.iloc[0])
```

## 2: Geographic Coordinate Systems

A geographic coordinate system allows us to locate any point on Earth using latitude and longitude coordinates.

![Latitude And Longitude On The Globe](https://s3.amazonaws.com/dq-content/latitude_longitude.png)

Here are the coordinates of 2 well known points of interest:

| Name            | City          | State | Latitude  | Longitude   |
| --------------- | ------------- | ----- | --------- | ----------- |
| White House     | Washington    | DC    | 38.898166 | -77.036441  |
| Alcatraz Island | San Francisco | CA    | 37.827122 | -122.422934 |

In most cases, we want to visualize latitude and longitude points on two-dimensional maps. Two-dimensional maps are faster to render, easier to view on a computer and distribute, and are more familiar to the experience of popular mapping software like Google Maps. Latitude and longitude values describe points on a sphere, which is three-dimensional. To plot the values on a two-dimensional plane, we need to convert the coordinates to the Cartesian coordinate system using a **map projection**.

A [map projection](https://en.wikipedia.org/wiki/Map_projection) transforms points on a sphere to a two-dimensional plane. When projecting down to the two-dimensional plane, some properties are distorted. Each map projection makes trade-offs in what properties to preserve and you can read about the different trade-offs [here](https://en.wikipedia.org/wiki/Map_projection#Metric_properties_of_maps). We'll use the [Mercator projection](https://en.wikipedia.org/wiki/Mercator_projection), because it is commonly used by popular mapping software.

## 3: Installing Basemap

Before we convert our flight data to Cartesian coordinates and plot it, let's learn more about the [basemap toolkit](http://matplotlib.org/basemap/). Basemap is an extension to Matplotlib that makes it easier to work with geographic data. The [documentation for basemap](http://matplotlib.org/basemap/users/intro.html) provides a good high-level overview of what the library does:

> The matplotlib basemap toolkit is a library for plotting 2D data on maps in Python. Basemap does not do any plotting on it’s own, but provides the facilities to transform coordinates to one of 25 different map projections.

Basemap makes it easy to convert from the spherical coordinate system (latitudes & longitudes) to the Mercator projection. While basemap uses Matplotlib to actually draw and control the map, the library provides many methods that enable us to work with maps quickly. Before we dive into how basemap works, let's get familiar with how to install it.

The easiest way to install basemap is through Anaconda. If you're new to Anaconda, we recommend checking out our [Python and Pandas installation project](https://www.dataquest.io/mission/118/project-python-and-pandas-installation):


```
conda install basemap
```

The Basemap library has some external dependencies that Anaconda handles the installation for. To test the installation, run the following import code:

```
from mpl_toolkits.basemap import Basemap
```

If an error is returned, we recommend searching for similar errors on [StackOverflow](http://stackoverflow.com/questions/29344064/cant-import-basemap-library-in-python) to help debug the issue. Because basemap uses matplotlib, you'll want to import `matplotlib.pyplot` into your environment when you use Basemap.

## 4: Workflow With Basemap

Here's what the general workflow will look like when working with two-dimensional maps:

- Create a new basemap instance with the specific map projection we want to use and how much of the map we want included.
- Convert spherical coordinates to Cartesian coordinates using the basemap instance.
- Use the matplotlib and basemap methods to customize the map.
- Display the map.

Let's focus on the first step and create a new basemap instance. To create a new instance of the basemap class, we call the [basemap constructor](http://matplotlib.org/basemap/api/basemap_api.html#mpl_toolkits.basemap.Basemap) and pass in values for the required parameters:

- `projection`: the map projection.
- `llcrnrlat`: latitude of lower left hand corner of the desired map domain
- `urcrnrlat`: latitude of upper right hand corner of the desired map domain
- `llcrnrlon`: longitude of lower left hand corner of the desired map domain
- `urcrnrlon`: longitude of upper right hand corner of the desired map domain

### Instructions

Create a new basemap instance with the following parameters:

- `projection`: `"merc"`
- `llcrnrlat`: `-80` degrees
- `urcrnrlat`: `80` degrees
- `llcrnrlon`: `-180` degrees
- `urcrnrlon`: `180` degrees

Assign the instance to the new variable `m`.

```
import matplotlib.pyplot as plt
from mpl_toolkits.basemap import Basemap
m = Basemap(projection='merc',llcrnrlat=-80,urcrnrlat=80,llcrnrlon=-180,urcrnrlon=180)
```

## 5: Converting From Spherical To Cartesian Coordinates

As we mentioned before, we need to convert latitude and longitude values to Cartesian coordinates to display them on a two-dimensional map. We can pass in a list of latitude and longitude values into the basemap instance and it will return back converted lists of longitude and latitude values using the projection we specified earlier. The constructor only accepts list values, so we'll need to use [Series.tolist()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.tolist.html) to convert the `longitude`and `latitude` columns from the `airports` dataframe to lists. Then, we pass them to the basemap instance with the longitude values first then the latitude values:


```
x, y = m(longitudes, latitudes)
```

The basemap object will return 2 list objects, which we assign to `x` and `y`. Finally, we display the first 5 elements of the original longitude values, original latitude values, the converted longitude values, and the converted latitude values.

### Instructions

- Convert the longitude values from spherical to Cartesian and assign the resulting list to `x`.
- Convert the latitude values from spherical to Cartesian and assign the resulting list to `y`.

```
m = Basemap(projection='merc', llcrnrlat=-80, urcrnrlat=80, llcrnrlon=-180, urcrnrlon=180)
m = Basemap(projection='merc', llcrnrlat=-80, urcrnrlat=80, llcrnrlon=-180, urcrnrlon=180)
longitudes = airports["longitude"].tolist()
latitudes = airports["latitude"].tolist()
x, y = m(longitudes, latitudes)
```

## 6: Generating A Scatter Plot

Now that the data is in the right format, we can plot the coordinates on a map. A scatter plot is the simplest way to plot points on a map, where each point is represented as an (x, y) coordinate pair. To create a scatter plot from a list of `x`and `y` coordinates, we use the [basemap.scatter()](http://matplotlib.org/basemap/api/basemap_api.html#mpl_toolkits.basemap.Basemap.scatter) method.


```
m.scatter(x,y)
```

The `basemap.scatter()` method has similar parameters to the [pyplot.scatter()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.scatter). For example, we can customize the size of each marker using the `s` parameter:


```
# Large markers.
m.scatter(x,y,s=10)
# Smaller markers.
m.scatter(x,y,s=5)
```

After we've created the scatter plot, use `plt.show()` to display the plot. We'll dive more into customizing the plot in the next step but now, create a simple scatter plot.

### Instructions

- Create a scatter plot using the converted latitude and longitude values using a marker size of `1`.
- Display the scatter plot.

```
m = Basemap(projection='merc', llcrnrlat=-80, urcrnrlat=80, llcrnrlon=-180, urcrnrlon=180)
x, y = m(longitudes, latitudes)
m = Basemap(projection='merc', llcrnrlat=-80, urcrnrlat=80, llcrnrlon=-180, urcrnrlon=180)
x, y = m(longitudes, latitudes)
m.scatter(x, y, s=1)
plt.show()
```

## 7: Customizing The Plot Using Basemap

You'll notice that the outlines of the coasts for each continent are missing from the map above. We can display the coast lines using the [`basemap.drawcoastlines()`](http://matplotlib.org/basemap/api/basemap_api.html#mpl_toolkits.basemap.Basemap.drawcoastlines)method.

### Instructions

- Use `basemap.drawcoastlines()` to enable the coast lines to be displayed.
- Display the plot using `plt.show()`.

```
m = Basemap(projection='merc', llcrnrlat=-80, urcrnrlat=80, llcrnrlon=-180, urcrnrlon=180)
longitudes = airports["longitude"].tolist()
latitudes = airports["latitude"].tolist()
x, y = m(longitudes, latitudes)
m.scatter(x, y, s=1)
m = Basemap(projection='merc', llcrnrlat=-80, urcrnrlat=80, llcrnrlon=-180, urcrnrlon=180)
longitudes = airports["longitude"].tolist()
latitudes = airports["latitude"].tolist()
x, y = m(longitudes, latitudes)
m.scatter(x, y, s=1)
m.drawcoastlines()
plt.show()
```

## 8: Customizing The Plot Using Matplotlib

Because basemap uses matplotlib under the hood, we can interact with the matplotlib classes that basemap uses directly to customize the appearance of the map.

We can add code that:

- uses `pyplot.subplots()` to specify the `figsize` parameter
- returns the Figure and Axes object for a single subplot and assigns to `fig`and `ax` respectively
- use the `Axes.set_title()` method to set the map title

### Instructions

- Before creating the basemap instance and generating the scatter plot, add code that:
  - creates a figure with a height of 15 inches and a width of 20 inches
  - sets the title of the scatter plot to `"Scaled Up Earth With Coastlines"`

```
# Add code here, before creating the Basemap instance.
m = Basemap(projection='merc', llcrnrlat=-80, urcrnrlat=80, llcrnrlon=-180, urcrnrlon=180)
longitudes = airports["longitude"].tolist()
latitudes = airports["latitude"].tolist()
x, y = m(longitudes, latitudes)
m.scatter(x, y, s=1)
m.drawcoastlines()
plt.show()
fig, ax = plt.subplots(figsize=(15,20))
plt.title("Scaled Up Earth With Coastlines")
m = Basemap(projection='merc', llcrnrlat=-80, urcrnrlat=80, llcrnrlon=-180, urcrnrlon=180)
longitudes = airports["longitude"].tolist()
latitudes = airports["latitude"].tolist()
x, y = m(longitudes, latitudes)
m.scatter(x, y, s=1)
m.drawcoastlines()
plt.show()
```

## 9: Introduction To Great Circles

To better understand the flight routes, we can draw **great circles** to connect starting and ending locations on a map. A great circle is the shortest circle connecting 2 points on a sphere.

![Great Circles](https://s3.amazonaws.com/dq-content/great_circles.png)

On a two-dimensional map, the great circle is demonstrated as a line because it is projected from three-dimensional down to two-dimensional using the map projection. We can use these to visualize the flight routes from the `routes`dataframe. To plot great circles, we need the source longitude, source latitude, destination longitude, and the destination latitude for each route. While the `routes`dataframe contains the source and destination airports for each route, the latitude and longitude values for each airport are in a separate dataframe (`airports`).

To make things easier, we've created a new CSV file called `geo_routes.csv` that contains the latitude and longitude values corresponding to the source and destination airports for each route. We've also removed some columns we won't be working with.

### Instructions

- Read `geo_routes.csv` into a dataframe named `geo_routes`.
- Use the `DataFrame.info()` method to look for columns containing any null values.
- Display the first five rows in `geo_routes`.

```
geo_routes = pd.read_csv("geo_routes.csv")
print(geo_routes.info())
print(geo_routes.head(5))
```

## 10: Displaying Great Circles

We use the [basemap.drawgreatcircle()](http://matplotlib.org/basemap/api/basemap_api.html#mpl_toolkits.basemap.Basemap.drawgreatcircle) method to display a great circle between 2 points. The `basemap.drawgreatcircle()`method requires four parameters in the following order:

- `lon1` - longitude of the starting point.
- `lat1` - latitude of the starting point.
- `lon2` - longitude of the ending point.
- `lat2` - latitude of the ending point.

The following code generates a great circle for the first three routes in the dataframe:


```
m.drawgreatcircle(39.956589, 43.449928, 49.278728, 55.606186)
m.drawgreatcircle(48.006278, 46.283333, 49.278728, 55.606186)
m.drawgreatcircle(39.956589, 43.449928, 43.081889 , 44.225072)
```

Unfortunately, basemap struggles to create great circles for routes that have an absolute difference of larger than 180 degrees for either the latitude or longitude values. This is because the `basemap.drawgreatcircle()` method isn't able to create great circles properly when they go outside of the map boundaries. This is mentioned briefly in the [documentation](http://matplotlib.org/basemap/api/basemap_api.html#mpl_toolkits.basemap.Basemap.drawgreatcircle) for the method:

> **Note**: Cannot handle situations in which the great circle intersects the edge of the map projection domain, and then re-enters the domain.

### Instructions

Write a function, named `create_great_circles()` that draws a great circle for each route that has an absolute difference in the latitude and longitude values less than 180. This function should:

- Accept a dataframe as the sole parameter
- Iterate over the rows in the dataframe using [`DataFrame.iterrows()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.iterrows.html)
- For each row:
  - Draw a great circle using the four geographic coordinates **only** if:The absolute difference between the latitude values (`end_lat` and `start_lat`) is less than 180.If the absolute difference between the longitude values (`end_lon` and `start_lon`) is less than 180.

Create a filtered dataframe containing just the routes that start at the DFW airport.

- Select only the rows in `geo_routes` where the value for the `source` column equals `"DFW"`.
- Assign the resulting dataframe to `dfw`.

Pass `dfw` into `create_great_circles()` and display the plot using the `pyplot.show()` function.

```
fig, ax = plt.subplots(figsize=(15,20))
m = Basemap(projection='merc', llcrnrlat=-80, urcrnrlat=80, llcrnrlon=-180, urcrnrlon=180)
m.drawcoastlines()
fig, ax = plt.subplots(figsize=(15,20))
m = Basemap(projection='merc', llcrnrlat=-80, urcrnrlat=80, llcrnrlon=-180, urcrnrlon=180)
m.drawcoastlines()

def create_great_circles(df):
    for index, row in df.iterrows():
        end_lat, start_lat = row['end_lat'], row['start_lat']
        end_lon, start_lon = row['end_lon'], row['start_lon']
        
        if abs(end_lat - start_lat) < 180:
            if abs(end_lon - start_lon) < 180:
                m.drawgreatcircle(start_lon, start_lat, end_lon, end_lat)

dfw = geo_routes[geo_routes['source'] == "DFW"]
create_great_circles(dfw)
plt.show()
```

## 11: Conclusion

In this mission, we learned how to visualize geographic data using basemap. This is the last mission in the **Storytelling Through Data Visualization** course. You should now have a solid foundation in data visualization for exploring data and communicating insights. We encourage you to keep exploring data visualization on your own. Here are some suggestions for what to do next:

- Plotting tools:
  - [Creating 3D plots using Plotly](https://plot.ly/python/3d-scatter-plots/)
  - [Creating interactive visualizations using bokeh](http://bokeh.pydata.org/en/latest/)
  - [Creating interactive map visualizations using folium](https://folium.readthedocs.io/en/latest/)
- The art and science of data visualization:
  - [Visual Display of Quantitative Information](https://www.amazon.com/Visual-Display-Quantitative-Information/dp/0961392142)
  - [Visual Explanations: Images and Quantities, Evidence and Narrative](https://www.amazon.com/Visual-Explanations-Quantities-Evidence-Narrative/dp/0961392126)


