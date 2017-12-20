# Data Cleaning Walkthrough

## 1: Introduction

Data science projects usually consist of one of two things:

- An exploration and analysis of a set of data. 
- An operational system that generates predictions based on data that updates continually.

[Here's](https://github.com/dataquestio/loan-prediction) an example of a finished project.

In this mission, we'll develop our data cleaning and storytelling skills.

Here are two ways to go about finding a good topic:

- Think about what sectors or angles you're really interested in, then find data sets relating to those sectors.
- Review several data sets, and find one that seems interesting enough to explore.

In real-world data science, you may not find an ideal data set. You might have to aggregate disparate data sources instead, or do a good amount of data cleaning.

For the purposes of this project, we'll be using data about New York City public schools, which can be found [here](https://data.cityofnewyork.us/browse?category=Education).

## 2: Finding All Of The Relevant Data Sets

One of the most controversial issues in the U.S. educational system is the efficacy of standardized tests, and whether they're unfair to certain groups. Given our prior knowledge of this topic, investigating the correlations between [SAT scores](https://en.wikipedia.org/wiki/SAT) and demographics might be an interesting angle to take. We could correlate SAT scores with factors like race, gender, income, and more.![SAT](https://s3.amazonaws.com/dq-content/sat.png)

SAT scores by school, School attendance, Class size, AP test results, Graduation outcomes, Demographics, School survey, all of these data sets are interrelated. We'll need to combine them into a single data set before we can find correlations.

## 3: Finding Background Information

We can learn a few different things from these resources. For example:

- Only high school students take the SAT, so we'll want to focus on high schools.
- New York City is made up of five boroughs, which are essentially distinct regions.
- New York City schools fall within several different school districts, each of which can contains dozens of schools.
- Our data sets include several different types of schools. We'll need to clean them so that we can focus on high schools only.
- Each school in New York City has a unique code called a `DBN`, or district borough number.
- Aggregating data by district will allow us to use the district mapping data to plot district-by-district differences.

## 4: Reading In The Data

We'll read each file into a [pandas dataframe](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html), and then store all of the dataframes in a dictionary. This will give us a convenient way to store them, and a quick way to reference them later on.

```python
import pandas as pd
data_files = [
    "ap_2010.csv",
    "class_size.csv",
    "demographics.csv",
    "graduation.csv",
    "hs_directory.csv",
    "sat_results.csv"
]
data = {}
for f in data_files:
    d = pd.read_csv("schools/{0}".format(f))
    key_name = f.replace(".csv", "")
    data[key_name] = d
```

## 5: Exploring The SAT Data

Corresponds to the dictionary key `sat_results`. 

Let's explore `sat_results` to see what we can discover. Exploring the dataframe will help us understand the structure of the data, and make it easier for us to analyze it.

```python
print(data["sat_results"].head())
```

## 6: Exploring The Remaining Data

When we printed the first five rows of the SAT data, the output looked like this:

```
DBN                                    SCHOOL NAME  \
0  01M292  HENRY STREET SCHOOL FOR INTERNATIONAL STUDIES
1  01M448            UNIVERSITY NEIGHBORHOOD HIGH SCHOOL
2  01M450                     EAST SIDE COMMUNITY SCHOOL
3  01M458                      FORSYTH SATELLITE ACADEMY
4  01M509                        MARTA VALLE HIGH SCHOOL

  Num of SAT Test Takers SAT Critical Reading Avg. Score SAT Math Avg. Score  \
0                     29                             355                 404
1                     91                             383                 423
2                     70                             377                 402
3                      7                             414                 401
4                     44                             390                 433

  SAT Writing Avg. Score
0                    363
1                    366
2                    370
3                    359
4                    384
```

We can make a few observations based on this output:

- The `DBN` appears to be a unique ID for each school.
- We only have data about high schools.
- There's only a single row for each high school, so each `DBN` is unique in the SAT data.
- We may eventually want to combine the three columns that contain SAT scores -- `SAT Critical Reading Avg.`, `Score SAT Math Avg. Score`, and `SAT Writing Avg. Score` -- into a single column to make the scores easier to analyze.

Given these observations, let's explore the other data sets to see if we can gain any insight into how to combine them.

```python
for k in data:
    print(data[k].head())
```

## 8: Reading In The Survey Data

We'll need to specify the `Windows-1252` encoding and delimiter *tab* to the pandas.

We'll want to combine it into a single dataframe. 

```
all_survey = pd.read_csv("schools/survey_all.txt", delimiter="\t", encoding='windows-1252')
d75_survey = pd.read_csv("schools/survey_d75.txt", delimiter="\t", encoding='windows-1252')
survey = pd.concat([all_survey, d75_survey], axis=0)

print(survey.head())
```

## 9: Cleaning Up The Surveys

There are two immediate facts that we can see in the data:

- We only need the following columns to give us aggregate survey data about how parents, teachers, and students feel about school safety, academic performance, and more.

```
["dbn", "rr_s", "rr_t", "rr_p", "N_s", "N_t", "N_p", "saf_p_11", "com_p_11", "eng_p_11", "aca_p_11", "saf_t_11", "com_t_11", "eng_t_11", "aca_t_11", "saf_s_11", "com_s_11", "eng_s_11", "aca_s_11", "saf_tot_11", "com_tot_11", "eng_tot_11", "aca_tot_11"]
```
- The survey data has a `dbn` column that we'll want to convert to uppercase (`DBN`). 

![img](https://s3.amazonaws.com/dq-content/xj5ud4r.png)

```
survey["DBN"] = survey["dbn"]

survey_fields = [
    "DBN", 
    "rr_s", 
    "rr_t", 
    "rr_p", 
    "N_s", 
    "N_t", 
    "N_p", 
    "saf_p_11", 
    "com_p_11", 
    "eng_p_11", 
    "aca_p_11", 
    "saf_t_11", 
    "com_t_11", 
    "eng_t_11", 
    "aca_t_11", 
    "saf_s_11", 
    "com_s_11", 
    "eng_s_11", 
    "aca_s_11", 
    "saf_tot_11", 
    "com_tot_11", 
    "eng_tot_11", 
    "aca_tot_11",
]
survey = survey.loc[:,survey_fields]
data["survey"] = survey

print(survey.head())
```

## 11: Inserting DBN Fields

In `class_size`, Padded CSD(two digits) + School Code = DBN

```
data["hs_directory"]["DBN"] = data["hs_directory"]["dbn"]

def pad_csd(num):
    string_representation = str(num)
    if len(string_representation) > 1:
        return string_representation
    else:
        return string_representation.zfill(2)
    
data["class_size"]["padded_csd"] = data["class_size"]["CSD"].apply(pad_csd)
data["class_size"]["DBN"] = data["class_size"]["padded_csd"] + data["class_size"]["SCHOOL CODE"]
print(data["class_size"].head())
```

## 12: Combining The SAT Scores

Convert the `SAT Math Avg. Score`, `SAT Critical Reading Avg. Score`, and `SAT Writing Avg. Score` columns in the `sat_results` data set from the object (string) data type to a numeric data type.  If we don't convert the values, we won't be able to add the columns together.

It's important to pass the keyword argument `errors="coerce"` when we call `pandas.to_numeric()`, so that pandas treats any invalid strings it can't convert to numbers as missing values instead.

After we perform the conversion, we can use the addition operator (`+`) to add all three columns together.

```
cols = ['SAT Math Avg. Score', 'SAT Critical Reading Avg. Score', 'SAT Writing Avg. Score']
for c in cols:
    data["sat_results"][c] = pd.to_numeric(data["sat_results"][c], errors="coerce")

data['sat_results']['sat_score'] = data['sat_results'][cols[0]] + data['sat_results'][cols[1]] + data['sat_results'][cols[2]]
print(data['sat_results']['sat_score'].head())
```

## 13: Parsing Geographic Coordinates For Schools

Next, we'll want to parse the latitude and longitude coordinates for each school. This will enable us to map the schools and uncover any geographic patterns in the data. The coordinates are currently in the text field `Location 1` in the `hs_directory` data set.

We want to extract the coordinates, which are in parentheses at the end of the field. Here's an example:

```
1110 Boston Road\nBronx, NY 10456\n(40.8276026690005, -73.90447525699966)
```

We want to extract the latitude, `40.8276026690005`, and the longitude, `-73.90447525699966`. We can do the extraction with a regular expression. 

```
import re
re.findall("\(.+, .+\)", "1110 Boston Road\nBronx, NY 10456\n(40.8276026690005, -73.90447525699966)")
```

This command will return `(40.8276026690005, -73.90447525699966)`.

```
import re
def find_lat(loc):
    coords = re.findall("\(.+, .+\)", loc)
    lat = coords[0].split(",")[0].replace("(", "")
    return lat

data["hs_directory"]["lat"] = data["hs_directory"]["Location 1"].apply(find_lat)

print(data["hs_directory"].head())
```

## 14: Extracting The Longitude

```
import re
def find_lon(loc):
    coords = re.findall("\(.+, .+\)", loc)
    lon = coords[0].split(",")[1].replace(")", "").strip()
    return lon

data["hs_directory"]["lon"] = data["hs_directory"]["Location 1"].apply(find_lon)

data["hs_directory"]["lat"] = pd.to_numeric(data["hs_directory"]["lat"], errors="coerce")
data["hs_directory"]["lon"] = pd.to_numeric(data["hs_directory"]["lon"], errors="coerce")

print(data["hs_directory"].head())
```

## 15: Next Steps

We're almost ready to combine our data sets! We've come a long way in this mission -- we've gone from choosing a topic for a project to acquiring the data to having clean data that we're almost ready to combine.

Along the way, we've learned how to:

- Handle files with different formats and columns
- Prepare to merge multiple files
- Use text processing to extract coordinates from a string
- Convert columns from strings to numbers

You'll always learn something new while working on a real-world data science project. Each project is unique, and there will always be quirks you don't quite know how to handle. The key is to be willing to try different approaches, and to have a general framework in your head for how to move from Step A to Step B.

In the next mission, we'll finish cleaning the data sets, then combine them so we can start our analysis.

# Data Cleaning Walk-Through: Combining the Data

## 3: Condensing The Class Size Data Set

The first data set that we'll condense is `class_size`. The first few rows all pertain to the same school, which is why the `DBN` appears more than once. 

If we look at the unique values for `GRADE`, we get the following:

```
array(['0K', '01', '02', '03', '04', '05', '0K-09', nan, '06', '07', '08',
       'MS Core', '09-12', '09'], dtype=object)
```

Because we're dealing with high schools, we're only concerned with grades `9` through `12`. That means we only want to pick rows where the value in the `GRADE` column is `09-12`.

If we look at the unique values for `PROGRAM TYPE`, we get the following:

```
array(['GEN ED', 'CTT', 'SPEC ED', nan, 'G&T'], dtype=object)
```

Each school can have multiple program types. Because `GEN ED` is the largest category by far, let's only select rows where `PROGRAM TYPE` is `GEN ED`.

```
class_size = data["class_size"]
class_size = class_size[class_size["GRADE "] == "09-12"]
class_size = class_size[class_size["PROGRAM TYPE"] == "GEN ED"]
print(class_size.head())
```

## 5: Computing Average Class Sizes

As we saw when we displayed `class_size` on the last screen, `DBN` still isn't completely unique. This is due to the `CORE COURSE (MS CORE and 9-12 ONLY)` and `CORE SUBJECT (MS CORE and 9-12 ONLY)`  seem to pertain to different kinds of classes.  We want our class size data to include every single class a school offers -- not just a subset of them. What we can do is take the average across all of the classes a school offers.

After we group a dataframe and aggregate data based on it, the column we performed the grouping on (in this case `DBN`) will become the index, and will no longer appear as a column in the data itself. To undo this change and keep `DBN` as a column, we'll need to use [pandas.DataFrame.reset_index()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.reset_index.html). This method will reset the index to a list of integers and make `DBN` a column again.

```
import numpy
class_size = class_size.groupby("DBN").agg(numpy.mean)
class_size.reset_index(inplace=True)
data["class_size"] = class_size
print(data["class_size"].head())
```
## 6: Condensing The Demographics Data Set

In `demographics`, the only column that prevents a given `DBN` from being unique is `schoolyear`. We only want to select rows where `schoolyear` is `20112012`. This will give us the most recent year of data, and also match our SAT results data.

```
data["demographics"] = data["demographics"][data["demographics"]["schoolyear"] == 20112012]
print(data["demographics"].head())
```

## 8: Condensing The Graduation Data Set

In `graduation` data, the `Demographic` and `Cohort` columns are what prevent `DBN` from being unique. We want to pick data from the most recent `Cohort` available, which is `2006`. We also want data from the full cohort, so we'll only pick rows where `Demographic` is `Total Cohort`.

```
data["graduation"] = data["graduation"][data["graduation"]["Cohort"] == "2006"]
data["graduation"] = data["graduation"][data["graduation"]["Demographic"] == "Total Cohort"]
print(data["graduation"].head())
```

## 10: Converting AP Test Scores

We're almost ready to combine all of the data sets. The only remaining thing to do is convert the [Advanced Placement](https://en.wikipedia.org/wiki/Advanced_Placement_exams) (AP) test scores from strings to numeric values. 

There are three columns we'll need to convert:

- `AP Test Takers` (note that there's a trailing space in the column name)
- `Total Exams Taken`
- `Number of Exams with scores 3 4 or 5`

```
cols = ['AP Test Takers ', 'Total Exams Taken', 'Number of Exams with scores 3 4 or 5']
for col in cols:
    data["ap_2010"][col] = pd.to_numeric(data["ap_2010"][col], errors="coerce")
    
print(data["ap_2010"].head())
```

## 12: Performing The Left Joins

Both the `ap_2010` and the `graduation`data sets have many missing `DBN` values, so we'll use a `left` join when we merge the `sat_results` data set with them. Because we're using a `left` join, our final dataframe will have all of the same `DBN`values as the original `sat_results`dataframe.

```
combined = data["sat_results"]
combined = combined.merge(data["ap_2010"], on="DBN", how="left")
combined = combined.merge(data["graduation"], on="DBN", how="left")
print(combined.head(5))
print(combined.shape)
```

