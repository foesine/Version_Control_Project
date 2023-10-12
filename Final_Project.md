# Rebasing and Cherry-Picking Project
by **Gabriela Moravčíková, Terézia Blihárová, Emma Fössing**

## Table of contents 
1. [Introduction](#introduction)
2. [Rebasing](#rebasing)   
3. [Cherry-Picking](#cherry-picking)
4. [Sources](#sources)


## Introduction
Rebasing and cherry-picking come to use when the work has diverged and branches were created. They are tools used by repository maintainers instead of merging branches from other contributers. The main advantage it has to the classical merge is the ability to maintain a mostly linear history ([Chacon & Straub, 2014](#pro-git-book)).

## Rebasing
### General idea of it

### Example
### When to use it 

The ```git rebase <basebranch>``` command takes all the commits of the currently checked out branch (topicbranch) and applies them on the basebranch, usually the master branch, which has to be specified in the command.
The rebase command finds the common anchestor of the two branches and finds the differences to this made on the topicbranch with each commit. These are temporarily stored, the topicbranch is then reset to the same commit as the basebranch and then the changes are applied to the basebranch. To have a clear linear commit history and the pointers to both point at the same commit, the basebranch has to be checked out and then a fast-forwad merge can be applied ([Chacon & Straub, 2014](#pro-git-book)).

#### Visual representation
![.](./pictures/diverged_work_history.png)<br>
*<font size="1"> Picture from [Chacon & Straub, 2014](#pro-git-book) p.69</font>* <br>
*This is what the initial situation might look like. C4 was commited on a seperate branch and now it should be applied to the master branch.* <br>

![.](./pictures/basic_rebase.png)<br>
*<font size="1"> Picture from [Chacon & Straub, 2014](#pro-git-book) p.69</font>* <br>
*This is what the situation looks like after running the rebase command* <br>

![.](./pictures/basic_rebase_after_merge_ff.png) <br>
*<font size="1"> Picture from [Chacon & Straub, 2014](#pro-git-book) p.70</font>* <br>
*This is the final history after the merge fast-forward.* <br>

There will not be any difference between the files after the rebasing or the applying of the ```merge``` command. Only the git history will be different as the log history looks linear ([Chacon & Straub, 2014](#pro-git-book)). 

A neccessity for rebasing is to have a branch checked out and a configured upstream, otherwise the command will abort ([Git-rebase documentation](#git-scm)).


There are some additional arguments that can be applied to the ```rebase``` command. <br>
Running ``` git rebase <basebranch> <topicbranch>``` the topicbranch does not have to be checked out initially. <br>
Problematic commits will cause git to report a merge conflict which can be resolved manually and then one has the option to ```rebase --continue``` or ```rebase --abort``` ([Git-rebase documentation](#git-scm)). <br>
After a topicbranch was rebased, the information is integrated on the basebranch and if the topicbranch is not used anymore it can be deleted with ```git branch -d <topicbranch>```.

If there are multiple branches and the topicbranch does not have a direct common anchestor with the basebranch as it is branching out from a further branch, then ```--onto``` can be used ([Chacon & Straub, 2014](#pro-git-book)).

#### Visual representation
![.](./pictures/rebasing_topic_off_another_topic.png)<br>
*<font size="1"> Picture from [Chacon & Straub, 2014](#pro-git-book) p.71</font>* <br>
*This is the client branch (commits C8 and C9) rebased onto the master branch (commits C8' and C9'). The command used for this is  ```git rebase --onto master server client``` ([Chacon & Straub, 2014](#pro-git-book))*. <br>

This is the same as ```git reset --hard <upstream>```. There are many branch scenarios in which ```--onto``` can be used, for more see the [Git-rebase documentation](#git-scm).

And similar to rebasing, there exists also [Cherry-picking](#cherry-picking), which follows the same idea and a good tool for more picky maintainers.

## Cherry-Picking
### General idea of it
Cherry-picking is a proccess to manually pick commints from one branch and introduce them from another branch. This feature is particularly usefull if we have a number of commits on the topic branch and we want to integrate only one of them.  We use the ```git cherry-pick``` command to take the changes introduced in a single Git commit and try to re-introduce it as a new commit on the branch we are currently on. This approach is in contrast to other methods, such as previously mentioned rebasing or merging, which typically involve the integration of multiple commits into another branch.
### Visual Representation 
We suppose our project looks as following: <br>
![History before cherry pick](./pictures/cherry.png)<br>
Now, if we want to pull, for example, the commit e43a6 into our master branch we can use ``` git cherry-pick <commit hash> ``` , which, assuming there are no conflicts, pulls the same change introduced in e43a6, but we get a new commit SHA-1 value and our history will look like the following: <br>
![History after cherry-picking a commit on a topic branch](./pictures/after_cherry.png)<br>

Each <commit-hash> represents the SHA-1 hash of the commit we want to apply. Additionally, while using this command, we have to make sure we are on the branch we want to apply the commit to. 

### Options 
We can also cherry-pick multiple commits. To do so, after the ``` git cherry-pick``` command we list their commit hashes sequentially : ``` git cherry-pick <commit-hash-1> <commit-hash-2> <commit-hash-3> ```
 

### When to use it 
Cherry-picking whilst interesting feature is a practice that needs to find a proper place to be used. There are few possible uses for this command such as -for exemple-  work in teams where tight collaboration needs to take place. Cherry-picking enables workers to pick a selected commit and proceed with only certain changes. This can be usefull for exemple when accidentaly making changes to a wrong branch.
Other possible use for cherry-picking is when some mistakes, in other words "bugs" needs to be fixed. This command makes the process of the fixes easier in a way, that it combines multiple git commands such as git show and process of committing and checking out and going from one branch to another to fix the problems into one singe cherry-pick commit.
Last but not least undoing certain changes or restoring lost commits can be provided by the use of cherry-picking. Using other git comands and the knowledge that git stores these data we can easily revive the data. 

### When to not use it
As glamurous as it sounds, there are some things that needs to be taken into consideration while cherry-picking. Cherry-picking essetially creates two copies of one commit. That being said, if one of the branches with the commit gets modified, it can lead to a bigger future problem when somebody would try to merge the two branches. 

## Sources
- <a id="cherry picking of code commits"></a>Bunyakiati, P., & Phipathananunth, C. (2017). *Cherry-Picking of Code Commits in Long-Running, Multi-release Software*. In Proceedings of 2017 11th Joint Meeting of the European Software Engineering Conference and the ACM SIGSOFT Symposium on the Foundations of Software Engineering. 

- <a id="pro-git-book"></a> Chacon, S., & Straub, B. (2014). *Pro git*. Springer Nature.

- <a id="git-scm"></a> Git-rebase documentation. https://git-scm.com/docs/git-rebase; retrieved on 11/10/2023.

- <a id="git-scm2"></a> Git-cherry-pick documentation. https://git-scm.com/docs/git-cherry-pick; retrieved on 12/10/2023.

- <a id="geeks"></a> Git-cherry-pick. https://www.geeksforgeeks.org/git-cherry-pick/; retrieved on 12/10/2023.



