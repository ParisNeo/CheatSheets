# Git Cheat Sheet

## Configuration

### Configure user information
Configure the name that will be associated with your commits and your email address.

```
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```


### Configure text editor
Configure the text editor used for commit messages.

```
git config --global core.editor "nano"
```


### Configure colors
Configure colors for Git output.

```
git config --global color.ui auto
```


## Basic Usage

### Clone a repository
Clone a remote repository to your local machine.

```
git clone https://github.com/user/repo.git
```

### Initialize a repository
Initialize a new Git repository in a directory.

```
git init
```

### Add files
Add one or more files to the index.

```
git add file.txt
git add *.txt
```


### Commit changes
Commit changes to the local repository.

```
git commit -m "Commit message"
```

### Push changes
Push changes to a remote repository.

```
git push origin master
```

### Pull changes
Pull changes from a remote repository.

```
git pull origin master
```

### Status
Check the status of the repository.

```
git status
```

### Log
View the commit history.

```
git log
```

## Branching

### Create a branch
Create a new branch.

```
git branch new_branch
```


### Switch to a branch
Switch to an existing branch.

```
git checkout existing_branch
```


### Create and switch to a branch
Create a new branch and switch to it.

```
git checkout -b new_branch
```

### Merge a branch
Merge changes from another branch into the current branch.

```
git merge other_branch
```


### Delete a branch
Delete a branch.

```
git branch -d branch_name
```

## Advanced

### Stash changes
Stash changes that are not yet ready to be committed.

```
git stash
```

### Apply stashed changes
Apply changes that were stashed.

```
git stash apply
```

### Rebase
Rebase the current branch on top of another branch.

```
git rebase other_branch
```

### Reset
Reset the current branch to a previous commit.

```
git reset --hard HEAD~1
```

# Submodule
To initialize submodules:
```git
git submodule init
```
To update submodules:
```git
git submodule update
```
To recursively initialize and update submodules:
```git
git submodule update --recursive
```
To do it all at once, you can do this:
```git
git submodule update --init --recursive
```
Note that if the repository has been updated to include new submodules, you will need to run the git submodule init command again to initialize them.

## Resources

- [Official Git documentation](https://git-scm.com/docs)
- [Atlassian Git tutorial](https://www.atlassian.com/git/tutorials)
- [Git Cheat Sheet by GitHub](https://education.github.com/git-cheat-sheet-education.pdf)
