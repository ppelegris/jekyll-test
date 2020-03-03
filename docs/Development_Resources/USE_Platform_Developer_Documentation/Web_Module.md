---
title: Web Module
has_children: true
parent: USE Platform Developer Documentation
grand_parent: Development Resources
nav_order: 1
---

# Web Module

## Overview

The Web module provides web browser access to various types of solution components by embedding a servlet container within a host product. 

## Dependencies & Environment

This module depends upon:

-   the modules platform, libs, eventbrowser, graph, icons, jhelp, udsr, variable

Notable dependencies within the `libs` module are Jetty, ZK, Struts and DWR.

Building

The Web Extension module's artifact is built using its Ant build script, located at:

```
./build/ant/AntWebWebappBuild.xml
```

(but note that the script builds a complete product add-in not just the web application component implied by the script name).

Design & Implementation

The Web Extension is implemented as a conventional product add-in that, on initialization during application startup, programmatically configures and starts the [Jetty](http://www.eclipse.org/jetty/) servlet container in the host product's process. A web application archive (war) file containing static resources (such as HTML and JavaScript files) is deployed but, unlike conventional war files, the Web Extension war file contains no Java class files; instead, the host product's classloader and classpath are used for classloading.

The Web Extension was originally based entirely on the Jakarta Struts presentation framework from the Apache project, and [Struts 1.1](http://struts.apache.org/development/1.x/) remains in use today. Struts is based on the Model-View-Controller design paradigm, a fact reflected in the package names and classes that comprise much of the Web Extension. Transfer of data from the web layer to the server layer uses Struts `ActionForm` subclasses, business logic is implemented in Struts `Action` subclasses, and views are implemented in a mixture of static HTML files, JSPs and `Servlet` subclasses (with the great majority of views being servlet-based).

Recent updates and additions to the Web Extension have been implemented using the [ZK](http://www.zkoss.org/) framework. ZK allows rich, event-driven applications to be developed more or less exclusively in Java, and adopting it has effectively removed the need for hand-crafted HTML and JavaScript (or the use of multiple 3rd-party libraries providing equivalent features). ZK allows the user interface to be defined in XML or Java and the latter approach has been taken for the Web Extension.

Important Directories and Packages

directory 'web'this is effectively the document root of the web application component of the Web [Extension.com](http://Extension.com).\_ureason.web.controllercontains packages and classes described by the 'controller' part of the Struts MVC paradigm; sub-packages contain classes that control the processing of user input, the execution of business logic and the return of an appropriate view to the [user.com](http://user.com).\_ureason.web.modelcontains packages and classes described by the 'model' part of the Struts MVC [paradigm.com](http://paradigm.com).\_ureason.web.servercontains classes that configure the embedded Jetty servlet container and override some of its default [behaviour.com](http://behaviour.com).\_ureason.web.viewcontains packages and classes described by the 'view' part of the Struts MVC paradigm. More recent classes added here have been based on ZK.

Important Files and Classes

web/WEB-INF/web.xmlthe deployment descriptor for the Web Extension web application componentweb/WEB-INF/struts-config.xmlthe configuration file for the Struts frameworkweb/WEB-INF/zk.xmlthe configuration file for the ZK frameworkcom.\_ureason.web.WebRegistration and com.\_ureason.web.auth.WebRoleInstaller'bootstrap' classes that register various aspects of the Web Extension with the [system.com](http://system.com).\_ureason.web.server.EmbeddedJettya class that configures the embedded Jetty servlet container.

Licensing

Struts 1 and Jetty 6 are subject to the Apache License 2.0

ZK Community Edition is subject to the LGPL

Notes

The Web Extension can be run in a development environment without building a war file. To do so, define these system properties:

-   `-Dcom._ureason.platform.web.server.EmbeddedServletContainer=com._ureason.web.server.EmbeddedJetty`
-   `-Dcom._ureason.platform.web.server.context=/`
-   `-Dcom._ureason.platform.web.server.docbase=../web/web`

Further configuration options exist, and the path in the 3rd property is to the 'web' directory within the 'web' module itself, relative to the working directory.
