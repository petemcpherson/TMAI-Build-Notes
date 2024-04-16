# 3 - Git & GitHub setup

1. Set up git
2. Set up remote repository using github

## 1 - Git
Make sure you're in your local directory (root) for the project:
```
git init
```

You can now run `git status` to quickly see what branch you're on--as well as the changes you need to add, then commit.

Next, add all changes to the commit with
```
git add .
```

Then commit everything to the branch with
```
git commit -m "write a brief committ message here. something like 'initial commit' for now, etc"
```


## 2 - Github
Github is just a "remote repository" for your code. Think of kinda like cloud storage.

First, sign up for a free account on Github.com

Then, **create a new repository on GitHub**: Click the "+" icon in the top right corner and select "New repository". Fill in the repository name, description, and visibility settings.

**NOTE**: Make sure to set the **PRIVACY** you want! 

Once the repository is created, you'll see a few options for adding code. Scroll down to "…or push an existing repository from the command line."

Copy that code, and paste it into your local terminal *(still in the root folder)*. You might have to press 'Enter' multiple times. It's multiple "runs" in the terminal.

The code usually looks something like this:

```
git remote add origin https://github.com/petemcpherson/NAMEOFPROJECT.git
git branch -M main
git push -u origin main
```

After those 3 commands are run--you should be able to refresh that GitHub url--and instead of seeing the instructions, you should see your project's code!!