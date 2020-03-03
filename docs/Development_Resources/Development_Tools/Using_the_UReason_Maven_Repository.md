# Using the UReason Maven Repository

Follow this guide if you want to use the Maven repository manager available on host nexus.ureason.co.uk. This repository manager hosts UReason build artifacts and acts as a proxy for various upstream Maven repositories.

 Step-by-step guide

By default, Maven reads configuration settings from the file `~/.m2/settings.xml` (where "`~`" represents your home directory) so placing information about the UReason Maven repository server in this file is the easiest way to ensure its use.  If you're familiar with Maven configuration (perhaps simply through IntelliJ IDEA's support) then feel free to use another mechanism to supply the information to your Maven projects.  The following steps assume the default settings file location:

1.  If it doesn't already exist, create a folder named `.m2` in your home directory (on Windows you should use a command prompt to do this since Windows Explorer will refuse to create a folder with a name that starts with a "`.`")
2.  Download the attached `settings.xml` file and save it in the `.m2` folder in your home directory (you can attempt to merge the content into any existing settings file that you may already have but don't do so blindly)
3.  If you open or create a Maven project in IntelliJ IDEA you should find that **Local Paths** and **UReason Repository Server** appear as enabled profiles in the Maven toolwindow:
    ![](/assets/attachments/19333162/19464203.png){width="500" height="156"}
    The checkboxes are tri-state controls that cycle between:
    1.  the enabled state specified in `settings.xml` (in this case the profile is **activeByDefault**)
    2.  explicitly **true** as an IDEA override
    3.  explicitly **false** as an IDEA override.

## Profile Explanation

Maven [profiles](http://maven.apache.org/guides/introduction/introduction-to-profiles.html) are, among other things, an aid to build portability. The sample settings file attached to this page contains two profiles:

1.  **UReason Repository Server**– when active, this profile causes Maven to search for artifacts in the Maven repository on host nexus.ureason.co.uk. If this profile is disabled Maven will revert to accessing its default set of repository managers (this could be useful if there is no access to the UReason server – *e.g.* because of VPN problems when out of the office – and new 3rd-party dependencies are needed).
2.  **Local Paths** – this profile is intended to be used where installation-specific filenames or paths are used. Instead of hard-coding a value into a Maven POM a property is used, and the property's value is defined in the profile.  At present only the path to the Bower executable used in the WAM Reporting 2 project is defined by this profile.

## Attachments

## Attachments:

![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [settings.xml](/assets/attachments/19333162/19730614.xml) (text/xml)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [IDEA-Maven-Profiles.png](/assets/attachments/19333162/19731229.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [settings.xml](/assets/attachments/19333162/19464202.xml) (text/xml)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [IDEA-Maven-Profiles.png](/assets/attachments/19333162/19464203.png) (image/png)

