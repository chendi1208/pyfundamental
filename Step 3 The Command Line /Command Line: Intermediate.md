# Working with Jupyter console

## 1: Jupyter Console

The [Jupyter console](https://github.com/jupyter/jupyter_console), formerly known as IPython, is an enhanced Python interpreter. From our earlier missions, you may recall that by typing `python` on the command line, you get access to an interactive shell that lets you write and execute Python code. Jupyter console enhances this shell, and adds several niceties that make working with data easier.

Generally, it's useful to use the shell in situations where you need to quickly test some code you're writing. This happens frequently when you're writing data analysis scripts. It can also be used to quickly explore datasets and do basic analysis. Another use case is prototyping code before later saving it to a script file.

The main difference between Jupyter console and Jupyter notebook is that the console functions in interactive mode. Whenever you type a line of code, it is immediately executed, and you can see the results. If you want to write medium-length pieces of code or do deep exploration of a dataset, the notebook is better. If you want to test out code you're writing, or run quick commands, the console is better.

The Jupyter project is in the midst of rebranding from IPython to Jupyter. Depending on the version of Jupyter you have installed, you can access the console by typing either `jupyter console` or `ipython` at the command line.

### Instructions

- Open the Jupyter console by typing `ipython`.
- Once you access it, you can run Python commands. Type `print(10)` to see what happens.
- Exit Jupyter console by typing `exit`.

## 2: Getting Help

Jupyter console has a robust built-in help system. You can get help in several ways:

- You can type `?` after starting the console. This will display help about Jupyter. You can exit by typing `q`.
- You can type `%quickref`. This is a **magic** that will tell you some useful commands. We'll talk more about Jupyter magics shortly.
- If you want information about a variable, just type the name of the variable, followed by `?`. For information on the `dq` variable, you'd type `dq?`.
- Type `help()` to get access to Python help. This will enable you to get help on all the modules and functions currently available. You can quit by typing `quit`.
- If you want to use the Python help system to get information on a variable, type `help(variable_name)`. If you wanted help with the variable `dq`, you'd type `help(dq)`.

Being able to get help will let you see which methods are allowed on which objects, and be able to better understand the capabilities of Jupyter console.

### Instructions

- Open the Jupyter console by typing `ipython`.
- Assign the value `5` to the variable `dq`.
- Get help with the variable `dq` using both `?` and `help(dq)`.
- Type `help()`, and explore the Python help functionality. See if you can look up some modules.
- Exit Jupyter console by typing `exit`.

## 3: Persistent Sessions

Just like with Jupyter notebook, Jupyter console starts a kernel session when you first load it. Every time you run code in the console, the variables are stored in the session. Any subsequent lines you execute can access those variables.

This functionality is extremely powerful, and allows you to execute longer scripts line by line and inspect the variables at each step.

### Instructions

- Open the Jupyter console by typing `ipython`.
- Assign the value `5` to the variable `dq`.
- Assign the value `dq * 10` to the variable `dq_10`.
- Exit Jupyter console by typing `exit`.

## 4: Jupyter Magics

You may have used the `%quickref` Jupyter magic in the last screen. Magics are special Jupyter commands that always start with `%`. They enable you to access Jupyter-specific functionality, without Python executing your commands.

Some useful magics are:

- `%run` -- allows you to run an external Python script. Any variables in the script will be stored in the current kernel session.
- `%edit` -- opens a file editor. Any code you type into the editor will be executed by Jupyter when you exit the editor.
- `%debug` -- if there's an error in any of your code, running `%debug`afterwards will open an interactive debugger you can use to trace the error.
- `%history` -- shows you the last few commands you ran.
- `%save` -- saves the last few commands you ran to a file.
- `%who` -- print all the variables in the session.
- `%reset` -- resets the session, and removes all stored variables.

You can see a full list of magics [here](http://ipython.readthedocs.org/en/stable/interactive/magics.html).

You can use the `%run`, `%who`, and `%debug` magics to iteratively develop scripts with Jupyter console. Have your favorite editor open, and start writing a Python script. In a separate shell, open Jupyter console. As you get to checkpoints in your script where you want to test it out, use the `%run` magic to run the script. Check the values of the variables using the `%who` magic. If you see any errors, debug them with the `%debug` magic. If you want to clear the session, use `%reset`.

### Instructions

- Create a Python script using `nano`.Add in whatever code you want, but make sure you add at least one print statement, and at least one variable definition.
- Open the Jupyter console by typing `ipython`.
- Use the `%run` magic to run the file you created.
- Use the `%who` magic to verify that the variable you defined exists.
- Exit Jupyter console by typing `exit`.

## 5: Autocompletion

If you hit the *TAB* key while typing a variable name, Jupyter will show you all the possible variables it could be, or auto-complete the name if there's only a single option. If you hit *TAB* after typing a variable name, Jupyter will show you the methods on the variable.

Autocompletion makes it much quicker to write code, and can also enable you to discover new methods on variables.

### Instructions

- Open the Jupyter console by typing `ipython`.
- Create a variable.
- Experiment with autocompletion by typing the variable name.
- Exit Jupyter console by typing `exit`.

## 5: Autocompletion

If you hit the *TAB* key while typing a variable name, Jupyter will show you all the possible variables it could be, or auto-complete the name if there's only a single option. If you hit *TAB* after typing a variable name, Jupyter will show you the methods on the variable.

Autocompletion makes it much quicker to write code, and can also enable you to discover new methods on variables.

### Instructions

- Open the Jupyter console by typing `ipython`.
- Create a variable.
- Experiment with autocompletion by typing the variable name.
- Exit Jupyter console by typing `exit`.

## 6: Accessing The Shell

You can run shell commands in Jupyter console. Just prefix your shell commands with an exclamation point(`!`). Running `!ls` in Jupyter will show the contents of the current directory.

This can be useful when you want to quickly inspect a file or check on the contents of a folder.

### Instructions

- Open the Jupyter console by typing `ipython`.
- Check the contents of the current directory with `!ls`.
- Exit Jupyter console by typing `exit`.

## 7: Pasting In Code

You'll often want to paste code into Jupyter console to see if it runs properly. Because of how Python handles indentation, nested for loops, functions, and if statements will fail if you just copy and paste them in.

In order to paste in code with indents, you'll need to use paste magics:

- `%cpaste` -- opens a special editing area where you can paste in code normally, without whitespace being a problem. You can type `--` alone on a line to exit. After you exit, any code you pasted in will be immediately executed.
- `%paste` -- takes code from your clipboard and runs it in Jupyter. This doesn't work on remote systems, where Jupyter doesn't have access to your clipboard.

### Instructions

Copy the code below:

```
for i in range(10):
    if i < 5:
        print(i)
    else:
        print(i * 2)

```

- Open the Jupyter console by typing `ipython`.
- Paste in the above code using `%cpaste`.
- Exit Jupyter console by typing `exit`.

## 8: Next Steps

You should now be familiar with how to use Jupyter console to interactively execute Python code. Jupyter console is a great addition to your development workflow, and can help you write and debug code.

We encourage you to keep exploring Jupyter console.

Some specific explorations you can try:

- Explore [more](http://ipython.readthedocs.org/en/stable/interactive/magics.html) of the magics.
- Try using Jupyter to debug exceptions.
- Develop a Python script locally, and see if Jupyter can help with your workflow.

### Instructions

Start the Jupyter console, do some exploration, and exit the console.

# Piping and redirecting output

## 1: Appending

In an earlier mission, we looked at how to redirect output from a command to a file using `>`. Here's an example:

```
echo "This is all a dream..." > dream.txt
```

If the file `dream.txt` already exists, the above code will overwrite the file with the string `This is all a dream...`. If the file `dream.txt` doesn't exist, it will be created, and the string `This is all a dream...` will be used as the content. This involves redirecting from the *standard output* of the command to the *standard input* of the file.

If we don't want to overwrite `dream.txt`, and we instead want to add to it, we can use `>>`.

```
echo "Wake up!" >> dream.txt
```

The above code will append `This is all a dream...` to the file `dream.txt`. The file will still be created if it didn't exist.

### Instructions

- Overwrite the file `beer.txt` with the string `99 bottles of beer on the wall...`.
- Append the string `Take one down, pass it around, 98 bottles of beer on the wall...` to the file `beer.txt`.

```
~$ echo '99 bottles of beer on the wall...' > beer.txt
~$ echo 'Take one down, pass it around, 98 bottles of beer on the wall...' >> beer.txt
```

## 2: Redirecting From A File

We've seen how to redirect from a command to a file. We can also redirect the other way, from a file to a command. This involves redirecting from the *standard output* of the file to the *standard input* of the command.

In our last screen, the file `beer.txt` ends up looking like this:

```

```

```
99 bottles of beer on the wall...
```

```
Take one down, pass it around, 98 bottles of beer on the wall...
```

The Linux [sort](https://en.wikipedia.org/wiki/Sort) command will sort the lines of a file in alphabetical order. If we pass the `-r` flag, the lines will be sorted in reverse order.

```

```

```
sort < beer.txt
```

The above code will sort each of the lines in `beer.txt` in order.

### Instructions

- Use the [sort](https://en.wikipedia.org/wiki/Sort) command to sort the lines of `beer.txt` in reverse order.

```
sort -r < beer.txt
```

## 3: The Grep Command

Sometimes, we'll want to search through the contents of a set of files to find a specific line of text. We can use the [grep](http://www.gnu.org/software/grep/manual/grep.html) command for this.

```

```

```
grep "pass" beer.txt
```

The above command will print any lines in `beer.txt` where the string `pass` appears, and highlight the string `pass`.

We can specify multiple files by passing in more arguments:

```

```

```
grep "beer" beer.txt coffee.txt
```

This will show all lines from either file that contain the string `beer`.

### Instructions

- Make a file called `coffee.txt` that has two lines of text in it:

  ```
  Coffee is almost as good as beer,
  But I could never drink 99 bottles of it

  ```

- Use the `grep` command to search `beer.txt` and `coffee.txt` for the string `bottles of`.

```
~$ echo 'Coffee is almost as good as beer,\nBut I could never drink 99 bottles of it' > coffee.txt
~$ grep "bottles of" beer.txt coffee.txt
```

## 4: Special Characters

Like we did in the last screen, sometimes we'll want to execute commands on a set of files. There were only `2` files in the last screen though, `beer.txt` and `coffee.txt`. But what if we wanted to search through all `1000` files in a folder? We definitely wouldn't want to type out all of the names. Let's say we have the following files in a directory:

```

```

```
beer.txt
```

```
beer1.txt
```

```
beer2.txt
```

```
coffee.txt
```

```
better_coffee.txt
```

If we wanted to search for a string in `beer1.txt` and `beer2.txt`, we could use this command:

```

```

```
grep "beer" beer1.txt beer2.txt
```

We could also use a wildcard character, `?`. `?` is used to represent a single, unknown character. We could perform the same search we did above like this:

```

```

```
grep "beer" beer?.txt
```

The wildcard above will match both `beer1.txt` and `beer2.txt`. We can use as many wildcards as we want in a filename.

### Instructions

- Create empty files called `beer1.txt` and `beer2.txt`.
- Use `grep` and the `?` wildcard character to search for `beer`in both `beer1.txt` and `beer2.txt`.

```
~$ touch beer1.txt
~$ touch beer2.txt
~$ grep "beer" beer?.txt
```

## 5: The Star Wildcard

We learned about the `?` wildcard character in the last screen, but there are also other wildcard characters. Let's say we again have the following files in a directory:

```

```

```
beer.txt
```

```
beer1.txt
```

```
beer2.txt
```

```
coffee.txt
```

```
better_coffee.txt
```

We can use the `*` character to match any number of characters, including `0`.

```

```

```
grep "beer" beer*.txt
```

The above command will search for the string `beer` in `beer.txt`, `beer1.txt`, and `beer2.txt`. We can also use the wildcard to match more than `1`character:

```

```

```
grep "beer" *.txt
```

The above command will search for the string `beer` in any file that has a name ending in `.txt`.

We can use wildcards anytime we would otherwise enter a filename. For example:

```

```

```
ls *.txt
```

The above command will list any files with names ending in `.txt` in the current directory.

### Instructions

- Use `grep` and the `*` wildcard character to search for `beer`in all the files ending in `.txt` in the home directory.

```
grep "beer" *.txt
```

## 6: Piping Output

The pipe character, `|`, allows you to send the *standard output* from one command to the *standard input* of another command. This can be very useful for chaining together commands.

For example, let's say we had a file called `logs.txt` with `100000` lines. We only want to search the last `10` lines for the string `Error`. We can use the `tail -n 10 logs.txt` to get the last `10` lines of `logs.txt`. We can then use the pipe character to chain it with a `grep`command to perform the search:

```

```

```
tail -n 10 logs.txt | grep "Error"
```

The above command will search the last `10` lines of `logs.txt` for the string `Error`.

We can also pipe the output of a Python script. Let's say we had this script called `rand.py`:

```

```

```
import random
```

```
for i in range(10000):
```

```
    print(random.randint(1,10))
```

The above script will use the [random](https://docs.python.org/2/library/random.html) library to generate a sequence of random integers, ranging in value from `0` to `10`, and will print them to the *standard output*.

This command will run the script, and search each line of output to see if a `9`occurs:

```

```

```
python rand.py | grep 9
```

Any lines that output a `9` will be printed.

### Instructions

- Make a Python script that generates output.
- Use pipes and `grep` to search the output of the script.

```
~$ echo -e "import random\nfor i in range(10000):\n    print(random.randint(1,10))\n" > rand.py
~$ python rand.py | grep 9
```

## 7: Chaining Commands

If we want to run two commands sequentially, but not pass output between them, we can use `&&` to chain them. Let's say we want to add some content to a file, then print the whole file:

```

```

```
echo "All the beers are gone" >> beer.txt && cat beer.txt
```

This will first add the string `All the beers are gone` to the file `beer.txt`, then print the entire contents of `beer.txt`. The `&&`only runs the second command if the first command doesn't return an error. If we instead tried this:

```

```

```
ec "All the beers are gone" >> beer.txt && cat beer.txt
```

We'd get an error, and nothing would be printed, because we used the command `ec` instead of `echo`.

### Instructions

- Add a line to `beer.txt`, and then print the contents of the file with `cat`.

```
echo "All the beers are gone" >> beer.txt && cat beer.txt
```

## 8: Escaping Characters

There are quite a few special characters that bash uses. A full list can be found [here](http://tldp.org/LDP/abs/html/special-chars.html). When you use these characters in a string or a command, and you don't want them to have a special effect, you may have to *escape* them.

Escaping tells the shell to not treat the character as special, but to treat it as a plain character instead. Here's an example:

```

```

```
echo ""Get out of here," said Neil Armstrong to the moon people." >> famous_quotes.txt
```

The above command won't work as we intend because the quotes inside the string will be treated as special. But what we want to do is add the quotes into the file.

We use a backslash (`\`) as an escape character -- if you add a backslash before a special character, the special character is treated like plain text.

```

```

```
echo "\"Get out of here,\" said Neil Armstrong to the moon people." >> famous_quotes.txt
```

The command above has the double quotes escaped with a backslash, so it will work as we intend.

### Instructions

- Use the `echo` command to add a double quote character into a file.

```
echo "\"Get out of here,\" said Neil Armstrong to the moon people." >> famous_quotes.txt
```

# Challenge: Data Munging Using The Command Line

## 1: Data Munging

In this challenge, you'll practice the command line concepts you've learned so far by munging datasets using just the command line. [Data munging](https://en.wikipedia.org/wiki/Data_wrangling) involves transforming datasets to make them easier to work with. Some datasets are too large to load into Python, so looking at them or transforming them beforehand can be useful. Even for smaller datasets, simple exploration on the command line is faster than exploration in Python, and file-based tasks like unifying datasets can be faster on the command line.

You'll be interacting with datasets on U.S. housing affordability from the [U.S. Department of Housing & Urban Development](http://www.huduser.org/portal/datasets/hads/hads.html) in this challenge. To start things off, let's explore the datasets in the first few steps.

### Instructions

- List all of the files in the current directory (the home directory), including the file names, permissions, formats, and sizes.

```
ls -l
```

## 2: Data Exploration

It looks like there are 3 different CSV files, each corresponding to a separate year.

You learned about the `tail` command to display the last `n` rows in a file. To display the first `n` rows (`10` by default), you can instead us [the `head` command](http://bit.ly/22Ia4gh).

### Instructions

- Use the `head` command to display the first 10 rows of each of the 3 CSV files.

```
~$ head Hud_2005.csv
~$ head Hud_2007.csv
~$ head Hud_2013.csv
```

## 3: Filtering

The goal is to eventually get this into a Pandas Dataframe so let's combine the datasets into one file so it can be read in easily. Since each dataset contains the same columns, you need to combine the datasets into one file. You can't, however, just use append the full contents of each file to one final file since each dataset contains the header row. The consolidated file should only contain the header row once (in the first row). You need to instead append the header row to the consolidated file once, then append only the non-header rows from the 3 datasets to the consolidated file.

Here's a reminder of how the first 10 rows of `Hud_2013.csv` looks like:

![Imgur](https://dq-content.s3.amazonaws.com/D1jLnjY.png)

Since the header row is always the first row in each of the datasets, you can just select all rows after the header row. You can use [the command `wc`](http://bit.ly/1ZqAXDh) along with the `l` flag to return the number of lines for a specified file. You can use each file's line count combined with the `tail` command to return the last `n` lines of a file.

### Instructions

- Create the file `combined_hud.csv` and append the header row from one of the datasets.
- Select all non-header rows from `Hud_2005.csv` and append to `combined_hud.csv`.
- Display the first 10 rows in `combined_hud.csv` to verify your work.

```
head -1 Hud_2005.csv > combined_hud.csv
wc -l Hud_2005.csv
tail -46853 Hud_2005.csv >> combined_hud.csv
head combined_hud.csv
```

## 4: Consolidating Datasets

Looks good! Now finish the job by adding in the data from the other datasets.

### Instructions

- Append the remaining datasets in the order of the years they describe.
  - Select all non-header rows from `Hud_2007.csv` and append to `combined_hud.csv`.
  - Select all non-header rows from `Hud_2013.csv` and append to `combined_hud.csv`.
- Display the last 10 rows of `combined_hud.csv` and verify that they match the last 10 rows of `Hud_2013.csv`.

```
wc -l Hud_2007.csv
tail -42729 Hud_2007.csv >> combined_hud.csv
wc -l Hud_2013.csv
tail -64535 Hud_2013.csv >> combined_hud.csv
```

## 5: Counting

Now that you have a consolidated dataset, you can start to answer basic questions on the entire dataset.

### Instructions

- Count and display the number of lines in `combined_hud.csv` containing `1980-1989`.

```
grep '1980-1989' combined_hud.csv | wc -l
```

## 6: Next Steps

In this challenge, you learned about a few useful commands for exploring files and practiced data munging from the command line. Next in this course is a guided project where you'll explore how to create Python scripts from the command line for more robust and reusable logic.

# Guided Project: Transforming data with Python

## 1: How Guided Projects Work

Welcome to the first Dataquest guided project! Guided projects are a way to help you synthesize concepts learned during the Dataquest missions, and start building a portfolio. Guided projects go above and beyond regular projects by providing an in-browser coding experience, along with help and hints. Guided projects bridge the gap between learning using the Dataquest missions, and applying the knowledge on your own computer.

Guided projects help you develop key skills that you'll need to perform data science work in the "real world". Doing well on these projects is slightly different from doing well in the missions, where there is a "right" answer. In the guided projects, you'll need to think up and create solutions on your own (although we'll be there to help along the way).

The guided project interface is structured much like an IDE on your local machine would be. This area contains text and instructions. You can advance between steps in the project whenever you want -- since there's no "right" answer for any screen, the text is mainly for you to use as a reference and guide as you build the project. To the right is a file browser interface, where you can view, create, and edit files. Under the file browser is a terminal window, where you can run shell commands.

**Note: Only files stored in the project folder, in this case /home/dq/scripts, will be saved! If you make changes to files elsewhere, they won't be saved.**

As you go through this project, [Google](https://www.google.com/), [StackOverflow](https://www.stackoverflow.com/), and the documentation for various packages will help you along the way. All data scientists make extensive use of these and other resources as they write code, and so should you.

We'd love to hear your feedback as you go through this project, and we hope it's a great experience!

## 2: The Dataset

In this project, you'll be working with a dataset of submissions to [Hacker News](http://news.ycombinator.com/) from 2006 to 2015. Hacker News is a site where users can submit articles from across the internet (usually about technology and startups), and others can "upvote" the articles, signifying that they like them. The more upvotes a submission gets, the more popular it was in the community. Popular articles get to the "front page" of Hacker News, where they're more likely to be seen by others.

The dataset you'll be using was compiled by Arnaud Drizard using the Hacker News API, and can be found [here](https://github.com/arnauddri/hn). We've sampled `10000` rows from the data randomly, and removed all extraneous columns. Our dataset only has four columns:

- `submission_time` -- when the story was submitted.
- `upvotes` -- number of upvotes the submission got.
- `url` -- the base domain of the submission.
- `headline` -- the headline of the submission. Users can edit this, and it doesn't have to match the headline of the original article.

You'll be writing scripts to answer some main questions:

- What words appear most often in the headlines?
- What domains were submitted most often to Hacker News?
- At what times are the most articles submitted?

You'll be answering these questions by writing command line scripts, instead of using IPython notebook. IPython notebooks are great for quick data visualization and exploration, but Python scripts are the way to put anything we learn into production. Let's say you want to make a website to help people write headlines that get as many upvotes as possible, and submit articles at the right time. To do this, you'll need scripts.

## 3: Reading The Data

There should be a file called `read.py`already open. You can run this from the command line by being in the same folder, and typing `python read.py`. Of course, there's nothing in the file right now. You might recall from the last mission that you can put this into a file to run it from the command line:

```

```

```
if __name__ == "__main__":
```

```
    print("Welcome to a Python script")
```

This will print `Welcome to a Python script` on the command line if you put it into a file and run it.

We can also add functions into a file by writing them like normal:

```

```

```
def load_data():
```

```
    pass
```

```

```

```
    if __name__ == "__main__":
```

```
        # This will call load_data if you run the script from the command line.
```

```
        load_data()
```

Function definitions should come before the `if __name__ == "__main__"` line. These functions can be imported from other files.

We'll be adding some code to the `read.py`file that will help us load in the dataset and do some initial processing. We'll then be able to import the code to read in the dataset from other scripts we develop.

### Instructions

- In the `read.py` file, read the `hn_stories.csv` file into a Pandas Dataframe.
- There is no header row in the data, so the columns don't have names. See [this stackoverflow thread](http://stackoverflow.com/questions/11346283/renaming-columns-in-pandas) for how to add column names. Add the column names from the last screen (`submission_time`, `upvotes`, `url`, and `headline`) to the Dataframe.
- Create a function called `load_data` that takes no inputs, but contains the code to read in and process the dataset.`load_data` should return a Pandas Dataframe with the column names set correctly.

As you work on these steps, you should be running your script on the command line every so often and verifying that things are working. You can run `read.py` from the command line by calling `python read.py`. The first verification is to make sure that you don't see any errors. The second one is to call `print` at key points in your code, and make sure that the output looks like what you expect. You might want to do this after each step above. This is a good general rule of thumb to follow when writing new code.

## 4: Which Words Appear In The Headlines Often?

We now want to figure out which words appear most often in the headlines. We'll be developing another script, called `count.py` to accomplish this. We'll need to import our `load_data` function from `read.py` into `count.py` so we can use it.

You'll recall that if you have a folder with two files, `read.py` and `count.py`, you can use the function `load_data` in `read.py` from `count.py` by writing the following code in `count.py`:

```

```

```
import read
```

```
df = read.load_data()
```

### Instructions

Writing the script for this will require a series of steps:

- Make a file called `count.py`, using the file browser, or the command line.
- Import `load_data` from `read.py`, and call the function to read in the dataset.
- The order in which you do the below two steps is up to you, but it's suggested to first combine all the headlines (you can use a *for* loop for this, among other methods), and then split everything into words.Combine all of the headlines together into one long string. You'll want to leave a space between each headline when you combine them. [Here's](http://stackoverflow.com/questions/4435169/good-way-to-append-to-a-string) a good reference on joining strings.Figure out how to split the long string into words. Each headline is a string, such as `Anticlimax As Motivation Killer`. Combining that with `Swype acquired by Nuance for 100 million` would look like `Anticlimax As Motivation Killer Swype acquired by Nuance for 100 million`. Adding more headlines would make a longer string. You'll need to figure out a way to split the long string, so you end up with a list of words. The documentation for [str](https://docs.python.org/3/library/stdtypes.html#textseq) might help here.
- You might want to think about lowercasing each word, so `Hello` and `hello` aren't treated as different words when you do a count.
- Find a way to count up how many times each word occurs in the list. The [Counter](https://docs.python.org/3/library/collections.html#collections.Counter) class might help you.
- Add code to print the `100` words that occur the most in your data.

## 5: Which Domains Were Submitted Most Often?

You can now move on to our second question, and explore which domains were submitted most often. We'll want to make a separate script, called `domains.py`, for this.

### Instructions

Here are the steps:

- Make a file called `domains.py`, using the file browser, or the command line.
- Add in the code to read the file `hn_stories.csv`, and add column names.
- You can think of each domain name as a "word". A domain will look like `scala-lang.org`, or `blog.iweb.com`.You can use the `value_counts` method in pandas to count the number of occurrences of each value in a column. [Here](http://pandas.pydata.org/pandas-docs/version/0.17.1/generated/pandas.Series.value_counts.html) are the docs.
- Print the `100` most submitted domains.

By default, Pandas only prints `10` rows of a Dataframe or Series. There is a pandas option to make it print more rows (see [this](http://stackoverflow.com/questions/19124601/is-there-a-way-to-pretty-print-the-entire-pandas-series-dataframe) thread on Stackoverflow), but there are bugs with it and Series. Instead, just loop through the series and print the index value, and the total. Here's some sample code:

```
for name, row in domains.items():
    print("{0}: {1}".format(name, row))

```

The above code assumes that the results of running `value_counts` is assigned to `domains`.

You can extend this analysis and make it a bit more robust by removing subdomains. For example, `blog.iweb.com` and `iweb.com` would be separate domains at the moment, but they are the same. By removing the subdomain, you can turn `blog.iweb.com` into `iweb.com`. You can remove the subdomain using the `apply` method on Pandas Series and Dataframes.[Here's](http://pandas.pydata.org/pandas-docs/version/0.17.1/generated/pandas.DataFrame.apply.html) the documentation.

## 6: When Are The Most Articles Submitted?

We want to know when the most articles are submitted. One easy way to reframe this is to look at what hour articles are submitted. To figure this out, we'll need to use the `submission_time` column.

The `submission_time` column contains timestamps, which look like this: `2011-11-09T21:56:22Z`. These times are expressed in [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), which is a universal time zone used by most software for consistency (imagine a database populated with times all having different timezones; it would be a huge pain to work with).

To get hour from a timestamp, we can use the `dateutil` library. The `parser`module in `dateutil` contains the `parse`function, which can take in a timestamp, and return a *datetime* object. [Here's](https://dateutil.readthedocs.org/en/latest/parser.html) a link to the documentation. After parsing the timestamp, the `hour` property of the resulting date object will tell you the hour the article was submitted.

### Instructions

- Make a file called `times.py` to find the submission times.
- Write a function to extract the hour from a timestamp. This function should first use `dateutil.parser.parse` to parse the timestamp, then extract the hour from the resulting *datetime* object, then return the hour.
- Use the pandas `apply` method to make a column of submission hours.
- Use the `value_counts` method to find the number of occurences of each hour.
- Print out the results.

You can repeat this procedure to find how many articles were submitted on each day of the month, year, minute, day of the week, and so on.

## 7: Next Steps

That's all for the guided steps, but feel free to keep going through the data and answering questions. We encourage you to think of your own questions, and to be creative in exploring the dataset!

If you can't think of any questions, some interesting ones are:

- What headline length leads to the most upvotes?
- What submission time leads to the most upvotes?
- How are the total numbers of upvotes changing over time?

You can write scripts and explore here, or download the code to your computer using the download icon to the right. You'll then be able to run the scripts on your own computer.

Hope this guided project has been a good experience, and please email us at hello@dataquest.io if you want to share your work -- we'd love to see it!

# Data Cleaning and Exploration Using Csvkit

## 1: Csvkit

So far, we've been using the default command line tools to clean, munge, and explore data. Tools like `wc` and `head` are useful tools, but weren't designed specifically for working with datasets and are limited in many ways. These tools lack features specific to working with tabular datasets, like parsing the header row or understanding the row and column layout. Because of this, in the [Data Munging Using the Command Line challenge](https://www.dataquest.io/mission/198), you had to specifically compute the number of lines in each CSV file using the `wc` tool and use that number to select just the non-header rows using the `tail` tool. You then had to repeat this for each CSV file you were trying to merge into the resulting, single file!

In this mission, we'll learn about the Csvkit library, which supercharges your workflow by adding 13 new command line tools specifically for working with CSV files. We'll focus on these 5 tools from Csvkit:

- **csvstack**: for stacking rows from multiple CSV files.
- **csvlook**: renders CSV in pretty table format.
- **csvcut**: for selecting specific columns from a CSV file.
- **csvstat**: for calculating descriptive statistics for some or all columns.
- **csvgrep**: for filtering tabular data using specific criteria.

We'll be using csvkit version 0.9.1 in this mission and you can read about the installation procedure in the [documentation](https://csvkit.readthedocs.io/en/0.9.1/install.html). We'll continue to work with the same 3 datasets on housing affordability:

- Hud_2005.csv,
- Hud_2007.csv,
- Hud_2013.csv.

## 2: Csvstack

To start, let's circle back to the task of merging 3 CSV files into 1 file. We can use [csvstack tool](http://csvkit.readthedocs.io/en/0.9.1/scripts/csvstack.html#description) to consolidate the rows from multiple CSV files and redirect the stdout to a new file:

```

```

```
csvstack file1.csv file2.csv file3.csv > final.csv
```

As long as the header row for each file in the stdin to csvstack is the same, the first row in the resulting file will match this header row. After the header row, `final.csv` will contain all of the non-header rows from `file1.csv`, then all of the non-header rows from `file2.csv`, then finally the non-header rows from `file3.csv`. If you don't redirect the stdout of csvstack to a file or a tool like `head`, the full output will be rendered in the terminal. This can cause your terminal to grind to a halt as it tries to process and display all of the output and you want to be extra careful to avoid doing so.

If you peeked at the [documentation](http://csvkit.readthedocs.io/en/0.9.1/scripts/csvstack.html#description), you may have noticed that the behavior of csvstack can be modified using a few different flags. For example,

if you want to be able to trace the file where each row originated from in the merged file, you can use the `-g` flag to specify a grouping value for each filename. When stacking the rows from a file, csvstack will add the corresponding value in a new column. Lastly, you can use the `-n` flag to specify the name of this new column. The following code will create a new column named `origin`, containing the values `1`, `2`, or `3` depending on which file that row originated from:

```

```

```
csvstack -n origin -g 1,2,3 file1.csv file2.csv file3.csv > final.csv
```

The rows in `final.csv` that originated from `file1.csv` will contain the value `1`in the `origin` column and those from `file2.csv` will contain the value `2` in the `origin` column. Let's now use csvstack to combine the 3 datasets on U.S. housing affordability from the last challenge.

### Instructions

- Merge `Hud_2005.csv`, `Hud_2007.csv`, and `Hud_2013.csv`in that order into one file:
  - Name the resulting file `Combined_hud.csv`.
  - Add an extra column named `year` which contains the year value from the file name for each row. E.g. the rows that originated from `Hud_2005.csv` should have `2005` as the value in the `year` column.
- Use `head` to preview the first few rows of `Combined_hud.csv`.
- Use the `wc` command with the `l` flag to confirm that the merged file contains `154118` rows.

```
ls -l
csvstack -n year -g 2005,2007,2013 Hud_2005.csv Hud_2007.csv Hud_2013.csv > Combined_hud.csv
head -5 Combined_hud.csv
```

## 3: Csvlook

While `head` allows you to quickly observe the first few rows in a file, it doesn't attempt to format the rendered output at all. CSV files are tabular and it's incredibly useful to observe this structure and other data tools like Pandas and Microsoft Excel factored that notion in when displaying tabular data. Thankfully, we can use the csvlook tool to display tabular data in the table format we're used to.

The [csvlook](http://csvkit.readthedocs.io/en/0.9.1/scripts/csvlook.html) tool parses CSV formatted data from it's stdin and outputs a pretty formatted table representation of that data to it's stdout:

```

```

```
head -10 final.csv | csvlook
```

Let's use csvlook to explore the first few rows from the CSV file we created in the last screen.

### Instructions

- Use **csvlook** to preview the first 10 rows from `Combined_hud.csv`.

```
head -10 Combined_hud.csv | csvlook
```

## 4: Csvcut

Csvlook returned a table formatted output of the merged CSV file. Let's now explore individual columns using the [csvcut](http://csvkit.readthedocs.io/en/0.9.1/scripts/csvcut.html) tool. Using the csvcut command with just the `-n` flag parses and displays all the columns in a CSV file along with an unique integer identifier for each column:

```

```

```
csvcut -n Combined_hud.csv
```

will output:

```

```

```
1: year
```

```
2: AGE1
```

```
3: BURDEN
```

```
4: FMR
```

```
5: FMTBEDRMS
```

```
6: FMTBUILT
```

```
7: TOTSAL
```

You can use the integer identifier for each column and the `-c` flag to select just a specific column:

```

```

```
csvcut -c 1 Combined_hud.csv
```

will output just the `year` column. You want to avoid displaying the entire column since it contains 154118 rows and your terminal window will severely come to a halt attempting to display all that information. Instead, you can pipe the column output to `head` to preview just the first `n` rows.

### Instructions

- Use csvcut to return all of the column names from `Combined_hud.csv`.
- Use csvcut to display **just** the first 10 values in the `AGE1`column.

```
~$ csvcut -n Combined_hud.csv
~$ csvcut -c 2 Combined_hud.csv | head -10
```

## 5: Csvstat

Now that we know how to select specific columns, we can select a column and pipe it to the **csvstat** tool to calculate summary statistics for that column:

```

```

```
csvcut -c 4 Combined_hud.csv | csvstat
```

This calculates a full suite of summary statistics, including:

- max,
- min,
- sum,
- mean,
- median,
- standard deviation.

Depending on the size of the data, the full summary statistics for a column can take a long time and you often just want a specific summary statistic. You can use `--` flags to choose specific summary statistics, which will greatly improve the speed:

```

```

```
# Just the max value.
```

```
csvcut -c 2 Combined_hud.csv | csvstat --max
```

```
# Just the mean value.
```

```
csvcut -c 2 Combined_hud.csv | csvstat --mean
```

```
# Just the number of null values.
```

```
csvcut -c 2 Combined_hud.csv | csvstat --nulls
```

You can see a full list of flags in the [documentation](http://csvkit.readthedocs.io/en/0.9.1/scripts/csvstat.html#description). If you want to calculate summary statistics over all the columns in a CSV file, you can pass the file to csvstat directly:

```

```

```
csvstat Combined_hud.csv
```

### Instructions

- Use csvstat to calculate just the mean for each column in `Combined_hud.csv`.

```
csvstat --mean Combined_hud.csv
```

## 6: Csvcut | Csvstat

Let's use csvcut and csvstat to search for any problematic values in the `AGE1`column.

### Instructions

- Use csvstat to calculate the full summary statistics for just the `AGE1` column.

```
csvcut -n Combined_hud.csv
csvcut -c 2 Combined_hud.csv | csvstat
```

## 7: Csvgrep

You'll notice that `-9` is the most common value in the `AGE1` column, which is problematic since age values have to be greater than `0`. We can use [csvgrep](http://csvkit.readthedocs.io/en/0.9.1/scripts/csvgrep.html) to select all the rows that match a specific pattern to dive a bit deeper. By default, csvgrep will search all of the rows in the dataset but we can restrict the search to specific columns using the `-c` flag (just like with csvcut). We then use the `-m` flag to specify the pattern:

```

```

```
csvgrep -c 2 -m -9 Combined_hud.csv
```

This command will return all rows from `Combined_hud.csv` with `-9` as the value for the `AGE1` column. The behavior of csvgrep can be customized using the flags. For example, you can use the `-r` flag to pass in a regular expression as the pattern instead. We're now going to combined several of the tools we've talked about so far so that you can see the real power of using the csvkit tools combined with other CLI tools.

### Instructions

- Display the first 10 rows from `Combined_hud.csv` where the value for the `AGE1` column is `-9` in a pretty table format.

```
csvgrep -c 2 -m -9 Combined_hud.csv | head -10 | csvlook
```

## 8: Filtering Out Problematic Rows

Let's now filter out all of these problematic rows from the dataset since they have data quality issues. Csvkit wasn't developed with a sharp focus on editing existing files, and the easiest way to filter rows is to create a separate file with just the rows we're interested in. To accomplish this, we can redirect the output of csvgrep to a file. So far, we've only used csvgrep to select rows that match a specific pattern. We need to instead select the rows that *don't* match a pattern, which we can specify with the `-i` flag. You can read more about this flag in the [documentation](http://csvkit.readthedocs.io/en/0.9.1/scripts/csvgrep.html).

### Instructions

- Select all rows where the value for `AGE1` isn't `-9` and write just those rows to `positive_ages_only.csv`.

```
csvgrep -c 2 -m -9 -i Combined_hud.csv > positive_ages_only.csv
```

## 9: Next Steps

In this challenge, you learned how to use the csvkit library to explore and clean CSV files. You should use csvkit whenever you need to quickly transform or explore data from the command line, but remember that it has a few limitations:

- Csvkit is not optimized for speed and struggles to run some commands over larger files.
- Csvkit has very limited capabilities for actually editing problematic values in a dataset, since the community behind the library aspired to keep the library small and lightweight.

