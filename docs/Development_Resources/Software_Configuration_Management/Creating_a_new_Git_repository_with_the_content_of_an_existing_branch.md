---
title: Creating a new Git repository with the content of an existing branch
parent: Software Configuration Management
grand_parent: Development Resources
nav_order: 1
---

# Creating a new Git repository with the content of an existing branch

These are the steps I followed when looking at how to split the WAM Reporting 2 code from the **urplatform** repository into a Git repository of its own.  The fact that all relevant commits were on a single branch *and* contained within a single top-level directory simplified things.  At the end of the process I had a (tiny – under 4MB space used on the server!) Stash repository containing only the commits on **wr\_develop** but with all history and branching information retained.  Commit hashes are however different to those in the **urplatform** repository.  The steps can be followed if you're ever faced with a similar task.

## Step-by-step guide

In the followings steps `$` represents the command prompt.  The initial clone in step 1 was actually done against a local Stash server so the size of the initial repository is probably smaller than would be the case for a checkout from the Stash server in Maidenhead (my local server had only the **develop** and **wr\_develop** branches pushed to it).

1.  Clone the source repository into an arbitrary directory
    `$ git clone http://mscott@stash.ureason.co.uk/stash/scm/urp/urplatform.git wr2nt-creation`

    Cloning into 'wr2nt-creation'...
    remote: Counting objects: 339396, done.
    remote: Compressing objects: 100% (59005/59005), done.
    remote: Total 339396 (delta 223198), reused 338247 (delta 222154)
    Receiving objects: 100% (339396/339396), 600.21 MiB | 64.45 MiB/s, done.
    Resolving deltas: 100% (223198/223198), done.
    Checking connectivity... done.
    Checking out files: 100% (20049/20049), done. 
     

2.  `$ cd wr2nt-creation/`
3.  `$ git checkout wr_develop`

    Branch wr\_develop set up to track remote branch wr\_develop from origin.
    Switched to a new branch 'wr\_develop'

     

4.  Make the repository look as if it were rooted in directory wamreporting2, and discard all history that didn't affect that directory
    `$ git filter-branch --subdirectory-filter wamreporting2 -- --all`

    *... history is rewritten for commits affecting 'wamreporting2' directory*
    *... various messages about what was or wasn't rewritten are logged
     *

5.  Remove the reference to the original upstream repository
    `$ git remote rm origin `

6.  Create a new Stash repository that will hold the split off code (I created a personal one called **wr2nt** on the Maidenhead Stash server)
7.  Add the new repository as the upstream
    `$ git remote add origin http://mscott@stash.ureason.co.uk/scm/~mscott/wr2nt.git `
8.  Push branch **wr\_develop** upstream but with the name **develop** (renaming is obviously optional)** 
    **
    `$ git push --set-upstream origin wr_develop:develop`

    Counting objects: 12291, done.
    Delta compression using up to 8 threads.
    Compressing objects: 100% (3232/3232), done.
    Writing objects: 100% (12291/12291), 3.57 MiB | 44.00 KiB/s, done.
    Total 12291 (delta 6613), reused 11591 (delta 6596)
    To <http://mscott@stash.ureason.co.uk/scm/~mscott/wr2nt.git>
    \* \[new branch\] wr\_develop -&gt; develop
    Branch wr\_develop set up to track remote branch develop from origin.

9.  Find the commit hash of the first ever commit on **wr\_develop**
    `$ git rev-list --max-parents=0 HEAD`
    ec474079f0cdfbd0830019cea2c053ec2b230459
     

10. Create a new branch called **master** based on the first commit on **wr\_develop** (optional, but a **master** branch is probably desirable)
    `$ git branch master ec474079f0`
     
11. Check out **master** and verify it contains only a single commit
    `$ git checkout master`
    Switched to branch 'master'
     `$ git log`

    commit ec474079f0cdfbd0830019cea2c053ec2b230459
    Author: Nathan Brown &lt;nbrown@[ureason.com](http://ureason.com)&gt;
    Date: Mon Jan 19 18:19:38 2015 +0000

    Added basic project structure

     

12. Push **master** upstream
    `$ git push --set-upstream origin master`

    Total 0 (delta 0), reused 0 (delta 0)
    To <http://mscott@stash.ureason.co.uk/scm/~mscott/wr2nt.git>
    \* \[new branch\] master -&gt; master
    Branch master set up to track remote branch master from origin.

     

13. Create a clean clone of upstream and check that the log for **develop** contains the entire history of commits from the start of wr\_develop (Stash has been configured to treat **develop** as the default branch for the wr2nt repository)
    `$ cd ..`
    `$ git clone http://mscott@stash.ureason.co.uk/scm/~mscott/wr2nt.git`

    Cloning into 'wr2nt'...
    remote: Counting objects: 12291, done.
    remote: Compressing objects: 100% (3215/3215), done.
    remote: Total 12291 (delta 6613), reused 12291 (delta 6613)
    Receiving objects: 100% (12291/12291), 3.57 MiB | 314.00 KiB/s, done.
    Resolving deltas: 100% (6613/6613), done.
    Checking connectivity... done.

    `$ cd wr2nt`

    $ git log

    commit 072ac877b32c2b2784f6f53a6ebbaf1a82d6df2f
    Merge: 525808c e2d5116
    Author: Richard Cumberland &lt;rcumberland@[ureason.com](http://ureason.com)&gt;
    Date: Mon Nov 9 13:01:49 2015 +0000

    ...

    commit ec474079f0cdfbd0830019cea2c053ec2b230459
    Author: Nathan Brown &lt;nbrown@[ureason.com](http://ureason.com)&gt;
    Date: Mon Jan 19 18:19:38 2015 +0000

## End result

Although [the repository created from the steps above](http://stash.ureason.co.uk/users/mscott/repos/wr2nt/commits) is accessible, Stash seems to have a limit on the amount of history it shows on any given branch.  Just to show that the history is retained here's a screenshot from SourceTree with the local repository created at step 13 open:

![](/assets/attachments/20643857/20643859.png){height="400"}

## Alternative/further steps

I don't know if it's possible to introduce another directory into the tree when `git filter-branch` is run (*e.g.* if it's not desired to have the content of the split off directory appear at the top level in the new repository).  If that's desirable but not possible then the worst case is moving everything as a final step - Git should preserve history for moved files.

Cleaning up the original repository to remove the split off branch is probably easiest achieved by simply deleting the branch once any commits on it that should be retained have been merged onto its parent branch. It would probably be possible to cleanly prune the split off branch's commits with some Git magic but in the case of removing **wamreporting2** from **urplatform** it's probably not worth the hassle of figuring out since the total number of commits on **wr\_develop** as a proportion of all commits in **urplatform** is tiny  – at best we'd clean up a few MB of objects from a repository that's over 1GB in size.

## Related articles

**Content by label**
There is no content with the specified labels

## Attachments:

![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [SourceTree-wr2nt.png](/assets/attachments/20643857/20643861.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [SourceTree-wr2nt.png](/assets/attachments/20643857/20643859.png) (image/png)

