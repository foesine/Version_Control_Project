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
### When to use it 
Rebasing in the comparison of the end product, is not that different from the basic merge command. One of the biggest pro to using rebasing is, as it was mentioned before, that it makes for a cleaner commit history by rewriting it to linear state even though the work was previously done in parallel. Other benefit coming from using ```git rebase``` command is that it limits the commits that would be needed to merge the same changes, so again, it leaves the commit history much more clean. ([Chacon & Straub, 2014](#pro-git-book)). <br>
### When not to use it 
The advantage of cleaner tree that rebasing provides could be also the reason to why not to use it. While rebasing would be usefull to make the commit history more readable, if the need for the outcome is that the tree should be more presentable or easy to orient in, still there would be the loss of a real work path that was taken to get to the final sate of the work.  With rebasing, problems might also occur when trying to rebase commits outside of the personal repository. Rebasing essentialy creates new slightly different commits which then pushed into the common repository can cause differences between coworkers commits history and shared repository cmmits history, leaving them with the only option to re-merge their work. ([Chacon & Straub, 2014](#pro-git-book)). <br>

## Cherry-Picking
### General idea of it
Both ```git rebase``` and ```git cherry-pick``` are  commands used to rewrite the commits of one branch on top of another ([Geeks for Geeks: Git cherry-pick](#geeks)). Cherry-picking includes manually picking commits from one branch and introduce them to another branch ([Bunyakiati & Phipathananunth, 2017](#cherry_picking_code_commits)). This feature is particularly usefull if we have a number of commits on the topic branch and we want to integrate only one of them.  We use the ```git cherry-pick``` command to take the changes introduced in a single Git commit and try to re-introduce it as a new commit on the branch we are currently on ([Chacon & Straub, 2014](#pro-git-book)). 
### Visual Representation 
We suppose our project looks as following: <br>
![History before cherry pick](./pictures/cherry.png)<br>
*<font size="1"> Picture from : [Chacon & Straub, 2014, p.127](#pro-git-book) </font>*  <br>
Now, if we want to pull, for example, the commit e43a6 into our master branch we can use ``` git cherry-pick <commit hash> ``` , which, assuming there are no conflicts, pulls the same change introduced in e43a6, but we get a new commit SHA-1 value and our history will look like the following: <br>
![History after cherry-picking a commit on a topic branch](./pictures/after_cherry.png)<br> 
*<font size="1"> Picture from : [Chacon & Straub, 2014, p.127](#pro-git-book) </font>*

Each ```<commit-hash>``` represents the SHA-1 hash of the commit we want to apply. Additionally, while using this command, we have to make sure we are on the branch we want to apply the commit to. 

### Options 
We can also cherry-pick multiple commits. To do so, after the ``` git cherry-pick``` command we list the commit hashes sequentially: ``` git cherry-pick <commit-hash-1> <commit-hash-2> <commit-hash-3> ```<br> 
Another usefull option is ```-e``` or ```--edit``` when we want to edit the commit message of a cherry-picked commit prior to commiting. <br>
For more  details about additional options available for the cherry-pick command, please refer to the official [Git cherry-pick documentation](#git-scm2).

 

### When to use it
Cherry-picking whilst interesting feature requires thoughtful application. There are many possible uses for this command such as -for exemple-  work in team environments where precise coordination is essential. Cherry-picking enables workers to select specific commits and incorporate only the necessary changes. For instance, this practice can be usefull when rectifying accidental changes made to a wrong branch. ([Git-cherry-pick](#geeks))
Another helpful application of ```git cherry-pick ``` is error correction, specifically adressing "bugs". This command makes the process of the fixes easier in a way, that it combines multiple git commands such as ```git show ```, committing process and branch switching into one single comand. Lastly, undoing certain changes or restoring lost commits can be also a useful function of cherry-pick comand. ([Git cherry pick](#Atlassian))

### When to not use it
As appealing as it may seem, there are certain limitations to ```git cherry-pick``` command. Cherry-picking essetially creates two copies of the same commit. That being said, if one of the branches with the commit gets modified, it can potentially create challenges when attempting to merge these two branches in the future.  This issue might cause problems, especially while working on collaborative team projects,  thus highlighting that the very advantage of this command can be considered a drawback. ([Chen, 2018](#devblogs))


## Sources
- <a id="cherry_picking_code_commits"></a>Bunyakiati, P., & Phipathananunth, C. (2017). *Cherry-Picking of Code Commits in Long-Running, Multi-release Software*. In Proceedings of 2017 11th Joint Meeting of the European Software Engineering Conference and the ACM SIGSOFT Symposium on the Foundations of Software Engineering. 

- <a id="pro-git-book"></a> Chacon, S., & Straub, B. (2014). *Pro git*. Springer Nature.

- <a id="git-scm2"></a> Git-cherry-pick documentation. https://git-scm.com/docs/git-cherry-pick; retrieved on 12/10/2023.

- <a id="geeks"></a> Geeks for Geeks: Git-cherry-pick. https://www.geeksforgeeks.org/git-cherry-pick/; retrieved on 12/10/2023.

- <a id="git-scm"></a> Git-rebase documentation. https://git-scm.com/docs/git-rebase; retrieved on 11/10/2023.

- Perschbacher,B. (2010) *Hopefully Interesting Git Topics.*

- <a id="atlassian"></a> Git cherry pick. https://www.atlassian.com/git/tutorials/cherry-pick; retrieved on 11.10.2023.

- <a id="devblogs"></a> Chen, R.(2018). *Stop cherry-picking, start merging, Part 1: The merge conflict.* In The Old New Thing. https://devblogs.microsoft.com/oldnewthing/20180312-00/?p=98215



