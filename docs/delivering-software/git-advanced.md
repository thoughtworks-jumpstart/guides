# Git advanced

## Commonly used commands
- Initialise a new git repository
`git init`
- View modifications made in working dir / View staged files
`git status`
- Undo modifications to file
`git checkout <file name>`
- Undo all modifications in working dir
`git checkout .`
- View details of local changes
`git diff`
- Add change to staging area
`git add`
`git add -p`
- Undo staging changes from specified file
`git reset <file name> `
- Undo all staged changes
`git reset .`
- Commit changes in staging to the local repository
`git commit`
`git commit -m "commit message here"`

## Commands to view commit history and details
- Show details of a commit
`git show <commit-sha>`
- Show git commit history list
`git log`
- Show all branches of commit history title in a graph
`git log --all --oneline --graph`
- View graphical UI of commit history graph and commit details
`gitk`

## Commands to modify local commits that have not been pushed
- Make changes to the last commit (eg: add or remove file, edit commit message)
`git add .`
`git commit --amend`
- Make changes to the last commit (eg: add or remove file, Do not edit commit message)
`git add .`
`git commit --amend --no-edit`
- Undo commits to a certain point in time (changes from commit dumped to working dir)
`git reset <commit SHA to reset to> `
- Delete commits to a certain point in time (**Warning!!** also deletes any uncommitted changes)
`git reset --hard <commit SHA to reset to> `
- Rewrite commit history with rebase interactive
decide what group of commits you would like to rewrite, pick the SHA just before that 
`git rebase -i HEAD~3` to change the last three commit, for example.
- pick the commit if you would like no changes to it
- drop the commit if you would like to delete it
- edit the commit if you would like to amend it
- squash the commit if you would like it to squash to the previous commit

For further details on rebase interactive please visit [Git-Tools-Rewriting-History](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History) 

## Commands to undo commits which are already pushed to remote
As we should never modify commits which are already on remote, we can only revert it with a new commit.
- Create a new commit which is the exact opposite of specified commit SHA
`git revert <SHA>`

## Commands to save local changes temporarily
eg: when you need to pause the feature you are working on, to work on something else urgent

- Saves changes from working dir and staging to stash area
`git stash`
- Saves changes from working dir and staging area and new files, to stash area
`git stash -u`
- Save stash with a description
`git stash save my-awesome-stash`
- Remove last stashed changes and dump them to the working dir
`git stash pop`
- Remove second last stashed and dump to the working dir
`git stash pop stash@{1}`
- Dump last stashed to the working dir, but leave stash intact
`git stash apply`
- List all stashed changes
`git stash list`

## Working with branches
Best practices: Do a git pull to get the lastest code before creating a branch
- Create branch
`git branch <branch-name>`
- Switch working dir to specified branch
`git checkout <branch-name>`
- Create branch and switch to branch (2-in-1 command)
`git checkout -b <branch name>`
- Delete specified branch
`git branch -d <branch-name>`
- Force delete specified branch (to delete branch even if it has changes unmerged to master)
`git branch -D <branch-name>`
