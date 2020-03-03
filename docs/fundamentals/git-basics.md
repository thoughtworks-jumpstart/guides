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

These are the files and folders inside `.git`

```sh
.git
├── COMMIT_EDITMSG
├── FETCH_HEAD
├── HEAD
├── config
├── description
├── hooks
├── index
├── info
├── logs
├── objects
└── refs
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

![Git lifecycle](https://git-scm.com/book/en/v2/images/lifecycle.png)

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

View changes you have made

```sh
git diff
```

Add your changes

```sh
git add README.md
git status
```

Commit your changes (local)

```sh
git commit -m "Initial commit"
```

Push your changes (remote)

```sh
git push origin master
```

## Ignore files and folders

You can use .gitignore to ignore files and folders that we do not want inside the repository. A folder commonly ignored is `node_modules` and a file commonly ignored is `.env`.

## Write good commit messages

Adding a good commit message is very important in a project. Follow these [rules](https://chris.beams.io/posts/git-commit/) for good commit messages.

The seven rules of a great Git commit message

0. Separate subject from body with a blank line
1. Limit the subject line to 50 characters
1. Capitalize the subject line
1. Do not end the subject line with a period
1. Use the imperative mood in the subject line
1. Wrap the body at 72 characters
1. Use the body to explain what and why vs. how

## Fork an existing repository

When you fork a repository, you are making a copy of that repository into your own GitHub account.

Head to an exisiting repository on GitHub and click on the `fork` button.

## Clone an existing repository

Once you have forked someone's repository, you want to clone it into your own machine.

```sh
git clone https://github.com/thoughtworks-jumpstart/git-basics.git
```
