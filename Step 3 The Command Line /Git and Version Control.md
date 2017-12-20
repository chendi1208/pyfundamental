# Introduction to Git

## 1: Introduction To Version Control Systems

When you're working on a team, everyone will generally be making changes to the same files. Imagine you're working on a project to write a Python script, and have a folder with the following two files:

```
script.py
README.md
```

Here are the contents of `script.py`:

```
if __name__ == "__main__":
    print("Welcome to a script!")
```

Imagine that you and a coworker are both working on the project at the same time. You modify `script.py` like this:

```
if __name__ == "__main__":
    print("Welcome to a script!")
    print("Here's my amazing contribution to this project!")
```

And your coworker does this:

```
import math
print(10 + 10)
if __name__ == "__main__":
    print("Welcome to a script!")
```

Imagine that you both have the folder on your local machine. To modify files, you make changes, then upload the entire folder to a centralized location like Dropbox or Google Drive to enable collaboration. If you didn't have a distributed version control system, whoever changed the file last would overwrite the changes of the other person.

This approach becomes extremely frustrating and impossible to manage as you start dealing with larger and larger chunks of code. What if the folder had 100 files, you modified 10 of them, and your coworker modified 30 at the same time? You don't want to lose your changes every time your coworker uploads his version of the folder. Now, imagine that instead of just you and a coworker, it's a project with 10 or 100 contributors.

Companies face this problem every day, which is why distributed **version control** systems exist. These systems will "merge" changes together intelligently, enabling multiple developers to work on a project at the same time.

Going back to the `script.py` file, if we intelligently merged the two versions, the end result would look like this:

```
import math
print(10 + 10)
if __name__ == "__main__":
    print("Welcome to a script!")
    print("Here's my amazing contribution to this project!")
```

There are a few distributed version control systems, including [Mercurial](https://www.mercurial-scm.org/) and [Subversion](https://subversion.apache.org/). [Git](https://git-scm.com/) is by far the most popular, however.

Git is a command-line tool we can access by typing `git` in the shell. The first step in using Git is to initialize a folder as a [repository](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository). A repository (or "repo") tracks multiple versions of the files in the folder, enabling collaboration.

We can initialize a repository by typing `git init` inside the folder we want to use for our project.

### Instructions

- Create a folder named `random_numbers`.
- Navigate into this folder and initialize a Git repository.

```
~$ mkdir random_numbers
~$ cd random_numbers
~$ git init
```

## 2: The .Git Folder

Initializing a Git repository will create a folder called `.git` inside the repository folder. That means there should now be a folder named `.git` inside of our `random_numbers` folder. Files and folders with a period prefix (`.`) are typically private, and don't show up by default when we list the files in a folder.

Let's verify that the file is there with `ls -al`. As you may recall, the `-a` flag will show everything in a folder, even if it starts with a period.

### Instructions

- Run `ls -al` to check the contents of the `random_numbers`folder.

```
ls -al
```

## 3: Creating Files In The Repository

The typical Git workflow involves adding files, making changes, and then storing a checkpoint (or "snapshot") of those changes. These checkpoints are called `commits`.

Instead of storing every file in every commit, Git stores the `diff`, or the things that change between commits.

For example, imagine that we created a file called `README.md` with this content:

```

```

```
Welcome to my readme!
```

When we commit it, Git would store the file. Let's say we added another line to the file later on and made another commit:

```

```

```
Welcome to my readme!
```

```
Here's another line.
```

Git would only store the part of the file that changed since the last commit, which is `Here's another line.`.

Every project is a sequence of commits. Commits give us a powerful way to merge the changes of multiple team members together. We can even restore the repository to an earlier checkpoint, or moment in time.

Before we make a commit, let's add some files to our folder.

### Instructions

Create a file named `README.md` with the following content:

```
Random number generator

```

Create a file named `script.py` with this content:

```
if __name__ == "__main__":
    print("10")
```

```
~$ echo -e "Random number generator" > README.md
~$ echo -e 'if __name__ == "__main__":\n    print("10")' > script.py
```

## 4: Checking File Status

Files can have one of three states in Git:

- `committed` - The current version of the file has been added to a commit, and Git has stored it.
- `staged` - The file has been marked for inclusion in the next commit, but hasn't been committed yet (and Git hasn't stored it yet). You might stage one file before working on a second file, for example, then commit both files at the same time when you're done.
- `modified` - The file has been modified since the last commit, but isn't staged yet.

After we make changes to a Git repository, we can run the `git status` command to check the state of each file within it. Any files that don't show up in `git status`are in the committed state (i.e., don't have unsaved changes).

Git will automatically show us which files have been modified since the last commit. If we're ready to commit the files we've modified, we can add them to the staging area using `git add`. Typing `git add script.py` will add `script.py` to the staging area, where it will be staged for the next commit.

### Instructions

- Check the status of the repo.
- Add `script.py` to the staging area.
- Add `README.md` to the staging area.

```
~$ git status
~$ git add script.py
~$ git add README.md
```

## 5: Configuring Identity In Git

Before we can make our first commit, we need to tell Git who we are so it can store that information along with the commit. This step ensures that all of the members on a team can tell who made a certain commit.

We can do this by running `git config`. We only need to run this command once per computer, because Git will save the information.

Git needs two pieces of information about you -- your email address and your name. You can configure your email with:

```

```

```
git config --global user.email "your.email@domain.com"
```

You can configure your name with:

```

```

```
git config --global user.name "Your name"
```

### Instructions

- Configure Git with your email address and name.

```
~$ git config --global user.email "user@dataquest.io"
~$ git config --global user.name "Dataquest User"
```

## 6: Committing Changes

Now that we've staged some files, we can make our first commit. A commit stores a snapshot of the files in the repository at a certain point in time. By building a history of these snapshots, we can rewind to an earlier point in time, or merge someone else's changes with our own.

To make a commit, we use `git commit -m "Commit message here"`. The `-m` flag indicates that we're adding a message, and the text in quotes that comes after it is the commit message itself. It's customary to make the commit message something informative, so if we do have to rewind or merge code, it's obvious what changes we made and when.

### Instructions

- Type `git commit -m "Initial commit. Added script.py and README.md"` to make the first commit to the repository with an informative message.

```
~$ git commit -m "Initial commit.  Added script.py and README.md"
```

## 7: Viewing File Differences

Let's modify our files and make another commit to see how the process works. Before we place a file in the staging area, we can use `git diff` to see all of the line differences between the current and previous version. We can scroll up and down with the arrow keys, and exit `git diff` with the `q` key. If we want to see the differences after we stage a file, we can use `git diff --staged`.

### Instructions

`script.py` isn't exactly a random number generator right now.

