---
title: Building UReason Platform, Products and Add-ins in a Development Sandbox
parent: Building UReason Products
grand_parent: Development Tools
has_children: false
nav_order: 1
---

# Building UReason Platform, Products and Add-ins in a Development Sandbox

This document is incomplete and needs more detail.

# Introduction

[TeamCity](Hosted-Services_2981901.html#HostedServices-TeamCity) is used for official product builds and can also be used to create personal builds (although this feature may not be enabled right now), but sometimes it's convenient to be able to build a product in your sandbox.  TeamCity "merely" sets up an environment and runs a series of Ant scripts and it's straightforward to do the same from within IntelliJ IDEA.  This can be useful if you're making changes to the build scripts themselves since it allows you to test the changes without having to push them to Stash so that TeamCity can attempt a build on your feature branch.  Follow the steps below to configure IntelliJ IDEA to run the various build steps.

# Configuring Common Build Properties

The Java properties file `platform/build/ant/common/common.build.properties` defines various properties that are common to all builds.  Two of these properties have values that are appropriate for the TeamCity build agent location on the build server but must be edited to reflect your own environment:

1.  set **staging.home** to the absolute path of a directory in which build output (compiled classes, jar files *etc.*) should be placed;
2.  set **libs.home** to the absolute path of the *libs* module in a *urplatform* sandbox.

This screenshot shows suitable values for a Mac where *urplatform* has been cloned into `/Users/mscott/Development/UReason/urplatform` :

![](/assets/attachments/4359173/4689075.png?effects=drop-shadow){.image-center}

The comment at the top of the file refers to CVS while we now use git for version control. The comment about this location supporting builds of multiple branches remains valid.

# Building the 'Product Base'

'Product Base' is a term used to describe a set of build artifacts comprising the core platform modules (*platform*, *eventbrowser*, *vismod* *etc.*) plus various addins (*RDBMS*, *UCSV*, *UScript etc.*) that form the foundation for the various UReason products.  Let's say some changes have been made to the core *platform* module and these changes are needed in new builds of OASYS-AM and USE : the product base would be built first and then both OASYS-AM and USE could be built.

The product base is built using the Ant script found at `platform/build/ant/common/build-product-base.xml`.  All IntelliJ IDEA project files under version control should have this file identified as an Ant build script and this can be checked in the Ant toolwindow :

![](/assets/attachments/4359173/4689076.png?effects=drop-shadow){.image-center}

If the script does *not* appear in the list in the Ant toolwindow then locate it in the Project toolwindow or open it in the editor and choose *Add as Ant Build File* from the right-click context menu.

The properties of the build script must be set to suitable values.  This has already been done on the main *develop* branch but may need to be done if you're working on another branch or have branched off *develop *prior to the change being made on that branch.

On the *Properties* tab ensure that the three properties shown below are set.  When building from TeamCity the **repository.branch.name** property is set to the name of the git branch on which the build is being made.  In the days when we used CVS this property was set by running a custom Ant task that examined the CVS metadata but since TeamCity 'knows' the name of the git branch it hasn't been necessary to create a corresponding custom Ant task for a git repository.  This could be done but at the moment local builds must set this value.  The value you choose is completely arbitrary – it will simply become the name of a directory created in the directory named by **staging.home** in `common.build.properties`.

![](/assets/attachments/4359173/4689100.png?effects=drop-shadow){.image-center}

On the *Additional Classpath* tab ensure that the pre-built jar file containing UReason custom Ant tasks is listed.  Use the *Add* button to add the entry if not.

![](/assets/attachments/4359173/4689101.png?effects=drop-shadow){.image-center}

If you had to add any of this configuration then your IntelliJ IDEA project file (the *.ipr* file if your project uses that project file format) will be locally modified but it should be acceptable to commit the changes so that others can use them.

With this configuration in place it should be possible to launch the Ant target within `build-product-base.xml` to build the product base.  Do this from the Ant toolwindow by double-clicking on the base-product-and-base-addins node or by right-clicking and selecting *Run Target* from the context menu.

![](/assets/attachments/4359173/4689102.png?effects=drop-shadow){.image-center}

The progress of the build will be reported in the Messages toolwindow.  The process may well finish with errors but they should be benign and caused by Ant being verbose about things like directories not existing when deletion tasks were run.  Such errors are not reported on the second and subsequent builds for any given value of **repository.branch.name** and, as a guide, the error count should be 2 from the second build onwards.

The build scripts on *develop* have been updated at various points and it may be that additional steps are necessary on branches that don't have the changes that are present on *develop*. Errors that may occur and make such additional steps necessary are:

-   a Java compilation error caused by a 'licman' class not being found. This is the result of a dependency on the 'old' licence manager web application that is no longer in place on the *develop* branch;
-   failure (during a product rather than product *base* build) to copy and unpack a product-specific documentation bundle.

These errors can be remedied by copying the content from the Windows share `\\Watson\staging\master` (*i.e.* the two directories `licman` and `documentation`) into the directory identified by **${staging.home}/${repository.branch.name}** *e.g.* to use the configuration settings from this document this would create `/Users/mscott/Development/Staging/UReason/LOCAL-BUILD/licman` and `/Users/mscott/Development/Staging/UReason/LOCAL-BUILD/documentation`.

## Attachments:

![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [common.build.properties.png](/assets/attachments/4359173/4689075.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [Ant-build-product-base.png](/assets/attachments/4359173/4689076.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [Build-File-Properties.png](/assets/attachments/4359173/4689100.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [Build-File-Properties2.png](/assets/attachments/4359173/4689101.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [run-base-product-and-base-addins.png](/assets/attachments/4359173/4689102.png) (image/png)

