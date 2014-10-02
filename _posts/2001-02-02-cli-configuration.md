---
layout: help
title:  "Configuring the Client"
categories:
- help
- cli
---

{{ site.cli_client_name }} requires User Configuration and Project-Version Configuration.

## User Configuration

User configuration stores your credentials so that {{ site.cli_client_name }} can prove to the server that requests are from you rather than an imposter. The information in your user config should be kept secret.

{{ site.cli_client_name }} expects to find user configuration in `.config/zanata.ini` within your user directory.

To add configuration for a Zanata server:

 1. Use your favourite text editor to create or open `zanata.ini` in `~/.config/`.
 1. Sign into the Zanata server and navigate to the user settings page
 1. Ensure that an API Key is shown. If you do not have an API Key, click 'Generate API Key' now.
![User settings page]({{ site.url }}/images/302-user-settings.png)

 1. Copy the contents of the text-box labeled 'Configuration [zanata.ini]'.
 1. Paste the copied lines into `zanata.ini` and save the file.


## Project-Version Configuration

Project configuration stores information about a project-version, and should be kept in the project directory.

{{ site.cli_client_name }} expects to find project-version configuration in a file named `zanata.xml` in the project directory.

To add project-version configuration to your project directory:

 1. Sign into the Zanata server and navigate to the appropriate version of your project.
 1. Click the `Download config file` link to initiate download of `zanata.xml`.
![Download config file link on version page]({{ site.url }}/images/350-version-config-file.png)

 1. Save `zanata.xml` in your project directory.


These steps should be repeated for each project-version before using any {{ site.cli_client_name }} commands for the project-version.

You can customize `zanata.xml` with command hooks so that other tools will automatically run before or after Zanata commands. Read about command hooks at the [command hook page on the wiki](https://github.com/zanata/zanata-server/wiki/Client-Command-Hooks).

### Translation files mapping rules

You can also customize the way translation files are found during push, as well as the location they will be saved after pull.
{% highlight xml %}
<!-- example rules definition in zanata.xml -->
<rules>
  <rule pattern="**/pot/*.pot">{locale}/{path}/{filename}.po</rule>
  <rule pattern="**/po/*.pot">{path}/{locale_with_underscore}.po</rule>
</rules>
{% endhighlight %}

"pattern" attribute is a [glob](http://en.wikipedia.org/wiki/Glob_(programming)) matching pattern to your source document file(s). You can define more than one rules and apply each rule to specific set of source documents using different patterns. The **first** matched rule will be applied to the file. 

"pattern" is optional. If not specified, the rule will be applied to all source documents in your project.
The actual rule consists of literal path and placeholders/variables.

Supported placeholders/variables are:
 
 1. **{path}** is the path between source document root (what you define as src-dir option) and the final file.
 1. **{filename}** the source document name without leading path and extension.
 1. **{locale}** the locale for the translation file. If you use "map-from" argument in your locale mapping, this will be the map-from value.
 1. **{locale\_with\_underscore}** same as above except all hyphen '-' will be replaced to underscore '_'. This is typically used in properties and gettext projects.
 1. **{extension}** the source document file extension

> For example, given a source document is found at `/home/user/myproject/src/main/resource/message.properties` and your source document root (src-dir option) is set to ".". Your zanata.xml is located at `/home/user/myproject`. For a locale mapping defined as `<locale map-from="zh-CN">zh</locale>`: The {path} variable will become `src/main/resource`. {filename} is `message`. {locale} is `zh-CN` . {locale\_with\_underscore} is `zh_CN`. {extension} is `properties`.

The mapping rules configuration is optional in zanata.xml. If not specified, standard rules are applied to your [project type](https://github.com/zanata/zanata-server/wiki/Project-Types).

 1. gettext: {path}/{locale\_with\_underscore}.po
 1. podir: {locale}/{path}/{filename}.po
 1. properties: {path}/{filename}\_{locale\_with\_underscore}.{extension}
 1. utf8properties: {path}/{filename}\_{locale\_with\_underscore}.{extension}
 1. xliff: {path}/{filename}\_{locale\_with\_underscore}.{extension}
 1. xml: {path}/{filename}\_{locale\_with\_underscore}.{extension}
 1. file: {locale}/{path}/{filename}.{extension}   

---

[Old instructions](https://github.com/zanata/zanata-server/wiki/Client-Configuration)