1. Modify it so that it prints a random integer from `0` to `10`. You can import and use [`random.randint`](https://docs.python.org/3/library/random.html#random.randint) for this.
2. Afterwards, type `git diff` to see how Git is tracking modifications.
3. Finally, type `git status` to see the status of the file you modified.

```
~$ echo -e 'import random\nif __name__ == "__main__":\n    print(random.randint(0,10))' > script.py
~$ git status
```

## 8: Making A Second Commit

Now that we've modified a file, we can add the changes to the staging area using `git add script.py`, and then commit them using `git commit`.

### Instructions

- Add the `script.py` file to the staging area, then make a new commit with an informative message.

```
~$ git add script.py
~$ git commit -m "Made the numbers random"
```

## 9: Reviewing The Commit History

We can pull up a repository's commit history using the `git log` command. This command will show us a list of all of the commits to the repository, in descending order by creation date. If the output is very long, it will allow us to scroll. We can scroll through the log with the up and down arrows, and use the `q` key to exit.

### Instructions

- Run `git log` to explore the commit history of the repository.

## 10: Viewing Commit Differences

We can use `git log --stat` to see more details about the commits in the `git log`output.

### Instructions

- Run `git log --stat` to explore the commit history of the repository.
- Press `q` to exit the screen once you've finished exploring.

# Git Remotes

## 1: Introduction To Remote Repositories

One of the most useful ways to use Git is in conjunction with [GitHub](http://www.github.com/), a website built on Git, but with a familiar GUI interface. Using Git with GitHub allows us to push our code to remote repositories. This enables us to:

- Share our code with others and build a portfolio.
- Collaborate with others on a project and build code together.
- Download and use code others have created.

[Here's](https://github.com/VikParuchuri/evolve-music2) an example of a remote repository on GitHub. People can view your public repositories on your [GitHub profile](https://github.com/VikParuchuri). Using GitHub is a great way to build a portfolio and get recruiters to notice you.

Remote repositories aren't just useful for building a portfolio. Pushing to GitHub also allows us to collaborate with others on code. For example, thousands of different [contributors](https://github.com/torvalds/linux/graphs/contributors) are developing [Linux](https://github.com/torvalds/linux) on GitHub. Many companies, including [Google](https://github.com/google) and [Facebook](https://github.com/facebook), also use GitHub to work on code projects across teams.

Remote repositories also enable us to access and use code we didn't write. For instance, [this repo](https://github.com/amznlabs/amazon-dsstne) will let us download Amazon's Deep Learning tools and start training models. Because the reposistory is public, anyone can download and use it. Repositories on GitHub can also be private, in which case they're hidden, and not accessible to others.

To download a remote repository to your own computer, you'll need to *clone* it.*cloning* copies a repository from one location (in this case, a remote one) to a folder on your computer. The repository retains all of its Git history, and you can work with it just like you would with a Git repository you created yourself.

We use the [git clone](https://git-scm.com/docs/git-clone) command to clone a remote repository. If we were cloning a repository we found on GitHub, we'd specify the GitHub URL for that repository.

Here's how we'd typically clone the [Amazon Deep Learning repo](https://github.com/amznlabs/amazon-dsstne) from GitHub:

```

```

```
git clone https://github.com/amznlabs/amazon-dsstne.git
```

`https://github.com/amznlabs/amazon-dsstne.git` is the URL for the Git repository we're cloning. Our clone command will automatically create a folder named `amazon-dsstne` in our current folder, and place the repository there.

However, because we're working with a simplified remote repository for the purposes of this mission, we'll clone it a bit differently:

```

```

```
git clone /dataquest/user/git/chatbot
```

This will clone the repository from `/dataquest/user/git/chatbot`, a path on our local computer, to our current folder, and place it in a subfolder named `chatbot`.

If we specify a second argument to `git clone`, we can change the folder the repository saves to:

```

```

```
git clone /dataquest/user/git/chatbot silentbot
```

This command will place the `chatbot`repository in a folder called `silentbot`.

### Instructions

- Clone the `/dataquest/user/git/chatbot` repository into the home folder of the current user. This should create a folder called `chatbot` in `/home/dq`.

```
~$ cd ~
~$ git clone /dataquest/user/git/chatbot
```

## 2: Making Changes To Cloned Repositories

Now that we've cloned a repository, we can makes changes to it, just like we did in the last mission. We'll be able to edit files, add them to the staging area, and then commit the changes. The local version of the repo will then reflect the changes, but the remote version won't.

Review the following diagram carefully. It illustrates the relationship between the local repo and the remote repo, and how they're separate:

RemoteLocalREADME.mdgitcloneREADME.mdThisisaThisisaREADME.README.gitcommitREADME.mdThisisthebestREADME!

After making the commit in the diagram, the local repo will have one more commit than the remote repo, and the file `README.md` will be different.

Most GitHub projects include a `README.md`file, which helps people understand what the project is about and how to install it. It's common to write the `README` file in [Markdown](https://daringfireball.net/projects/markdown/syntax) format, which allows us to create lists and other complex but useful structures in plain text. The Markdown format has an `.md` file extension.

Similar to the diagram above, we'll edit the `README.md` file to add a line, then commit it to the repository. It's important to add informative messages when comitting to shared repositories, so that other people can figure out what each commit is doing without having to read through the code. This is very important when debugging code that multiple people are working on.

### Instructions

- `cd` into the `/home/dq/chatbot` folder to navigate to the `chatbot` repo.
- Add the line `This project needs no installation` to the bottom of `README.md`.
- Add your changes to the staging area using `git add`.
- Commit your changes using `git commit`, with the commit message `Updated README.md`.
- Run `git status` to see the status of the repo.

```
~$ cd /home/dq/chatbot
~$ printf "This project needs no installation" >> README.md
~$ git add .
~$ git commit -m "Updated README.md"
~$ git status
```

## 3: Overview Of The Master Branch

When you ran [git status](https://git-scm.com/docs/git-status) on the last screen, your output looked something like this:

```

```

```
On branch master                                                                
```

```
Your branch is ahead of 'origin/master' by 1 commit.                            
```

```
  (use "git push" to publish your local commits)
```

```

```

```
nothing to commit, working directory clean
```

The first two lines mention the terms `branch`, `master`, and `origin`, all of which may be unfamiliar. We'll look at `branch` and `master` on this screen, and `origin` on the next one.

Every Git repository consists of one or more branches. Each branch contains a slightly different version of the code. We'll discuss branches and how they work in greater detail in the next mission. For now, the important fact to know is that the main branch of a Git repo is typically called `master`. Developers create separate branches when they want to work on new features for a project, then add the commits in those branches back into `master` when the features are ready.

All of the changes we've made so far have been on the `master` branch of the `chatbot` repo. The `master` branch is usually the most up-to-date shared version of any code project.

We can check which branch we're on with the [git branch](https://git-scm.com/docs/git-branch) command. This command will list all of the branches in the repo. It will also highlight the currently active branch, and add an asterisk next to its name.

### Instructions

- Use `git branch` to see all of the branches that exist in the `chatbot` repo.

```
~$ cd /home/dq/chatbot
~$ git branch
```

## 4: Pushing Changes To The Remote

Once we've made changes to the local version of a repo, we can push those changes to the remote repo so that everyone can see them. Edits we make locally are only reflected in our local repo. Unless we push them to the remote, the remote repo doesn't change.

To do this, we'll need to use the [git push](https://git-scm.com/docs/git-push) command, which pushes commits from our local repo to the remote repo. Here's a diagram showing what happens when we run `git push`:

RemoteLocalREADME.mdgitcloneREADME.mdThisisaThisisaREADME.README.gitcommitREADME.mdgitpushThisisthebestREADME.mdREADME!ThisisthebestREADME!

As the diagram shows, until we push the branch to the remote repo, the changes are only in our local repo. Pushing to the remote will update the remote with our latest changes. Anyone else who pulls from the remote repo will then have access to the same two commits that we have in our local repo.

When we run `git push`, we need to specify both the name of the remote repo to push to, and the name of the branch we're pushing. When we clone a repo, Git automatically names the remote repo `origin`. This means that the following command will push the `master` branch to the remote repo:

```

```

```
git push origin master
```

It's possible, but rare, that a remote will have a name other than `origin`. If we're unsure, we can list the remote(s) associated with our local repo using [git remote](https://git-scm.com/docs/git-remote).

The `git remote` command will list all of the repo's remotes. If we specify the `-v`option, we'll get additional information about where the remote repos are located.

### Instructions

- Push the `master` branch of the local `chatbot` repo to the remote `origin`.

```
~$ cd /home/dq/chatbot
~$ git push origin master
```

## 5: Viewing Individual Commits

As you may recall from the previous mission, Git stores a repo's history as a series of commits. Each commit contains anything that changed since the previous commit. This allows Git to store history very efficiently, and replay that history to reconstruct the working directory -- the folder on your computer where you edit files, add the changes, and then make commits. Commits are separate from the working directory. They're essentially snapshots of all of the files in the working directory at specific points in time.

You can see the full commit history of the `master` branch of the local `chatbot` repo with [git log](https://git-scm.com/docs/git-log). Here's the output you might get from `git log`:

```

```

```
commit 6a95e94ea10caa28013b767510d4bc59369d83fa                                 
```

```
Author: Dataquest <me@dataquest.io>                                             
```

```
Date:   Wed May 18 21:56:27 2016 +0000                                          
```

```

```

```
    Updated README.md                                                           
```

```

```

```
commit 8a1ca35dd5c5de8f93aa6cbbd153caa40233386c                                 
```

```
Author: Dataquest <me@dataquest.io>                                             
```

```
Date:   Wed May 18 21:55:33 2016 +0000                                          
```

```

```

```
    Add the initial version of README.md    
```

```
</me@dataquest.io></me@dataquest.io>
```

This history shows two commits -- the first one with the message `Add the initial version of README.md`, and the second with the message `Updated README.md`. The great thing about Git is that it stores both commits, so we can quickly revert back to a previous commit if we want to.

To do this, we'd need to use the commit's hash, or unique identifier. Hashes allow us to perform operations like revert to a specific commit. We can find the hash for a commit in the output from `git log`. In the output we generated above, the first commit has the ID `8a1ca35dd5c5de8f93aa6cbbd153caa40233386c`, and the second commit has the ID `6a95e94ea10caa28013b767510d4bc59369d83fa`.

We can use the [git show](https://git-scm.com/docs/git-show) command with a hash to see what changed in a specific commit. For example, running `git show 6a95e94ea10caa28013b767510d4bc59369d83fa`would return:

```

```

```
commit 6a95e94ea10caa28013b767510d4bc59369d83fa                                 
```

```
Author: Dataquest <me@dataquest.io>                                             
```

```
Date:   Wed May 18 21:56:27 2016 +0000                                          
```

```

```

```
    Updated README.md                                                           
```

```

```

```
diff --git a/README.md b/README.md                                              
```

```
index f4871de..9c05964 100644                                                   
```

```
--- a/README.md                                                                 
```

```
+++ b/README.md                                                                 
```

```
@@ -1,3 +1,3 @@                                                                 
```

```
 README
```

```

```

```
-This is a README file.  It's typical for GitHub projects to have a README.  A README gives information about what the project is about, and usually how to install and use it.
```

```
\ No newline at end of file                                                     
```

```
+This is a README file.  It's typical for GitHub projects to have a README.  A README gives information about what the project is about, and usually how to install and use it.This project needs no installation!
```

```
</me@dataquest.io>
```

This output indicates that someone changed the `README.md` file in this commit, and added the line `This project needs no installation!`. `a/README.md` is the file state before the commit, and `b/README.md` is the file state after the commit.

`git show` will allow us to scroll up and down and side to side. We can exit by typing `q`.

### Instructions

- Use the `git log` command to find the commit hash corresponding to the most recent commit in the `chatbot`repo.
- Use the commit hash along with `git show` to see what the commit did.

```
~$ cd /home/dq/chatbot
~$ HASH=`git rev-parse HEAD`
~$ git show $HASH -q
```

## 6: Commits And The Working Directory

Before we explore commits further, let's take a closer look at the working directory and how it interacts with commits. As you may recall from the previous mission, the Git commit workflow has three main components:

- The working directory
- The staging area
- Commits

The working directory is the folder we're version controlling with Git, and the contents of the working directory are what we see when we list the contents of the folder with `ls`. In our case, `/home/dq/chatbot` is the working directory. We can edit the working directory by changing or adding files.

So let's say our working directory looks like this:

WorkingDirectoryStagingAreaCommitsREADME.mdThisisaREADME!

In this example, we have one file named `README.md` in the working directory. There are no files in the staging area, and no commits.

When we run `git add`, Git adds the difference between the most recent commit and the current status of our working directory to the staging area, like this:

WorkingDirectoryStagingAreaCommitsREADME.mdREADME.mdThisisThisisaREADME!aREADME!

When we run `git commit`, we create a commit that contains all of the changes Git added to the staging area. The commit has a unique commit hash, so we can refer to it later. Note how making a commit removes all changes from the staging area:

WorkingDirectoryStagingAreaCommits53dREADME.mdREADME.mdThisisThisisaREADME!aREADME!

We now have a commit with the hash `53d`. This commit is a snapshot of the working directory at the moment it contained a file called `README.md` that had the text `This is a README!`.

Next, we can add a new file to the working directory, and edit `README.md`. This will only affect the working directory, where we're making changes -- not the remote:

WorkingDirectoryStagingAreaCommits53dREADME.mdbot.pyREADME.mdThisisn'tprint(1)ThisisaREADME!aREADME!

Then we can use `git add` to stage our changes:

WorkingDirectoryStagingAreaCommits53dREADME.mdbot.pyREADME.mdbot.pyREADME.mdThisisn'tprint(1)Thisisn'tprint(1)ThisisaREADME!aREADME!aREADME!

In this case, Git adds both the new file (bot.py) and the changed file to the staging area. Then we can commit the changes:

WorkingDirectoryStagingAreaCommits53dREADME.mdbot.pyREADME.mdThisisn'tprint(1)ThisisaREADME!aREADME!12cREADME.mdbot.pyThisisn'tprint(1)aREADME!

We now have two commits, each storing a snapshot of our working directory at a different point in time. On the next screen, we'll cover how to switch between commits to change the files in our working directory.

We can pull up the difference between two commits with the [git diff](https://git-scm.com/docs/git-diff) command -- we just pass the two commit hashes as arguments to `git diff`. To save typing time, we can also just write the first few characters of the hash to uniquely identify the commit (four is usually enough). The order in which we pass the two hashes to `git diff` influences whether changes appear as deletions or additions.

We need to use `q` to exit `git diff` when we're done.

### Instructions

- Use `git diff` to find the difference between your two commits.

```
~$ cd /home/dq/chatbot
~$ HASH=`git rev-parse HEAD`
~$ HASH2=`git rev-parse HEAD~1`
~$ git --no-pager diff $HASH2 $HASH
```

## 7: Switching To A Specific Commit

Now that we know about commit hashes, we can use them to switch to a specific commit. Switching between commits allows us to quickly move between different historical versions of a project. If we introduce a change that causes issues and want to revert to an earlier version, for example, switching between commits will let us do so.

Commit hashes are permanent; Git preserves them and includes them in transfers between the local repo and the remote repo. For instance, let's say we have two commits, `c12` and `c53`. The following diagram shows what happens to them as we clone, commit, and push.

RemoteLocalc12c12README.mdgitcloneREADME.mdThisisaThisisaREADME.README.gitcommitc53README.mdc53gitpushThisisthebestREADME.mdREADME!ThisisthebestREADME!

`c12` originally existed on the remote, but when we pulled it locally, the commit kept the same hash. This is because the commit is the same in the remote and our local repo -- the same changes were made to the same files.

When we changed a file and made a commit locally, Git gave it the hash `c53`. When we pushed this commit to the remote later on, it kept the same hash because it was still the same commit. In the diagram above, both the local repo and the remote repo have two commits, `c12`and `c53`. We can switch between commits in the local repo without changing what commits are in the remote repo. We can do this with the [git reset](https://git-scm.com/docs/git-reset) command:

LocalWorkingDirectoryc12README.mdREADME.mdThisisaThisisaREADME.README.gitcommitc53README.mdREADME.mdThisisthebestThisisthebestREADME!README!gitreset--hardc12c12README.mdREADME.mdThisisaThisisaREADME.README.

The diagram shows the commit on the left, and a representation of our working directory on the right. If we type `git reset --hard c12`, Git switches back to the commit with the hash `c12`, and changes all of the files in the working directory so that they're exactly the same as the files in the commit. This will essentially let us rewind the repo to past commits if there are problems with more recent ones, or if we want to see what the project looked like at an earlier point in time.

The `--hard` flag resets both the working directory and the Git history to a specific state. If we omitted the flag, or used the `--soft` flag instead, it would skip making changes to the working directory, and only reset the Git history.

### Instructions

- Use the `git log` command to find the commit hash corresponding to the oldest commit in the `chatbot` repo.
- Use `git reset` to reset the `chatbot` repo to the oldest commit.
- Explore `README.md` and see what text it contains.

```
~$ cd /home/dq/chatbot
~$ HASH=`git rev-list --max-parents=0 HEAD`
~$ git reset --hard $HASH
```

## 8: Pulling From A Remote Repo

Now that we've reverted our local `chatbot` repo to an older version, the remote repo actually has a newer commit that our local repo doesn't have. This often happens when other people make changes to a project's code, and then push those changes to a remote repo. Here's a diagram showing which commits exist in which locations:

RemoteLocalc12c12README.mdREADME.mdThisisaThisisaREADME.README.c53README.mdThisisthebestREADME!

When the latest commit in our local repo is older than the latest commit in the remote repo, we can use [git pull](https://git-scm.com/docs/git-pull) to update the current branch with the latest commits. The `git pull` command will also update our working directory so that it has the same files as the latest commit.

In our case, we'll be updating the `master`branch, because the `chatbot` repo only has a single branch.

### Instructions

- Pull the latest commits from the `chatbot` remote repo.
- Inspect the working directory and Git history to see what happened.

```
~$ cd /home/dq/chatbot
~$ git pull
```

## 9: Referring To The Most Recent Commit

When using Git, we'll often want to refer to the most recent commit. While we can use the full commit hash, that approach can be cumbersome. Fortunately, Git has a special variable called `HEAD` that always refers to the most recent commit in the current branch.

We can use the `HEAD` variable to switch to the most recent commit more easily. Let's say we modify a file and then want to undo our changes; using `HEAD` will revert the working directory to the state of the most recent commit.

We can also use shortcuts to get older commit hashes. `HEAD~1` will get the second newest commit in the local repo, `HEAD~2` will get the third newest commit, and so on. Here's a diagram of a local repo where `646` is the newest hash on the `master` branch, and `c12` is the oldest:

LocalCommitHashHEADReferencec12HEAD~4c53HEAD~3d24HEAD~2t35HEAD~1646HEAD

We can use [git rev-parse](https://git-scm.com/docs/git-rev-parse) along with the `HEAD` variable to find the commit hash corresponding to a particular commit number. In the diagram above, `git rev-parse HEAD` will return `646`, and `git rev-parse HEAD~3` will return `c53`.

### Instructions

- Use `git reset` with the `HEAD` variable to reset the `chatbot` repo to the most recent commit.
- Use `git rev-parse` with the `HEAD` variable to get the commit hash of the most recent commit.

```
~$ cd /home/dq/chatbot
~$ git reset --hard HEAD~1
~$ git rev-parse HEAD
```

# Git Branches

## 1: Introduction To Git Branches

As you may recall from the last mission, it's very common for large teams to use Git. That's because Git enables smooth collaboration between many programmers who are all making changes to a repo at the same time.

Even so, this type of situation makes it difficult for everyone to work off of the `master` branch. To understand why, let's imagine that we start out with two files, `bot.py` and `README.md`:

README.mdbot.pyprint("Here'ssomeI'mabotstuff!")

Let's say we have three people working on a team -- `Seashell Sally`, `Rocky Raccoon`, and `Superman`. Each person makes the following changes:

SupermanREADME.mdbot.pySupermanisprint("thebest!I'mSuperman!")SeashellSallyREADME.mdbot.pyYoudidn'tprint("hearitBUYfromme,MYbutIdon'tSHELLS")liketheseaRockyRaccoonREADME.mdbot.pyprint("Where'sI'mabotNancy?raccoon")

Each person commits their changes. Because they're all working on the master branch, the commit history will end up looking like this:

RemoteSupermanRockySallyf34f34f34f34b53456765

Notice that the most recent commit is different for each person. What's even worse is that all of the commits make changes to the same files. There's no way for Git to determine which changes are the "correct" ones, so the team members will have issues if they all try to push to the remote. Let's see what happens when Superman pushes his changes:

RemoteSupermanRockySallyf34f34f34f34b53b53456765

Now Rocky and Sally can't push to the remote, because commits `456` and `765`conflict with Superman's commit `b53`. Rocky, Sally, and Superman edited the same lines in the same files. This results in something called a merge conflict, which is painful to fix, and something we'll talk about in the next mission.

Luckily, Git gives us a few ways to avoid merge conflicts. The best method involves using branches. Similar to the way tree branches diverge from the trunk, Git branches allow us to create several different work areas within the same repo. It's common to create a new branch whenever we want to make a change to a project, and then merge that branch back into the master branch when we're done.

Here's an example where Superman, Rocky, and Sally each make their own branches:

RemoteSupermanRockySallymastermasterenhancementmasterrockymastershellsf34f34f34f34f34f34f34b53456765

Each of them originally pulled the master branch, which has the commit `f34`. Then they created separate branches; Superman created `enchancement`, Rocky created `rocky`, and Sally created `shells`. They each made a commit on their own branch.

When each person pushes their branch to the remote, Git stores it separately so that it doesn't conflict with everyone else's changes. Afterwards, the remote will have the branches `master`, `enhancement`, `rocky`, and `shells`. While the team will eventually want to merge all of the branches into master, using branches allows them to make changes to the repo separately while avoiding conflicts.

There are also other benefits to using branches. Imagine that Sally wants to make `10` more commits before merging, and Rocky wants to make `2`. Working on different branches will make their software development process go much faster.

We can create a branch with the [git branch](https://git-scm.com/docs/git-branch) command. For example, `git branch rocky` will create a branch called `rocky`.

Afterwards, we can switch to the new branch using the [git checkout](https://git-scm.com/docs/git-checkout) command. To do this, we'd type `git checkout rocky`. Alternatively, we can create a shortcut by combining the two commands with `git checkout -b rocky`. This will create a branch named `rocky` and then switch to it right away, without our having to type a second, separate command.

### Instructions

- Clone the repo `chatbot` from `/dataquest/user/git/chatbot` to `/home/dq/chatbot`.
- Create a branch called `more-speech` in the repo `chatbot`.
- Switch to the branch `more-speech`.
- Run `bot.py` using `python` to see what happens.

```
~$ cd ~
~$ git clone /dataquest/user/git/chatbot
~$ cd chatbot
~$ git checkout -b more-speech
```

## 2: Switching Branches

Once we create a new branch, we can make changes to it the same way we change the `master` branch. We just have to alter the files, add them to the staging area, and then commit them.

The main wrinkles come into play when we switch branches. Switching to a different branch will change the working directory to reflect the latest commit in that branch. Switching to a new branch, making a new commit, then switching back to master will switch our working directory back to the state of the latest commit in master. Here's a diagram that illustrates what happens:

masterenhancementf34f34bot.pybot.pyprint(1)print(1)gitcommitb53bot.pyprint(2)

In the example above, the branch `enhancement` has one commit more than master. When we switch to the `enhancement` branch, the working directory will contain a file called `bot.py`that contains the code `print('2')`. If we switch to the master branch, Git will alter the working directory to contain a file called `bot.py` that contains the code `print('1')`.

Switching between branches is very useful when we want to work on changes to a project that require different amounts of testing or development time. By putting them in separate branches, we can save the state of each addition, without them conflicting with each other.

The [git checkout](https://git-scm.com/docs/git-checkout) command allows us to switch branches. If we still have changes in the working directory we haven't committed yet, though, Git will check whether there's a potential merge conflict with the branch we're switching to. If there is, Git won't let us change branches.

### Instructions

- Switch to the `more-speech` branch.
- Edit `bot.py` so that it will print more output when it runs.
- Make a commit with the message `Added more output`.

```
~$ cd /home/dq/chatbot
~$ git checkout more-speech
~$ printf "\nprint('Kind of dull in here, right?')" >> bot.py
~$ git add .
~$ git commit -m "Added more output"
```

## 3: Pushing A Branch To A Remote

Once we've created a branch and added a commit, we can push the branch to the remote repo. This allows other people to see our changes and collaborate with us.

As you'll recall from the previous mission, we push a branch to a remote using `git push`. Afterwards, we can use `git branch -r` to show all of the branches on the remote and confirm that ours is there. In contrast, `git branch -a` will show all of the branches available locally.

`git branch -r` might result in the following output:

```

```

```
origin/HEAD -> origin/master                                                  
```

```
origin/master                                                                 
```

```
origin/more-speech
```

The output shows that there are two branches (`master` and `more-speech`) on the remote called `origin`, and that the current branch (the `HEAD` branch) is `master`. Git uses `HEAD` to refer to the current branch, as well as the latest commit in that branch.

### Instructions

- Push the `more-speech` branch to the remote `origin`.
- List the branches available on the remote.

```
~$ cd /home/dq/chatbot
~$ git push origin more-speech
```

## 4: Merging Branches

When someone downloads a project, they typically download and use a single version, or branch. Let's take [Django](https://github.com/django/django) as an example. Django is a popular Web framework for Python that programmers develop using Git and GitHub.

When a user installs Django, he or she install a single version -- not dozens or hundreds of separate branches. If we look at the GitHub repo for Django, however, there are lots of different branches people have used to develop features. [Here's](https://github.com/dataquestio/solutions/pull/4) an example of a separate branch a programmer used to develop a feature.

The release a user downloads and works with is typically from the `master` branch. This means that all of the changes programmers made in other branches need to be pulled into the master branch. We do this through a Git concept called *merging*. Merging allows us to copy commits from one branch into another. This enables us to efficiently develop features for projects on their own branches without conflicts, then merge them into master so that end users will have access to them.

We use the [git merge](https://git-scm.com/docs/git-merge) command to merge a branch into another branch. Here's a diagram illustrating what happens in a merge:

masterenhancementf34f34bot.pybot.pyprint(1)print(1)gitcommitb53b53gitmergebot.pybot.pyprint(2)print(2)

As you can see, merging the branch `enhancement` into the branch `master` will pull the commit `b53` into `master`, and make `b53` the latest commit in `master`. Whenever anyone switches to the `master`branch, their working directory will contain the file `bot.py`, which has the contents `print(2)`.

In order to merge branch b into branch a, we first have to switch to branch a, then run `git merge`.

Merging allows us to efficiently combine changes from multiple branches together, so that we have a working directory that reflects all of the changes in all of the branches.

### Instructions

- Switch to the `master` branch of the `chatbot` repo.
- Merge `more-speech` into `master`.
- Push `master` to the remote repo.

```
~$ cd /home/dq/chatbot
~$ git checkout master
~$ git merge more-speech
~$ git push origin master
```

## 5: Deleting Branches

It's helpful to think of a branch as a collection of commits. Here's an example:

masterenhancementf34f34bot.pybot.pyprint(1)print(1)b53b53bot.pybot.pyprint(2)print(2)

In the example above, both `master` and `enhancement` have `b53` as the latest commit. In fact, both branches have identical commits. This isn't necessarily bad, but it does mean that the original, separate branch is redundant -- it no longer contains any unique commits. It's typical to use a branch to develop a single feature, merge that branch into master, and then delete the branch.

To delete a branch once we've merged all of its commits into another branch, we use the `git branch -d` command. `git branch -d` requires us to specify the name of a branch when we call it. Git will completely remove the branch from our local repo. If a branch has unmerged commits inside it, Git will prevent it from being deleted, so its generally safe to delete branches we think are old or unnecessary.

Having too many branches can make the repo hard to use, so most software teams tend to delete branches once they've merged them. If we have many branches, for example, listing all of them can print hundreds or thousands of lines, making it hard to find the ones we want. It also makes cloning or updating the repo more difficult, because Git needs to download information on all of the branches.

### Instructions

- Delete the `more-speech` branch.

```
~$ cd /home/dq/chatbot
~$ git branch -d more-speech
```

## 6: Checking Out Branches From The Remote

In order to see what other collaborators in the remote repo are working on, we can check out their branches. This will automatically create a local branch with the same name, and copy any commits from the remote branch to the local branch.

Let's say we want to check out a branch called `angry-bot` from the remote. We'll need to use two different commands:

- [git fetch](https://git-scm.com/docs/git-fetch) will fetch all of the current branches and commits from the remote. This won't make any changes to the working directory, but will update Git's list of branch names and commits.
- `git checkout angry-bot` will look for the `angry-bot` branch in the local repo and remote repo. Because it only exists in the remote repo, Git will copy it into the local repo. Git will also make `angry-bot` the current branch.

### Instructions

- Simulate a second collaborator working on the remote:
  - Clone `/dataquest/user/git/chatbot` into `/home/dq/chatbot2`.
  - `cd` into `chatbot2`.
  - Create a new branch called `happy-bot`.
  - Edit `bot.py` to output happy messages.
  - Commit your changes with the message `Made the bot 20% happier!`.
  - Push the `happy-bot` branch to the remote.
- In your local repo `/home/dq/chatbot`, check out the branch.Run `git fetch` to update the Git history.Check out the `happy-bot` branch.Run `bot.py` to make sure the file changed.

```
~$ cd /home/dq
~$ git clone /dataquest/user/git/chatbot chatbot2
~$ cd chatbot2
~$ git checkout -b happy-bot
~$ printf "\nprint('Happiness level: 120')" >> bot.py
~$ git add .
~$ git commit -m "Made the bot 20% happier!"
~$ git push origin happy-bot
~$ cd ..
~$ cd chatbot
~$ git fetch
~$ git checkout happy-bot
~$ python bot.py
```

## 7: Finding Differences Across Branches

The typical Git workflow looks like this:

- Create a branch off of `master` with the name of your feature. Let's say `feature/better-algo`.
- Make your changes on the branch and create commits.
- Push the branch to the remote repo.
- Ask others to review and evaluate your branch.
- Merge the branch into `master` once everyone thinks it looks okay.
- Delete the branch.

This is how most teams of developers work. In order to review a team member's changes, though, it's critical to be able to see the differences between the feature branch and `master`. We refer to these differences as "the diff."

When we use GitHub as our remote repo, [pull requests](https://help.github.com/articles/using-pull-requests/) will show us the differences between branches in an attractive interface, and allow other developers to add comments.

If we're not using GitHub, we can type `git diff` on the command line to see the differences between branches. This command shows line-by-line changes to the code. If the changes are additions, Git will display them in green and prefix them with a plus sign (`+`). If they're deletions, Git will display them in red and prefix them with a minus sign (`-`). It shows new files as additions.

The order in which we specify the two branches as arguments to `git diff`influences whether Git sees the changes as additions or deletions. It's generally preferable to put the "older" branch first.

### Instructions

- Use `git diff` to see the differences between `happy-bot`and `master`.

```
~$ cd /home/dq/chatbot
~$ git --no-pager diff master happy-bot
```

## 8: Branch Naming Conventions

When naming branches, it's common to use a prefix that describes the type of branch, then a slash, then the name of the feature or fix we're making. Here are some typical prefixes, along with example branch names:

- Feature - `feature/happy-bot`
- Fix - `fix/remove-error`
- Chore - `chore/add-analytics`

Features tend to be commits that add functionality to a project, while fixes resolve bugs and other issues. Chores are things that end users won't necessarily notice, but help us reorganize the project or make the code more efficient. These naming conventions make it easier to organize branches, and figure out what each one does without having to look at the full diff.

### Instructions

- Create a new branch called `feature/random-messages`.
- Edit `bot.py` to output one of several possible messages, based on a random number generator.
- Commit and push your branch to the remote.

```
~$ cd /home/dq/chatbot
~$ git checkout -b feature/random-messages
~$ printf "\nimport random\nmessages=['Hi', 'Hi.', 'How are you?', 'Today is a long day...']\nn=random.randint(1,len(messages))\nprint(messages[n])" >> bot.py
~$ git add .
~$ git commit -m "Added random messaging to the bot!"
~$ git push origin feature/random-messages
```

## 9: Branch History

When we create a branch, it takes on the commit history of the branch we started from. Here's an example:

masterfeature/new-interfacef12f12e34e3467f67f45g

There are two branches in this example, `master` and `feature/new-interface`.`feature/new-interface` has one more commit than `master` -- `45g`. If our current branch is `feature/new-interface`and we create a new branch, the most recent commit in the branch we create will be `45g`. If our current branch is `master`and we create a new branch off of it, the most recent commit in the new branch will be `67f`.

### Instructions

- Check out the `feature/random-messages` branch.
- Create a new branch called `feature/spam-messages`.
- Verify that the histories for `feature/random-messages`and `feature/spam-messages` are the same, and different from `master`.

```
~$ cd /home/dq/chatbot
~$ git checkout feature/random-messages
~$ git checkout -b feature/spam-messages
```

# Merge Conflicts

## 1: Introduction

When we merge a branch into another one, our changes can sometimes conflict with other people's commits. Let's say that you're working on a project with another developer named `Ninja Jane`. You both edit the same file on your own branches, then Jane pushes her branch to the remote and merges it into `master`:

RemoteJaneYoumastershurikensuperbotgitpushbot.pyandbot.pybot.pygitmergei=10i=1i=7ifi>1:ifi>1:ifi>1:print(1)print(4)print(8)else:else:else:print(2)print(6)print(3)

This chain of events results in the following situation:

RemoteJaneYoumastershurikensuperbotbot.pybot.pybot.pyi=1i=1i=7ifi>1:ifi>1:ifi>1:print(4)print(4)print(8)else:else:else:print(6)print(6)print(3)

At this point, the commit history for each branch looks like this:

mastershurikensuperbotf12f12f12e34e34e3467f67f67f45g45g782

Because you and Jane branched off of `master` at the same time and you both made one commit, the histories for the two branches are almost identical. However, the latest commit for `master`and `shuriken` is `45g`, and the latest commit for `superbot` is `782`.

When you try to merge `superbot` into master, Git will notice that `45g` and `782`both come immediately after `67f`, and both edit the same lines in the same files. Because both commits are based on `67f`, they're equally valid, and this causes a merge conflict. Git can't overwrite the changes in `master` with the changes from `superbot` because it doesn't know which changes are the "correct" ones. Git is designed to preserve everyone's work, so it won't cause a loss of effort by intentionally overwriting one person's commit with another's.

It's not possible for Git to just layer the commits on top of each other, because `782` and `45g` both come immediately after commit `67f`. If Git layered the changes on top of each other and applied commit `782` from `superbot`, Jane's changes in commit `45g would be overwritten and lost. If commit`782`in`superbot`came after commit`45g` in the Git history instead, there would be no conflict.

When Git can't merge commits automatically, it informs the user of a merge conflict and asks them to sort it out. Sorting out a merge conflict involves editing the code that conflicts to create the "correct" version. This way, the person who wrote the code is in charge of resolving the issue, and Git isn't intentionally overwriting anyone's changes.

### Instructions

- Clone the repo `chatbot` from `/dataquest/user/git/chatbot` to `/home/dq/chatbot`.
- Create a branch.
  - Create a branch called `feature/king-bot` in the repo `chatbot`.
  - Switch to the branch `feature/king-bot`.
  - Edit `bot.py` and add an appropriately kingly print statement at the end of the file.
  - Commit your changes.
- Create another branch with conflicts.
  - Switch to the master branch.
  - Create a branch called `feature/queen-bot` in the repo `chatbot`.
  - Switch to the branch `feature/queen-bot`.
  - Edit `bot.py` and add an appropriately queenly print statement at the end at the end of the file.
  - Commit your changes.
- Create a merge conflict.
  - Merge `feature/king-bot` into `master`.
  - Try merging `feature/queen-bot` into master.
  - At this point, you should trigger a conflict.

```
~$ cd ~
~$ git clone /dataquest/user/git/chatbot
~$ cd chatbot
~$ git checkout -b feature/king-bot
~$ printf "\nprint('I am the king')" >> bot.py
~$ git add .
~$ git commit -m "Make more kinglike"
~$ git checkout master
~$ git checkout -b feature/queen-bot
~$ printf "\nprint('I am the queen)" >> bot.py
~$ git add .
~$ git commit -m "Make more queenlike"
~$ git checkout master
~$ git merge feature/king-bot
~$ git merge feature/queen-bot
```

## 2: Aborting A Merge

When a merge conflict occurs, Git adds markup lines to the problem files to identify where the conflicts are. These lines can prevent code from executing properly, because the Python interpreter doesn't understand them. It's important to resolve conflicts and remove the markup immediately for this reason.

One way to resolve a conflict is to abort the merge altogether. We can do this with [git merge --abort](https://git-scm.com/docs/git-merge).

We'd typically do an abort if we merged one branch into another by accident, or wanted to deal with large merge conflicts in another way. When we abort a merge, Git resets the working directory and Git history to the state they were in before we tried to merge.

### Instructions

- Abort the merge from the last screen, which had conflicts.

```
~$ cd /home/dq/chatbot
~$ git merge --abort
```

## 3: Resolving Conflicts

When a merge conflict occurs, Git will edit the problematic file to add markup indicating where the conflicts are. Here's the markup Git added to our `bot.py` file from the first screen, when we tried to merge `feature/queen-bot` into `master`:

```

```

```
<<<<<<< HEAD                                                                    
```

```
print('I am the king')                                                          
```

```
=======                                                                         
```

```
print('I am the queen)                                                          
```

```
>>>>>>> feature/queen-bot
```

This conflict markup indicates that the current branch (or `HEAD` branch) contains the line `print('I am the king')`, but the branch we're trying to merge, `feature/queen-bot`, contains the line `print('I am the queen')`. Because the last commit in each branch is exclusive to that branch, Git can't automatically determine which line is the most recent edit. This means we have to manually edit the file to remove the lines that Git added, and leave only the code we want.

Here's how we might edit the `bot.py` file to address the conflict:

```

```

```
print('I am the queen')
```

We removed all of the Git confict markup and the alternate code so that only the version we want, `print('I am the queen)`, remains. After doing this for each section of conflict markup (if there are multiple conflicts), we would then commit the file, which would resolve the merge.

### Instructions

- Swich to the `master` branch of the `chatbot` repo.
- Merge `feature/queen-bot` into `master`.
- Fix the merge markup so the lines from `feature/queen-bot` are the ones Git retains.
- Add the changes to the staging area and merge them with the commit message `Fixed conflicts`.
- Push `master` to the remote.

```
~$ cd /home/dq/chatbot
~$ git merge feature/queen-bot
~$ echo "print('I am the queen')" > bot.py
~$ git add .
~$ git commit -m "Fixed conflicts"
~$ git push origin master
```

## 4: Resolving Multi-Line Conflicts

In the previous example, only one line conflicted. When we're working with larger teams and bigger features, however, it's possible to have a conflict across multiple lines.

Let's say `bot.py` in the `master` branch contains the following code:

```

```

```
for i in range(4,20):
```

```
    print("This is the {0}th time I've complimented you!".format(i))
```

`bot.py` in the `feature/king-bot` branch contains this code:

```

```

```
for i in range(0,3):
```

```
    print("Off with his head!")
```

```
for i in range(4,20):
```

```
    print("This is the {0}th time I've complimented you!".format(i))
```

Finally, `bot.py` in the `feature/queen-bot`branch contains this code:

```

```

```
print("Hello")
```

```
print("Off with your head!")
```

```
for i in range(4,20):
```

```
    print("This is the {0}th time I've complimented you!".format(i))
```

In this case, the first two lines of `bot.py`would conflict if we tried to merge both `feature/king-bot` and `feature/queen-bot`into `master`. We'd get conflict markup that looks like this:

```

```

```
<<<<<<< HEAD                                                                    
```

```
for i in range(0,3):
```

```
    print("Off with his head!")                                                          
```

```
=======                                                                         
```

```
print("Hello")
```

```
print("Off with your head!")                                                         
```

```
>>>>>>> feature/queen-bot 
```

```
for i in range(4,20):
```

```
    print("This is the {0}th time I've complimented you!".format(i))
```

When Git detects a multi-line conflict, it places the entire block into a single merge conflict. This makes it much easier to handle large conflicts, like ones that involve entire functions. The process for addressing them is the same as what we did before; we just need to remove the markup and any conflicting lines we don't want to keep.

### Instructions

- Switch into the `/home/dq/chatbot` repo.
- Switch to the `master` branch.
- Create a branch that randomly prints statements.
  - Create a branch called `feature/random-printing` in the repo `chatbot`.
  - Switch to the branch `feature/random-printing`.
  - Edit `bot.py` to add a block that prints one of three random messages at the end.
  - Commit your changes.
- Create another branch with conflicts.
  - Switch to the master branch.
  - Create a branch called `feature/dice-roller` in the repo `chatbot`.
  - Switch to the branch `feature/dice-roller`.
  - Edit `bot.py` to add a block that generates and displays two random numbers at the end.
  - Commit your changes.
- Create a merge conflict.
  - Merge `feature/random-printing` into `master`.
  - Try merging `feature/dice-roller` into master.
  - At this point, you should trigger a conflict.
- Resolve the merge conflict.
  - Resolve the conflict by editing `bot.py`.Remove the merge conflict markup.Keep whatever lines of code you'd like.
  - Commit `bot.py` with the message `Resolved multi-line conflict`.
- Push the `master` branch of `chatbot` to the remote repo.

```
~$ cd ~
~$ cd chatbot
~$ git checkout -b feature/random-printing
~$ printf "\nmessages=['I am great', 'You are great', 'I need more outside time']\nimport random\nmsg=random.choice(messages)\nprint(msg)" >> bot.py
~$ git add .
~$ git commit -m "Add random printing"
~$ git checkout master
~$ git checkout -b feature/dice-roller
~$ printf "\nimport random\nd1=random.randint(1,6)\nd2=random.randint(1,6)\nprint('D1: {0} D2: {1}'.format(d1, d2)" >> bot.py
~$ git add .
~$ git commit -m "Add dice rolling"
~$ git checkout master
~$ git merge feature/random-printing
~$ git merge feature/dice-roller
~$ printf "\nimport random\nd1=random.randint(1,6)\nd2=random.randint(1,6)\nprint('D1: {0} D2: {1}'.format(d1, d2)" > bot.py
~$ git add .
~$ git commit -m "Resolved multi-line conflict"
~$ git push origin master
```

## 5: Resolving Multiple Conflicts

With larger teams, it's possible to have multiple merge conflicts. That can mean several conflicts within a single file, or individual conflicts spread out across different files. When working on large projects involving many files, it's common for a single branch to alter dozens of files. When this happens, you may face merge conflicts that are tricky to resolve.

Although we won't be using them here, this is where Git's graphical merge tools can be helpful. To use them, we'd need to run the [git mergetool](https://git-scm.com/docs/git-mergetool) command, along with the `--tool` option flag to specify which graphical tool to use. We can pull up a full list of available tools by running `git mergetool --tool-help`.

A graphical merge tool will show us the branches side by side and highlight the differences visually, like this:

![img](https://dq-content.s3.amazonaws.com/bDtKd0S.png)

This particular tool displays the `HEAD`branch on the right and calls it the `REMOTE` branch. It displays the branch we're merging on the left and calls it the `LOCAL` branch. The final version is in the center. We need to edit the center version to get the result we want, then save it.

In the absence of a graphical tool, it's still possible to sort through multiple merge conflicts -- it's just a bit more work. That's because we have to remove all of the merge conflict markup for each individual conflict manually.

### Instructions

- Switch into the `/home/dq/chatbot` repo.
- Switch to the `master` branch.
- Create a branch that inserts print statements into `bot.py`.Create a branch called `feature/more-printing` in the repo `chatbot`.Switch to the branch `feature/more-printing`.Edit `bot.py` and add multiple lines that print some text (whatever you'd like).Commit your changes.
- Create another branch that inserts print statements into `bot.py`.Switch to the master branch.Create a branch called `feature/more-printing-2` in the repo `chatbot`.Switch to the branch `feature/more-printing-2`.Edit `bot.py` and add different print statements to the same lines you edited in `feature/more-printing`.Commit your changes.
- Create a merge conflict.
  - Merge `feature/more-printing` into `master`.
  - Try merging `feature/more-printing-2` into master.
  - At this point, you should trigger multiple conflicts.
- Resolve the merge conflict.
  - Resolve the conflicts by editing `bot.py` and keeping the lines you want.
  - Commit `bot.py` with the message `Resolved multiple conflicts`.
- Push the `master` branch of `chatbot` to the remote repo.

```
~$ cd ~
~$ cd chatbot
~$ git checkout -b feature/more-printing
~$ printf "\nmessages=['I am great', 'You are great', 'I need more outside time']\nimport random\nmsg=random.choice(messages)\nprint(msg)" >> bot.py
~$ git add .
~$ git commit -m "Add more printing"
~$ git checkout master
~$ git checkout -b feature/more-printing-2
~$ printf "\nimport random\nd1=random.randint(1,6)\nd2=random.randint(1,6)\nprint('D1: {0} D2: {1}'.format(d1, d2))" >> bot.py
~$ git add .
~$ git commit -m "Add even more printing"
~$ git checkout master
~$ git merge feature/more-printing
~$ git merge feature/more-printing-2
~$ printf "\nimport random\nd1=random.randint(1,6)\nd2=random.randint(1,6)\nprint('D1: {0} D2: {1}'.format(d1, d2))" > bot.py
~$ git add .
~$ git commit -m "Resolved multiple conflicts"
~$ git push origin master
```

## 6: Accepting Changes From Only One Branch

With some merges, we know that one branch has the "correct" changes, and want to ignore the other branch. We can keep files from one of the conflicting branches only by using `git checkout` with the `--ours` and `--theirs` options when we run into a merge conflict.

For example, if we were trying to merge files from `feature/queen-bot` into `master`, we could use `git checkout --ours .` to only keep the files from `master`, and `git checkout --theirs .` to only keep files from `feature/queen-bot`. In general, `--ours` will keep files from the current branch, and `--theirs` will keep files from the branch we're merging in.

Note that in these examples we used `.` at the end of each of these commands, which acts as a wildcard that means `all files`(similar to the command `git add .` ). You can also perform this on a file by file basis by using `git checkout --ours [filename]`.

Currentbranch:masterMerging:shurikenOursTheirsmastershurikenbot.pybot.pyi=10i=1ifi^1:ifi^1:print(1)print(4)else:else:print(2)print(6)

After running `git checkout`, we'll need to commit the files to complete the merge.

### Instructions

- Switch into the `/home/dq/chatbot` repo.
- Switch to the `master` branch.
- Create a branch.
  - Create a branch called `feature/remove-bot` in the repo `chatbot`.
  - Switch to the branch `feature/remove-bot`.
  - Delete `bot.py`.
  - Stage the deleted file using the command `git rm bot.py`.
  - Commit your changes with the commit message `Remove bot`.
- Create another branch with conflicts.
  - Switch to the master branch.
  - Create a branch called `feature/keep-bot` in the repo `chatbot`.
  - Switch to the branch `feature/keep-bot`.
  - Edit `bot.py` and add a print statement to the end of the file.
  - Commit your changes with the message `Keeping bot.py`.
- Create a merge conflict.
  - Merge `feature/remove-bot` into `master`.
  - Try merging `feature/keep-bot` into master.
  - At this point, you should trigger a conflict.
- Keep only the files from `feature/keep-bot`.
- Finish the merge by committing with the message `Keeping bot.py`.
- Push the branch `master` to the remote.

```
~$ cd ~
~$ cd chatbot
~$ git checkout -b feature/remove-bot
~$ git rm bot.py
~$ git commit -m "Remove bot"
~$ git checkout master
~$ git checkout -b feature/keep-bot
~$ printf "\nprint('I want to live')" >> bot.py
~$ git add .
~$ git commit -m "Keep the bot around"
~$ git checkout master
~$ git merge feature/remove-bot
~$ git merge feature/keep-bot
~$ git checkout --theirs .
~$ git add .
~$ git commit -m "Keeping bot.py"
~$ git push origin master
```

## 7: Ignoring Files

There are some files that change very often, and aren't particularly useful to a project. One example is the `.DS_Store` file OS X puts in directories. Another is the `.pyc` files that Python produces when it compiles source files. Neither of these are necessary for the project to work properly, but because they change rapidly, they can create merge conflicts and other problems.

The best way to handle these types of files is to tell Git to ignore them. That means Git won't add them to commits or track them, so we won't have to deal with merge conflicts and other issues they may cause.

To do this, we create a file called `.gitignore`. Then, we add lines to it indicating which files Git should ignore when adding to the staging area and committing. These lines accept wildcard characters, so we can ignore all files that have a certain extension in a single line.

For example, the following lines in `.gitignore` instruct Git to ignore all files called `.DS_Store`, and all files ending with `.pyc`:

```

```

```
.DS_Store
```

```
*.pyc
```

Once we've included those lines, Git won't add new files named `.DS_Store` or that end in `.pyc` to the staging area. It also won't commit them in future commits. It will still track changes to existing files it's already added to a commit, however, and also continue adding them to new commits.

You can find default `.gitignore`configurations for several popular languages [in this GitHub repo](https://github.com/github/gitignore).

### Instructions

- Switch into the `/home/dq/chatbot` repo.
- Switch to the `master` branch.
- Create a file called `.gitignore`.
- Add the following lines to `.gitignore`:`.DS_Store``*.pyc`
- Commit the changes with the message `Added gitignore`.
- Push the `master` branch to the remote.



```
~$ cd ~
~$ cd chatbot
~$ git checkout master
~$ printf ".DS_Store\n*.pyc" > .gitignore
~$ git add .
~$ git commit -m "Added gitignore"
~$ git push origin master
```

## 8: Removing Cached Files

As we mentioned on the previous screen, adding files to `.gitignore` doesn't remove any files that have already been added to a Git commit. Git will still track changes to these files, and add them to future commits. This can be frustrating, especially when those files cause merge conflicts that require a lot of effort to resolve.

Removing files from the Git cache can be helpful in these situations. This will prevent Git from tracking changes to those files, and adding them to future commits.

We can remove files from the Git cache with the [git rm](https://git-scm.com/docs/git-rm) command and the `--cached` flag. For example, the command below will remove any file called `.DS_Store` from the Git cache, and prevent Git from tracking it:

```

```

```
git rm --cached .DS_Store
```

This will remove any files called `.DS_Store` from our Git repo, but not from our working directory. The files will still exist on the computer, but will be invisible to Git for version tracking purposes.

### Instructions

- Switch into the `/home/dq/chatbot` repo.
- Switch to the `master` branch.
- Remove `bot.py` from the Git cache.
- Commit your changes with the message `Removed bot.py`. Remember not use `git add` here, because that would add `bot.py` back in!
- Push the `master` branch to the remote.



```
~$ cd ~
~$ cd chatbot
~$ git checkout master
~$ git rm --cached bot.py
~$ git commit -m "Removed bot.py"
~$ git push origin master
```

# Project: Git Installation and GitHub Integration

## 1: Introduction

In the last mission, we explored the basics of Git version control. In this project, we'll walk you through the process of setting Git up on your own machine and authenticating with GitHub. Afterwards, you'll be able to sync the changes you make to data science projects locally with GitHub. Finally, when you're ready, you can publish your code to GitHub and build a portfolio of projects for others to see.

Unfortunately, debugging your installation is outside the scope of this project, because there can be many potential reasons for a failure. If you encounter an error during the installation process, use Google and StackOverflow to attempt to debug and fix the issue. If you're still having difficulty, post about the issue you're experiencing in the [project forum](https://www.dataquest.io/forum/mission/204/project-git-installation-and-github-integration).

## 2: Installing Git

Fortunately, installing Git only involves a few steps.

- Navigate to the [Git downloads page](https://git-scm.com/downloads) and download the appropriate installer for your operating system.
- Run the installer and step through the installation wizard.
- Open the command line program (**Terminal** for Linux and Mac and **Command Prompt** for Windows) and run `git version`.

If the output describes the current Git version and doesn't throw an error, you've successfully installed Git!

## 3: Configuring Git

Now let's configure Git with your name and email address. Run the following shell command, but replace "YOUR NAME" with your actual name:

```

```

```
git config --global user.name "YOUR NAME"
```

Then, run the following shell command, but replace "YOUR EMAIL ADDRESS" with your actual email address:

```

git config --global user.email "YOUR EMAIL ADDRESS"
```

## 4: Creating A GitHub Account

Use a Web browser to navigate to [GitHub](https://www.dataquest.io/m/128/project-git-installation-and-github-integration/4/www.github.com) and create an account. There are three main steps you'll have to complete:

- Create a personal account. Select a unique username and password and enter your email.
- Choose a plan. If you select the free plan, all of your code (which is organized in repositories) will be public. Select the free plan for now. You can always upgrade to a paid plan later on, which would allow you to have private repositories.
- Read the GitHub [Hello World guide](https://guides.github.com/activities/hello-world/).

Complete the instructions in step 1 of the Hello World guide to create your first repository on GitHub.

## 5: Authenticating With GitHub

Now you need to authenticate your computer with GitHub so you can push code to your remote repositories. The easiest way to do this is to clone the repo you created on GitHub to your local computer. Click the **Clone or Download** button on the repository page, and then click **Use HTTPS** in the floating window to reveal the clone URL:

![Imgur](https://dq-content.s3.amazonaws.com/TemL0hd.png)

Then, run `git clone {url}` from the command line (replace `{url}` with the clone URL). Git will ask you to log in with your GitHub username and password. After you log in, Git will download your repository to your local computer as a new folder (if there are any files in the repo).

To verify that you can now push changes to your GitHub repo:

- Create a branch (`git checkout -b testbranch`).
- Make a small change (use `nano` if you want to edit a file from the command line).
- Commit the change (`git commit -am 'test'`).
- Push the branch to GitHub (`git push origin testbranch`).

By default, you'll be asked to log in every time you run `git push`, `git pull`, or another Git command to the remote repository. You can use a credential helper to get around this, however. To learn more about this option, see the [GitHub documentation](https://help.github.com/articles/which-remote-url-should-i-use/#cloning-with-https-urls-recommended).