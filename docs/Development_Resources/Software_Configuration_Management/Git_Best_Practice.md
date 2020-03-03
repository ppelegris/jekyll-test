---
title: Git Best Practice
parent: Software Configuration Management
grand_parent: Development Resources
nav_order: 3
---

# Git Best Practice

### Fundamentals

You should read the online [Git Pro book](http://git-scm.com/book) to get a good foundation in using Git and its command line. IDEA will help you a lot in the day to day interactions with Git but it's good to know what's going on underneath, and about the design and capability of Git. Also, sometimes IDEA may get confused about the state of the repository, and so it's always good to be able to drop down to the command line and do a '`git status`' to see what's going on, especially for more complex operations. Plus sometimes you might feel better about doing certain tasks in the command line, where you have more control.

### Golden Rule \#1 : If in doubt, use '`git status`' to tell you what's going wrong.

Git is very good at explaining what state it's in and what you can do to rectify issues or move forward with processes.

### Commits

In Git, all commits are safe as they are only done locally. This means you should **commit soon and often**, and try and keep commits contained to a specific logical change. This doesn't mean you commit every file individually, rather you do work in small logical units that could potentially be 'cherry-picked' later on.

Commits only affect other people when they are '**pushed**' to the Stash server, thus pushing is the equivalent to the old '**cvs commit**' and should be done with care if working on any critical branch. Usually though you will be working on a feature/bugfix branch (see later) so this will only be an issue if you are working with other people on the same branch.

##### Commit Comments

The convention in Git is to structure commit comments like email messages:

-   The first line is the 'subject' of the commit - the summary of the task done in the commit. It should usually be in the imperative rather than the past tense i.e. 'Add button to dialog' instead of 'Added button to dialog'
-   After a blank line the next section is the 'message' of the commit - a description of the actual changes performed to implement the subject.
-   After this, you can put any other information, such as JIRA issue id's for linking to issues.

Example:

    Fix the button alignment.
     
    Set up the layout constraints to make the button look better.
    Reduced the button margin
     
    MYADDIN-3 : Improve cosmetics

##### JIRA Comments

There is no need any more to add comments to JIRA issues (e.g. listing all the files changed/added/deleted using the CommitLog plugin)  - JIRA links to Stash to find all commits related to the ticket under the Development section. This means Comments can now be used for what they are supposed to be used for - conversations about the issue!

However, you should always add a comment to the corresponding JIRA issue if you push interim code so that people know the current state of the issue with respect to the repository code - unless you are resolving an issue due to code you have pushed of course, in which case the issue will be updated as part of that.

##### Pushing Commits

Pushing your commits from your local branches to the Stash server means that your work is backed up and available to all, which is good if you're e.g. going away on holiday. Be aware though that once you push a commit up to Stash, anyone may have pulled and based some work upon it, so be careful what you do with that branch afterwards - there are some commands in Git like rebase which can cause headaches for other people using your branches if used incorrectly.

### **Golden Rule \#2 : Always commit as much as you can before you start trying to do other git operations, like switching branches or merging.**

Git won't let you do things like checkout a new branch or merge unless you have a clean working copy. Commit as much as you can first, and anything you really can't commit you can stash to be dealt with later. IDEA will help by automatically stashing and un-stashing for you, but that won't happen if you drop to command line.

### Branches

One of the great features of Git is that you are able to work effortlessly with branches, and switch and merge between them easily. Always create a branch for each new bit of work you start, that way you can work on multiple things at the same time in the same repository without 'collisions'.

To interact with the branching features of Git, it's easiest to use the Branching control in the bottom left of the IDEA window:

![](/assets/attachments/622704/3899398.png){width="500"}

Here you can switch branches, create new ones and merge and delete others. Remember that this only changes your local repository - any branches and commits must be pushed before they can be seen by others.

##### Creating Branches

As mentioned above, you can create branches from within IDEA, but it's usually much easier to create them from a JIRA issue. In general, all branches should be associated with a JIRA issue by naming convention, so this will automate that association for you. You can create a branch for your ticket via the 'Create Branch' link in your JIRA issue:

 

### Golden Rule \#3 : Pay careful attention to 'Branch from' selection when you create a new branch. You need a good reason if you choose something other than develop.

Think about where it should be coming from. Perhaps you need specific functionality that isn't on develop, so you need to inherit from another branch. Bear in mind that if you do this, you may have to delay merging your branch back to develop until the branch you branched from is merged as otherwise you'll pull in those commits too.

If you branch from the wrong parent branch it is not the end of the world, but it can become very hard to fix once you've pushed your commits.

Generally, 'Branch from' should be develop, unless you are working in one of the product specific streams, in which case you should choose the appropriate develop with prefix e.g. etc\_develop, hb\_develop. Another reason you might want to choose a different branch is that your work is an extension of or relies on another piece of work that hasn't yet been merged to develop. Be aware though that your branch will not be able to be merged until the work on the parent branch is also approved for merging, otherwise your merge would pull across the parent changes.

##### Branch Naming

JIRA/Stash will automatically fill in the name for you, based on the issue number and an abbreviated summary. As we are following the 'GitFlow' strategy (see below) you should select the appropriate prefix for the branch name accordingly: e.g. feature/;bugfix/;hotfix/ etc.

Note that if you are working in one of the product specific streams (see below) you'll need to select 'Custom' and append your own prefix to the branch name e.g. etc\_feature/

Once the branch is created you can check it out locally and start working. Remember that no one else will see your work until you push your commits though.

##### Pushing Locally Created Branches

Note that If you create a local branch yourself and want to push it up to the server, you have to tell IDEA that's what you want to do. If you click the VCS|Git|Push... menu item it will display a message at the top saying the local branch is not tracked - there is no remote branch it corresponds to. You need to check the box at the bottom that says 'Push current branch to alternative branch' and a remote branch of the specified name will be created with the contents of your local one.

### Branching Strategy

We generally follow the 'GitFlow' strategy outlined in [this blog post](http://nvie.com/posts/a-successful-git-branching-model/) for our Git branching strategy.

##### **Main Branches**

The main branches that will always be there are **master** and **develop**. **Master** is the 'sacred' branch, only to be committed to by members of the '*stash-masters*' group in JIRA - The master branch is the branch on which release builds are performed. **Develop** is the safe development branch, designed to house work in progress, so there may well be bugs in the work committed to this branch, but **merges to develop should only be done which will not break the build**. Nightly builds will be done on develop, so issues with it should hopefully be picked up quickly.

If you are coming from the old CVS system **develop** can be seen as the equivalent of the trunk in CVS.

##### Supplementary Branches

There will also be feature branches that hold work done on new features or bug fixes by one or more persons. You should always create a local feature branch for any new bit of work you start, and this can then be pushed up to the Stash server so others can see it and work on it with you. 

Version or *Release branches* are used to take milestones for release which are merged on to master.

##### Multiple Products

There may be multiple sets of these branches within the repo's corresponding to different products, prefixed by a common prefix e.g. etc\_master, etc\_develop etc.

### Merging

Often you want to bring commits on to your feature branch from other branches, usually from the parent branch to bring your branch up to date with it. The safest way to do this is by **merging**.

Say for example someone has committed a framework feature and it's been merged on to the develop branch. You want to use that feature but your current branch was branched off of develop before the feature was merged. To bring your branch up to date with the parent branch you should first bring the parent branch up to date, and then merge the parent branch on to your current branch.

To bring the parent branch up to date, you need to switch to the parent branch, and Pull/Update the branch (see below). This will make sure your local version of the parent branch is up to date.

Then you need to switch back to the feature branch, and merge in the parent branch:

![](/assets/attachments/622704/1015819.png?effects=border-simple,shadow-kn){width="300"}

You may of course need to deal with some conflicts if others have modified the same files on the parent branch that you have on your feature branch, but once you have finished you'll have all the changes from develop in your branch; your branch will now be in the same state as if you had branched off of the latest commit in the develop branch.

While merging is 'safe' you should still only merge when absolutely necessary for a couple of reasons:

#### **Merging always has the potential to introduce conflicts, whether at the source level or functionality level.**

 If you're in the middle of working on something you might not appreciate taking time out to have to fix conflicts or pore over others' changes. Also it might destabilize your work which can make it more difficult.

#### **If you've merged, you ideally should not rebase until you have pushed the merge. **

Therefore you will be restricted in what you can do locally. This is because rebasing will create a copy of the commits you've merged in, so if you've merged from develop and rebase the merged commits, once the branch is merged back into develop, develop will have duplicates!

The best strategys is to try to push the result of the merge as soon as possible if you merge from your parent branch. If you cannot, then do not do any rebasing until you do - this includes `git pull --rebase.` In this case just do a standard `git pull`. (see Pulling/Updating below)

 

### Golden Rule \#4 : It is best practice to only do operations on git that are meant to be permanent and ultimately seen by everyone else.

Unless you are going to throw your branch away, there is no such thing as a 'temporary merge' or similar. If you need code from another branch temporarily, copy it across using the file system, don't use git to do it. (However, see Golden Rule \#5)

### Golden Rule \#5 : If you're copying and pasting files between branches to commit, you're probably doing something wrong.

You should probably be cherry-picking or rebasing instead (see [Rebasing](#GitBestPractice-rebasing) below). Don't copy-paste-commit - it's error prone and loses information.

### Rebasing

The only issue with merging is that it will often create a 'merge commit' that will pollute the commit history somewhat. This isn't a problem, but sometimes you can achieve a cleaner result via the **rebase **command. Rebasing effectively creates **brand new copies of commits** on to a different point in the commit graph. So, in the previous example, we could rebase our commits on to the new tip of the develop branch, so that the commits will 'inherit' the new features on the develop branch.

Commits that are generated by rebasing are completely new, they just have the same content as their source commits. You are not moving the commits, simply copying them - the old commits are still there, just with nothing pointing to them any more.

**Rebase with caution : Rebasing can cause issues if you rebase commits that have already been pushed.** Others may have created their own commits based on these commits, and because rebasing creates new commits, effectively orphaning the old ones, it can cause big problems when others then try to reconcile their commits with yours. Also as mentioned above in the Merging section,** rebasing is not recommended for commits that you've merged from other branches** to avoid duplicates.

There are 2 types of rebasing : **normal**, and **interactive**.

-   With normal rebasing, you allow git to just do the whole process for you.
-   With interactive rebasing you can modify any or all of the commits as they happen. This is useful if, for instance, you made a mistake with a previous commit message or commit that you'd like to fix before pushing.

Be sure that your working copy is clean before you start. IDEA provides UI for doing rebasing and helps you with the process - access it via the VCS|Git|Rebase... menu item. You can find documentation for it here : <http://www.jetbrains.com/idea/webhelp/rebase-branches-dialog.html>

### Cherry-picking

Cherry picking is essentially Rebasing, just with a single commit. You can cherry-pick any commit from another branch on to your current branch using IDEA; here's a guide : <http://www.jetbrains.com/idea/webhelp/applying-changes-from-a-specific-commit-to-other-branches-(cherry-picking).html>

As with Rebasing, the commits that are generated on the target branch are completely new, even though they have the same content as their source commits on the source branch. This means that if you merge the 2 branches at a later date you may find you get conflicts due to this.

### Pulling/Updating

If you're collaborating with others on a branch and you want to bring yourself up to date with the latest commits then you need to do a `git pull` .

IDEA hides the simple `git pull`  of the current branch behind a more generic 'Update Project' menu action under the VCS menu.

If you are updating one of the main branches e.g. develop or master, you can just do a standard merge update, or `git pull.`

Otherwise, if you're working on feature branches that you and others are also committing to then when possible (see warning below) you should select **Rebase** as the update type in the dialog (or add `--rebase` to your `git pull` command). The reason we want to do a rebase pull is that we don't want others' commits to be merged with ours and pollute the branch with merge commits. If you are on a long running development project on a feature branch where more than one person is working these will cause a lot of detritus to accumulate. What you want is for your commits to be replayed after their commits in one line, and that's what rebase will do.

The other benefit is that you can deal with conflicts on a per commit basis. If there have been a lot of commits to a file, then when merging you will have to deal with all the changes at once, and it can be unclear just what went on or how it should be rectified. When rebasing, the commits are replayed one by one, so you can resolve commits in a more fine grained manner.

As mentioned in the Merging section **: Rebasing is not recommended if your local branch contains commits that you've merged from other branches,** **to avoid duplicates.** In this case do a standard `git pull.`

### Pushing

Pushing is when you publish your commits made locally on to the central Stash server. Check your work carefully before you push - you should have got the idea by now that fixing things after pushing is a lot harder than before pushing! IDEA's Push action will show you the list of commits that you'll be pushing so that you can check them over before they go up.

Bear in mind that if you've merged from another branch (e.g. develop) there will be a lot of commits from other people in your push. This is expected so don't panic - you're pushing the merge after all.

Before you can push you need to make sure you've Pulled/Updated, as you can't push if your local branch is behind the branch on the server.

After you pull, make sure to do a test of your code before attempting to push again - something in the pull may have broken your code or even stopped it from compiling!

### Pull Requests

So you've finished your work on your feature branch and you want to submit it to be merged on to the develop branch. You will probably not have permission to merge on to develop, in which case you will have to request that your work be merged to develop by someone who has authority via a '**Pull Request**'. This is a feature of Stash that allows you to post a request for merging from e.g. a feature branch to develop, for approval by a senior developer.

Don't get confused between **Pulling** (`git pull`) and **Pull Requests**. Pull requests are named as such for historical reasons - the concept has come from GitHub where forking of repositories are common. As code changes in forked repositories need to be pulled in to the origin repository, that is why they are called Pull Requests. Within the same repository they would really be better called *Merge Requests*.

Essentially, you create a Pull Request and specify the source and target branches, plus one or more reviewers. The target branch is usually the parent branch, which will most of the time be develop (or the equivalent for the stream).

##### Preparing your branch for your Pull Request

There are a couple of things you should do before creating a Pull Request. First, merge the target branch of your Pull Request with your working branch; so in this case you'll merge develop on to your feature branch (see [Merging](#GitBestPractice-merging) above). This is to ensure there are no conflicts with the target branch that would prevent a merge. At this point you'll probably also want to do a quick test to make sure your code still compiles and that the application still works as expected - who knows what the merge might have done!

Next you need to push your feature branch commits up to Stash. This will ensure the Pull Request encompasses all commits to the feature branch, including any merge commits done as a result of merging in the target branch.

Once you've done this, you can go ahead and create your Pull Request.  

##### Creating a Pull Request

The outline of creating a Pull Request can be found [here](https://confluence.atlassian.com/display/STASH/Using+pull+requests+in+Stash).

Creating a pull request for a branch will automatically transition the associated issue for the branch into Code In Review.

Reviewers should then subsequently check over the changes that you're submitting and comment accordingly, rejecting the request if necessary. Otherwise they will approve the request and make the Merge action available. Once the branch is merged the associated issue will be automatically marked as Resolved/Fixed.

While it is In Review the JIRA issue is still assigned to you, so you have the responsibility for prompting the reviewers to review your code. However they will also be reminded automatically once a week by email.

## Attachments:

![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [image2014-6-18 11:57:8.png](/assets/attachments/622704/1015816.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [image2014-6-18 12:13:6.png](/assets/attachments/622704/1015817.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [image2014-6-18 12:55:18.png](/assets/attachments/622704/1015818.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [image2014-6-18 12:57:18.png](/assets/attachments/622704/1015819.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [image2014-8-26 18:27:29.png](/assets/attachments/622704/3899398.png) (image/png)

