# Rebasing and Cherry-Picking Project
by **Gabriela Moravčíková, Terézia Blihárová, Emma Fössing**

## Table of contents 
1. [Introduction](#introduction)
2. [Rebasing](#rebasing)   
3. [Cherry-Picking](#cherry-picking)
4. [Sources](#sources)


## Introduction
Rebasing and cherry-picking come to use when the work has diverged and branches were created. They are tools used by repository maintainers instead of merging branches from other contributers. The main advantage it has to the classical merge is the ability to maintain a mostly linear history.

## Rebasing
### General idea of it
The ```git rebase <branchname>``` command takes all the commits of the currently checked out branch and applies them on another, usually the master branch, which has to be specified in the command.
The rebase command finds the common anchestor of the two branches and finds the differences to this made on the seperate (new) branch with each commit. These are temporarily stored, the current branch is then reset to the same commit as the branch that will be rebased on and then the changes are applied. To have a clear linear commit history and the pointers to both point at the same commit, the old branch has to be checked out and then a fast-forwad merge can be applied.

There are some additional arguments that can be applied to the 

### Example


### Advantage

## Cherry-Picking
### General idea of it
Cherry-picking is a proccess to manually pick commints from one branch and introduce them from another branch. This feature is particularly usefull if we have a number of commits on the topic branch and we want to integrate only one of them. We use the ```git cherry-pick``` command to take the changes introduced in a single Git commit and try to re-introduce it as a new commit on the branch we are currently on. 
### Example
### Advantage


