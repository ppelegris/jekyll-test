---
title: Getting_Started_with_Git
parent: Software Configuration Management
grand_parent: Development Resources
nav_order: 2
---

# Getting Started with Git

#### In order to develop code based on the UReason Platform you'll need to be working with a Git repository.

### Installing Git

First you'll need to install Git. The following instructions are for installing git on Windows-based machines.

##### Download Git installer

The installer can be downloaded from <http://git-scm.com/downloads>.

##### Install Git

It is recommended that you install Git with the default components selected.

On the PATH environment screen, ensure you have the second option 'Run Git from the Windows Command Prompt' selected:

This will mean you have command line access if you need it.

On the line-ending screen, retain the default first selection:

Allow the installation to complete, and Git should now be installed - you can test this by typing **git -- version** in a command prompt.

### Configuring Git

There's a few configuration steps you'll want to do before you start using Git proper.

First tell it who you are. Enter the following commands:

    git config --global user.name "<USERNAME>"
    git config --global user.email <USERNAME>@ureason.com

Next you'll want to configure yourself a decent text editor for working with commit comments and the like from the command line. For instance, to configure Notepad++ in Windows as your editor, enter the following:

    git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin" 

If you're using Mac or Linux, you can choose one of the built in terminal based editors accordingly.

### Cloning the Repository

Next you'll need to 'clone' the repository to your local system.

##### Getting the repository URL

The repository is hosted via the Atlassian Stash server at [http://stash.ureason.co.uk](http://stash.ureason.co.uk/). Find the urplatform repository in the UReason Platform and Products project - a direct link is here : <http://stash.ureason.co.uk/projects/URP/repos/urplatform/browse>

In the top right of the page<sup>1</sup> you will find the Clone button; click this and copy the URL given in the box that pops up. This is your Repository URL.

 

|                                                       |
|-------------------------------------------------------|
| <sup>1</sup>or possibly down the left under *Actions* |

 

##### Performing the Clone

The easiest way to do this is to use IntelliJ IDEA's 'Checkout from Version Control' functionality. You can either do this from the initial IDEA Welcome page or from within IDEA via the VCS menu.

Select 'Checkout from Version Control' and select the Git option. Paste the URL you got in the previous step into the first box, and select the Parent Directory to clone into in the second. Note that a directory will be created in the Parent Directory which will hold the contents of the repos - the name of which is specified by the third box. Unless you'll have multiple local repositories you can leave this as 'urplatform'' which is the default.

Note : If you intend to have multiple local repositories - for instance for different branch sets - resist the temptation to name them after specific branches e.g. 'master'. A given repos will be switched to different branches often and so you can easily confuse matters. It's generally best to give the local repos dir a logical name based on the function of the repos - e.g. use a product name. 

Click OK and the repos will now clone.

Note: if IntelliJ IDEA asks you if you would like to create a project for the freshly cloned repository, choose No. The project files are in git and can be opened as an existing project.
