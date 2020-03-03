# Creating Installer Resources and Configuration Files for a new UReason Product

# Introduction

UReason products (**OASYS-AM**, **U.S.E.** *etc.*) are delivered to end users as Windows installers built using InstallAnywhere.  It's possible to use InstallAnywhere to produce installers for other operating systems but this is not something we currently do.  Production of an installer involves running an InstallAnywhere build tool over a set of input files and processing them according to an InstallAnywhere "project file".  We've modified the process slightly by creating a "project file template" that can be turned into a product-specific project file by replacing placeholder values with product-specific values.  The template contains placeholders for product resource strings, internal IDs, version numbers and so on.  Each UReason product's source module contains various configuration files that are used when generating the product-specific project file from the template.  This document describes the process of creating these resources for a new UReason product (in this case **WAM Reporting 2**).

Flexera Warning Regarding Project File Editing

The template project file has remained essentially unchanged for many years, with most changes being caused by upgrades from one version of InstallAnywhere to another. The file was created at a time when InstallAnywhere offered little flexibility like that which the template file provides. Today, however, that seems to have changed and a recent issue raised with Flexera Customer Support elicited the warning that Flexera advises *"strongly against ever directly editing a project file."* because of its *"volatility"*. Instead, build-time variables and source paths can now be used to achieve the same effect as our template project file. The use of these officially supported features should be considered for any new installers that we create.

# The Process

## Generate product-specific identifiers

InstallAnywhere uses GUID-style identifiers for various purposes.  The identifiers comprise 32 hexadecimal characters in hyphen-separated groups : *&lt;8 chars&gt;-&lt;4 chars&gt;-&lt;4 chars&gt;-&lt;4 chars&gt;-&lt;12 chars&gt;*.  The UReason template project file contains placeholders for three such product-specific IDs: one for the product itself, one for the product component (an InstallAnywhere concept) and one for the product uninstaller.  InstallAnywhere records these IDs in its installation registry on each machine on which an InstallAnywhere installer is run and uses the information for such things as detecting previous versions of a product, preventing duplicate installation *etc.*).

The three IDs will be stored in a Java properties file using the keys `product.installer.product.id`, `product.installer.component.id` and `product.installer.uninstaller.id`.  Any unique values are acceptable but the following convention has been used for existing UReason products:

For `product.installer.product.id` : the first 16 characters (excluding hyphens) are the first 16 characters of the md5 hash of "**UReason product**" (without the quote characters); the second 16 characters (excluding hyphens) are the md5 hash of the product name.

For `product.installer.component.id` : the first 16 characters (excluding hyphens) are the first 16 characters of the md5 hash of "**UReason product**" (without the quote characters); the second 16 characters (excluding hyphens) are the md5 hash of the product name suffixed with " **default component**" (without the quote characters).

For `product.installer.uninstaller.id` : the first 16 characters (excluding hyphens) are the first 16 characters of the md5 hash of "**UReason product**" (without the quote characters); the second 16 characters (excluding hyphens) are the md5 hash of the product name suffixed with "** uninstaller**" (without the quote characters).

Generating md5 hash values

If you can generate md5 hash values in an OS shell you can do so using an approach such as this:

`$ echo -n WAM Reporting 2 default component | md5`

3000eb185b079f52d2e0c1db88c21938

Alternativey, a website such as Miracle Salad can be used.

 

Using a new product name of **WAM Reporting 2** we have:

| String                            | md5 hash                           | First 16 characters of md5 hash |
|-----------------------------------|------------------------------------|---------------------------------|
| UReason Product                   | `c9a200f6d265cd4298b2c3c77e9073d1` | `c9a200f6d265cd42`              |
| WAM Reporting 2                   | `685bf7d6d85b9f7c7bb193b934d022be` | `685bf7d6d85b9f7c`              |
| WAM Reporting 2 default component | `3000eb185b079f52d2e0c1db88c21938` | `3000eb185b079f52`              |
| WAM Reporting 2 uninstaller       | `b510dfc56f3ad270a917cb9406092674` | `b510dfc56f3ad270`              |

We use the values from the table above to generate the table below:

| Property key                       | First 16 characters | Second 16 characters | Unformatted ID                     | Formatted ID                           |
|------------------------------------|---------------------|----------------------|------------------------------------|----------------------------------------|
| `product.installer.product.id`     | `c9a200f6d265cd42`  | `685bf7d6d85b9f7c`   | `c9a200f6d265cd42685bf7d6d85b9f7c` | `c9a200f6-d265-cd42-685b-f7d6d85b9f7c` |
| `product.installer.component.id`   | `c9a200f6d265cd42`  | `3000eb185b079f52`   | `c9a200f6d265cd423000eb185b079f52` | `c9a200f6-d265-cd423-000e-b185b079f52` |
| `product.installer.uninstaller.id` | `c9a200f6d265cd42`  | `b510dfc56f3ad270`   | `c9a200f6d265cd42b510dfc56f3ad270` | `c9a200f6-d265-cd42-b510-dfc56f3ad270` |

