# Command Line Basics

## 1: Introduction

Most people interact with a computer through a [graphical user interface](https://en.wikipedia.org/wiki/Graphical_user_interface) (GUI), a visual interface that looks like this:

![img](https://dq-content.s3.amazonaws.com/P8qZfFC.jpg)

This is a screenshot of the desktop from a Macintosh computer. There's an icon for each program, and we can click on one to launch the corresponding program. Before GUIs came along, the most common way to interact with a computer was through a [command line interface](https://en.wikipedia.org/wiki/Command-line_interface), which we also refer to as a *shell* or *terminal* (we'll use these terms interchangeably in this mission). A command line interface lets us switch between folders and launch programs by typing commands. A command line interface is often faster and more powerful than a GUI for programming tasks.

Virtually all programmers and data scientists make extensive use of the terminal, and knowing how to interact with it is a critical skill. Every operating system has a slightly different terminal with different commands and syntax. Because [Linux](https://en.wikipedia.org/wiki/Linux) and [OS X](https://en.wikipedia.org/wiki/OS_X) are both based on an older operating system called [UNIX](https://en.wikipedia.org/wiki/Unix), they both have very similar terminals.[Windows](https://en.wikipedia.org/wiki/Microsoft_Windows) isn't based on UNIX, so it has different commands. It's more common to use Linux and OS X for data science work, so our terminal missions will use Linux. In particular, we'll be running [Ubuntu 14.04](https://en.wikipedia.org/wiki/List_of_Ubuntu_releases#Ubuntu_14.04_LTS_.28Trusty_Tahr.29).

To the right is a terminal interface. The dollar sign is called the **command prompt**. Anything we type to the right of it is a shell command that will execute immediately when we press the Enter key.

### Instructions

Type `pwd` into the terminal to the right, and press the **Enter** key on your keyboard.

- This command returns the current directory (folder you're located in). It's an acronym that stands for [**print working directory**](http://linux.die.net/man/1/pwd).

```
pwd
```

## 2: Overview Of The Filesystem

When we typed `pwd` on the last screen, the terminal printed out the name of the folder we were in. The terminal was designed to navigate and switch between directories. You've probably browsed through directories using a GUI like [Explorer](https://en.wikipedia.org/wiki/File_Explorer) in Windows, or [Finder](http://bit.ly/1NSrDmS) in OS X. The terminal lets us do the same thing, except that we type a command instead of clicking on a folder name. Once you learn the terminal, you'll see that using it to navigate through directories is much faster than using a GUI.

Computers store files in directories, or folders. Each directory can have subdirectories or files within it. Right now, we're in the `dq` folder, which is a subfolder of the `home` folder. That means our current folder path is `/home/dq`.`home` is a folder that exists at the root of the filesystem; consider it the "master" or "original" folder. We indicate that `dq` is in the `home` folder by separating the two folder names with a forward slash -- `home/dq`. We indicate that `home` is at the root of the filesystem with the leading slash -- `/home`.

The root directory, or `/`, is a special directory that will navigate to the root of the filesystem. We can think of our filesystem as a tree that looks like this:

```
/
|__home
    |
    |__dq
```

`home` is a folder at the root of the filesystem, and `dq` is a folder inside the `home` folder.

We can use the `cd` command to switch directories. For example, we can type `cd /` to switch to the root directory.

### Instructions

- Type `cd /` to switch to the root directory.

```
cd /
```

## 3: Absolute Vs. Relative Paths

When we typed the backslash (`/`), the terminal switched to the root directory. Any path that starts with `/` is an **absolute path** that's written in relation to the root of the filesystem. No matter what folder we're in, typing `cd /home/dq` will switch to the `dq` folder, inside the `home`folder, which is at the root of the filesystem.

On the other hand, **relative paths** are relative to the directory we're currently in. These don't start with a forward slash. If we're in the `home` folder, for example, typing `cd dq` will move us to `/home/dq`. If we're in the root (`/`) of the filesystem, however, typing `cd dq` will cause an error because the shell thinks we're asking for a `dq` subfolder within the current directory (which doesn't exist), rather than the one in the `home` folder.

### Instructions

Type `cd home` to move into the `home` folder.

- This is a *relative* path -- not an *absolute* path.
- This works because the `home` folder is located in the root of the filesystem, and you're currently in the root folder.

```
cd home
```

## 4: Getting Back To The Dq Folder

Now that we're in the `home` folder, let's navigate back to the `/home/dq` folder.

### Instructions

- Type `cd dq` to go to the `/home/dq` folder.

```
cd dq
```

## 5: Understanding Users

Most popular operating systems have a concept of **users**. Users have certain permissions within the system, can create their own files, and run their own programs. Users can also restrict other users from accessing their files and running their programs.

Right now, we're logged in as a user called `dq` (short for Dataquest). We can check which user we're logged in as with the `whoami` command.

### Instructions

- Type `whoami` to check your username.

```
whoami
```

## 6: The Home Directory

Every user has their own home directory within `/home` where they can add files specific to their username. For example, the home directory for `dq` is at `/home/dq`. The tilde (`~`) is a shortcut for referring to the home directory. Typing `cd ~` will automatically take us to the current user's home directory.

### Instructions

- Type `cd /` to switch to the root directory.
- Then, type `cd ~` to switch back to the home directory for `dq`.

```
cd /
cd ~
```

## 7: Making A New Directory

We can also create files and directories with the terminal. We'll explore how to make a directory first. We can do this with the `mkdir` command. We just have to type `mkdir test` to create a directory called `test`. This will also add a folder icon to the corresponding location in your GUI file browser (Explorer in Windows, or Finder in OS X).

Note that rules about absolute and relative paths apply here, and in almost every command that involves paths. If we type `mkdir test`, the shell will make a directory called `test` in the current folder, because it's a relative path. If we type `mkdir /home/dq/test` instead, the shell will make a folder called `test`inside the `/home/dq` folder, because it's an absolute path.

### Instructions

- Type `mkdir test` to create a directory called `test` inside the user `dq`s home folder.

```
mkdir test
```

## 8: Using Command Options To Modify Behavior

Commands have *options* that can modify their behavior. We specify these options by adding them, preceded by one dash, after we invoke the command.

For example, adding the `-v` option, or "flag," after the `mkdir` command will turn on "verbose" mode, and print output when it makes the folder.

### Instructions

- Type `mkdir -v test2` to check out the verbose option.

```
mkdir -v test2
```

## 9: Reviewing Available Command Options

Most commands will let us use the `--help` flag to see what all of the possible options are. A flag comes after a command. When you use the `--help` flag, you don't need to specify a directory -- the shell will automatically know that you want to see the help contents.

### Instructions

- Type `mkdir --help` to see the help contents for the `mkdir`command.

```
mkdir --help
```

## 10: Listing The Contents Of A Directory

Now that we've made a couple of directories, let's see what's in our home folder. We can use the `ls` command to list all of the files and folders in a directory. If we pass in the `-l` option, it will format the list nicely as a table.

### Instructions

- Type `ls -l` to see the contents of the home folder for `dq`.

```
ls -l
```

## 11: Removing A Directory

There are two directories, `test` and `test2`. Let's clean up the clutter by removing one of them. We can use the `rmdir` command to delete a directory.

### Instructions

- Delete the `test2` directory using `rmdir`.

```
rmdir test2
```

# Working with Files

## 1: Making A File

We explored directories and looked at files in the last mission. In this mission, we'll look at files more closely and learn how to interact with them on the command line.

First, we need to create a file. While there are several ways to do this, we'll start with the `touch` command. This command will create an empty file with the name we give it. For example, typing `touch file.txt` will create a new file called `file.txt` in the current directory. We can open the file and edit it later on if we want.

We can also use the touch command to change the date we last accessed a file if we need to. You can read more about the touch command on [Wikipedia](https://en.wikipedia.org/wiki/Touch_(Unix)).

### Instructions

- Use the `touch` command to create a file named `test.txt`in the home directory for the user `dq`.
```
touch test.txt
```

## 2: Understanding Standard Streams

Now that you've created the `test.txt`file, you can add text to it in a few different ways. The first is with the `echo`command, which simply prints whatever you tell it to as output. If you type `echo "Dataquest is awesome"`, it will print `Dataquest is awesome`.

It prints this text into a stream called **standard output**, or `stdout`. Every program writes to standard output, and receives input through standard input (`stdin`). Whenever a program experiences an error while running, it writes the error message to standard error (`stderr`). These standard streams are how programs show us output in the terminal, and how we enter input.

`stdout` and `stderr` usually display on the monitor, while `stdin` is the input from the keyboard. In this case, `echo` is taking a string from `stdin`, and printing that string to `stdout`. By default, we see the message that it prints to `stdout`, because it shows on the monitor.

The interfaces look something like this:

Keyboard—Stdin—>Program—Stdout—>/—Stderr—>Display

`stdout`, `stderr`, and `stdin` exist because these standard streams allow the interfaces to be abstract. A program doesn't need to care whether it's getting input from a keyboard, file, or somewhere else. A program also doesn't need to care if it's outputting to the display, a file, or somewhere else. The standard streams allow us to hook various inputs and outputs up to programs without the programs having to concern themselves with what those inputs and outputs are.

### Instructions

- Type `echo "All bears should juggle"` to write `All bears should juggle` to stdout.
```
echo "All bears should juggle"
```
## 3: Redirecting Standard Streams

We can redirect standard streams in order to connect them to different sources. For example, we can connect `stdout` to a file. Afterwards, the program the stream is connected to will write to a file instead of the screen.

To redirect, we use the greater than sign (`>`). For example, `echo "Dataquest is awesome" > dataquest.txt` will write `Dataquest is awesome` to `stdout`, then redirect `stdout` to the file `dataquest.txt`. The end result is that the stream will write `Dataquest is awesome` to the file `dataquest.txt`.

### Instructions

- Echo `All bears should juggle` into the `test.txt` file that we created earlier.
```
echo "All bears should juggle" > test.txt
```

## 4: Editing A File

We can also edit a file directly from the terminal, without redirection. While there are a few programs that let us do this, the simplest is called `nano`. Nano is a command line text editor that lets us edit and save files directly from the terminal.

To run `nano`, type `nano`, followed by the name of the file you want to edit. For example, `nano test.txt` will open the `test.txt` file for editing.

Once a file is open, we can make whatever changes we want, then hit `ctrl+x` to quit. When we quit, the terminal will prompt us to save our work. Typing `Y` (for yes), then pressing Enter will save all changes.

### Instructions

- Open `test.txt` with `nano`, delete the text in the file, and save the file.
```
#
```
## 5: Overview Of File Permissions

In Unix, every file and folder has permissions associated with it. These permissions have three scopes:

- `owner` - The user who created the file or folder
- `group` - Users in the owner's group (on Unix systems, an owner can place users in groups)
- `everyone` - All other users on the system who aren't the user or in the user's group

Each scope can have any of three permissions (a scope can have multiple permissions at once):

- `read` - The ability to see what's in a file (if defined on a folder, the ability to see what files are in a folder)
- `write` - The ability to modify a file (if a folder, the ability to delete, modify, and rename files in the folder)
- `execute` - The ability to run a file (some files are executable, and need this permission to run)

Each permission can be granted or denied to each scope.

You can view the permissions on files and folders using `ls -l`. This command will display the permissions to the left of each file name. Here's an example of what the output looks like:

```
~$ ls -l                                                                                   
    total 4                                                                                    
    -rw-r--r-- 1 dq dq 10 Nov 14 00:08 test.txt
```

In the example above, the permissions for the file `test.txt` are `-rw-r--r--`. There are 10 characters in that string.

| Breaking down the permissions string |      |       |          |
| ------------------------------------ | ---- | ----- | -------- |
| -                                    | rw-  | r--   | r--      |
| Ignore                               | User | Group | Everyone |

We can ignore the first character for now. Starting at the second character, the permissions are split into three groups -- one for `user`, one for `group`, and one for `everyone`.

The owner has the permissions `rw-`, which corresponds to characters `2`through `4` in the string. This means that the owner can read and write the file, but not execute it. The first character represents read permissions, the second represents write permissions, and the third execute permissions. The character for read is `r`, the character for write is `w`, and the character for execute is `x`. If a scope doesn't have a permission, it displays as a dash (`-`). If the permissions for the owner were `rwx` instead, the owner would be able to execute as well.

The permissions for `group` are represented by characters `5` through `7`in the string, which corresponds to `r--`. This means that people in the owner's group can only read the file.

The permissions for `everyone` are `r--`, meaning that anyone who has an account on this machine can read the file.

### Instructions

- Type `ls -l` to see the file permissions in the `/home/dq`directory.
```
ls -l
```
## 6: Octal Notation For File Permissions

We just looked at file permissions that looked like this:

`-rw-r--r--`

We call this symbolic notation for permissions, because it expresses each permission as a symbol. The downside to symbolic notation is that if we want to change permissions, it takes a long time to type the changes out. We can do this more quickly by representing permissions with octal notation.

Octal notation allows us to represent the permissions for all scopes with just `4`digits, rather than the `10` characters involved in symbolic notation. There are `8` possible combinations of the three permissions `r`, `w`, and `x`. We can express each combination, or scope, as a single digit in an octal (base `8`) counting system.

Here are the combinations and their corresponding digits:

- `---` : No permissions; corresponds to `0`
- `--x` : Execute only permission; corresponds to `1`
- `-w-` : Write only permissions; corresponds to `2`
- `-wx` : Write and execute permissions; corresponds to `3`
- `r--` : Read only permissions; corresponds to `4`
- `r-x` : Read and execute permissions; corresponds to `5`
- `rw-` : Read and write permissions; corresponds to `6`
- `rwx` : Read, write, and execute permissions; corresponds to `7`

We can use this system to convert the permissions string `-rw-r--r--` to `0644`. Just like with symbolic notation, don't worry about the first digit in octal notation right this second -- we'll get to it later.

We can pull up a file's octal permissions with the `stat` command. Typing `stat test.txt` will show us some information about the file `test.txt`, including the octal permissions.

```
~$ stat test.txt                                                                           
File: ‘test.txt’                                                                         
Size: 10              Blocks: 8          IO Block: 4096   regular file                   
Device: 60h/96d Inode: 2625        Links: 1                                                
Access: (0644/-rw-r--r--)  Uid: ( 1000/      dq)   Gid: ( 1000/      dq)                   
Access: 2015-11-14 00:08:58.299959914 +0000                                                
Modify: 2015-11-14 00:08:57.930008711 +0000                                                
Change: 2015-11-14 00:08:57.930008711 +0000                                                
Birth: -
```

The `stat` command returns quite a bit of detailed information about the file. Let's focus on the permissions for the moment; we can find them in the `Access` section.

### Instructions

- Type `stat test.txt` to see the file permissions for the `test.txt` file.
```
stat test.txt
```
## 7: Modifying File Permissions

Now that we understand file permissions, we can modify them using the `chmod`command. If we pass in an octal permissions string and a file name, the command will modify the file to assign it the permissions we specified in the string.

Typing `chmod 0664 test.txt` will give the `owner` read and write permissions, the `group` read and write permissions, and everyone else read-only permissions.

### Instructions

Modify `test.txt` so that it has the following permissions:

- `owner` - Read, write, and execute
- `group` - Read and write
- `everyone` - No permissions

Remember to add a `0` in front of the octal permissions string.
```
chmod 0760 test.txt
```
## 8: Moving Files

We can move files with the `mv` command. Typing `mv test.txt /dq` will move the `test.txt` file to the `/dq` folder. This assumes that `test.txt` is in the current directory.

### Instructions

Create a directory named `test` in the home directory of `dq`.

- The full path should be `/home/dq/test`.

Type `mv test.txt test` to move the `test.txt` file to the `test`folder.
```
~$ mkdir test
~$ mv test.txt test
```
## 9: Copying Files

Instead of moving a file, sometimes you'll want to make a copy, and then move that copy somewhere else. The `cp` command is useful for this. `cp test.txt test2.txt`will copy the `test.txt` file, and create a new file called `test2.txt` containing the contents of `test.txt`.

### Instructions

- Copy the file at `/home/dq/test/test.txt` to `/home/dq/test/test2.txt`.
```
cp test/test.txt test/test2.txt
```
## 10: Overview Of File Extensions

Files typically have extensions like `.txt`and `.csv` that indicate the file type. Operating systems use them to determine the default program to open a file with. On Windows, for instance, a text editor will be the default program for files with the `.txt` extension.

Rather than relying on extensions to determine file type, Unix-based operating systems like Linux use media types, which are also called MIME types. The MIME type `application/pdf` indicates that a file is a `pdf`, and the MIME type `image/png`indicates that a file is a `png` image. The first part of the MIME type string is for the type, such as `application` or `image`, and the second part is for the subtype, such as `pdf` or `png`.

There are MIME types for every type of file. The MIME type is stored in the file metadata (which is stored as part of the file). As a result, Linux can determine a file's type and open it properly, even if it doesn't have an extension.

We can rename files and remove extensions whenever we want. In fact, we'll often come across files that don't have extensions, such as `test`.

Specifying a folder as the second argument to `mv` will preserve the file name, and move it into the folder. If we specify a full path instead, including the file name, it will move the original file to the new file name, essentially renaming it. For example, `mv test.txt test2.txt` will move the file `test.txt` to `test2.txt`. This will basically rename `test.txt`.

### Instructions

- Rename `test.txt` in the `/home/dq/test` folder to `test_no_extension`.
```
mv test/test.txt test/test_no_extension
```
## 11: Deleting A File

We can delete a file with the `rm`command. Typing `rm test.txt` will remove the `test.txt` file, for example, provided that it's in the current directory.

### Instructions

- Remove the `/home/dq/test/test2.txt` file.
```
rm /home/dq/test/test2.txt
```
## 12: Bypassing Permissions As The Root User

Unix systems have a special user called the root user. We can run commands as the root user using `sudo`. Adding `sudo` to the beginning of any command will run that command as the root user.

For example, typing `sudo rm test.txt`will switch to the root user, then delete the `test.txt` file as the root user. This is useful in situations where the current user doesn't have permission to delete the file. The root user has all permissions and access to all files by default.

You'll typically need to enter a password to switch to the root user, which makes sense. For security reasons, we don't want anyone to be able to switch to the root user just by typing `sudo` whenever they want.

In the Dataquest terminal, access to the root user is restricted for security reasons. Adding `sudo` to a command will result in an error.

### Instructions

- Try adding `sudo` to a command to see what happens.
```
sudo echo "Hello"
```
# Challenge: Navigating the File System

## 1: Introduction

In this challenge, you'll practice the basics of using the command line by working with files in the filesystem. Working in a command line environment is often faster than using a GUI or writing a Python program to accomplish a specific task. In this challenge, you'll be interacting with a directory of data sets used in other Dataquest missions.

Let's start things off by becoming familiar with the file system.

### Instructions

- Display the current directory.
- Display a list of files in the current directory, along with each file's metadata. Use the appropriate flag to display the file permissions, file size, etc.

```
~$ pwd
~$ ls -l    
```

## 2: Moving Problematic Files To A Separate Folder

There are five data sets in your home directory. While all of them are actually CSV files that should have the `.csv` file extension, three of them have issues with their file extensions. Let's move the problematic files into their own directory.

### Instructions

- Move all of the files that don't have `.csv` as the file extension to a new folder called `problematic`.

```
~$ mkdir /home/dq/problematic
~$ mv /home/dq/crime_rates.json /home/dq/problematic/crime_rates.json
~$ mv /home/dq/forest_fires.cssv /home/dq/problematic/forest_fires.cssv
~$ mv /home/dq/legislators /home/dq/problematic/legislators
```

## 3: Fixing File Extensions

Now that you've quarantined the files with problematic extensions in their own folder, let's fix those extensions.

### Instructions

- Change all of the extensions for the files in the `problematic` folder to `.csv`.
- Display the files in `problematic` to confirm.

```
~$ cd problematic/
~$ mv crime_rates.json crime_rates.csv
~$ mv forest_fires.cssv forest_fires.csv
~$ mv legislators legislators.csv
~$ ls -l
```

## 4: Consolidating Files

Now that you've fixed the files with problematic extensions, consolidate all of the CSV files in one folder.

### Instructions

- Move the remaining files into the `problematic` folder.
- Rename the `problematic` folder to `csv_datasets`.

```
~$ cd /home/dq/
~$ mv nfl.csv problematic/nfl.csv
~$ mv titanic_survival.csv problematic/titanic_survival.csv
~$ mv problematic/ csv_datasets/
```

## 5: Restricting Permissions

Now let's change the permissions for these files so nobody else can mess up the file extensions!

### Instructions

Change the file permissions for all of the CSV files in the `csv_datasets` folder to:

- Owner - Read, write, and execute
- Group - Read
- Everyone - No permissions

Display the list of files in the `csv_datasets` directory to confirm the permissions.

```
cd /home/dq/csv_datasets
chmod 0740 nfl.csv
chmod 0740 titanic_survival.csv
chmod 0740 crime_rates.csv
chmod 0740 forest_fires.csv
chmod 0740 legislators.csv
ls -l
```

## 6: Next Steps

In this challenge, you practiced working with the file system and managing file permissions using the command line. Solidifying these basic concepts has prepared you for the next mission, where we'll explore how to work with variables and programs.

# Working with Programs

## 1: Setting Variables

In these shell tutorials, we've been interacting with a computer through the command line. We type commands in, they execute, and we see the results. Those interactions occur within a [shell](http://bit.ly/1lilHtX), which is a way to access and control a computer. Command line shells have a text interface for typing commands and viewing results, as opposed to graphical shells, which allow us to click on icons with a mouse.

While there are many [UNIX shells](https://en.wikipedia.org/wiki/Unix_shell), [**Bash**](http://bit.ly/1lKpkJy) is one of the most popular. Bash is the default shell on most Linux and OS X computers.

Bash is essentially a program that lets us run other programs. It does this by implementing a command language. This language specifies how to type and structure the commands we want to execute.

A command language is a special kind of programming language through which we can control applications and the system as a whole. Just like Python and other programming languages, we can use Bash to create scripts, set variables, and more. Because it's a language, Bash is far more powerful than a graphical shell.

For example, we can set variables on the command line by assigning values to them. In the command line environment, variables consist entirely of uppercase characters, numbers, and underscores. We can assign any data type to a variable. Here are a few examples of how we can set variables on the command line:

```
OS=linux
OPERATING_SYSTEM="linux"
```

Both of the variables above, `OS` and `OPERATING_SYSTEM`, will actually end up with the same value. That's because **quotes are optional when using strings in Bash, unless the string contains a space**. Bash is sensitive to spaces, so strings that have them won't work properly if we don't surround them with quotes.

This assignment won't work, for example:

```
ANIMAL=Shark with a laser beam on its head
```

This one will:

```
ANIMAL="Shark with a laser beam on its head"
```

It's also important to **avoid adding stray spaces around the equals sign**. For example, this assignment will fail:

```
ANIMAL = "Shark with a laser beam on its head"
```

### Instructions

- Create a variable `FOOD` containing the value `Shrimp gumbo`.

```
FOOD="Shrimp gumbo"
```

## 2: Accessing Variables

In Bash, we can access the value of a variable again after we set it, just like we can with other programming languages like Python. There's one major difference, though -- **we need to add a dollar sign to the beginning of the variable name when we try to retrieve its value**.

If we create a variable named `FOOD` with the value `Shrimp gumbo`, for example, we'll need to use `$FOOD` when we want to access the value again later. This is because typing `FOOD` at the command prompt will attempt to call the command `FOOD`. It will return an error, because there's no executable command named `FOOD` in `PATH`.

Another difference between Python variables and Bash variables is that when we type `$FOOD` at the command prompt, it will resolve to the value of the variable, or `Shrimp gumbo`. By default, Bash will try to turn this value into a command named `Shrimp`, and then call it. Because there's no executable named `Shrimp` in `PATH`, this will generate an error.

If we want to see the value of a variable named `FOOD`, we'll need to type `echo $FOOD`. This will become `echo "Shrimp gumbo"`, which will print `Shrimp gumbo` to `stdout`.

### Instructions

- Type `echo $FOOD` to print the value of the `FOOD` variable.

```
echo $FOOD
```

## 3: Setting Environment Variables

So far, we've been creating shell variables. We can only access these variables within the Bash shell.

Another type of variable is an [environment variable](https://help.ubuntu.com/community/EnvironmentVariables). We can access these through any program we run from the shell.

We can create environment variables using the `export` command. For example, `export FOOD="Chicken and waffles"` will create an environment variable called `FOOD`.

### Instructions

- Type `export FOOD="Chicken and waffles"` to create an environment variable called `FOOD`.

```
export FOOD="Chicken and waffles"
```

## 4: Accessing Environment Variables

We can run many programs from Bash, including Python. To run the Python interpreter from the Bash shell, we type `python` at the command prompt.

Once we're inside the command prompt, we can access the environment variables with commands that look like this:

```
import os
print(os.environ["FOOD"])
```

First, we imported the `os` package, which is built into the Python standard library. It contains many useful functions for working with the operating system.

Then, we used `os.environ`, a dictionary containing all of the values for the environment variables. We can access any environment variable by specifying it as a key, just like we can with any Python dictionary.

This should give you a feel for the power of environment variables -- we can use them to set configuration in Python scripts and other places. This functionality is useful when configuration is secret (like with access keys), or changing rapidly.

### Instructions

Type `python` to fire up the Python interpreter.

- You can run Python commands in the interpreter by typing them in and pressing the Enter key at the end of each line.
- Run the following code in the interpreter:

```
import os
print(os.environ["FOOD"])

```

- When you're done, type `exit()` to exit the interpreter.

Finally, type `echo $FOOD` to verify that the value of the `FOOD`variable is the same in Python and the shell.

## 5: Calling Programs

On the last screen, we accessed Python by typing `python` in the shell. We can run many programs this way. There's nothing special about a program -- it's just a file somewhere on the system.

We can access any program by typing its full path. The full path for Python, which itself is a program, is `/usr/bin/python`.

### Instructions

- Type `/usr/bin/python` to get to the Python interpreter.
- Type `exit()` to leave the interpreter.

## 6: The PATH Variable

On the last screen, we typed `/usr/bin/python` to access the Python interpreter. If the Python interpreter is at that location, though, how come we can also access it by typing `python`?

We can do this because of the `PATH`environment variable, which is configured to point to several folders (creating a "shortcut"). We can run any program in any one of these folders just by typing the program's name. Because `/usr/bin` is one of the folders in `PATH` and `python` is in that folder, we can access the python interpreter just by typing `python`, instead of the full path.

### Instructions

- Type `echo $PATH` to see what folders are in `PATH`.

## 7: Flags

Some of the programs we've been running have arguments, and some don't. When we type `echo $FOOD`, we're passing in the value of the `$FOOD` variable as a positional argument to the `echo` program. This is similar to a function in Python, which has positional and keyword arguments. Programs can have any number of positional arguments, including zero.`python` is an example of a program that doesn't require any positional arguments.

The copy command (`cp`) is an example of a command with two positional arguments -- we need to pass in the file name, as well as the path we want to copy it to.

Programs can also have optional flags. These are like keyword arguments in Python, which modify program behavior. For example, if we pass the `-l` flag ( for "long mode") to `ls` (the "list" command), the command will list the files in the directory in long mode, meaning that it will show more information about them.

### Instructions

- Type `ls -l` to see how `ls` works when we add the `-l`flag.

## 8: Combining Flags

We'll often want to specify multiple flags. Most flags have short, single-character names, as well as longer versions of those names. See the [ls manual page](http://man7.org/linux/man-pages/man1/ls.1.html) for a closer look at this.

For example, `ls -a`, and `ls --all` do the same thing -- they'll both list all of the files in a directory, rather than hiding files that begin with a dot (`.`). The commands are equivalent.

When we have multiple flags with short, single-character names, we can chain them together to save time. `ls -la` will list all of the files in long format; it's equivalent to `ls -a -l`. The order of the `l` and the `a` don't matter. While experienced programmers do this all the time, it can be a bit confusing to parse at first.

### Instructions

- Use `ls -al` to list all of the files in long format.

## 9: Long Flags

We can specify longer flags with two dashes. One such longer flag for `ls` is `--ignore`. Using `ls --ignore=test.txt`won't include any files named `test.txt`in the output of `ls`.

### Instructions

- Use `ls -al` with the `--ignore` flag to ignore any files named `.ipython`.

```
ls -al --ignore=.ipython
```

# Command Line Python Scripting

## 1: Introduction To Command Line Python

We looked at the command line Python interpreter in the last mission. The interpreter lets us run Python commands and see their results immediately. It's very useful for testing snippets of code quicky, as well as debugging. But it's not a good way to develop Python programs, because the commands aren't saved anywhere.

In order to develop Python programs, we'll need to make files containing Python code. Then we'll be able to use the interpreter to run them from the command line. This way, we can save all of our commands, but still see what's happening.

This is a very common way to develop with Python -- use an IDE or text editor to create Python files, then run them from the command line.

We can make a file that Python can execute on the command line by adding some lines of Python code to a blank file. Here's an example of Python code:

```
if __name__ == "__main__":
    print("Welcome to a Python script")
```

The code above will print `Welcome to a Python script` when we run it from the command line. To run it, we just need to put those lines into a file, save the file as `file.py`, and then call it with `python file.py`.

This code works because the `__name__`variable in Python scripts is automatically set to the name of the module. If the module is being run from the command line, the `__name__` variable will be `__main__` by default. **Checking the `__name__` variable allows us to tell whether a script is running from the command line or not.**

### Instructions

Create a file called `script.py` in the `/home/dq` folder that contains the following code:

```
if __name__ == "__main__":
    print("Welcome to a Python script")

```

Finally, save the file and run it using `python script.py`.

```
~$ echo -e 'if __name__ == "__main__":\n    
print("Welcome to a Python script")' > script.py
~$ python script.py
```

## 2: Using Different Python Versions

There are actually two versions of Python on this machine. We ran the last script using the default `python` executable, which is Python version `2`. We'll want to use Python `3` instead, which we can access with the `python3` executable.

### Instructions

- Type `python3 script.py` to run the file `script.py` using Python 3.

```
python3 script.py
```

## 3: Installing Packages That Extend Python

Packages are an important way to extend Python's functionality. We've worked with packages like `matplotlib` and `pandas`. The best way to install packages is to use the command line and a program called **pip**. The newest versions of Python include pip by default, so installing Python will automatically give you access to pip.

In order to install a package with pip, we just use `pip install`. `pip install requests` will install the *requests* package, which developers use to interact with websites and APIs.

### Instructions

Type `pip install requests` to install the requests package.

- The `requests` library is already installed, so you'll see a message indicating that.

```
pip install requests
```

## 4: Overview Of Virtual Environments

On the previous screen, we used the default version of pip to install `requests`for the `python` executable, which is Python version 2.

What if we had wanted to install `requests` for Python 3 instead? Different projects can require different packages and Python versions. This type of version switching can become confusing.

For this reason, a computer system has one `python` executable, and we have to install all packages and libraries globally. This means that every single project on a machine has to use the same version of Python, and the same version of every package.

By default, we can't use different versions of Python without some hacks. One such hack is renaming `python` to `python3` so we can have access to both Python `2` and Python `3`.

A better solution is for each project we write to have its own version of Python, along with its own packages. This way, we don't need to worry that upgrading the version of a package will affect other projects on the system and cause them to stop working.

**Virtual environments**, or virtualenvs, let us do this. We can create a new virtualenv with the `virtualenv` command. While we normally have to install the virtualenv package first in order to access this command, we've already installed it for you to simplify the process.

Typing `virtualenv main` will create a virtualenv named `main`. It will create a folder in the current directory called `main`that will hold all of the packages we install into the virtual environment.

### Instructions

Type `virtualenv python2` in the `/home/dq` directory to create a new virtual environmented named `python2`.

- Note how it makes a folder called `python2`.

```
virtualenv python2
```

## 5: Creating A Python 3 Virtualenv

By default, virtualenv will use the `python`executable when it makes a new virtualenv, which means that it has the same version of Python as the system. In this case, we want to use `python3` for our virtualenv instead. In order to do this, we pass the `-p` flag to the `virtualenv`command, which will allow us to change the Python interpreter that virtualenv uses.

In this case, we can type `virtualenv -p /usr/bin/python3 python3` to use Python 3 instead of Python 2.

### Instructions

- Create a virtualenv called `python3` in the `/home/dq` folder that uses Python 3.

```
virtualenv -p /usr/bin/python3 python3
```

## 6: Activating A Virtualenv

Once we've created a virtualenv, we can activate it using `source python3/bin/activate` (this assumes that the virtualenv is called `python3`, and that the folder for the virtualenv is in our current directory).

Once we activate a virtualenv, the Python version and packages installed in it will become the default Python version and packages that run when we type `python`.

### Instructions

- Activate the `python3` virtualenv.

```
source python3/bin/activate
```

## 7: Verifying The Installed Packages

We can find out which version of Python we're using with `python -V`. We can also look up which packages are currently installed (along with their versions) with `pip freeze`. If we activate a virtualenv, all of the packages, including `pip`, will be from the virtualenv instead of the main system Python executable.

### Instructions

- Type `python -V` to verify that Python 3 is the current Python version after activating the virtualenv.
- Type `pip freeze` to check which packages are installed, and their versions.

## 8: Importing Saved Functions Into A File

One of the great things about Python is that we can import functions from a package into a file. We can also import functions and classes from one file into another file. This gives us a powerful way to structure larger projects without having to put everything into one file.

We'll experiment with this style of import by writing a function in a file, and then importing it into another file.

If there's a file named `utils.py`, we can import it into another file in the same directory using `import utils`. All of the functions and classes defined in `utils.py`will then be available using dot notation. If there's a function called `keep_time()` in `utils.py`, we can access it with `utils.keep_time()` after importing it.

### Instructions

Create a file called `utils.py` that contains the following code:

```
def print_message():
    print("Hello from another file!")

```

Modify the original `script.py` file to contain this code instead:

```
import utils

if __name__ == "__main__":
    utils.print_message()

```

Both `script.py` and `utils.py` should be in the same folder.

Finally, run `python script.py` to print out the message.

```
~$ echo -e 'def print_message():\n    print("Hello from another file!")' > utils.py
~$ echo -e 'import utils\n\nif __name__ == "__main__":\n    utils.print_message()' > script.py
~$ python script.py
```

## 9: Accessing Command Line Arguments

We can also pass command line options into Python scripts. We can retrieve them from inside the script through the `sys`package.

Once we import the `sys` package, the `argv` list will allow us to retrieve the positional arguments passed into the script. We learned about positional arguments in the last mission -- they're the arguments that come after the command name. `python script.py 82` is one example. The first positional argument is `script.py`, and the second is `82`.

The following code will read input from the command line and print it back out. If the code is in a file named `script.py`, we'd call `python script.py "Hello from the command line"` to pass in the text we want to display.

```
import sys
```

```

```

```
if __name__ == "__main__":
```

```
    print(sys.argv[1])
```

Notice that we printed the second item in the `argv` list (`sys.argv[1]`). This is because the arguments come after the `python` command, so the first argument is the name of the file we want to run. The second argument is the actual text that we want to print.

### Instructions

- Modify `script.py` to accept and print a command line argument.
- Then, call the script and pass in `"Hello from the command line"`.

```
~$ echo -e 'import sys\n\nif __name__ == "__main__":\n    print(sys.argv[1])' > script.py
~$ python script.py "Hello from the command line"
```

## 10: Deactivating A Virtualenv

To switch a virtualenv off so we can move to a different project, we deactivate it with the `deactivate` command. This command will automatically shut down the current virtualenv, so we don't need to pass in its name.

### Instructions

- Deactivate the virtualenv.

```
deactivate
```

# Challenge: Working with the Command Line

## 1: Introduction

Over the past few missions, you learned how to navigate the filesystem, create and modify files, and work with Python on the command line. You discovered it isn't enough just to know how to write code in Python -- we also have to know how to modify and execute Python programs on a computer.

In this challenge, you'll pull all of the concepts from the past few missions together to create and run a Python script on the command line. Here's a rough outline of the steps in this exercise:

- Create a Python script.
- Create a virtual environment.
- Change file permissions.
- Execute a Python script from the command line.

### Instructions

- Familiarize yourself with the current folder.
  - Change out of the current directory, then back in.
  - Print the path of the current folder to the standard output.

```
~$ cd ..
~$ cd ~
~$ pwd
```

## 2: Create A Script

Now you'll be able to create a Python script in the current folder.

### Instructions

- Create a Python script that takes in a command line argument and prints it out.
  - Use output redirection or a command line editor to create a Python script called `script.py` in the `/home/dq/` folder.
  - Add code to the file that will read the first command line argument we pass in and print it out.

```
echo -e 'import sys\n\nif __name__ == "__main__":\n    print(sys.argv[1])' > script.py
```

## 3: Change File Permissions

You don't want just anyone to be able to use your script. By editing permissions, you can make it so that you're the only person who can run it.

### Instructions

- Edit the file permissions for `script.py` so that only the current user can access it.Assign read, write, and execute permissions to the current user.Don't assign any permissions to your group, or to other users.

```
chmod 0700 script.py
```

## 4: Create A Virtual Environment

You'll want to isolate the Python environment your script is in by creating a virtual environment. This will allow you to install appropriate Python packages that will work with the script.

### Instructions

- Create a Python 3 virtualenv called `script`.
- Activate the `script` virtualenv.

```
~$ virtualenv -p /usr/bin/python3 script
~$ source script/bin/activate
```

## 5: Move The Script

Moving the script to its own folder will keep your home directory clean. If you end up creating more scripts, it will also help you tell your programs apart more easily.

### Instructions

- Create a folder called `printer` in the current user's home directory (`/home/dq/`).
- Move `script.py` into the `printer` folder.

```
mkdir printer
mv script.py printer
```

## 6: Execute The Script

Now you're ready to execute the Python script.

### Instructions

- Change the current directory to the `printer` directory.
- Execute `script.py`, and pass in the string `I'm so good at challenges!`.

```
cd printer
python script.py "I'm so good at challenges!"
```

# Guided Project: Working With Data Downloads

## 1: Introduction

Almost all of the data you work with as a data scientist will come from a remote source, such as another website on the Internet. File downloads sometimes come in analysis-ready formats like CSV. At other times, the data will be in an archive format like TAR or ZIP. These formats compress files to reduce overall size, which makes them faster to download. Archive formats can also bundle multiple data files into a single archive file.

In this guided project, we'll be working with an education data set in ZIP format. We'll learn how to extract the files inside the ZIP download and then work with them.

You may have noticed that the interface for this guided project is a bit different. There's a terminal at the bottom, where you'll be running commands. There's also an editor at the top, where you can edit files like Python scripts. The file browser on the left allows you to view folders and open files for editing.

![Interface](https://dq-content.s3.amazonaws.com/220_dq_interface.png)

The data set we'll work with is called the Civil Rights Data Collection. It contains information on educational achievement and opportunities in the U.S., broken down by race and school. For example, it records the racial composition of the students enrolled in advanced classes at each school. Each row represents a school, while each column records an indicator of academic achievement.

For the purposes of this exercise, we'll be using a subset of the data that only contains `1` out of every `100` rows in the original data set. If you'd like to work with the original version, you can download it from [data.gov](https://catalog.data.gov/dataset/civil-rights-data-collection-2013-14).

Here are the first few rows:

|      | LEA_STATE | LEA_NAME            | SCH_NAME                      | COMBOKEY     | LEAID   | SCHID | JJ   | CCD_LATCOD | CCD_LONCOD | NCES_SCHOOL_ID | ...  | SCH_FTE_TEACH_WOFED | SCH_SAL_TEACH_WOFED | SCH_NPE_WOFED | DSO_SCH_FTE_TEACH_WOFED | DSO_SCH_NPE_WOFED | DSO_SCH_SAL_INSTR_WOFED | DSO_SCH_SAL_TEACH_WOFED | SCH_JJTYPE | SCH_JJSYDAYS | SCH_JJHOURS |
| ---- | --------- | ------------------- | ----------------------------- | ------------ | ------- | ----- | ---- | ---------- | ---------- | -------------- | ---- | ------------------- | ------------------- | ------------- | ----------------------- | ----------------- | ----------------------- | ----------------------- | ---------- | ------------ | ----------- |
| 0    | TX        | DENTON ISD          | RYAN EL                       | 481674008530 | 4816740 | 8530  | NO   | 33.1604    | -97.1354   | 4.816740e+11   | ...  | 39.00               | 2274986.15          | 316158.74     | 0                       | 0                 | 0                       | 0                       | -9         | -9           | -9          |
| 1    | LA        | LAFAYETTE PARISH    | JUDICE MIDDLE SCHOOL          | 220087000675 | 2200870 | 675   | NO   | 30.1673    | -92.1403   | 2.200870e+11   | ...  | 32.95               | 1585446.66          | 32769.37      | 0                       | 0                 | 0                       | 0                       | -9         | -9           | -9          |
| 2    | GA        | HENRY COUNTY        | EAGLE'S LANDING MIDDLE SCHOOL | 130282000399 | 1302820 | 399   | NO   | 33.4885    | -84.2086   | 1.302820e+11   | ...  | 48.18               | 2294195.77          | 0.00          | 0                       | 0                 | 0                       | 0                       | -9         | -9           | -9          |
| 3    | FL        | MONROE              | KEYS CENTER                   | 120132007521 | 1201320 | 7521  | NO   | 24.5638    | -81.7980   | 1.201320e+11   | ...  | 1.00                | 61369.15            | 34275.00      | 0                       | 0                 | 0                       | 0                       | -9         | -9           | -9          |
| 4    | OH        | CLARK-SHAWNEE LOCAL | POSSUM ELEMENTARY SCHOOL      | 390462802499 | 3904628 | 2499  | NO   | 39.8864    | -83.8388   | 3.904628e+11   | ...  | 14.02               | 840251.25           | 616504.50     | 0                       | 0                 | 0                       | 0                       | -9         | -9           | -9          |

5 rows × 1929 columns

Before we can load and analyze the data, we'll need to extract the files that contain it from the archive file, `crdc201314csv.zip`. We can call the [unzip](https://linux.die.net/man/1/unzip) command on an archive file to extract the files within it. Here's an example:

```

```

```
unzip test.zip
```

This command will extract all of the files from the archive `test.zip` into the current directory. Once we've extracted the files inside an archive file, it's good practice to delete the original archive to save space.

### Instructions

- List the contents of the current directory with the `ls`command, and take note of the archive file `crdc201314csv.zip`.
- Extract the files in `crdc201314csv.zip` using the `unzip`command.
- List the contents of the current directory, and make sure there are `4` new data files.
- Delete `crdc201314csv.zip`.

## 2: Running A Python Script To Explore The Columns

Because the data set has more than `2000`columns, there's a separate file that explains what each column means. The explanatory file (or "data dictionary") is `CRDC2013_14content.csv`, while the data file itself is `CRDC2013_14.csv`.

We'll need to create a Python script, load `CRDC2013_14content.csv` using pandas, and then decide on a few columns to explore in greater depth.

Recall that in order to run a script from the command line, we'll need to create a few lines of code and save them as a file. Let's say the following lines are already in a file called `read.py`:

```

```

```
if __name__ == "__main__":
```

```
    print("Program executed successfully!")
```

We could then run that script by typing `python read.py` on the command line.

We'll have to run our Python script using a virtual environment that has a pandas installation on it. The virtual environment we need to activate is in the `/dataquest/system/env/python3` folder. As you may recall from a previous mission, we'll need to run `source /dataquest/system/env/python3/bin/activate` to activate the virtual environment.

### Instructions

- Activate the `python3` virtual environment.
- Run `pip freeze` to verify that pandas is installed and available.
- Edit `read.py` so that it will run from the command line.
- Add code to `read.py` to:Import pandas.Read `CRDC2013_14content.csv` into a pandas dataframe called `contents`.Print the first few rows of `contents`.
- Run `read.py` from the terminal, and verify that it worked properly.
- Continue exploring the `contents` dataframe to find any column names that interest you.

## 3: Using Pandas To Find Patterns In The Data

Now that we've looked at the column names more closely, here are some potentially interesting ones that popped out:

- `JJ` - Indicates whether the school is part of a [juvenile justice facility](https://en.wikipedia.org/wiki/Youth_detention_center), or youth prison.
- `SCH_STATUS_MAGNET` - Indicates whether the school is a [magnet school](https://en.wikipedia.org/wiki/Magnet_school), an advanced school for students who achieve high scores on certain tests.

We can dig around for interesting patterns here by using [Series.value_counts()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.value_counts.html) to find unique values in each column. This will tell us how many schools are juvenile justice facilities or magnet schools.

We can also count how many students are in juvenile justice facilities by using the [pandas.pivot_table()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.pivot_table.html) function to create a pivot table. Building a pivot table will allow us to aggregate `TOT_ENR_M` and `TOT_ENR_F` (which record school enrollment by gender) by `JJ` and `SCH_STATUS_MAGNET`. This will count up how many students are in magnet schools or juvenile justice facilities.

The Python code below, for example, will create a pivot table that counts the total number of male and female students in juvenile justice facilities:

```

```

```
import pandas as pd
```

```

```

```
pd.pivot_table(data, values=["TOT_ENR_M", "TOT_ENR_F"], index="JJ", aggfunc="sum")
```

### Instructions

- Create a new file called `exploration.py` that you can run from the command line.
- In `exploration.py`:Read `CRDC2013_14.csv` into a pandas dataframe called `data`.Be sure to specify the keyword argument `encoding="Latin-1"` so that pandas reads the file in properly.Call the `value_counts` method on the `JJ` and `SCH_STATUS_MAGNET` columns to count the number of schools that fall within each category.Print out the results so you see them when you run the script.
- Execute `exploration.py`.
- In `exploration.py`:Construct two pivot tables that aggregate `TOT_ENR_M`and `TOT_ENR_F` based on `JJ` and `SCH_STATUS_MAGNET`.Print out the results so you see them when you run the script.
- Execute `exploration.py`.
- Create a text file called `findings.txt` that summarizes any interesting patterns you observe.

## 4: Using Pandas To Explore Enrollment By Race

Several of the columns in our data break school attributes down by race and gender. The names of these columns begin with an abbreviation for the attribute name. `SCH_ENR` stands for "school enrollment," for example. Next, they include a code name for race, such as `HP`. The last part is a code name for gender, which is either`F` or `M`. For example, the complete name for the column that records hispanic female enrollment is `SCH_ENR_HI_F`.

Here are the code names for each race:

- `HI` - Hispanic
- `AM` - American Indian
- `AS` - Asian
- `HP` - Hawaiian or Pacific Islander
- `BL` - Black
- `WH` - White
- `TR` - Two or more races

Here are the gender code names:

- `F` - Female
- `M` - Male

The data set contains one column for every possible combination of racial and gender code names associated with an attribute -- that's why there are more than 2,000 columns!

Here's a list of all of the columns that indicate school enrollment, for example:

- `SCH_ENR_HI_M`
- `SCH_ENR_HI_F`
- `SCH_ENR_AM_M`
- `SCH_ENR_AM_F`
- `SCH_ENR_AS_M`
- `SCH_ENR_AS_F`
- `SCH_ENR_HP_M`
- `SCH_ENR_HP_F`
- `SCH_ENR_BL_M`
- `SCH_ENR_BL_F`
- `SCH_ENR_WH_M`
- `SCH_ENR_WH_F`
- `SCH_ENR_TR_M`
- `SCH_ENR_TR_F`

There are also two columns that indicate total enrollment by gender:

- `TOT_ENR_M` - Total male enrollment
- `TOT_ENR_F` - Total female enrollment

Several other column names combine race and gender codes, including:

- `SCH_HBREPORTED_DIS` - Students who report being harrased or bullied
- `SCH_DISCWODIS_EXPWOE` - Students without disabilities who were expelled from school
- `SCH_RET_G12` - Students who started and completed grade 12

### Instructions

- Create a new file called `enrollment.py` that will run from the command line.
- In `enrollment.py`:Read in the data file using pandas.Create a column called `total_enrollment` that adds the `TOT_ENR_M` and `TOT_ENR_F` columns.Compute the sums of all of the columns that break down enrollment by race and gender.Compute the sum of the `total_enrollment` column, and assign the result to the variable `all_enrollment`.Divide the sums of the columns by `all_enrollment`to determine the percentage of enrollment that each race and gender makes up.Print out the results.
- Run `enrollment.py`.
- Compare the results to the overall population of the United States, broken down by race and gender. You can find the data on race at [Wikipedia's U.S. race demography page](https://en.wikipedia.org/wiki/Demography_of_the_United_States#Race_and_ethnicity), and the data on gender at [Wikipedia's U.S. sex ratios page](https://en.wikipedia.org/wiki/Demography_of_the_United_States#Sex_ratios).To make the analysis simpler, you can assume that the gender ratio in the U.S. is 1:1.
- Add any interesting patterns you've found to `findings.txt`.

## 5: Moving The Data Files To A Separate Folder

When we're working with data files, it's common to put them in a folder named `data`. This makes it easy to separate our scripts and findings from the source data. In cases where the source data is several hundred megabytes, it also makes it easier to share our code and findings only.

In order to do this, we'll need to create a folder, then move the data files into it. Finally, we'll need to rewrite our Python scripts so that they load the data files in that folder.

### Instructions

- Create a folder called `data` inside the current folder.
- Move the data files to the `data` folder. These include:`CRDC_documentation_csv.txt``CRDC_usage_agreement.txt``CRDC2013_14.csv``CRDC2013_14content.csv`
- Edit `read.py`, `exploration.py`, and `enrollment.py` so that they access the files inside the `data` folder.

## 6: Next Steps

Now that you've read in the files and found some interesting columns, you can dig in and analyze the data more. There are quite a few interesting angles you could explore:

- Review expulsions (which refers to when students are kicked out of school permanently). Columns like `SCH_DISCWODIS_EXPWOE_HI_M` and `TOT_DISCWODIS_EXPZT_F` contain information on expulsions.
- Explore gender and race differences in SAT scores. Columns like `SCH_SATACT_HI_M` contain this information.
- Figure out the racial and gender breakdowns for different types of schools, such as magnet schools.
- Determine how many students are in gifted and talented programs, or advanced placement classes.
- Investigate how racial differences in enrollment change from preschool to high school.
- Explore school bullying. The `SCH_HBDISCIPLINED_DIS_HI_M` column contains some of this information.

We recommend that you download these files and work on your own machine. We also recommend that you download the full data set from [data.gov](https://catalog.data.gov/dataset/civil-rights-data-collection-2013-14). If you find anything interesting while exploring the data, please let us know!