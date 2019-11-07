# Visualizing Git

It often helps to visualize what you're actually doing when you make a commit. This [visual tool](https://git-school.github.io/visualizing-git/) helps to visualize what happens when you add commits to a branch.

Note that this is only a visualization tool and most of the Git commands will not work.

Show commit log

```sh
git log
```

Create a new commit

```sh
git commit -m "My first commit"
```

Navigate to a previous commit

```sh
git checkout <commit-number>
```

Navigate back to the head of the master branch

```sh
git checkout master
```

Navigate back to the head of another branch

```sh
git checkout <branch-name>
```

Create a new branch

```sh
git checkout -b <branch-name>
```
