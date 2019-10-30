# Git basics

It is worth noting that there are GUI tools to help you with Git commands, but we do not recommend you start learning Git this way. To become a proficient software developer, you must learn Git through the command line.

## Cloning an existing repository

Go to an existing repository and clone it using the following command.

```sh
git clone https://github.com/thoughtworks-jumpstart/git-basics.git
```

This clones the repository locally on your computer.

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

## Git lifecycle

There are two types of files in Git - tracked and untracked files. When you add a new file, it will be untracked. Once Git tracks a file, it falls under one of three states - unmodified, modified, and staged.

![Wow](https://git-scm.com/book/en/v2/images/lifecycle.png)

## Add your changes

Check the status of your files

```sh
git status
```

Create a `README.md` file and add some content to it. Now check the status again.

You should see that `README.md` is now an untracked file.

To tell Git to track your `README.md` file, run the following command

```sh
git add -N README.md
git status
```

What do you notice about `README.md` now?
