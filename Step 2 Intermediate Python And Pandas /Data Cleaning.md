# Data Cleaning Walkthrough

## 1: Introduction

At many points in your career, you'll need to be able to build complete, end-to-end data science projects on your own. Data science projects usually consist of one of two things:

- An exploration and analysis of a set of data. One example might involve analyzing donors to political campaigns, creating a plot, and then sharing an analysis of the plot with others.
- An operational system that generates predictions based on data that updates continually. An algorithm that pulls in daily stock ticker data and predicts which stock prices will rise and fall would be one example.

You'll find the ability to create data science projects useful in several different contexts:

- Projects will help you build a portfolio, which is critical to finding a job as a data analyst or scientist.
- Working on projects will help you learn new skills and reinforce existing concepts.
- Most "real-world" data science and analysis work consists of developing internal projects.
- Projects allow you to investigate interesting phenomena and satisfy your curiosity.

Whether you aim to become a data scientist or analyst or you're just curious about the world, building projects can be immensely rewarding.

[Here's](https://github.com/dataquestio/loan-prediction) an example of a finished project.

In this mission, we'll walk through the first part of a complete data science project, including how to acquire the raw data. The project will focus on exploring and analyzing a data set. We'll develop our data cleaning and storytelling skills, which will enable us to build complete projects on our own.

We'll focus primarily on data exploration in this mission. We'll also combine several messy data sets into a single clean one to make analysis easier. Over the next few missions, we'll work through the rest of our project and perform the actual analysis.

The first step in creating a project is to decide on a topic. You want the topic to be something you're interested in and motivated to explore. It's very obvious when people are making projects just to make them, rather than out of a genuine interest in the topic.

Here are two ways to go about finding a good topic:

- Think about what sectors or angles you're really interested in, then find data sets relating to those sectors.
- Review several data sets, and find one that seems interesting enough to explore.

Whichever approach you take, you can start your search at these sites:

- [Data.gov](https://www.data.gov/) - A directory of government data downloads
- [/r/datasets](https://reddit.com/r/datasets) - A subreddit that has hundreds of interesting data sets
- [Awesome datasets](https://github.com/caesar0301/awesome-public-datasets) - A list of data sets hosted on GitHub
- [rs.io](http://rs.io/100-interesting-data-sets-for-statistics/) - A great blog post with hundreds of interesting data sets

In real-world data science, you may not find an ideal data set. You might have to aggregate disparate data sources instead, or do a good amount of data cleaning.

For the purposes of this project, we'll be using data about New York City public schools, which can be found [here](https://data.cityofnewyork.us/browse?category=Education).

## 2: Finding All Of The Relevant Data Sets

Once you've chosen a topic, you'll want to pick an angle to investigate. It's important to choose an angle that has enough depth to analyze, but isn't so complicated that it's difficult to get started. You want to finish the project, and you want your results to be interesting to others.

One of the most controversial issues in the U.S. educational system is the efficacy of standardized tests, and whether they're unfair to certain groups. Given our prior knowledge of this topic, investigating the correlations between [SAT scores](https://en.wikipedia.org/wiki/SAT) and demographics might be an interesting angle to take. We could correlate SAT scores with factors like race, gender, income, and more.

The SAT, or Scholastic Aptitude Test, is an exam that U.S. high school students take before applying to college. Colleges take the test scores into account when deciding who to admit, so it's fairly important to perform well on it.

The test consists of three sections, each of which has 800 possible points. The combined score is out of 2,400 possible points (while this number has changed a few times, the data set for our project is based on 2,400 total points). Organizations often rank high schools by their average SAT scores. The scores are also considered a measure of overall school district quality.

New York City makes its [data on high school SAT scores](https://data.cityofnewyork.us/Education/SAT-Results/f9bf-2cp4) available online, as well as the [demographics for each high school](https://data.cityofnewyork.us/Education/DOE-High-School-Directory-2014-2015/n3p6-zve2). The first few rows of the SAT data look like this:

![SAT](https://s3.amazonaws.com/dq-content/sat.png)

Unfortunately, combining both of the data sets won't give us all of the demographic information we want to use. We'll need to supplement our data with other sources to do our full analysis.

The same website has several related data sets covering demographic information and test scores. Here are the links to all of the data sets we'll be using:

- [SAT scores by school](https://data.cityofnewyork.us/Education/SAT-Results/f9bf-2cp4) - SAT scores for each high school in New York City
- [School attendance](https://data.cityofnewyork.us/Education/School-Attendance-and-Enrollment-Statistics-by-Dis/7z8d-msnt) - Attendance information for each school in New York City
- [Class size](https://data.cityofnewyork.us/Education/2010-2011-Class-Size-School-level-detail/urz7-pzb3) - Information on class size for each school
- [AP test results](https://data.cityofnewyork.us/Education/AP-College-Board-2010-School-Level-Results/itfs-ms3e) - Advanced Placement (AP) exam results for each high school (passing an optional AP exam in a particular subject can earn a student college credit in that subject)
- [Graduation outcomes](https://data.cityofnewyork.us/Education/Graduation-Outcomes-Classes-Of-2005-2010-School-Le/vh2h-md7a) - The percentage of students who graduated, and other outcome information
- [Demographics](https://data.cityofnewyork.us/Education/School-Demographics-and-Accountability-Snapshot-20/ihfw-zy9j) - Demographic information for each school
- [School survey](https://data.cityofnewyork.us/Education/NYC-School-Survey-2011/mnz3-dyi8) - Surveys of parents, teachers, and students at each school

All of these data sets are interrelated. We'll need to combine them into a single data set before we can find correlations.

## 3: Finding Background Information

Before we move into coding, we'll need to do some background research. A thorough understanding of the data will help us avoid costly mistakes, such as thinking that a column represents something other than what it does. Background research will also give us a better understanding of how to combine and analyze the data.

In this case, we'll want to research:

- [New York City](https://en.wikipedia.org/wiki/New_York_City)
- [The SAT](https://en.wikipedia.org/wiki/SAT)
- [Schools in New York City](https://en.wikipedia.org/wiki/List_of_high_schools_in_New_York_City)
- [Our data](https://data.cityofnewyork.us/data?cat=education)

We can learn a few different things from these resources. For example:

- Only high school students take the SAT, so we'll want to focus on high schools.
- New York City is made up of five boroughs, which are essentially distinct regions.
- New York City schools fall within several different school districts, each of which can contains dozens of schools.
- Our data sets include several different types of schools. We'll need to clean them so that we can focus on high schools only.
- Each school in New York City has a unique code called a `DBN`, or district borough number.
- Aggregating data by district will allow us to use the district mapping data to plot district-by-district differences.

## 4: Reading In The Data

Once we've done our background research, we're ready to read in the data. For your convenience, we've placed all the data into the `schools` folder. Here are all of the files in the folder:

- `ap_2010.csv` - Data on [AP test results](https://data.cityofnewyork.us/Education/AP-College-Board-2010-School-Level-Results/itfs-ms3e)
- `class_size.csv` - Data on [class size](https://data.cityofnewyork.us/Education/2010-2011-Class-Size-School-level-detail/urz7-pzb3)
- `demographics.csv` - Data on [demographics](https://data.cityofnewyork.us/Education/School-Demographics-and-Accountability-Snapshot-20/ihfw-zy9j)
- `graduation.csv` - Data on [graduation outcomes](https://data.cityofnewyork.us/Education/Graduation-Outcomes-Classes-Of-2005-2010-School-Le/vh2h-md7a)
- `hs_directory.csv` - A directory of [high schools](https://data.cityofnewyork.us/Education/DOE-High-School-Directory-2014-2015/n3p6-zve2)
- `sat_results.csv` - Data on [SAT scores](https://data.cityofnewyork.us/Education/SAT-Results/f9bf-2cp4)
- `survey_all.txt` - Data on [surveys](https://data.cityofnewyork.us/Education/NYC-School-Survey-2011/mnz3-dyi8) from all schools
- `survey_d75.txt` - Data on [surveys](https://data.cityofnewyork.us/Education/NYC-School-Survey-2011/mnz3-dyi8) from New York City [district 75](http://schools.nyc.gov/academics/specialEducation/D75/default.htm)

`survey_all.txt` and `survey_d75.txt` are in more complicated formats than the other files. For now, we'll focus on reading in the `CSV` files only, and then explore them.

We'll read each file into a [pandas dataframe](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html), and then store all of the dataframes in a dictionary. This will give us a convenient way to store them, and a quick way to reference them later on.

### Instructions

- Read each of the files in the list `data_files` into a pandas dataframe using the [pandas.read_csv()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html) function.Recall that all of the data sets are in the `schools`folder. That means the path to `ap_2010.csv` is `schools/ap_2010.csv`.
- Add each of the dataframes to the dictionary `data`, using the base of the filename as the key. For example, you'd enter `ap_2010` for the file `ap_2010.csv`.
- Afterwards, `data` should have the following keys:`ap_2010``class_size``demographics``graduation``hs_directory``sat_results`
- In addition, each key in `data` should have the corresponding dataframe as its value.

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

What we're mainly interested in is the SAT data set, which corresponds to the dictionary key `sat_results`. This data set contains the SAT scores for each high school in New York City. We eventually want to correlate selected information from this data set with information in the other data sets.

Let's explore `sat_results` to see what we can discover. Exploring the dataframe will help us understand the structure of the data, and make it easier for us to analyze it.

### Instructions

- Display the first five rows of the SAT scores data.
  - Use the key `sat_results` to access the SAT scores dataframe stored in the dictionary `data`.
  - Use the [pandas.DataFrame.head()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.head.html) method along with the [print()](https://docs.python.org/3/library/functions.html#print) function to display the first five rows of the dataframe.

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
- We can tell from the first few rows of names that we only have data about high schools.
- There's only a single row for each high school, so each `DBN` is unique in the SAT data.
- We may eventually want to combine the three columns that contain SAT scores -- `SAT Critical Reading Avg.`, `Score SAT Math Avg. Score`, and `SAT Writing Avg. Score` -- into a single column to make the scores easier to analyze.

Given these observations, let's explore the other data sets to see if we can gain any insight into how to combine them.

### Instructions

- Loop through each key in `data`. For each key:Display the first five rows of the dataframe associated with the key.

```python
for k in data:
    print(data[k].head())
```

## 7: Reading In The Survey Data

In the last step, we saw a group of dataframes that looked like this:

```
CSD BOROUGH SCHOOL CODE                SCHOOL NAME GRADE  PROGRAM TYPE  \
0    1       M        M015  P.S. 015 Roberto Clemente     0K       GEN ED
1    1       M        M015  P.S. 015 Roberto Clemente     0K          CTT
2    1       M        M015  P.S. 015 Roberto Clemente     01       GEN ED
3    1       M        M015  P.S. 015 Roberto Clemente     01          CTT
4    1       M        M015  P.S. 015 Roberto Clemente     02       GEN ED
```

We can make some observations based on the first few rows of each one.

- Each data set appears to either have a `DBN` column, or the information we need to create one. That means we can use a `DBN` column to combine the data sets. First we'll pinpoint matching rows from different data sets by looking for identical `DBN`s, then group all of their columns together in a single data set.
- Some fields look interesting for mapping -- particularly `Location 1`, which contains coordinates inside a larger string.
- Some of the data sets appear to contain multiple rows for each school (because the rows have duplicate `DBN` values). That means we’ll have to do some preprocessing to ensure that each `DBN` is unique within each data set. If we don't do this, we'll run into problems when we combine the data sets, because we might be merging two rows in one data set with one row in another data set.

Before we proceed with the merge, we should make sure we have all of the data we want to unify. We mentioned the survey data earlier (`survey_all.txt` and `survey_d75.txt`), but we didn't read those files in because they're in a slightly more complex format.

Each survey text file looks like this:

```
dbn bn  schoolname  d75 studentssurveyed    highschool  schooltype  rr_s
"01M015"    "M015"  "P.S. 015 Roberto Clemente" 0   "No"    0   "Elementary School"     88
```

The files are *tab delimited* and encoded with `Windows-1252` encoding. An encoding defines how a computer stores the contents of a file in binary. The most common encodings are `UTF-8` and `ASCII`. `Windows-1252` is rarely used, and can cause errors if we read such a file in without specifying the encoding. If you'd like to read more about encodings, [here's](http://kunststube.net/encoding/) a good primer.

We'll need to specify the encoding and delimiter to the pandas [pandas.read_csv()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html) function to ensure it reads the surveys in properly.

After we read in the survey data, we'll want to combine it into a single dataframe. We can do this by calling the [pandas.concat()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.concat.html) function:

```
z = pd.concat([x,y], axis=0)
```

The code above will combine dataframes `x` and `y` by essentially appending `y` to the end of `x`. The combined dataframe `z` will have the number of rows in `x` plus the number of rows in `y`.

## 8: Reading In The Survey Data

### Instructions

- Read in `survey_all.txt`.Use the [pandas.read_csv()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html) function to read `survey_all.txt` into the variable `all_survey`. Recall that this file is located in the `schools` folder.Specify the keyword argument `delimiter="\t"`.Specify the keyword argument `encoding="windows-1252"`.
- Read in `survey_d75.txt`.Use the `pandas.read_csv()` function to read `schools/survey_d75.txt` into the variable `d75_survey`. Recall that this file is located in the `schools` folder.Specify the keyword argument `delimiter="\t"`.Specify the keyword argument `encoding="windows-1252"`.
- Combine `d75_survey` and `all_survey` into a single dataframe.Use the pandas [concat()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.concat.html) function with the keyword argument `axis=0` to combine `d75_survey` and `all_survey` into the dataframe `survey`.Pass in `all_survey` first, then `d75_survey` when calling the `pandas.concat()` function.
- Display the first five rows of `survey` using the [pandas.DataFrame.head()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.head.html) function.

```
all_survey = pd.read_csv("schools/survey_all.txt", delimiter="\t", encoding='windows-1252')
d75_survey = pd.read_csv("schools/survey_d75.txt", delimiter="\t", encoding='windows-1252')
survey = pd.concat([all_survey, d75_survey], axis=0)

print(survey.head())
```

## 9: Cleaning Up The Surveys

In the last step, you should have seen output that looked like this:

```
N_p  N_s  N_t  aca_p_11  aca_s_11  aca_t_11  aca_tot_11    bn  com_p_11  \
0   90  NaN   22       7.8       NaN       7.9         7.9  M015       7.6   
1  161  NaN   34       7.8       NaN       9.1         8.4  M019       7.6
```

There are two immediate facts that we can see in the data:

- There are over `2000` columns, nearly all of which we don't need. We'll have to filter the data to remove the unnecessary ones. Working with fewer columns will make it easier to print the dataframe out and find correlations within it.
- The survey data has a `dbn` column that we'll want to convert to uppercase (`DBN`). The conversion will make the column name consistent with the other data sets.

First, we'll need filter the columns to remove the ones we don't need. Luckily, there's a data dictionary at the [original data download location](https://data.cityofnewyork.us/Education/NYC-School-Survey-2011/mnz3-dyi8). The dictionary tells us what each column represents. Based on our knowledge of the problem and the analysis we're trying to do, we can use the data dictionary to determine which columns to use.

Here's a preview of the data dictionary:

![img](https://s3.amazonaws.com/dq-content/xj5ud4r.png)

Based on the dictionary, it looks like these are the relevant columns:

```
["dbn", "rr_s", "rr_t", "rr_p", "N_s", "N_t", "N_p", "saf_p_11", "com_p_11", "eng_p_11", "aca_p_11", "saf_t_11", "com_t_11", "eng_t_11", "aca_t_11", "saf_s_11", "com_s_11", "eng_s_11", "aca_s_11", "saf_tot_11", "com_tot_11", "eng_tot_11", "aca_tot_11"]
```

These columns will give us aggregate survey data about how parents, teachers, and students feel about school safety, academic performance, and more. It will also give us the `DBN`, which allows us to uniquely identify the school.

Before we filter columns out, we'll want to copy the data from the `dbn` column into a new column called `DBN`. We can copy columns like this:

```
survey["new_column"] = survey["old_column"]
```

### Instructions

- Copy the data from the `dbn` column of `survey` into a new column in `survey` called `DBN`.
- Filter `survey` so it only contains the columns we listed above. You can do this using [pandas.DataFrame.loc[\]](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.loc.html).Remember that we renamed `dbn` to `DBN`; be sure to change the list of columns we want to keep accordingly.
- Assign the dataframe `survey` to the key `survey` in the dictionary `data`.
- When you're finished, the value in `data["survey"]` should be a dataframe with `23` columns and `1702` rows.

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

## 10: Inserting DBN Fields

When we explored all of the data sets, we noticed that some of them, like `class_size` and `hs_directory`, don't have a `DBN` column. `hs_directory` does have a `dbn` column, though, so we can just rename it.

However, `class_size` doesn't appear to have the column at all. Here are the first few rows of the data set:

```
CSD BOROUGH SCHOOL CODE                SCHOOL NAME GRADE  PROGRAM TYPE  \
0    1       M        M015  P.S. 015 Roberto Clemente     0K       GEN ED
1    1       M        M015  P.S. 015 Roberto Clemente     0K          CTT
2    1       M        M015  P.S. 015 Roberto Clemente     01       GEN ED
3    1       M        M015  P.S. 015 Roberto Clemente     01          CTT
4    1       M        M015  P.S. 015 Roberto Clemente     02       GEN ED
```

Here are the first few rows of the `sat_results` data, which does have a `DBN` column:

```
DBN                                    SCHOOL NAME  \
0  01M292  HENRY STREET SCHOOL FOR INTERNATIONAL STUDIES
1  01M448            UNIVERSITY NEIGHBORHOOD HIGH SCHOOL
2  01M450                     EAST SIDE COMMUNITY SCHOOL
3  01M458                      FORSYTH SATELLITE ACADEMY
4  01M509                        MARTA VALLE HIGH SCHOOL
```

From looking at these rows, we can tell that the `DBN` in the `sat_results` data is just a combination of the `CSD` and `SCHOOL CODE` columns in the `class_size` data. The main difference is that the `DBN` is padded, so that the `CSD` portion of it always consists of two digits. That means we'll need to add a leading `0` to the `CSD` if the `CSD` is less than two digits long. Here's a diagram illustrating what we need to do:

| CSD  | Padded CSD |
| ---- | ---------- |
| 1    | 01         |
| 19   | 19         |
| 2    | 02         |
| 99   | 99         |

As you can see, whenever the `CSD` is less than two digits long, we need to add a leading `0`. We can accomplish this using the pandas [pandas.DataFrame.apply()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.apply.html) method, along with a custom function that:

- Takes in a number.
- Converts the number to a string using the [str()](https://docs.python.org/3/library/stdtypes.html#str) function.
- Check the length of the string using the [len()](https://docs.python.org/3/library/functions.html#len) function.If the string is two digits long, returns the string.If the string is one digit long, adds a `0` to the front of the string, then returns it.You can use the string method [zfill()](https://docs.python.org/3/library/stdtypes.html#str.zfill) to do this.

Once we've padded the `CSD`, we can use the addition operator (`+`) to combine the values in the `CSD` and `SCHOOL CODE` columns. Here's an example of how we would do this:

```
dataframe["new_column"] = dataframe["column_one"] + dataframe["column_two"]
```

And here's a diagram illustrating the basic concept:

Padded CSD + School Code = DBN

01 + M015 = 01M015

## 11: Inserting DBN Fields

### Instructions

- Copy the `dbn` column in `hs_directory` into a new column called `DBN`.
- Create a new column called `padded_csd` in the `class_size` data set.Use the [pandas.DataFrame.apply()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.apply.html) method along with a custom function to generate this column.Make sure to apply the function along the `data["class_size"]["CSD"]` column.
- Use the addition operator (`+`) along with the `padded_csd`and `SCHOOL CODE` columns of `class_size`, then assign the result to the `DBN` column of `class_size`.
- Display the first few rows of `class_size` to double check the `DBN` column.

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

Now we're almost ready to combine our data sets. Before we do, let's take some time to calculate variables that will be useful in our analysis. We've already discussed one such variable -- a column that totals up the SAT scores for the different sections of the exam. This will make it much easier to correlate scores with demographic factors because we'll be working with a single number, rather than three different ones.

Before we can generate this column, we'll need to convert the `SAT Math Avg. Score`, `SAT Critical Reading Avg. Score`, and `SAT Writing Avg. Score` columns in the `sat_results` data set from the object (string) data type to a numeric data type. We can use the [pandas.to_numeric()](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.to_numeric.html) method for the conversion. If we don't convert the values, we won't be able to add the columns together.

It's important to pass the keyword argument `errors="coerce"` when we call `pandas.to_numeric()`, so that pandas treats any invalid strings it can't convert to numbers as missing values instead.

After we perform the conversion, we can use the addition operator (`+`) to add all three columns together.

### Instructions

- Convert the `SAT Math Avg. Score`, `SAT Critical Reading Avg. Score`, and `SAT Writing Avg. Score`columns in the `sat_results` data set from the object (string) data type to a numeric data type.Use the [pandas.to_numeric()](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.to_numeric.html) function on each of the columns, and assign the result back to the same column.Pass in the keyword argument `errors="coerce"`.
- Create a column called `sat_score` in `sat_results` that holds the combined SAT score for each student.Add up `SAT Math Avg. Score`, `SAT Critical Reading Avg. Score`, and `SAT Writing Avg. Score`, and assign the total to the `sat_score` column of `sat_results`.
- Display the first few rows of the `sat_score` column of `sat_results` to verify that everything went okay.

```
cols = ['SAT Math Avg. Score', 'SAT Critical Reading Avg. Score', 'SAT Writing Avg. Score']
for c in cols:
    data["sat_results"][c] = pd.to_numeric(data["sat_results"][c], errors="coerce")

data['sat_results']['sat_score'] = data['sat_results'][cols[0]] + data['sat_results'][cols[1]] + data['sat_results'][cols[2]]
print(data['sat_results']['sat_score'].head())
```

## 13: Parsing Geographic Coordinates For Schools

Next, we'll want to parse the latitude and longitude coordinates for each school. This will enable us to map the schools and uncover any geographic patterns in the data. The coordinates are currently in the text field `Location 1` in the `hs_directory` data set.

Let's take a look at the first few rows:

```
0    883 Classon Avenue\nBrooklyn, NY 11225\n(40.67...
1    1110 Boston Road\nBronx, NY 10456\n(40.8276026...
2    1501 Jerome Avenue\nBronx, NY 10452\n(40.84241...
3    411 Pearl Street\nNew York, NY 10038\n(40.7106...
4    160-20 Goethals Avenue\nJamaica, NY 11432\n(40...
```

As you can see, this field contains a lot of information we don't need. We want to extract the coordinates, which are in parentheses at the end of the field. Here's an example:

```
1110 Boston Road\nBronx, NY 10456\n(40.8276026690005, -73.90447525699966)
```

We want to extract the latitude, `40.8276026690005`, and the longitude, `-73.90447525699966`. Taken together, latitude and longitude make up a pair of coordinates that allows us to pinpoint any location on Earth.

We can do the extraction with a regular expression. The following expression will pull out everything inside the parentheses:

```
import re
re.findall("\(.+, .+\)", "1110 Boston Road\nBronx, NY 10456\n(40.8276026690005, -73.90447525699966)")
```

This command will return `(40.8276026690005, -73.90447525699966)`. We'll need to process this result further using the string methods [split()](https://docs.python.org/3/library/stdtypes.html#str.split) and [replace()](https://docs.python.org/3/library/stdtypes.html#str.replace) methods to extract each coordinate.

### Instructions

- Write a function that:
  - Takes in a string
  - Uses the regular expression above to extract the coordinates
  - Uses string manipulation functions to pull out the latitude
  - Returns the latitude
- Use the [df.apply()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.apply.html) method to apply the function across the `Location 1` column of `hs_directory`. Assign the result to the `lat` column of `hs_directory`.
- Display the first few rows of `hs_directory` to verify the results.

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

On the last screen, we parsed the latitude from the `Location 1` column. Now we'll just need to do the same for the longitude.

Once we have both coordinates, we'll need to convert them to numeric values. We can use the `pandas.to_numeric()` function to convert them from strings to numbers.

### Instructions

- Write a function that:
  - Takes in a string.
  - Uses the regular expression above to extract the coordinates.
  - Uses string manipulation functions to pull out the longitude.
  - Returns the longitude.
- Use the [df.apply()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.apply.html) method to apply the function across the `Location 1` column of `hs_directory`. Assign the result to the `lon` column of `hs_directory`.
- Use the [to_numeric()](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.to_numeric.html) function to convert the `lat` and `lon`columns of `hs_directory` to numbers.Specify the `errors="coerce"` keyword argument to handle missing values properly.
- Display the first few rows of `hs_directory` to verify the results.

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

## 1: Introduction

In the last mission, we began investigating possible relationships between SAT scores and demographic factors. In order to do this, we acquired several data sets about [New York City public schools](https://data.cityofnewyork.us/data?cat=education). We manipulated these data sets, and found that we could combine them all using the `DBN` column. All of the data sets are currently stored as keys in the `data` dictionary. Each individual data set is a pandas dataframe.

In this mission, we'll clean the data a bit more, then combine it. Finally, we'll compute correlations and perform some analysis.

The first thing we'll need to do in preparation for the merge is condense some of the data sets. In the last mission, we noticed that the values in the `DBN` column were unique in the `sat_results` data set. Other data sets like `class_size` had duplicate `DBN` values, however.

We'll need to condense these data sets so that each value in the `DBN` column is unique. If not, we'll run into issues when it comes time to combine the data sets.

While the main data set we want to analyze, `sat_results`, has unique `DBN` values for every high school in New York City, other data sets aren't as clean. A single row in the `sat_results` data set may match multiple columns in the `class_size` data set, for example. This situation will create problems, because we don't know which of the multiple entries in the `class_size` data set we should combine with the single matching entry in `sat_results`. Here's a diagram that illustrates the problem:

| sat_results |      | class_size |      |
| ----------- | ---- | ---------- | ---- |
| DBN         | ...  | DBN        | ...  |
| 01M022      | ...  | 01M022     | ...  |
| 05M345      | ...  | 01M022     | ...  |
| 02M456      | ...  | 05M345     | ...  |
| 99M520      | ...  | 05M345     | ...  |

In the diagram above, we can't just combine the rows from both data sets because there are several cases where multiple rows in `class_size` match a single row in `sat_results`.

To resolve this issue, we'll condense the `class_size`, `graduation`, and `demographics` data sets so that each `DBN` is unique.

## 2: Condensing The Class Size Data Set

The first data set that we'll condense is `class_size`. The first few rows of `class_size` look like this:

|      | CSD  | BOROUGH | SCHOOL CODE | SCHOOL NAME               | GRADE | PROGRAM TYPE | CORE SUBJECT (MS CORE and 9-12 ONLY) | CORE COURSE (MS CORE and 9-12 ONLY) | SERVICE CATEGORY(K-9* ONLY) | NUMBER OF STUDENTS / SEATS FILLED | NUMBER OF SECTIONS | AVERAGE CLASS SIZE | SIZE OF SMALLEST CLASS | SIZE OF LARGEST CLASS | DATA SOURCE | SCHOOLWIDE PUPIL-TEACHER RATIO | padded_csd | DBN    |
| ---- | ---- | ------- | ----------- | ------------------------- | ----- | ------------ | ------------------------------------ | ----------------------------------- | --------------------------- | --------------------------------- | ------------------ | ------------------ | ---------------------- | --------------------- | ----------- | ------------------------------ | ---------- | ------ |
| 0    | 1    | M       | M015        | P.S. 015 Roberto Clemente | 0K    | GEN ED       | -                                    | -                                   | -                           | 19.0                              | 1.0                | 19.0               | 19.0                   | 19.0                  | ATS         | NaN                            | 01         | 01M015 |
| 1    | 1    | M       | M015        | P.S. 015 Roberto Clemente | 0K    | CTT          | -                                    | -                                   | -                           | 21.0                              | 1.0                | 21.0               | 21.0                   | 21.0                  | ATS         | NaN                            | 01         | 01M015 |
| 2    | 1    | M       | M015        | P.S. 015 Roberto Clemente | 01    | GEN ED       | -                                    | -                                   | -                           | 17.0                              | 1.0                | 17.0               | 17.0                   | 17.0                  | ATS         | NaN                            | 01         | 01M015 |
| 3    | 1    | M       | M015        | P.S. 015 Roberto Clemente | 01    | CTT          | -                                    | -                                   | -                           | 17.0                              | 1.0                | 17.0               | 17.0                   | 17.0                  | ATS         | NaN                            | 01         | 01M015 |
| 4    | 1    | M       | M015        | P.S. 015 Roberto Clemente | 02    | GEN ED       | -                                    | -                                   | -                           | 15.0                              | 1.0                | 15.0               | 15.0                   | 15.0                  | ATS         | NaN                            | 01         | 01M015 |

As you can see, the first few rows all pertain to the same school, which is why the `DBN` appears more than once. It looks like each school has multiple values for `GRADE`, `PROGRAM TYPE`, `CORE SUBJECT (MS CORE and 9-12 ONLY)`, and `CORE COURSE (MS CORE and 9-12 ONLY)`.

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

## 3: Condensing The Class Size Data Set

### Instructions

- Create a new variable called `class_size`, and assign the value of `data["class_size"]` to it.
- Filter `class_size` so the `GRADE` column only contains the value `09-12`. Note that the name of the `GRADE` column has a space at the end; you'll generate an error if you don't include it.
- Filter `class_size` so that the `PROGRAM TYPE` column only contains the value `GEN ED`.
- Display the first five rows of `class_size` to verify.

```
class_size = data["class_size"]
class_size = class_size[class_size["GRADE "] == "09-12"]
class_size = class_size[class_size["PROGRAM TYPE"] == "GEN ED"]
print(class_size.head())
```

## 4: Computing Average Class Sizes

As we saw when we displayed `class_size` on the last screen, `DBN` still isn't completely unique. This is due to the `CORE COURSE (MS CORE and 9-12 ONLY)` and `CORE SUBJECT (MS CORE and 9-12 ONLY)` columns.

`CORE COURSE (MS CORE and 9-12 ONLY)` and `CORE SUBJECT (MS CORE and 9-12 ONLY)` seem to pertain to different kinds of classes. For example, here are the unique values for `CORE SUBJECT (MS CORE and 9-12 ONLY)`:

```
array(['ENGLISH', 'MATH', 'SCIENCE', 'SOCIAL STUDIES'], dtype=object)
```

This column only seems to include certain subjects. We want our class size data to include every single class a school offers -- not just a subset of them. What we can do is take the average across all of the classes a school offers. This will give us unique `DBN` values, while also incorporating as much data as possible into the average.

Fortunately, we can use the [pandas.DataFrame.groupby()](http://pandas.pydata.org/pandas-docs/stable/groupby.html) method to help us with this. The `DataFrame.groupby()` method will split a dataframe up into unique groups, based on a given column. We can then use the [agg()](http://pandas.pydata.org/pandas-docs/stable/groupby.html#aggregation) method on the resulting `pandas.core.groupby` object to find the mean of each column.

Let's say we have this data set:

| DBN    | CORE SUBJECT (MS CORE and 9 \| 12 ONLY) | AVERAGE CLASS SIZE |
| ------ | --------------------------------------- | ------------------ |
| 01M292 | ENGLISH                                 | 21.0               |
| 01M292 | ENGLISH                                 | 26.3               |
| 01M292 | ENGLISH                                 | 19.0               |
| 01M292 | ENGLISH                                 | 23.0               |
| 01M292 | MATH                                    | 17.7               |
| 01M292 | MATH                                    | 32.0               |
| 01M292 | MATH                                    | 19.7               |
| 01M292 | SCIENCE                                 | 31.3               |
| 01M292 | SCIENCE                                 | 29.0               |
| 01M292 | SCIENCE                                 | 19.6               |
| 01M292 | SCIENCE                                 | 13.0               |
| 01M292 | SCIENCE                                 | 21.3               |
| 01M292 | SOCIALSTUDIES                           | 22.3               |
| 01M292 | SOCIALSTUDIES                           | 20.7               |
| 01M332 | ENGLISH                                 | 20.0               |
| 01M332 | ENGLISH                                 | 24.0               |
| 01M378 | MATH                                    | 33.0               |
| 01M448 | ENGLISH                                 | 17.7               |
| 01M448 | ENGLISH                                 | 16.6               |
| 01M448 | ENGLISH                                 | 21.3               |

Using the groupby() method, we'll split this dataframe into four separate groups -- one with the `DBN` `01M292`, one with the `DBN` `01M332`, one with the `DBN` `01M378`, and one with the `DBN` `01M448`:

| DBN    | CORE SUBJECT (MS CORE and 9 \| 12 ONLY) | AVERAGE CLASS SIZE |
| ------ | --------------------------------------- | ------------------ |
| 01M292 | ENGLISH                                 | 21.0               |
| 01M292 | ENGLISH                                 | 26.3               |
| 01M292 | ENGLISH                                 | 19.0               |
| 01M292 | ENGLISH                                 | 23.0               |
| 01M292 | MATH                                    | 17.7               |
| 01M292 | MATH                                    | 32.0               |
| 01M292 | MATH                                    | 19.7               |
| 01M292 | SCIENCE                                 | 31.3               |
| 01M292 | SCIENCE                                 | 29.0               |
| 01M292 | SCIENCE                                 | 19.6               |
| 01M292 | SCIENCE                                 | 13.0               |
| 01M292 | SCIENCE                                 | 21.3               |
| 01M292 | SOCIALSTUDIES                           | 22.3               |
| 01M292 | SOCIALSTUDIES                           | 20.7               |

| DBN    | CORE SUBJECT (MS CORE and 9 \| 12 ONLY) | AVERAGE CLASS SIZE |
| ------ | --------------------------------------- | ------------------ |
| 01M332 | ENGLISH                                 | 20.0               |
| 01M332 | ENGLISH                                 | 24.0               |


| DBN    | CORE SUBJECT (MS CORE and 9 \| 12 ONLY) | AVERAGE CLASS SIZE |
| ------ | --------------------------------------- | ------------------ |
| 01M378 | MATH                                    | 33.0               |


| DBN    | CORE SUBJECT (MS CORE and 9 \| 12 ONLY) | AVERAGE CLASS SIZE |
| ------ | --------------------------------------- | ------------------ |
| 01M448 | ENGLISH                                 | 17.7               |
| 01M448 | ENGLISH                                 | 16.6               |
| 01M448 | ENGLISH                                 | 21.3               |
Then, we can compute the averages for the `AVERAGE CLASS SIZE` column in each of the four groups using the agg() method:

| DBN    | AVERAGE CLASS SIZE |
| ------ | ------------------ |
| 01M292 | 22.564286          |
| 01M332 | 22.000000          |
| 01M378 | 33.000000          |
| 01M448 | 18.533333          |

After we group a dataframe and aggregate data based on it, the column we performed the grouping on (in this case `DBN`) will become the index, and will no longer appear as a column in the data itself. To undo this change and keep `DBN` as a column, we'll need to use [pandas.DataFrame.reset_index()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.reset_index.html). This method will reset the index to a list of integers and make `DBN` a column again.

## 5: Computing Average Class Sizes

### Instructions

- Find the average values for each column associated with each `DBN` in `class_size`.Use the [pandas.DataFrame.groupby()](http://pandas.pydata.org/pandas-docs/stable/groupby.html) method to group `class_size` by `DBN`.Use the [agg()](http://pandas.pydata.org/pandas-docs/stable/groupby.html#aggregation) method on the resulting `pandas.core.groupby` object, along with the `numpy.mean()` function as an argument, to calculate the average of each group.Assign the result back to `class_size`.
- Reset the index to make `DBN` a column again.Use the [pandas.DataFrame.reset_index()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.reset_index.html) method, along with the keyword argument `inplace=True`.
- Assign `class_size` back to the `class_size` key of the `data` dictionary.
- Display the first few rows of `data["class_size"]` to verify that everything went okay.

```
import numpy
class_size = class_size.groupby("DBN").agg(numpy.mean)
class_size.reset_index(inplace=True)
data["class_size"] = class_size
print(data["class_size"].head())
```

## 6: Condensing The Demographics Data Set

Now that we've finished condensing `class_size`, let's condense `demographics`. The first few rows look like this:

|      | DBN    | Name                      | schoolyear | fl_percent | frl_percent | total_enrollment | prek | k    | grade1 | grade2 | ...  | black_num | black_per | hispanic_num | hispanic_per | white_num | white_per | male_num | male_per | female_num | female_per |
| ---- | ------ | ------------------------- | ---------- | ---------- | ----------- | ---------------- | ---- | ---- | ------ | ------ | ---- | --------- | --------- | ------------ | ------------ | --------- | --------- | -------- | -------- | ---------- | ---------- |
| 0    | 01M015 | P.S. 015 ROBERTO CLEMENTE | 20052006   | 89.4       | NaN         | 281              | 15   | 36   | 40     | 33     | ...  | 74        | 26.3      | 189          | 67.3         | 5         | 1.8       | 158.0    | 56.2     | 123.0      | 43.8       |
| 1    | 01M015 | P.S. 015 ROBERTO CLEMENTE | 20062007   | 89.4       | NaN         | 243              | 15   | 29   | 39     | 38     | ...  | 68        | 28.0      | 153          | 63.0         | 4         | 1.6       | 140.0    | 57.6     | 103.0      | 42.4       |
| 2    | 01M015 | P.S. 015 ROBERTO CLEMENTE | 20072008   | 89.4       | NaN         | 261              | 18   | 43   | 39     | 36     | ...  | 77        | 29.5      | 157          | 60.2         | 7         | 2.7       | 143.0    | 54.8     | 118.0      | 45.2       |
| 3    | 01M015 | P.S. 015 ROBERTO CLEMENTE | 20082009   | 89.4       | NaN         | 252              | 17   | 37   | 44     | 32     | ...  | 75        | 29.8      | 149          | 59.1         | 7         | 2.8       | 149.0    | 59.1     | 103.0      | 40.9       |
| 4    | 01M015 | P.S. 015 ROBERTO CLEMENTE | 20092010   |            | 96.5        | 208              | 16   | 40   | 28     | 32     | ...  | 67        | 32.2      | 118          | 56.7         | 6         | 2.9       | 124.0    | 59.6     | 84.0       | 40.4       |

In this case, the only column that prevents a given `DBN` from being unique is `schoolyear`. We only want to select rows where `schoolyear` is `20112012`. This will give us the most recent year of data, and also match our SAT results data.

## 7: Condensing The Demographics Data Set

### Instructions

- Filter `demographics`, only selecting rows in `data["demographics"]` where `schoolyear` is `20112012`.`schoolyear` is actually an integer, so be careful about how you perform your comparison.
- Display the first few rows of `data["demographics"]` to verify that the filtering worked.

```
data["demographics"] = data["demographics"][data["demographics"]["schoolyear"] == 20112012]
print(data["demographics"].head())
```

## 8: Condensing The Graduation Data Set

Finally, we'll need to condense the `graduation` data set. Here are the first few rows:

|      | Demographic  | DBN    | School Name                           | Cohort   | Total Cohort | Total Grads - n | Total Grads - % of cohort | Total Regents - n | Total Regents - % of cohort | Total Regents - % of grads | ...  | Regents w/o Advanced - n | Regents w/o Advanced - % of cohort | Regents w/o Advanced - % of grads | Local - n | Local - % of cohort | Local - % of grads  | Still Enrolled - n | Still Enrolled - % of cohort | Dropped Out - n | Dropped Out - % of cohort |
| ---- | ------------ | ------ | ------------------------------------- | -------- | ------------ | --------------- | ------------------------- | ----------------- | --------------------------- | -------------------------- | ---- | ------------------------ | ---------------------------------- | --------------------------------- | --------- | ------------------- | ------------------- | ------------------ | ---------------------------- | --------------- | ------------------------- |
| 0    | Total Cohort | 01M292 | HENRY STREET SCHOOL FOR INTERNATIONAL | 2003     | 5            | s               | s                         | s                 | s                           | s                          | ...  | s                        | s                                  | s                                 | s         | s                   | s                   | s                  | s                            | s               | s                         |
| 1    | Total Cohort | 01M292 | HENRY STREET SCHOOL FOR INTERNATIONAL | 2004     | 55           | 37              | 67.3%                     | 17                | 30.9%                       | 45.9%                      | ...  | 17                       | 30.9%                              | 45.9%                             | 20        | 36.4%               | 54.1%               | 15                 | 27.3%                        | 3               | 5.5%                      |
| 2    | Total Cohort | 01M292 | HENRY STREET SCHOOL FOR INTERNATIONAL | 2005     | 64           | 43              | 67.2%                     | 27                | 42.2%                       | 62.8%                      | ...  | 27                       | 42.2%                              | 62.8%                             | 16        | 25%                 | 37.200000000000003% | 9                  | 14.1%                        | 9               | 14.1%                     |
| 3    | Total Cohort | 01M292 | HENRY STREET SCHOOL FOR INTERNATIONAL | 2006     | 78           | 43              | 55.1%                     | 36                | 46.2%                       | 83.7%                      | ...  | 36                       | 46.2%                              | 83.7%                             | 7         | 9%                  | 16.3%               | 16                 | 20.5%                        | 11              | 14.1%                     |
| 4    | Total Cohort | 01M292 | HENRY STREET SCHOOL FOR INTERNATIONAL | 2006 Aug | 78           | 44              | 56.4%                     | 37                | 47.4%                       | 84.1%                      | ...  | 37                       | 47.4%                              | 84.1%                             | 7         | 9%                  | 15.9%               | 15                 | 19.2%                        | 11              | 14.1%                     |

The `Demographic` and `Cohort` columns are what prevent `DBN` from being unique in the `graduation` data. A `Cohort` appears to refer to the year the data represents, and the `Demographic` appears to refer to a specific demographic group. In this case, we want to pick data from the most recent `Cohort` available, which is `2006`. We also want data from the full cohort, so we'll only pick rows where `Demographic` is `Total Cohort`.

## 9: Condensing The Graduation Data Set

### Instructions

- Filter `graduation`, only selecting rows where the `Cohort`column equals `2006`.
- Filter `graduation`, only selecting rows where the `Demographic` column equals `Total Cohort`.
- Display the first few rows of `data["graduation"]` to verify that everything worked properly.

```
data["graduation"] = data["graduation"][data["graduation"]["Cohort"] == "2006"]
data["graduation"] = data["graduation"][data["graduation"]["Demographic"] == "Total Cohort"]
print(data["graduation"].head())
```

## 10: Converting AP Test Scores

We're almost ready to combine all of the data sets. The only remaining thing to do is convert the [Advanced Placement](https://en.wikipedia.org/wiki/Advanced_Placement_exams) (AP) test scores from strings to numeric values. High school students take the AP exams before applying to college. There are several AP exams, each corresponding to a school subject. High school students who earn high scores may receive college credit.

AP exams have a `1` to `5` scale; `3` or higher is a passing score. Many high school students take AP exams -- particularly those who attend academically challenging institutions. AP exams are much more rare in schools that lack funding or academic rigor.

It will be interesting to find out whether AP exam scores are correlated with SAT scores across high schools. To determine this, we'll need to convert the AP exam scores in the `ap_2010` data set to numeric values first.

There are three columns we'll need to convert:

- `AP Test Takers` (note that there's a trailing space in the column name)
- `Total Exams Taken`
- `Number of Exams with scores 3 4 or 5`

Note that the first column name above, `AP Test Takers`, has a trailing space at the end.

### Instructions

- Convert each of the following columns in `ap_2010` to numeric values using the [pandas.to_numeric()](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.to_numeric.html) function with the keyword argument `errors="coerce"`.`AP Test Takers``Total Exams Taken``Number of Exams with scores 3 4 or 5`
- Display the first few rows of `ap_2010` to confirm.

```
cols = ['AP Test Takers ', 'Total Exams Taken', 'Number of Exams with scores 3 4 or 5']
for col in cols:
    data["ap_2010"][col] = pd.to_numeric(data["ap_2010"][col], errors="coerce")
    
print(data["ap_2010"].head())
```

## 11: Left, Right, Inner, And Outer Joins

Before we merge our data, we'll need to decide on the merge strategy we want to use. We'll be using the pandas [pandas.DataFrame.merge()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.merge.html) function, which supports four types of joins -- `left`, `right`, `inner`, and `outer`. Each of these join types dictates how pandas combines the rows.

We'll be using the `DBN` column to identify matching rows across data sets. In other words, the values in that column will help us know which row from the first data set to combine with which row in the second data set.

There may be `DBN` values that exist in one data set but not in another. This is partly because the data is from different years. Each data set also has inconsistencies in terms of how it was gathered. Human error (and other types of errors) may also play a role. Therefore, we may not find matches for the `DBN` values in `sat_results`in all of the other data sets, and other data sets may have `DBN` values that don't exist in `sat_results`.

We'll merge two data sets at a time. For example, we'll merge `sat_results` and `hs_directory`, then merge the result with `ap_2010`, then merge the result of that with `class_size`. We'll continue combining data sets in this way until we've merged all of them. Afterwards, we'll have roughly the same number of rows, but each row will have columns from all of the data sets.

The merge strategy we pick will affect the number of rows we end up with. Let's take a look at each strategy.

Let's say we're merging the following two data sets:

| sat_results |           | class_size |                |
| ----------- | --------- | ---------- | -------------- |
| DBN         | sat_score | DBN        | avg_class_size |
| 01          | 1800      | 01         | 20             |
| 03          | 2200      | 03         | 30             |
| 99          | 1600      | 55         | 50             |
| 101         | 2300      | 101        | 30             |

With an `inner` merge, we'd only combine rows where the same `DBN` exists in both data sets. We'd end up with this result:

| combined |           |                |
| -------- | --------- | -------------- |
| DBN      | sat_score | avg_class_size |
| 01       | 1800      | 20             |
| 03       | 2200      | 30             |
| 101      | 2300      | 30             |

With a `left` merge, we'd only use `DBN` values from the dataframe on the "left" of the merge. In this case, `sat_results` is on the left. Some of the DBNs in `sat_results` don't exist in `class_size`, though. The merge will handle this by assiging null values to the columns in `sat_results` that don't have corresponding data in `class_size`.
| combined |           |                |
| -------- | --------- | -------------- |
| DBN      | sat_score | avg_class_size |
| 01       | 1800      | 20             |
| 03       | 2200      | 30             |
| 99       | 1600      | null           |
| 101      | 2300      | 30             |

With a `right` merge, we'll only use `DBN` values from the dataframe on the "right" of the merge. In this case, `class_size` is on the right:

| combined |           |                |
| -------- | --------- | -------------- |
| DBN      | sat_score | avg_class_size |
| 01       | 1800      | 20             |
| 03       | 2200      | 30             |
| 55       | null      | 50             |
| 101      | 2300      | 30             |

With an `outer` merge, we'll take any `DBN` values from either `sat_results` or `class_size`:

| combined |           |                |
| -------- | --------- | -------------- |
| DBN      | sat_score | avg_class_size |
| 01       | 1800      | 20             |
| 03       | 2200      | 30             |
| 99       | 1600      | null           |
| 55       | null      | 50             |
| 101      | 2300      | 30             |


As you can see, each merge strategy has its advantages. Depending on the strategy we choose, we may preserve rows at the expense of having more missing column data, or minimize missing data at the expense of having fewer rows. Choosing a merge strategy is an important decision; it's worth thinking about your data carefully, and what trade-offs you're willing to make.

Because this project is concerned with determing demographic factors that correlate with SAT score, we'll want to preserve as many rows as possible from `sat_results` while minimizing null values.

This means that we may need to use different merge strategies with different data sets. Some of the data sets have a lot of missing `DBN` values. This makes a `left` join more appropriate, because we don't want to lose too many rows when we merge. If we did an `inner` join, we would lose the data for many high schools.

Some data sets have `DBN` values that are almost identical to those in `sat_results`. Those data sets also have information we need to keep. Most of our analysis would be impossible if a significant number of rows was missing from `demographics`, for example. Therefore, we'll do an inner join to avoid missing data in these columns.

## 12: Performing The Left Joins

Both the `ap_2010` and the `graduation`data sets have many missing `DBN` values, so we'll use a `left` join when we merge the `sat_results` data set with them. Because we're using a `left` join, our final dataframe will have all of the same `DBN`values as the original `sat_results`dataframe.

We'll need to use the pandas df.merge() method to merge dataframes. The "left" dataframe is the one we call the method on, and the "right" dataframe is the one we pass into df.merge().

Because we're using the `DBN` column to join the dataframes, we'll need to specify the keyword argument `on="DBN"` when calling `pandas.DataFrame..merge()`.

First, we'll assign `data["sat_results"]` to the variable `combined`. Then, we'll merge all of the other dataframes with `combined`. When we're finished, `combined` will have all of the columns from all of the data sets.

### Instructions

- Use the pandas [pandas.DataFrame.merge()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.merge.html) method to merge the `ap_2010` data set into `combined`.Make sure to specify `how="left"` as a keyword argument to indicate the correct join type.Make sure to assign the result of the merge operation back to `combined`.
- Use the pandas df.merge() method to merge the `graduation` data set into `combined`.Make sure to specify `how="left"` as a keyword argument to get the correct join type.Make sure to assign the result of the merge operation back to `combined`.
- Display the first few rows of `combined` to verify that the correct operations occurred.
- Use the [pandas.DataFrame.shape()](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.DataFrame.shape.html) method to display the shape of the dataframe and see how many rows now exist.

```
combined = data["sat_results"]
combined = combined.merge(data["ap_2010"], on="DBN", how="left")
combined = combined.merge(data["graduation"], on="DBN", how="left")
print(combined.head(5))
print(combined.shape)
```

## 13: Performing The Inner Joins

Now that we've performed the `left`joins, we still have to merge `class_size`, `demographics`, `survey`, and `hs_directory` into `combined`. Because these files contain information that's more valuable to our analysis and also have fewer missing `DBN` values, we'll use the `inner` join type.

### Instructions

- Merge `class_size` into `combined`. Then, merge `demographics`, `survey`, and `hs_directory` into `combined` one by one, in that order.Be sure to follow the exact order above.Remember to specify the correct column to join on, as well as the correct join type.
- Display the first few rows of `combined` to verify that the correct operations occurred.
- Call `pandas.DataFrame.shape()` to display the shape of the dataframe to see how many rows now exist.

```
to_merge = ["class_size", "demographics", "survey", "hs_directory"]

for m in to_merge:
    combined = combined.merge(data[m], on="DBN", how="inner")

print(combined.head(5))
print(combined.shape)
```

## 14: Filling In Missing Values

You may have noticed that the inner joins resulted in `116` fewer rows in `sat_results`. This is because pandas couldn't find the `DBN` values that existed in `sat_results` in the other data sets. While this is worth investigating, we're currently looking for high-level correlations, so we don't need to dive into which `DBNs`are missing.

You may also have noticed that we now have many columns with null (`NaN`) values. This is because we chose to do `left` joins, where some columns may not have had data. The data set also had some missing values to begin with. If we hadn't performed a `left` join, all of the rows with missing data would have been lost in the merge process, which wouldn't have left us with many high schools in our data set.

There are several ways to handle missing data, and we'll cover them in more detail later on. For now, we'll just fill in the missing values with the overall mean for the column, like so:

| combined |           |                | combined |           |                |
| -------- | --------- | -------------- | -------- | --------- | -------------- |
| DBN      | sat_score | avg_class_size | DBN      | sat_score | avg_class_size |
| 01       | 1800      | 20             | 01       | 1800      | 20             |
| 03       | 2200      | 30             | 03       | 2200      | 30             |
| 99       | 1600      | null           | 99       | 1600      | 32.5           |
| 55       | null      | 50             | 55       | 1965      | 50             |
| 101      | 2300      | 30             | 101      | 2300      | 30             |

In the diagram above, the mean of the first column is `(1800 + 1600 + 2200 + 2300) / 4`, or `1975`, and the mean of the second column is `(20 + 30 + 30 + 50) / 4`, or `32.5`. We replace the missing values with the means of their respective columns, which allows us to proceed with analyses that can't handle missing values (like correlations).

We can fill in missing data in pandas using the [pandas.DataFrame.fillna()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.fillna.html) method. This method will replace any missing values in a dataframe with the values we specify. We can compute the mean of every column using the [pandas.DataFrame.mean()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.mean.html) method. If we pass the results of the df.mean() method into the df.fillna() method, pandas will fill in the missing values in each column with the mean of that column.

Here's an example of how we would accomplish this:

```
means = df.mean()
df = df.fillna(means)
```

Note that if a column consists entirely of null or `NaN` values, pandas won't be able to fill in the missing values when we use the df.fillna() method along with the df.mean() method, because there won't be a mean.

We should fill any `NaN` or null values that remain after the initial replacement with the value `0`. We can do this by passing `0` into the df.fillna() method.

## 15: Filling In Missing Values

### Instructions

- Calculate the means of all of the columns in `combined`using the [pandas.DataFrame.mean()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.mean.html) method.
- Fill in any missing values in `combined` with the means of the respective columns using the [pandas.DataFrame.fillna()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.fillna.html) method.
- Fill in any remaining missing values in `combined` with `0`using the df.fillna() method.
- Display the first few rows of `combined` to verify that the correct operations occurred.

```
combined = combined.fillna(combined.mean())
combined = combined.fillna(0)

print(combined.head(5))
```

## 16: Adding A School District Column For Mapping

We've finished cleaning and combining our data! We now have a clean data set on which we can base our analysis. Mapping the statistics out on a school district level might be an interesting way to analyze them. Adding a column to the data set that specifies the school district will help us accomplish this.

The school district is just the first two characters of the `DBN`. We can apply a function over the `DBN` column of `combined` that pulls out the first two letters.

For example, we can use indexing to extract the first few characters of a string, like this:

```
name = "Sinbad"
print(name[0:2])
```

### Instructions

- Write a function that extracts the first two characters of a string and returns them.
- Apply the function to the `DBN` column of `combined`, and assign the result to the `school_dist` column of `combined`.
- Display the first few items in the `school_dist` column of `combined` to verify the results.

```
def get_first_two_chars(dbn):
    return dbn[0:2]

combined["school_dist"] = combined["DBN"].apply(get_first_two_chars)
print(combined["school_dist"].head())
```

## 17: Next Steps

We now have a clean data set we can analyze! We've done a lot in this mission. We've gone from having several messy sources to one clean, combined, data set that's ready for analysis.

Along the way, we've learned about:

- How to handle missing values
- Different types of merges
- How to condense data sets
- How to compute averages across dataframes

Data scientists rarely start out with tidy data sets, which makes cleaning and combining them one of the most critical skills any data professional can learn.

In the next mission, we'll analyze our clean data to find correlations and create maps.

# Data Cleaning Walk-Through: Analyzing and Visualizing the Data

## 1: Introduction

Over the last two missions, we began investigating possible relationships between SAT scores and demographics. In order to do this, we acquired several data sets containing information about [New York City public schools](https://data.cityofnewyork.us/data?cat=education). We cleaned them, then combined them into a single data set named `combined` that we're now ready to analyze and visualize.

In this mission, we'll discover correlations, create plots, and then make maps. The first thing we'll do is find any correlations between any of the columns and `sat_score`. This will help us determine which columns might be interesting to plot out or investigate further. Afterwards, we'll perform more analysis and make maps using the columns we've identified.

## 2: Finding Correlations With The R Value

Correlations tell us how closely related two columns are. We'll be using the [r value](https://en.wikipedia.org/wiki/Pearson_product-moment_correlation_coefficient), also called [Pearson's correlation coefficient](https://en.wikipedia.org/wiki/Pearson_product-moment_correlation_coefficient), which measures how closely two sequences of numbers are correlated.

An r value falls between `-1` and `1`. The value tells us whether two columns are positively correlated, not correlated, or negatively correlated. The closer to `1` the r value is, the stronger the positive correlation between the two columns. The closer to `-1` the r value is, the stronger the negative correlation (i.e., the more "opposite" the columns are). The closer to `0`, the weaker the correlation. To learn more about r values, see the [statistics course](https://www.dataquest.io/course/probability-statistics-beginner).

The columns in the following diagram have a strong positive correlation -- when the value in `class_size` is high, the corresponding value in `sat_score` is also high, and vice versa:

| sat_score | avg_class_size |
| --------- | -------------- |
| 1800      | 30             |
| 2200      | 50             |
| 1600      | 20             |
| 1975      | 40             |
| 2300      | 60             |

The r value for the columns in the diagram above is `.99`.

The columns in the following diagram have a strong negative correlation -- when the value in `class_size` is high, the corresponding value in `sat_score` is low, and when the value in `sat_score` is high, the value in `class_size` is low:

| sat_score | avg_class_size |
| --------- | -------------- |
| 1800      | 50             |
| 2200      | 60             |
| 1600      | 50             |
| 1975      | 40             |
| 2300      | 30             |

The r value for the columns in the diagram above is `-.99`.

In the next diagram, the columns aren't correlated -- `class_size` and `sat_score` don't have any strong pattern in their values:

sat_scoreavg_class_size180050220060160050197540230030

The r value for the columns in the diagram above is `-.02`, which is very close to `0`.

In general, r values above `.25` or below `-.25` are enough to qualify a correlation as interesting. An r value isn't perfect, and doesn't indicate that there's a correlation -- just the possiblity of one. To really assess whether or not a correlation exists, we need to look at the data using a scatterplot to see its "shape." For example, here's a scatterplot with a very strong negative r value of `-.73`:

![img](https://s3.amazonaws.com/dq-content/correlation.png)

Notice how in the image above, all of the points appear to fall along a line. This pattern indicates a correlation.

Here's a scatterplot with an r value of `.15`, which indicates a weak correlation:

![img](https://s3.amazonaws.com/dq-content/no_correlation.png)

Notice how the data points in the image go in several directions, and there's no clear linear relationship. We'll explore correlations in greater detail later on in the statistics content. For now, this quick primer should be enough to get us through this project.

Because we're interested in exploring the fairness of the SAT, a strong positive or negative correlation between a demographic factor like race or gender and SAT score would be an interesting result meriting investigation. If men tended to score higher on the SAT, for example, that would indicate that the SAT is potentially unfair to women.

We can use the pandas [pandas.DataFrame.corr()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.corr.html) method to find correlations between columns in a dataframe. The method returns a new dataframe where the index for each column and row is the name of a column in the original data set.

## 3: Finding Correlations With The R Value

### Instructions

- Use the [pandas.DataFrame.corr()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.corr.html) method on the `combined`dataframe to find all possible correlations. Assign the result to `correlations`.
- Filter `correlations` so that it only shows correlations for the column `sat_score`.
- Display all of the rows in `correlations` and look them over.

```
correlations = combined.corr()
correlations = correlations["sat_score"]
print(correlations)
```

## 4: Plotting Enrollment With The Plot() Accessor

On the last screen, you should have seen output that looked like this:

```
SAT Critical Reading Avg. Score         0.986820
SAT Math Avg. Score                     0.972643
SAT Writing Avg. Score                  0.987771
sat_score                               1.000000
AP Test Takers                          0.523140
Total Exams Taken                       0.514333
Number of Exams with scores 3 4 or 5    0.463245
Total Cohort                            0.325144
CSD                                     0.042948
NUMBER OF STUDENTS / SEATS FILLED       0.394626
NUMBER OF SECTIONS                      0.362673
AVERAGE CLASS SIZE                      0.381014
SIZE OF SMALLEST CLASS                  0.249949
SIZE OF LARGEST CLASS                   0.314434
SCHOOLWIDE PUPIL-TEACHER RATIO               NaN
schoolyear                                   NaN
fl_percent                                   NaN
frl_percent                            -0.722225
total_enrollment                        0.367857
ell_num                                -0.153778
ell_percent                            -0.398750
sped_num                                0.034933
sped_percent                           -0.448170
asian_num                               0.475445
asian_per                               0.570730
black_num                               0.027979
black_per                              -0.284139
hispanic_num                            0.025744
hispanic_per                           -0.396985
white_num                               0.449559
                                          ...   
rr_p                                    0.047925
N_s                                     0.423463
N_t                                     0.291463
N_p                                     0.421530
saf_p_11                                0.122913
com_p_11                               -0.115073
eng_p_11                                0.020254
aca_p_11                                0.035155
saf_t_11                                0.313810
com_t_11                                0.082419
eng_t_10                                     NaN
aca_t_11                                0.132348
saf_s_11                                0.337639
com_s_11                                0.187370
eng_s_11                                0.213822
aca_s_11                                0.339435
saf_tot_11                              0.318753
com_tot_11                              0.077310
eng_tot_11                              0.100102
aca_tot_11                              0.190966
grade_span_max                               NaN
expgrade_span_max                            NaN
zip                                    -0.063977
total_students                          0.407827
number_programs                         0.117012
priority08                                   NaN
priority09                                   NaN
priority10                                   NaN
lat                                    -0.121029
lon                                    -0.132222
Name: sat_score, dtype: float64
```

The numbers above represent the r value between the `sat_score` column and the named column. The numbers are in [scientific notation](https://en.wikipedia.org/wiki/Scientific_notation), which is a system for representing numbers with many decimal places more easily. For example, `9.868201e-01` is scientific notation for `.9868201`, `4.294755e-02` is notation for `.04294755`, and `-3.969849e-01` is notation for `-.3969849`. The number after the `e-` just means "move the decimal point this many places to the left."

Unsurprisingly, `SAT Critical Reading Avg. Score`, `SAT Math Avg. Score`, `SAT Writing Avg. Score`, and `sat_score` are strongly correlated with `sat_score`.

We can also make some other observations:

- `total_enrollment` has a strong positive correlation with `sat_score`. This is surprising because we'd expect smaller schools where students receive more attention to have higher scores. However, it looks like the opposite is true -- larger schools tend to do better on the SAT.Other columns that are proxies for enrollment correlate similarly. These include `total_students`, `N_s`, `N_p`, `N_t`, `AP Test Takers`, `Total Exams Taken`, and `NUMBER OF SECTIONS`.
- Both the percentage of females (`female_per`) and number of females (`female_num`) at a school correlate positively with SAT score, whereas the percentage of males (`male_per`) and the number of males (`male_num`) correlate negatively. This could indicate that women do better on the SAT than men.
- Teacher and student ratings of school safety (`saf_t_11`, and `saf_s_11`) correlate with `sat_score`.
- Student ratings of school academic standards (`aca_s_11`) correlate with `sat_score`, but this does not hold for ratings from teachers and parents (`aca_p_11` and `aca_t_11`).
- There is significant racial inequality in SAT scores (`white_per`, `asian_per`, `black_per`, `hispanic_per`).
- The percentage of English language learners at the school (`ell_percent`, `frl_percent`) has a strong negative correlation with SAT scores.

Because enrollment seems to have such a strong correlation, let's make a scatterplot of `total_enrollment` vs `sat_score`. Each point in the scatterplot will represent a high school, so we'll be able to see if there are any interesting patterns.

We can plot columns in a dataframe using the [pandas.DataFrame.plot()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.plot.html) accessor on a dataframe. We can also specify a certain plot type. For example, `df.plot.scatter(x="A", y="b")` will create a scatterplot of columns `A`and `B`.

## 5: Plotting Enrollment With The Plot() Accessor

### Instructions

- Create a scatterplot of `total_enrollment` versus `sat_score`.

```
import matplotlib.pyplot as plt
combined.plot.scatter(x='total_enrollment', y='sat_score')
plt.show()
```

## 6: Exploring Schools With Low SAT Scores And Enrollment

Judging from the plot we just created, it doesn't appear that there's an extremely strong correlation between `sat_score` and `total_enrollment`. If there was a very strong correlation, we'd expect all of the points to line up. Instead, there's a large cluster of schools, and then a few others going off in three different directions.

However, there's an interesting cluster of points at the bottom left where `total_enrollment` and `sat_score` are both low. This cluster may be what's making the r value so high. It's worth extracting the names of the schools in this cluster so we can research them further.

### Instructions

- Filter the `combined` dataframe to keep only those rows where `total_enrollment` is under `1000` and `sat_score`is under `1000`. Assign the result to `low_enrollment`.
- Display all of the items in the `School Name` column of `low_enrollment`.
- Use [Wikipedia](https://www.wikipedia.org/) and [Google](https://www.google.com/) to research the names of the schools. Can you discover anything interesting about them?

```
low_enrollment = combined[combined["total_enrollment"] < 1000]
low_enrollment = low_enrollment[low_enrollment["sat_score"] < 1000]
print(low_enrollment["School Name"])
```

## 7: Plotting Language Learning Percentage

Our research on the last screen revealed that most of the high schools with low total enrollment and low SAT scores have high percentages of English language learners. This indicates that it's actually `ell_percent` that correlates strongly with `sat_score`, rather than `total_enrollment`. To explore this relationship further, let's plot out `ell_percent` vs `sat_score`.

### Instructions

- Create a scatterplot of `ell_percent` versus `sat_score`.

```
combined.plot.scatter(x='ell_percent', y='sat_score')
plt.show()
```

## 8: Mapping The Schools With Basemap

It looks like `ell_percent` correlates with `sat_score` more strongly, because the scatterplot is more linear. However, there's still the cluster of schools that have very high `ell_percent` values and low `sat_score`values. This cluster represents the same group of international high schools we investigated earlier.

In order to explore this relationship, we'll want to map out `ell_percent` by school district. The map will show us which areas of the city have a lot of English language learners.

We learned how to use the [Basemap](http://matplotlib.org/basemap/) package to create maps in the [Visualizing Geographic Data mission](https://www.dataquest.io/mission/224/visualizing-geographic-data). The Basemap package enables us to create high-quality maps, plot points over them, and then draw coastlines and other features.

We extracted the coordinates for all of the schools earlier, and stored them in the `lat` and `lon` columns. The coordinates will enable us to plot all of the schools on a map of New York City.

We can set up the map with this code:

```
from mpl_toolkits.basemap import Basemap
m = Basemap(
    projection='merc', 
    llcrnrlat=40.496044, 
    urcrnrlat=40.915256, 
    llcrnrlon=-74.255735, 
    urcrnrlon=-73.700272,
    resolution='i'
)

m.drawmapboundary(fill_color='#85A6D9')
m.drawcoastlines(color='#6D5F47', linewidth=.4)
m.drawrivers(color='#6D5F47', linewidth=.4)
```

This code snippet will create a map that centers on New York City (`llcrnrlat`, `urcrnrlat`, `llcrnrlon`, and `urcrnrlon` define the corners of the geographic area the map depicts). It will also draw coastlines and rivers accordingly.

Now all we need to do is convert our `lat` and `lon` coordinates to x and y coordinates so we can plot them on top of the map. This will show us where all of the schools in our data set are located.

As you may recall, in order to plot coordinates using Basemap, we need to:

- Convert the pandas series containing the latitude and longitude coordinates to lists using the [pandas.Series.tolist()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.tolist.html) method.
- Make a scatterplot using the `longitudes` and `latitudes` with the [scatter()](http://matplotlib.org/basemap/api/basemap_api.html#mpl_toolkits.basemap.Basemap.scatter) method on the `Basemap`object.
- Show the plot using the [pyplot.show()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.show) method.

We also need to make sure we pass a few keyword arguments to the scatter() method:

- `s` - Determines the size of the point that represents each school on the map.
- `zorder` - Determines where the method draws the points (that represent schools) on the `z` axis. In other words, it determines the order of the layers on the map. If we set `zorder` to `2`, the method will draw the points on top of the continents, which is where we want them.
- `latlon` - A Boolean value that specifies whether we're passing in latitude and longitude coordinates instead of x and y plot coordinates.

## 9: Mapping The Schools With Basemap

### Instructions

- Set up the map using the code snippet you saw above -- the one that creates a map, then draws rivers, coastlines, and boundaries.
- Convert the `lon` column of `combined` to a list, and assign it to the `longitudes` variable.
- Convert the `lat` column of `combined` to a list, and assign it to the `latitudes` variable.
- Call the [Basemap.scatter()](http://matplotlib.org/basemap/api/basemap_api.html#mpl_toolkits.basemap.Basemap.scatter) method on `m`, and pass in `longitudes` and `latitudes` as arguments.Make sure to pass in `longitudes` and `latitudes` in the correct order.Pass in the keyword argument `s=20` to increase the size of the points in the scatterplot.Pass in the keyword argument `zorder=2` to plot the points on top of the rest of the map. Otherwise the method will draw the points underneath the land.Pass in the keyword argument `latlon=True` to indicate that we're passing in latitude and longitude coordinates, rather than axis coordinates.
- Show the plot using the [pyplot.show()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.show) method.

```
from mpl_toolkits.basemap import Basemap

m = Basemap(
    projection='merc', 
    llcrnrlat=40.496044, 
    urcrnrlat=40.915256, 
    llcrnrlon=-74.255735, 
    urcrnrlon=-73.700272,
    resolution='i'
)

m.drawmapboundary(fill_color='#85A6D9')
m.drawcoastlines(color='#6D5F47', linewidth=.4)
m.drawrivers(color='#6D5F47', linewidth=.4)

longitudes = combined["lon"].tolist()
latitudes = combined["lat"].tolist()
m.scatter(longitudes, latitudes, s=20, zorder=2, latlon=True)
plt.show()
```

## 10: Plotting Out Statistics

From the map above, we can see that school density is highest in Manhattan (the top of the map), and lower in Brooklyn, the Bronx, Queens, and Staten Island.

Now that we've plotted the school locations, we can begin to display meaningful information on the maps, such as the percentage of English language learners by area.

We can shade each point in the scatterplot by passing the keyword argument `c` into the scatter() method. This argument accepts a variable containing a sequence of numbers, assigns different colors to those numbers, and then shades the points on the plot associated with those numbers accordingly.

The method will convert the sequence of numbers we pass into the `c` keyword argument to values ranging from `0` to `1`. It will then map these values onto a colormap. Matplotlib has quite a few default [colormaps](http://matplotlib.org/users/colormaps.html). In our case, we'll use the `summer` colormap, which results in green points for low numbers, and yellow points for high numbers.

For example, let's say we plotted `ell_percent` by school. If we pass in the keyword argument `c=combined["ell_percent"]`, then the method would shade a school with a high `ell_percent` yellow, and a school with a low `ell_percent` green. We can specify the colormap we want to use by passing the `cmap` keyword argument to the scatter() method.

### Instructions

- Set up the map using the code snippet that creates a map, then draws rivers, coastlines, and boundaries.
- Call the [scatter()](http://matplotlib.org/basemap/api/basemap_api.html#mpl_toolkits.basemap.Basemap.scatter) method on `m`, and pass in `longitudes`and `latitudes` as arguments.Make sure to pass in `longitudes` and `latitudes` in the correct order.Pass in the keyword argument `s=20` to increase the size of the points in the scatterplot.Pass in the keyword argument `zorder=2` to plot the points on top of the rest of the map. Otherwise the method will draw the points underneath the land.Pass in the keyword argument `latlon=True` to indicate that we're passing in latitude and longitude coordinates, rather than axis coordinates.Pass in the keyword argument `c` with the value `combined["ell_percent"]` to plot the `ell_percent`.Pass in the keyword argument `cmap="summer"` to get the right color scheme.
- Show the plot using the [show()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.show) method.

```
from mpl_toolkits.basemap import Basemap

m = Basemap(
    projection='merc', 
    llcrnrlat=40.496044, 
    urcrnrlat=40.915256, 
    llcrnrlon=-74.255735, 
    urcrnrlon=-73.700272,
    resolution='i'
)

m.drawmapboundary(fill_color='#85A6D9')
m.drawcoastlines(color='#6D5F47', linewidth=.4)
m.drawrivers(color='#6D5F47', linewidth=.4)

longitudes = combined["lon"].tolist()
latitudes = combined["lat"].tolist()
m.scatter(longitudes, latitudes, s=20, zorder=2, latlon=True, c=combined["ell_percent"], cmap="summer")
plt.show()
```

## 11: Calculating District-Level Statistics

Unfortunately, due to the number of schools, it's hard to interpret the map we made on the last screen. It looks like uptown Manhattan and parts of Queens have a higher `ell_percent`, but we can't be sure. One way to make very granular statistics easier to read is to aggregate them. In this case, we can aggregate by district, which will enable us to plot `ell_percent` district-by-district instead of school-by-school.

In the last mission, we used the [pandas.DataFrame.groupby()](http://pandas.pydata.org/pandas-docs/stable/groupby.html) followed by the [agg()](http://pandas.pydata.org/pandas-docs/stable/groupby.html#aggregation) method on the resulting object to find the mean class size for each unique `DBN`. The principle is exactly the same, except that here we'd find the mean of each column for each unique value in `school_dist`.

### Instructions

- Find the average values for each column for each `school_dist` in `combined`.Use the [pandas.DataFrame.groupby()](http://pandas.pydata.org/pandas-docs/stable/groupby.html) method to group `combined` by `school_dist`.Use the [agg()](http://pandas.pydata.org/pandas-docs/stable/groupby.html#aggregation) method, along with the `numpy.mean`function as an argument, to calculate the average of each group.Assign the result to the variable `districts`.
- Reset the index of `districts`, making `school_dist` a column again.Use the [pandas.DataFrame.reset_index()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.reset_index.html) method with the keyword argument `inplace=True`.
- Display the first few rows of `districts` to verify that everything went okay.

```
import numpy
districts = combined.groupby("school_dist").agg(numpy.mean)
districts.reset_index(inplace=True)
print(districts.head())
```

## 12: Plotting Percent Of English Learners By District

Now that we've taken the means of all of the columns, we can plot out `ell_percent`by district. Not only did we find the mean of `ell_percent`, but we also took the means of the `lon` and `lat` columns, which will give us the coordinates for the center of each district.

### Instructions

- Use the code snippet from before that creates a map, then draws rivers, coastlines, and boundaries.
- Convert the `lon` column of `districts` to a list, and assign it to the `longitudes` variable.
- Convert the `lat` column of `districts` to a list, and assign it to the `latitudes` variable.
- Call the [scatter()](http://matplotlib.org/basemap/api/basemap_api.html#mpl_toolkits.basemap.Basemap.scatter) method on `m`, and pass in `longitudes`and `latitudes` as arguments.Make sure to pass in `longitudes` and `latitudes` in the correct order.Pass in the keyword argument `s=50` to increase the size of the points in the scatterplot.Pass in the keyword argument `zorder=2` to plot the points on top of the rest of the map. Otherwise the method will draw the points underneath the land.Pass in the keyword argument `latlon=True` to indicate that we're passing in latitude and longitude coordinates, rather than axis coordinates.Pass in the keyword argument `c` with the value `districts["ell_percent"]` to plot the `ell_percent`.Pass in the keyword argument `cmap="summer"` to get the right color scheme.
- Show the plot using the [show()](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.show) method.

```
from mpl_toolkits.basemap import Basemap

m = Basemap(
    projection='merc', 
    llcrnrlat=40.496044, 
    urcrnrlat=40.915256, 
    llcrnrlon=-74.255735, 
    urcrnrlon=-73.700272,
    resolution='i'
)

m.drawmapboundary(fill_color='#85A6D9')
m.drawcoastlines(color='#6D5F47', linewidth=.4)
m.drawrivers(color='#6D5F47', linewidth=.4)

longitudes = districts["lon"].tolist()
latitudes = districts["lat"].tolist()
m.scatter(longitudes, latitudes, s=50, zorder=2, latlon=True, c=districts["ell_percent"], cmap="summer")
plt.show()
```

## 13: Next Steps

In this mission, we found correlations, created visualizations, and mapped out our schools! Now we have all the tools we need to analyze the data in greater depth.

Along the way, we learned:

- How to create school and district-level maps
- How to find correlations, and what those correlations mean
- Why we should plot data out, rather than relying on the r value alone
- That ell_percent has a strong negative correlation with sat_score

We now have the skills to analyze data sets and explain the quirks we find. These are essential skills for any data science professional.

Next, we'll use the skills and tools we've developed to analyze the New York City high school data more fully in a guided project.

# Guided Project: Analyzing NYC High School Data

## 1: Introduction

Over the last three missions, we explored relationships between SAT scores and demographic factors in New York City public schools. For a brief bit of background, the [SAT](https://en.wikipedia.org/wiki/SAT), or Scholastic Aptitude Test, is a test that high school seniors in the U.S. take every year. The SAT has three sections, each of which is worth a maximum of `800` points. Colleges use the SAT to determine which students to admit. High average SAT scores are usually indicative of a good school.

New York City has published data on [student SAT scores](https://data.cityofnewyork.us/Education/SAT-Results/f9bf-2cp4) by high school, along with additional demographic data sets. Over the last three missions, we combined the following data sets into a single, clean pandas dataframe:

- [SAT scores by school](https://data.cityofnewyork.us/Education/SAT-Results/f9bf-2cp4) - SAT scores for each high school in New York City
- [School attendance](https://data.cityofnewyork.us/Education/School-Attendance-and-Enrollment-Statistics-by-Dis/7z8d-msnt) - Attendance information for each school in New York City
- [Class size](https://data.cityofnewyork.us/Education/2010-2011-Class-Size-School-level-detail/urz7-pzb3) - Information on class size for each school
- [AP test results](https://data.cityofnewyork.us/Education/AP-College-Board-2010-School-Level-Results/itfs-ms3e) - Advanced Placement (AP) exam results for each high school (passing an optional AP exam in a particular subject can earn a student college credit in that subject)
- [Graduation outcomes](https://data.cityofnewyork.us/Education/Graduation-Outcomes-Classes-Of-2005-2010-School-Le/vh2h-md7a) - The percentage of students who graduated, and other outcome information
- [Demographics](https://data.cityofnewyork.us/Education/School-Demographics-and-Accountability-Snapshot-20/ihfw-zy9j) - Demographic information for each school
- [School survey](https://data.cityofnewyork.us/Education/NYC-School-Survey-2011/mnz3-dyi8) - Surveys of parents, teachers, and students at each school

New York City has a significant immigrant population and is very diverse, so comparing demographic factors such as race, income, and gender with SAT scores is a good way to determine whether the SAT is a fair test. For example, if certain racial groups consistently perform better on the SAT, we would have some evidence that the SAT is unfair.

In the last mission, we began performing some analysis. We'll extend that analysis in this mission. As you can see, we've included the code to read in all of the data, combine it, and create correlations in the notebook. If you'd like to see the finished notebook that contains solutions for all of the steps, you can find it [in the GitHub repo for this mission](https://github.com/dataquestio/solutions/blob/master/Mission217Solutions.ipynb).

The dataframe `combined` contains all of the data we'll be using in our analysis.

### Instructions

- Set up matplotlib to work in Jupyter notebook.
- There are several fields in `combined` that originally came from a [survey of parents, teachers, and students](https://data.cityofnewyork.us/Education/NYC-School-Survey-2011/mnz3-dyi8). Make a bar plot of the correlations between these fields and `sat_score`.You can find a list of the fields in the `survey_fields`variable in the notebook.
- Consult the data dictionary that's part of the `zip` file you can download [from the City of New York's website](https://data.cityofnewyork.us/Education/NYC-School-Survey-2011/mnz3-dyi8).Did you find any surprising correlations?
- Write up your results in a Markdown cell.

## 2: Exploring Safety And SAT Scores

On the last screen, you may have noticed that `saf_t_11` and `saf_s_11`, which measure how teachers and students perceive safety at school, correlated highly with `sat_score`. On this screen, we'll dig into this relationship a bit more, and try to figure out which schools have low safety scores.

### Instructions

- Investigate safety scores.
  - Make a scatter plot of the `saf_s_11` column vs. the `sat_score` in `combined`.
  - Write up your conclusions about safety and SAT scores in a Markdown cell.
- Map out safety scores.
  - Compute the average safety score for each district.
  - Make a map that shows safety scores by district.
  - Write up your conclusions about safety by geographic area in a Markdown cell. You may want to read up on the [boroughs of New York City](http://www.nycgo.com/boroughs-neighborhoods).

## 3: Exploring Race And SAT Scores

There are a few columns that indicate the percentage of each race at a given school:

- `white_per`
- `asian_per`
- `black_per`
- `hispanic_per`

By plotting out the correlations between these columns and `sat_score`, we can determine whether there are any racial differences in SAT performance.

### Instructions

- Investigate racial differences in SAT scores.
  - Make a bar plot of the correlations between the columns above and `sat_score`.
  - Write up a Markdown cell containing your findings. Are there any unexpected correlations?
- Explore schools with low SAT scores and high values for `hispanic_per`.Make a scatter plot of `hispanic_per` vs. `sat_score`.What does the scatter plot show? Record any interesting observsations in a Markdown cell.
- Research any schools with a `hispanic_per` greater than `95%`.Find the school names in the data.Use Wikipedia and Google to research the schools by name.Is there anything interesting about these particular schools? Record your findings in a Markdown cell.
- Research any schools with a `hispanic_per` less than `10%`and an average SAT score greater than `1800`.Find the school names in the data.Use Wikipedia and Google to research the schools by name.Is there anything interesting about these particular schools? Record your findings in a Markdown cell.

## 4: Exploring Gender And SAT Scores

There are two columns that indicate the percentage of each gender at a school:

- `male_per`
- `female_per`

We can plot out the correlations between each percentage and `sat_score`.

### Instructions

- Investigate gender differences in SAT scores.
  - Make a scatter plot of the correlations between the columns above and `sat_score`.
  - Record your findings in a Markdown cell. Are there any unexpected correlations?
- Investigate schools with high SAT scores and a high `female_per`.Make a scatter plot of `female_per` vs. `sat_score`.What does the scatter plot show? Record any interesting observations in a Markdown cell.
- Research any schools with a `female_per` greater than `60%`and an average SAT score greater than `1700`.Find the school names in the data.Use Wikipedia and Google to research the schools by name.Is there anything interesting about these particular schools? Record your findings in a Markdown cell.

## 5: Exploring AP Scores Vs. SAT Scores

In the U.S., high school students take [Advanced Placement](https://en.wikipedia.org/wiki/Advanced_Placement_exams) (AP) exams to earn college credit. There are AP exams for many different subjects.

It makes sense that the number of students at a school who took AP exams would be highly correlated with the school's SAT scores. Let's explore this relationship. Because `total_enrollment` is highly correlated with `sat_score`, we don't want to bias our results. Instead, we'll look at the percentage of students in each school who took at least one AP exam.

### Instructions

- Calculate the percentage of students in each school that took an AP exam.
  - Divide the `AP Test Takers` column by the `total_enrollment` column.The column name `AP Test Takers` has a space at the end -- don't forget to add it!
  - Assign the result to the `ap_per` column.
- Investigate the relationship between AP scores and SAT scores.
  - Make a scatter plot of `ap_per` vs. `sat_score`.
  - What does the scatter plot show? Record any interesting observations in a Markdown cell.

## 6: Next Steps

We've done quite a bit of investigation into relationships between demographics and SAT scores in this guided project. There's still quite a bit of analysis left to do, however. Here are some potential next steps:

- Determing wheter there's a correlation between class size and SAT scores
- Figuring out which neighborhoods have the best schools
  - If we combine this information with a dataset containing property values, we could find the least expensive neighborhoods that have good schools.
- Investigating the differences between parent, teacher, and student responses to surveys.
- Assigning scores to schools based on `sat_score` and other attributes.

We recommend creating a [GitHub](https://github.com/) repository and placing this project there. It will help other people see your work, including employers. As you start to put multiple projects on GitHub, you'll have the beginnings of a strong portfolio.

You're welcome to keep working on the project here, but we recommend downloading it to your computer using the download icon above and working on it there.

We hope this guided project has been a good experience. Please email us at [hello@dataquest.io](mailto:hello@dataquest.io) if you'd like to share your work!

## 7: [Solution](https://github.com/dataquestio/solutions/blob/master/Mission217Solutions.ipynb)

# Challenge: Cleaning Data

## 1: Introduction

At Dataquest, we're huge believers in learning through doing, and we hope this shows in your experience with the missions. While missions focus on introducing concepts, challenges allow you to perform deliberate practice by completing structured problems. You can read more about deliberate practice [on Wikipedia](http://bit.ly/2cJ2Qt7) and [at *Nautilus*](http://nautil.us/issue/35/boundaries/not-all-practice-makes-perfect). Challenges will feel similar to missions, but with little instructional material and a larger focus on exercises.

If you have questions or run into issues, head over to the [Dataquest forums](https://www.dataquest.io/forum/) or our [Slack community](https://www.dataquest.io/chat).

## 2: Life And Death Of The Avengers

The Avengers are a well-known and widely-loved team of superheroes in the Marvel universe that were originally introduced in the 1960's comic book series. The recent Disney movies re-popularized them, as part of the new [Marvel Cinematic Universe](https://en.wikipedia.org/wiki/Marvel_Cinematic_Universe).

Because the writers killed off and revived many of the superheroes, the team at FiveThirtyEight was curious to explore data from the [Marvel Wikia site](http://marvel.wikia.com/wiki/Main_Page) further. To learn how they collected their data, which is available in their [GitHub repository](https://github.com/fivethirtyeight/data/tree/master/avengers), read the write-up they published on [the FiveThirtyEight website](http://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/).

## 3: Exploring The Data

While the FiveThirtyEight team did a wonderful job acquiring the data, it still has some inconsistencies. Your mission, **if you choose to accept it**, is to clean up their data set so it can be more useful for analysis in pandas. Let's read it into pandas as a dataframe and preview the first five rows to get a better sense of it.

## 4: Filtering Out Bad Data

Because the data came from a crowdsourced community site, it could contain errors. If you plot a histogram of the values in the `Year` column, which describes the year Marvel introduced each Avenger, you'll immediately notice some oddities. For example, there are quite a few Avengers who look like they were introduced in 1900, which we know is a little fishy -- the Avengers weren't introduced in the comic series until the 1960's!

This is obviously a mistake in the data. As a result, you should remove all of the Avengers introduced before 1960 from the dataframe.

### Instructions

- We only want to keep the Avengers who were introduced after 1960.
  - Store only the rows describing Avengers added in 1960 or later in `true_avengers`.

```
import matplotlib.pyplot as plt
true_avengers = pd.DataFrame()

avengers['Year'].hist()
true_avengers = avengers[avengers["Year"] > 1959]
```

## 5: Consolidating Deaths

We're interested in the total number of deaths each character experienced, so we'd like to have a single field containing that information. Right now, there are five fields (`Death1` to `Death5`), each of which contains a binary value representing whether a superhero experienced that death or not. For example, a superhero could experience `Death1`, then `Death2`, and so on until the writers decided not to bring the character back to life.

We'd like to combine that information in a single field so we can perform numerical analysis on it more easily.

### Instructions

- Create a new column, `Deaths`, that contains the number of times each superhero died. The possible values for each death field are `YES`, `NO`, and `NaN` for missing data.Keep all of the original columns (including `Death1` to `Death5`) and update `true_avengers` with the new `Deaths` column.

```
def clean_deaths(row):
    num_deaths = 0
    columns = ['Death1', 'Death2', 'Death3', 'Death4', 'Death5']
    
    for c in columns:
        death = row[c]
        if pd.isnull(death) or death == 'NO':
            continue
        elif death == 'YES':
            num_deaths += 1
    return num_deaths

true_avengers['Deaths'] = true_avengers.apply(clean_deaths, axis=1)
```

## 6: Verifying Years Since Joining

For our final task, we want to verify that the `Years since joining` field accurately reflects the `Year` column. For example, if an Avenger was introduced in the `Year`1960, is the `Years since joining` value for that Avenger 55?

### Instructions

- Calculate the number of rows where `Years since joining` is accurate.Because this challenge was created in 2015, use that as the reference year.We want to know for how many rows `Years since joining` was correctly calculated as the `Year` value subtracted from 2015.Assign the *integer* value describing the number of rows with a correct value for `Years since joining`to `joined_accuracy_count`.

```
joined_accuracy_count  = int()
correct_joined_years = true_avengers[true_avengers['Years since joining'] == (2015 - true_avengers['Year'])]
joined_accuracy_count = len(correct_joined_years)
```

# Guided Project: Star Wars Survey

## 1: How Guided Projects Work

Welcome to this guided project! Guided projects help you synthesize the concepts you learned in the Dataquest missions and start building a portfolio. Guided projects provide an in-browser coding experience, along with help and hints. Guided projects bridge the gap between learning through the Dataquest missions, and applying what you've learned on your own computer.

Guided projects help you develop the key skills you'll need to perform data science work in the "real world." Doing well on these projects is a bit different from doing well on the missions, where there's a "right" answer. In the guided projects, you'll need to create solutions on your own (although we'll be there to help you along the way).

In this project, you'll be working with Jupyter notebook and analyzing data on the *Star Wars* movies. When you're finished, you'll have a notebook you can either add to your portfolio, or expand on your own.

[Google](https://www.google.com/), [StackOverflow](https://www.stackoverflow.com/), and the documentation for various packages will help you as you progress through this project. All data scientists make extensive use of resources like these as they write code.

We'd love to hear your feedback as you go through this project. We hope it's a great experience!

### Instructions

For now, just click "Next" to get started with the project!

## 2: Overview

While waiting for [*Star Wars: The Force Awakens*](https://en.wikipedia.org/wiki/Star_Wars:_The_Force_Awakens) to come out, the team at [FiveThirtyEight](http://fivethirtyeight.com/) became interested in answering some questions about *Star Wars* fans. In particular, they wondered: **does the rest of America realize that “The Empire Strikes Back” is clearly the best of the bunch?**

The team needed to collect data addressing this question. To do this, they surveyed *Star Wars* fans using the online tool SurveyMonkey. They received 835 total responses, which you download from [their GitHub repository](https://github.com/fivethirtyeight/data/tree/master/star-wars-survey).

For this project, you'll be cleaning and exploring the data set in Jupyter notebook. To see a sample notebook containing all of the answers, visit [the project's GitHub repository](https://github.com/dataquestio/solutions/blob/master/Mission201Solution.ipynb).

The following code will read the data into a pandas dataframe:

```
import pandas as pd
star_wars = pd.read_csv("star_wars.csv", encoding="ISO-8859-1")
```

We need to specify an `encoding` because the data set has some characters that aren't in Python's default `utf-8`encoding. You can read more about character encodings [on developer Joel Spolsky's blog](http://www.joelonsoftware.com/articles/Unicode.html).

The data has several columns, including:

- `RespondentID` - An anonymized ID for the respondent (person taking the survey)
- `Gender` - The respondent's gender
- `Age` - The respondent's age
- `Household Income` - The respondent's income
- `Education` - The respondent's education level
- `Location (Census Region)` - The respondent's location
- `Have you seen any of the 6 films in the Star Wars franchise?` - Has a `Yes` or `No` response
- `Do you consider yourself to be a fan of the Star Wars film franchise?` - Has a `Yes` or `No` response

There are several other columns containing answers to questions about the *Star Wars* movies. For some questions, the respondent had to check one or more boxes. This type of data is difficult to represent in columnar format. As a result, this data set needs a lot of cleaning.

First, you'll need to remove the invalid rows. For example, `RespondentID` is supposed to be a unique ID for each respondent, but it's blank in some rows. You'll need to remove any rows with an invalid `RespondentID`.

### Instructions

- Read the data set into a dataframe.
- Explore the data by entering `star_wars.head(10)`. Look for any strange values in the columns and rows.
- Review the column names with `star_wars.columns`.
- Remove any rows where `RespondentID` is `NaN`. You can use the [pandas.notnull()](http://pandas.pydata.org/pandas-docs/version/0.17.1/generated/pandas.notnull.html) function to accomplish this. Only select rows where the `RespondentID` column is not null.
- When you're finished, `star_wars` should only consist of rows where `RespondentID` is not `NaN`.

## 3: Cleaning And Mapping Yes/No Columns

Take a look at the next two columns, which are:


```
* `Have you seen any of the 6 films in the Star Wars franchise?`
* `Do you consider yourself to be a fan of the Star Wars film franchise?`
```

Both represent `Yes/No` questions. They can also be `NaN` where a respondent chooses not to answer a question. We can use the [pandas.Series.value_counts()](http://pandas.pydata.org/pandas-docs/version/0.17.1/generated/pandas.Series.value_counts.html) method on a series to see all of the unique values in a column, along with the total number of times each value appears.

Both columns are currently string types, because the main values they contain are `Yes` and `No`. We can make the data a bit easier to analyze down the road by converting each column to a Boolean having only the values `True`, `False`, and `NaN`. Booleans are easier to work with because we can select the rows that are `True` or `False` without having to do a string comparison.

We can use the [pandas.Series.map()](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.Series.map.html) method on series objects to perform the conversion.

For example, imagine we have a series that looks like this:


```
series = ["Yes", "No", NaN, "Yes"]
```

We can use a dictionary to define a mapping from each value in `series` to a new value:


```
yes_no = {
    "Yes": True,
    "No": False
}
```

Then, we can call the map() function to perform the mapping:


```
series = series.map(yes_no)
```

`series` will end up looking like this:


```
[True, False, NaN, True]
```

### Instructions

- Convert the `Have you seen any of the 6 films in the Star Wars franchise?` column to the Boolean type.
- Convert the `Do you consider yourself to be a fan of the Star Wars film franchise?` column to the Boolean type.
- When you're finished, both columns should only contain the values `True`, `False`, and `NaN`.

## 4: Cleaning And Mapping Checkbox Columns

The next six columns represent a single checkbox question. The respondent checked off a series of boxes in response to the question, `Which of the following Star Wars films have you seen? Please select all that apply.`

The columns for this question are:

- `Which of the following Star Wars films have you seen? Please select all that apply.` - Whether or not the respondent saw `Star Wars: Episode I The Phantom Menace`.
- `Unnamed: 4` - Whether or not the respondent saw `Star Wars: Episode II Attack of the Clones`.
- `Unnamed: 5` - Whether or not the respondent saw `Star Wars: Episode III Revenge of the Sith`.
- `Unnamed: 6` - Whether or not the respondent saw `Star Wars: Episode IV A New Hope`.
- `Unnamed: 7` - Whether or not the respondent saw `Star Wars: Episode V The Empire Strikes Back`.
- `Unnamed: 8` - Whether or not the respondent saw `Star Wars: Episode VI Return of the Jedi`.

For each of these columns, if the value in a cell is the name of the movie, that means the respondent saw the movie. If the value is `NaN`, the respondent either didn't answer or didn't see the movie. We'll assume that they didn't see the movie.

We'll need to convert each of these columns to a Boolean, then rename the column something more intuitive. We can convert the values the same way we did earlier, except that we'll need to include the movie title and `NaN` in the mapping dictionary.

For example, imagine we had this column series:


```
["Star Wars: Episode I  The Phantom Menace", NaN, "Star Wars: Episode I  The Phantom Menace"]
```

We could convert the values using this mapping dictionary:


```
{
    "Star Wars: Episode I  The Phantom Menace": True,
    NaN: False
}
```

After calling the `map()` method on a series, the column should only contain the values `True` and `False`.

Next, we'll need to rename the columns to better reflect what they represent. We can use the [pandas.DataFrame.rename()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.rename.html) method on dataframes to accomplish this.

The `df.rename()` method works a lot like `map()`. We pass it a dictionary that maps the current column names to new ones:


```
star_wars = star_wars.rename(columns={
    "Which of the following Star Wars films have you seen? Please select all that apply.": "seen_1"
})
```

The `pandas.DataFrame.rename()` method will only rename the columns we specify in the dictionary, and won't change the names of other columns. The code above will rename the `Which of the following Star Wars films have you seen? Please select all that apply.` column to `seen_1`.

### Instructions

- Convert each column above so that it only contains the values `True` and `False`.You can select the column names more quickly by entering `star_wars.columns[3:9]`, rather than typing them out.Be very careful with spacing when constructing your mapping dictionary! In the cells, `Star Wars: Episode I The Phantom Menace` has two spaces between the end of `Episode I` and the start of `The Phantom`, but this is not the case in `Star Wars: Episode VI Return of the Jedi`. Check the values in the cells carefully to make sure you use the appropriate spacing.
- Rename each of the columns above so the names are more intuitive. We recommend using `seen_1` to indicate whether the respondent saw `Star Wars: Episode I The Phantom Menace`, `seen_2` for `Star Wars: Episode II Attack of the Clones`, and so on.
- When you're finished, the columns should have intuitive names, along with `True` and `False` values that indicate whether the respondent saw each of the six *Star Wars* movies.

## 5: Cleaning The Ranking Columns

The next six columns ask the respondent to rank the *Star Wars* movies in order of least favorite to most favorite. `1` means the film was the most favorite, and `6`means it was the least favorite. Each of the following columns can contain the value `1`, `2`, `3`, `4`, `5`, `6`, or `NaN`:

- `Please rank the Star Wars films in order of preference with 1 being your favorite film in the franchise and 6 being your least favorite film.` - How much the respondent liked `Star Wars: Episode I The Phantom Menace`
- `Unnamed: 10` - How much the respondent liked `Star Wars: Episode II Attack of the Clones`
- `Unnamed: 11` - How much the respondent liked `Star Wars: Episode III Revenge of the Sith`
- `Unnamed: 12` - How much the respondent liked `Star Wars: Episode IV A New Hope`
- `Unnamed: 13` - How much the respondent liked `Star Wars: Episode V The Empire Strikes Back`
- `Unnamed: 14` - How much the respondent liked `Star Wars: Episode VI Return of the Jedi`

Fortunately, these columns don't require a lot of cleanup. We'll need to convert each column to a numeric type, though, then rename the columns so that we can tell what they represent more easily.

We can do the numeric conversion with the [pandas.DataFrame.astype()](http://pandas.pydata.org/pandas-docs/version/0.17.1/generated/pandas.DataFrame.astype.html) method on dataframes. In this case, we can use code that looks like this:


```
star_wars[star_wars.columns[9:15]] = star_wars[star_wars.columns[9:15]].astype(float)
```

The code above will convert column `9` up to but not including column `15` to the float data type.

### Instructions

- Convert each of the columns above to a `float` type.You can select all of the column names with `star_wars.columns[9:15]`, rather than typing each one in.
- Give each column a more descriptive name. We suggest `ranking_1`, `ranking_2`, and so on.You can use the `df.rename()` method from the last screen to accomplish this.

## 6: Finding The Highest-Ranked Movie

Now that we've cleaned up the ranking columns, we can find the highest-ranked movie more quickly. To do this, take the mean of each of the ranking columns using the [pandas.DataFrame.mean()](http://pandas.pydata.org/pandas-docs/version/0.17.1/generated/pandas.DataFrame.mean.html) method on dataframes.

### Instructions

- Use the `pandas.DataFrame.mean()` method to compute the mean of each of the `ranking` columns from the last screen.
- Make a bar chart of each ranking. You can use a matplotlib [bar chart](http://matplotlib.org/examples/api/barchart_demo.html) for this.Make sure to run `%matplotlib inline` beforehand to show your plots in the notebook.
- Write up a summary of what you've done so far in a Markdown cell. Also discuss why you think the respondents ranked the movies the way they did.
  - Remember that a lower ranking is better!

## 7: Finding The Most Viewed Movie

Earlier in this project, we cleaned up the `seen` columns and converted their values to the Boolean type. When we call methods like [pandas.DataFrame.sum()](http://pandas.pydata.org/pandas-docs/version/0.17.1/generated/pandas.DataFrame.sum.html) or mean(), they treat Booleans like integers. They consider `True` a `1`, and `False` a `0`. That means we can figure out how many people have seen each movie just by taking the sum of the column (even though they contain Boolean values).

### Instructions

- Use the `df.sum()` method to compute the sum of each of the `seen` columns.
- Make a bar chart of each ranking. You can use a matplotlib [bar chart](http://matplotlib.org/examples/api/barchart_demo.html) for this.
- Write up your thoughts on why the results look the way they do in a Markdown cell. Also discuss how the results correlate with the rankings.

## 8: Exploring The Data By Binary Segments

We know which movies the survey population as a whole has ranked the highest. Now let's examine how certain segments of the survey population responded. There are several columns that segment our data into two groups. Here are a few examples:

- `Do you consider yourself to be a fan of the Star Wars film franchise?` - True or False
- `Do you consider yourself to be a fan of the Star Trek franchise?` - `Yes`or `No`
- `Gender` - `Male` or `Female`

We can split a dataframe into two groups based on a binary column by creating two subsets of that column. For example, we can split on the `Gender` column like this:


```
males = star_wars[star_wars["Gender"] == "Male"]
females = star_wars[star_wars["Gender"] == "Female"]
```

The subsets will allow us to compute the most viewed movie, the highest-ranked movie, and other statistics separately for each group.

### Instructions

- Split the data into two groups based on one of the binary columns above.
- Redo the two previous analyses (find the most viewed movie and the highest-ranked movie) separately for each group, and then compare the results.
- If you see any interesting patterns, write about them in a Markdown cell.

## 9: Next Steps

That's it for the guided steps! We highly recommend exploring the data further on your own.

Here are some potential next steps:

- Try to segment the data based on columns like `Education`, `Location (Census Region)`, and `Which character shot first?`, which aren't binary. Are they any interesting patterns?
- Clean up columns `15` to `29`, which contain data on the characters respondents view favorably and unfavorably.Which character do respondents like the most?Which character do respondents dislike the most?Which character is the most controversial (split between likes and dislikes)?

We highly recommend creating a [GitHub](https://github.com/) repository and placing this project there. It will help other people see your work, including employers. As you start to put multiple projects on GitHub, you'll have the beginnings of a strong portfolio.

You're welcome to keep working on the project here, but we highly recommend downloading it to your computer using the download icon above and working on it there.

We hope this guided project has been a good experience. Please email us at [hello@dataquest.io](https://www.dataquest.io/m/201/guided-project-star-wars-survey/9/hello@dataquest.io) if you want to share your work!