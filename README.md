Jekyll Notes
============


##Main config via _config.yml

You can set default in main config

*The file front matter takes precendence over the file !!!*

defaults:
  -
    scope:
      path: "" # empty string means all files in path
    values:
      layout: "default"
      author: "UReason"


##Front Matter
---
layout: <points to layout dir>
permalink: <controls url> 
published: <set to false to prevent is from publishing>
    * you can run jekyll with --unpunlished to view unpublished pages
<var>: <value>
    * set a variable e.g. project: focus - use => {{page.focus}}
<description>: <text>

---

| Attribute | Description                                                  |
| --------- | ------------------------------------------------------------ |
| Name      | The Name of the Property                                     |
| Type      | The value type of the property. When edited provides a dropdown allowing you to select string, integer, number or boolean. |
| Default   | The default value of the Property. This is the value assigned to the property when a new instance is created. |
| T         | T is short for Transient. Transient properties are typically values that do not need to be persisted with the project/application definition files. In the example above the Speed value, provided by your data source, will change over time so it does not need to be stored in the definition files. Note that non-transient properties are written to disk when they change, so this may cause unnecessary load to your system. Only use non-transient properties for static values that need to be persisted when APM is reloaded. |
| H         | H is short for History. When the H value is set to True the system will retain History on the property value for instances for the number of History Point configured in HPoints. History is required for example when you want to display graphs of values. |
| HPoints   | The number of HistoryPoints to store in memory when H is set to True. |
