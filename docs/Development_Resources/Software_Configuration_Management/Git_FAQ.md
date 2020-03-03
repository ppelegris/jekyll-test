---
title: Git FAQ
parent: Software Configuration Management
grand_parent: Development Resources
nav_order: 4
---

# Git FAQ

For all solutions that involve command line, it is assumed that you first change the current directory to one that is in your git repository.

-   [I branched from the wrong parent branch, what do I do?](#GitFAQ-Ibranchedfromthewrongparentbranch,whatdoIdo?)
-   [I made a mistake in my last commit comment. Can I edit it?](#GitFAQ-Imadeamistakeinmylastcommitcomment.CanIeditit?)
-   [I made a mistake in my last commit. Can I correct and recommit it?](#GitFAQ-Imadeamistakeinmylastcommit.CanIcorrectandrecommitit?)
-   [I made a mistake in previous commit(s)/comment(s) but not the most recent one. What do I do?](#GitFAQ-Imadeamistakeinpreviouscommit(s)/comment(s)butnotthemostrecentone.WhatdoIdo?)
-   [I have submitted a Pull Request for my feature branch but it cannot be merged as it is showing a conflict.](#GitFAQ-IhavesubmittedaPullRequestformyfeaturebranchbutitcannotbemergedasitisshowingaconflict.)
-   [I want my branch to be based on the latest commit in develop / I want the changes that have been committed to my parent branch since my branch was created](#GitFAQ-Iwantmybranchtobebasedonthelatestcommitindevelop/Iwantthechangesthathavebeencommittedtomyparentbranchsincemybranchwascreated)
-   [I need changes that have been done in another branch ](#GitFAQ-Ineedchangesthathavebeendoneinanotherbranch)
-   [I'm trying to do something in git, but it's failing with errors like 'You have unstaged changes' or 'Your index contains uncommitted changes.'](#GitFAQ-I'mtryingtodosomethingingit,butit'sfailingwitherrorslike'Youhaveunstagedchanges'or'Yourindexcontainsuncommittedchanges.')
-   [I can't check out a repository, getting 'EOF/server closed connection unexpectedly' errors](#GitFAQ-Ican'tcheckoutarepository,getting'EOF/serverclosedconnectionunexpectedly'errors)
-   [Related articles](#GitFAQ-Relatedarticles)

## I branched from the wrong parent branch, what do I do?

There are 3 scenarios:

##### 1) You haven't committed yet.

Stash your changes, delete your branch, and create it from the right place.

##### 2) You have some commits, but haven't pushed them yet.

You'll need to Rebase - see [Rebasing](Git-Best-Practice_622704.html#GitBestPractice-rebasing).

##### 3) You have committed and have pushed.

It depends on whether others have based any work on your commits themselves. If they haven't you're in luck, you can just Rebase as in scenario 2 (perhaps following [this example of just such a situation](Rebasing_a_pushed_git_branch_from_its_original_starting_commit_onto_a_commit_on_another_branch)). If they have, you're going to have to get them to rebase their work too, and you aren't going to be popular.

## I made a mistake in my last commit comment. Can I edit it?

 Yes, as long as you haven't pushed. Just go to command line/terminal, and type:

    git commit --amend

You'll be presented with your configured editor containing the commit message from the last commit - simply modify it, save, and exit to update the commit.

If you've pushed, then there is no real easy option.

## I made a mistake in my last commit. Can I correct and recommit it?

The fix depends on whether you've pushed your commits or not.

##### If you haven't yet pushed:

Go to command line/terminal, and type:

    git reset HEAD^

This will remove the commit you did, but return you back to the state you were in before you committed - i.e. with the changes you mistakenly committed in your sandbox.

##### If you have pushed:

You have no real option but to do a revert commit, as others may have already used your commit to base their work on.

Go to command line/terminal, and type:

    git revert HEAD
    git commit

This will generate a reversing commit that will effectively return the repository to the state that it was in before you made the bad commit. The bad commit will still be in the history, but it will be nullified by the revert commit.

Don't forget to push your revert commit once you're happy with it, as others will still see the result of your bad one otherwise!

## I made a mistake in previous commit(s)/comment(s) but not the most recent one. What do I do?

First you need to find the commits that are wrong. Go to command line/terminal, and type:

    git log --oneline

This will display all of the recent commits in the current branch, latest first.

`6a3163f OK Commit.`
`fad7f06 Bad commit message.`
`8e8b15a Deleted wrong file.`
`c8d8c5f Previous commit (OK)`

##### If you haven't yet pushed:

As long as you haven't pushed you can do an **interactive rebase**. Find the hash of the first commit in the list after the earliest commit you want to modify - i.e. the parent commit of the first bad commit. So in the list above the first parent commit would be `c8d8a5f`.

To start the rebase progress, go to command line/terminal, and type:

    git rebase --interactive <hash-of-first-parent>

So in our example it would be

    git rebase --interactive c8d8c5f

This will launch your default editor with a list of all commits since the specified parent commit. Next to each commit will be a keyword, which by default is pick. Depending on what you want to do with each commit:

-   replace pick with reword to modify the commit comment
-   replace pick with edit to modify the actual commit

Then save and close the editor.

For each commit you want to *reword*, Git will bring up your editor with the existing commit comment. For each commit you want to *edit*, Git drops you into the shell. If you’re in the shell:

1.  Change the commit in any way you like.
2.  `git commit --amend`
3.  `git rebase --continue`

Most of this sequence will be explained to you by the output of the various commands as you go.

##### If you have pushed:

Tough luck on the bad commit messages - you can't rewrite history on pushed commits without causing potential issues for others.

However, you can revert specific commits. Based on our log from before:

`6a3163f OK Commit.`
`fad7f06 Bad commit message.`
`8e8b15a Deleted wrong file.`
`c8d8c5f Previous commit (OK)`

You can revert the commit where you deleted the wrong file by typing:

    git revert 8e8b15a
    git commit

This will generate a reversing commit that will undo the changes made in the bad commit. The bad commit will still be in the history, but it will be nullified by the revert commit. Make sure that you don't cause any other issues by doing this though - there may be changes elsewhere in the repository that relied on the change that you're reversing.

Don't forget to push your revert commit once you're happy with it, as others will still see the result of your bad one otherwise!

## I have submitted a Pull Request for my feature branch but it cannot be merged as it is showing a conflict.

To remedy this you need to merge the target branch in the Pull Request on to your feature branch, and resolve the conflicts. Checkout your feature branch, [Update it](Git_Best_Practice) to make sure you have all the commits from others locally, and then merge the target branch into your branch - see [Merging](Git_Best_Practice).

After you've done the merge and resolved all conflicts, push the feature branch back up so that the Pull Request can continue.

## I want my branch to be based on the latest commit in develop / I want the changes that have been committed to my parent branch since my branch was created

You simply need to merge your parent branch into your branch - see [Merging](Git-Best-Practice_622704.html#GitBestPractice-merging)

## I need changes that have been done in another branch 

You can merge the other branch into your branch - see [Merging](Git-Best-Practice_622704.html#GitBestPractice-merging).

However, bear in mind that this will drag in all the changes on that branch to your branch, so you may have to delay merging your branch back to develop until the other branch is merged.

## I'm trying to do something in git, but it's failing with errors like 'You have unstaged changes' or 'Your index contains uncommitted changes.'

Your current working copy isn't clean. You need to commit anything that is committable, or stash it if it is not yet ready for commit.

Sometimes IDEA cannot handle the auto-stashing or smart checkout for you. If this is the case:

1.  Use IDEA to stash anything you definitely want to keep. You can also revert anything you no longer want.
2.  Open a command line or terminal and do a `git status` to see what's going on.
3.  If you're sure that you don't want any of the unstaged changes you have locally, you can type '`git checkout .`' to remove them.

## I can't check out a repository, getting 'EOF/server closed connection unexpectedly' errors

This can be due to Anti-Virus packages incorrectly identifying patterns in the data coming down from Git. Try disabling your anti-virus or installing AVG which doesn't seem to have this problem.

## Related articles

-   Page:

    [Getting Started with Git](/display/RND/Getting+Started+with+Git)

-   Page:

    [Git FAQ](/display/RND/Git+FAQ)

-   Page:

    [Creating a new Git repository with the content of an existing branch](/display/RND/Creating+a+new+Git+repository+with+the+content+of+an+existing+branch)

-   Page:

    [Git Best Practice](/display/RND/Git+Best+Practice)

-   Page:

    [Rebasing a pushed git branch from its original starting commit onto a commit on another branch](/display/RND/Rebasing+a+pushed+git+branch+from+its+original+starting+commit+onto+a+commit+on+another+branch)

## Comments:

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>I can't edit this page :</p>
<p>I wanted to add that if you experience the following symptoms with git fetch</p>
<ul>
<li>Fetch operation seems to be taking too long (&gt;10 minutes)</li>
<li>Error messages such as unexpected eof in the stream</li>
</ul>
<p>Try disabling the network component of your antivirus as it might be modyfying the stream after picking up byte patterns that it considers suspicious.<br />
(Happened with BitDefender) </p>
<p> </p>
<p> </p>
<div class="smallfont" align="left" style="color: #666666; width: 98%; margin-bottom: 10px;">
<img src="assets/images/icons/contenttypes/comment_16.png" width="16" height="16" /> Posted by ppelegris at Jan 08, 2015 11:52
</div></td>
</tr>
</tbody>
</table>