## Run InstallAnywhere by opening the *NewUReasonProductNameProject* file

InstallAnywhere is installed on Watson, the build server.  The InstallAnywhere licence key is associated with the *teamcity* user account so the process described here should be performed after logging in as that user (the password is stored in the UReason password store).  Running the InstallAnywhere project editor requires privilege escalation so you must be able to provide the password of an administrator account.

An InstallAnywhere project named *NewUReasonProductNameProject* has been created as a convenience.  It can used each time a new product is created.  Run InstallAnywhere by, for instance, double-clicking on the project file in Windows Explorer:

![](/assets/attachments/9143998/9307947.png){.image-center width="1104" height="543"}

## Edit the *Product Code*

Select *Project* then* General Settings* in the user interface and enter the value of `product.installer.product.id` in the *Product Code* field of the *Product Information* section:

![](/assets/attachments/9143998/9307948.png){.image-center}

## Enter the passphrase for the UReason code-signing certificate

The installers we build are digitally signed so that end users installing our software can be sure that they're running a genuine UReason product installer.  The signing process uses the UReason code-signing certificate and requires that the passphrase that protects the certificate is available to InstallAnywhere.  InstallAnywhere doesn't save the passphrase in plain text in the project file, instead encoding it in a way that prevents anyone with access to the project file from discovering the passphrase value.  An input to the encoding process is the product ID entered in the previous step (thus the encoded passphrase is product-specific).  Select *Project* then *Platforms* in the user interface and enter the plain-text code-signing certificate passphrase in the *Password* field of the *Windows* | *Digital Signing* node (obscured in this image):

![](/assets/attachments/9143998/9307949.png){.image-center}

The code-signing certificate passphrase can be obtained from the UReason password store.

## Save the InstallAnywhere project file

Saving the project file causes (among other things) the plain-text code-siging certificate passphrase entered in the previous step to be saved in its product-specific, encoded form.  The project can be saved from the *File* menu - there's no need to save under a different name.  Close the InstallAnywhere editor.

## Copy the encoded code-signing certificate passphrase

Since the InstallAnywhere editor UI doesn't show the encoded, product-specific code-signing certificate passphrase we must obtain it by other means.  The project file contains XML-formatted plain text so can be opened in any text editor.  Notepad++ is installed on Watson so can be used (select *Edit with Notepad++* from the project file's context menu in Windows Explorer).  Search for the string *codesign* to find the XML *&lt;property name="winCodeSign"&gt;* element:

![](/assets/attachments/9143998/9307952.png){.image-center}

Copy the value of the CDATA within the nested *&lt;property name="password"&gt;* element (this is the product-specific encoding of the code-signing certificate passphrase):

![](/assets/attachments/9143998/9307953.png){.image-center}

## Create a Java properties file containing installer placeholder property keys and values

The product-specific installer placeholder values are stored in a Java properties file named installer.properties in the product's module in the source tree.  The location is defined by convention and reflects the fact that for each UReason product there is typically more than one "incarnation" (for instance, OASYS-AM has "incarnations" named *engineering*, *offline* and *online*; U.S.E. has "incarnations" named *professional* and *standard*).  The generic product build scripts work because the location of source code and metadata files for each product incarnation can be found from the (normalised) product and incarnation names.

Taking **U.S.E. Professional** as an example, the file of placeholder values for the installer can be found at `${sandbox}/use/build/installer/professional/installer.properties`

Generalising this path we have `${sandbox}/${product}/build/installer/${incarnation}/installer.properties`

**Reporting** and **Engineering** were originally incarnations of a single product named **WAM** but this was changed and there's now a **WAM** product with an **Engineering** incarnation and a **WAM Reporting** product with a **Reporting** incarnation.  Having the concept of an incarnation when there's only one is redundant but is necessary to use the generic product build scripts.  Consequently, for our new **WAM Reporting 2** product we must follow the pattern and create an incarnation - we can call it **Reporting** (for consistency if nothing else).  The path to installer.properties is therefore: `${sandbox}/wamreporting2/build/installer/reporting/installer.properties`

The complete properties file is shown here:

The first 8 values are used for presentation strings in the installer and for defining installation path prefixes.  The remaining entries are as described above or control other aspects of the installation (*e.g.* whether or not the installer will offer to install the product as a Windows service).

## Attachments:

![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [NewProduct-Installer-Step-1.png](/assets/attachments/9143998/9307947.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [NewProduct-Installer-Step-2.png](/assets/attachments/9143998/9307948.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [NewProduct-Installer-Step-3.png](/assets/attachments/9143998/9307949.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [NewProduct-Installer-Step-4.png](/assets/attachments/9143998/9307952.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [NewProduct-Installer-Step-5.png](/assets/attachments/9143998/9307953.png) (image/png)
![](assets/images/icons/bullet_blue.gif){width="8" height="8"} [NewProduct-installer-Step-6.png](/assets/attachments/9143998/9307954.png) (image/png)

