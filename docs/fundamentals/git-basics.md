# Git basics

It is worth noting that there are GUI tools to help you with Git commands, but we do not recommend you start learning Git this way. To become a proficient software developer, you must learn Git through the command line. 

## Create a local repository

Create a new directory from the Terminal

```sh
mkdir my-first-repo
cd my-first-repo
```

Initialize your Git repository. 

```sh
git init
```

This creates a new `.git` folder. Let's take a look inside.

```sh
ls -lah .git
```

## Create a remote repository

You've just created a local Git repository, but if anything was to happen to your computer you would lose all your data. Now we want to create a remote repository and push our changes there so that we have a backup. 

You can use any Git SaaS service provider, such as GitLab, GitHub, Bitbucket, or even roll out your own. We will be using GitHub. 

Create a new repository on GitHub and copy the Git repository URL. 

```sh
git remote add origin <your-git-repo-url>
```

Now check that you have added it correctly.

```sh
git remote --verbose
```
