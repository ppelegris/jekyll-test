---
title: ZK Configuration
parent: USE Platform Developer Documentation
grand_parent: Development Resources
nav_order: 1
---
# ZK Configuration

# Introduction

This page describes how [ZK](http://www.zkoss.org/) configuration is performed for Platform-based UReason products.  It's essentially a description of how we mould the way ZK processes configuration files to suit our goal of having fine-grained, per-component/per-add-in ZK configurations to avoid having to ship the Webserver Extension Reporting Add-in with every UReason product that provide web-accessible content.  The information presented is correct as of ZK version 7.0.

# Motivation

AlarmExpert has hitherto required the Webserver Extension Reporting Add-in to be present.  This doesn't make sense.

ZK was originally configured for use by UReason more or less in the "conventional" way by a single `zk.xml` file provided by the Webserver Extension in the **web** add-in.  An override file with reporting-specific configurations was provided by the **webreporting** add-in, to be used in place of the **web** module's configuration file when a product with reporting capabilities was built.  A customised `/WEB-INF` resource lookup figured out which `zk.xml` file to use at runtime based on the presence or absence of the Webserver Extension Reporting Add-in (this customised lookup has been rendered redundant and has been removed).  This two-configuration-file approach soon became unwieldy and the distinction between the two configuration files was soon lost.  The problem of having one (or two) main configuration file(s) was compounded when other modules (*e.g.* the **alarmkb** and **mobile** add-ins) that generated web-accessible content were developed: their web-related (including ZK) configuration had to be added to the **web** and/or** webreporting** module(s) - in practice this resulted in all components with web-accessible content requiring the Webserver Extension Reporting Add-in to be present.  When the Web Extension was opened up to allow other core platform components and add-ins to contribute web-accessible content one problem to solve was how these new components could keep ownership of their individual ZK configurations and provide them to ZK at runtime.

# ZK Configuration Processing

Options for providing finer-grained ZK configuration files depend on how ZK processes configuration files.  The class `org.zkoss.zk.ui.http.WebManager` is responsible for generating a single `org.zkoss.zk.ui.util.Configuration` object that is used to configure ZK during its startup.  The WebManager constructor uses a `org.zkoss.zk.ui.sys.ConfigParser` to achieve this by parsing various configuration files:

1.  all configuration files found at `/metainfo/zk/config.xml` on the classpath are processed (note that multiple resources at the same classpath location can exist and be found and processed); these `config.xml` files seem to be used by ZK itself to declare the functionality provided by each ZK component and the dependencies between them, and often also to define default values for various system properties).
2.  all configuration files found at `/metainfo/zk/zk.xml` on the classpath are processed; these `zk.xml` files seem to contain configurations similar to those in our own `zk.xml` file.
3.  the "conventional", user-provided `/WEB-INF/zk.xml` file is processed (this is the monolithic file that we'd like to split into finer-grained, component- and add-in-specific chunks).
4.  the configuration file (if any) identified by the system property `org.zkoss.zk.config.path` is processed.
5.  the single `Configuration` object generated from the aggregate of all configuration files is used to configure ZK.

The same parser is used for all 4 steps so although no explicit validation against any XML schema is performed it seems reasonable to assume that any configuration file can contain any XML elements that are recognised as ZK configuration elements.  Providing a configuration file at `/metainfo/zk/zk.xml` seemed like a good way for a UReason component or add-in to provide its own ZK configuration data and this is the case in practice, with this approach now used by the **alarmkb** add-in to configure a ZK richlet and a CSS file that should be applied to AlarmExpert ZK desktops.

The `zk.xml` file provided by the **web** module remains the "main" ZK configuration file processed at step 3.  This file configures ZK in those cases where the **webreporting** module is *not* present.

The `zk-webreporting.xml` file provided by the **webreporting** module is specified by setting the `org.zkoss.zk.config.path` system property in `com._ureason.webreporting.auth.WebReportingRoleInstaller`.  This file configures ZK in those cases where the **webreporting** module *is* present.  The name `zk-webreporting.xml` is used to reinforce that this is a configuration file related to the **webreporting** module (but also to avoid having to perform "weird" `/WEB-INF` resource lookup).  The purpose of this file is mainly to override the ZK richlet associated with the identifier **Administration** to change the view shown on the **Admin** tab when the **webreporting** add-in is present.  The override works because the configuration file from the **web** module is processed *after* the configuration file from the **webreporting** module.

# Summary

If you're providing an add-in or core platform component that provides some standalone ZK-based functionality then it can be configured in a `/metainfo/zk/zk.xml` file (see the **alarmkb** add-in for an example).  If we ever need to override anything in the **web** module's zk.xml or the **webreporting** module's zk-webreporting.xml** **configuration file *from some other module* then we'll either have to handle the merging of configuration files ourselves before passing them to ZK or modify ZK itself to allow more than one override file.  On the face if it the second approach looks feasible by simply allowing the `org.zkoss.zk.config.path` system property to specify a path-like structure where each component is a single configuration file *e.g.* `org.zkoss.config.path=/foo/zk.xml:/bar/zk.xml:/baz/zk.xml`.  I wonder if that was the original intention of the property (given its name) and came unstuck because the configuration file identifiers can be URIs (which contain : characters, thus complicating the parsing).
