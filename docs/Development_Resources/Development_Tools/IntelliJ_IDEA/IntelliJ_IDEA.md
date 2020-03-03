# IntelliJ IDEA

IntelliJ IDEA is our common Java IDE used in UReason. This page contains information about how to use IDEA effectively and suggestions for settings and configuration options that help you produce clean, compatible code and files.

------------------------------------------------------------------------

## Editing Java Properties Files

`java.util.Properties.load(InputStream)` uses the ISO-8859-1 character encoding to interpret the content of files.  The locale-specific property bundles represented by `java.util.ResourceBundle` and related classes (used by `com.ureason.util.TextResource` *et al*) use `Properties.load(InputStream)` when loading locale-specific property resource files.  Consequently, care must be taken to ensure that properties files containing resource bundle strings *are* saved using the ISO-8859-1 character encoding.  In IntelliJ IDEA this is a simple matter of configuring two project settings – the encoding to use for properties files and the use of transparent native-to-ascii conversion on load/save :

![](/assets/attachments/1114312/1671189.png)

With these settings, locale-specific strings should appear correctly during development and in deployment regardless of operating system.

------------------------------------------------------------------------

## Adding the UReason Plugin Repository

IntelliJ IDEA ships with the ability to install plugins from the official JetBrains plugin repository.  It's also possible to add other repositories *e.g.* so that organisations can distribute their own IntelliJ IDEA plugins.  UReason has just such a plugin repository available at the url <http://plugins.ureason.co.uk> (**plugins.ureason.co.uk** is a virtual hostame that currently resolves to **holly.ureason.co.uk**).

The screenshots below show how to configure IntelliJ IDEA 14.1 to use the UReason plugin repository.  The steps are likely to be similar for other versions.

#### Configure plugins:

![](/assets/attachments/1114312/9308601.png)

 

#### Browse plugin repositories:

![](/assets/attachments/1114312/9308603.png)

 

#### Manage plugin repositories:

![](/assets/attachments/1114312/9308604.png)

 

#### Add a new plugin repository entry:

![](/assets/attachments/1114312/9308605.png)

 

#### Enter the plugin repository URL and check its availability:

![](/assets/attachments/1114312/9308606.png)

 

#### Commit all changes with the OK buttons:

![](/assets/attachments/1114312/9308607.png)

 

#### Verify the new plugin repository is available for browsing:

![](/assets/attachments/1114312/9308610.png)

## Attachments:

![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [IDEA-FileEncodingsConfiguration.png](/assets/attachments/1114312/1671189.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [IDEA-add-plugin-repository-step-1.png](/assets/attachments/1114312/9308602.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [IDEA-add-plugin-repository-step-1.png](/assets/attachments/1114312/9308601.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [IDEA-add-plugin-repository-step-2.png](/assets/attachments/1114312/9308603.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [IDEA-add-plugin-repository-step-3.png](/assets/attachments/1114312/9308604.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [IDEA-add-plugin-repository-step-4.png](/assets/attachments/1114312/9308605.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [IDEA-add-plugin-repository-step-5.png](/assets/attachments/1114312/9308606.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [IDEA-add-plugin-repository-step-6.png](/assets/attachments/1114312/9308607.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [IDEA-add-plugin-repository-step-7.png](/assets/attachments/1114312/9308610.png) (image/png)

