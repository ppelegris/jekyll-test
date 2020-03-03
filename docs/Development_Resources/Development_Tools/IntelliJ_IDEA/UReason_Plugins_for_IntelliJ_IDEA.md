# UReason Plugins for IntelliJ IDEA

## UReason Add-in Support

This page is incomplete...

Add-in status

This add-in is a (languishing...) work in progress but is useful in its current state. Two manual steps remain necessary as explained on this page.

The *UReason Add-in Support* plugin automates the creation of the boilerplate needed for new add-ins based on UReason Platform.  The plugin can be downloaded and installed from within IntelliJ IDEA by [configuring access to the UReason plugin repository](IntelliJ_IDEA).  With the UReason plugin repository available, the plugin may be selected and installed:

![](/assets/attachments/9145948/9308614.png)

 

Installing a plugin requires IntelliJ IDEA to be restarted.  After restarting, the new plugin is available for use:

![](/assets/attachments/9145948/9308615.png)

 

Follow these steps to use the plugin (*Todo* - expand this section with more text and screenshots):

1.  install the plugin;
2.  open a project to which you want to add a UReason add-in;
3.  open the Project Structure dialog and add a new module;
4.  select “UReason Add-in” from the list of module types;
5.  click Next to move to the last page of the wizard;
6.  enter the module name and the dir in which you want to place it (you probably want to select a sibling directory of oasys, web etc. - be careful as I think IDEA will offer a location that has ‘use’ or ‘etcudl’ on the end *i.e.* the location of your current .ipr file);
7.  replace the key ‘MY\_ADDIN\_DOMAIN’ with your own key (should be unique among all add-ins), enter a display name and a category (or leave ‘Datasources’ in place for now);
8.  press Finish to exit the wizard and create the module.

 

## Attachments:

![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [IDEA-add-ureason-addin-support-plugin-1.png](/assets/attachments/9145948/9308614.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [IDEA-add-ureason-addin-support-plugin-2.png](/assets/attachments/9145948/9308615.png) (image/png)

