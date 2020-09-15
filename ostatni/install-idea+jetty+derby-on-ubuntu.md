---
description: >-
  This page describes how to install IntelliJ Idea, Apache Derby DB and Jetty
  application server on Ubuntu OS or other Ubuntu based OS.
---

# Install Idea+Jetty+Derby on Ubuntu

We expect to use Java 11 in this tutorial. For other versions \(8 or lower\), adapt the downloads appropriately.

## Downloads

1. Download _IntelliJ Idea_ from _Jetbrains_ official download site \(https://www.jetbrains.com/idea/download/index.html\)
   1. Select and download _Ultimate_ edition \(free trial\)
   2. Save _ideaIU.....tar.gz_ file into _Downloads_ folder
2. Download _Jetty_ server from _Eclipse_ official site \(https://www.eclipse.org/jetty/download.html\)
   1. Select and download _.tgz_ file
   2. Save _jetty-distribution-.....tar.gz_ file into _Downloads_ folder
3. Download _Apache Derby_ from _Apache_ official download site \(https://db.apache.org/derby/derby\_downloads.html\)

   1. Select corresponding Java version \(9 and Higher\), version 10.15.1.3 currently
   2. Select and download _db-derby-....bin.tar.gz_ file 
   3. Save the file into _Downloads_ folder

4. Download _Java JDK_ from _Oracle_ official site \(https://www.oracle.com/technetwork/java/javase/downloads\)
   1. Confirm the Java license
   2. Select and download _jdk-....bin.tar.gz_ file
   3. Login \(or register\) to Oracle web, if required
   4. Save the file into _Downloads_ folder
5. Open the _Downloads_ folder and extract each _...tar.gz_ file into its separate folder.
6. Create a new directory _WebDev_ under your _HOME_ directory and move all folders from _Downloads_ directory into _WebDev_ directory.

## Apache Derby Execution

To execute Apache Derby, a terminal must be used. As a set of commands is required to start the server, it is useful to create a custom _shell batch_ script to automatize all the commands.

{% hint style="info" %}
**Note:** Remember to update all paths by your correct username and Java and Apache Derby versions.
{% endhint %}

To start the Apache Derby server:

1. Start the _Terminal_
2. Prepare _Java_ environment:
   1. Set Java home directory path: `export JAVA_HOME=/home/userName/WebDev/jdk-11.0.6`
   2. Add Java home directory to the global PATH variable: `export PATH=$JAVA_HOME/bin:$PATH`
3. Prepare _Apache Derby_ environment:
   1. Set Apache Derby path: `export DERBY_INSTALL=/bin/userName/WebDev/db-derby-10.15.1.3-bin`
4. Navigate _Terminal_ to the Derby directory: `cd $DERBY_INSTALL$/bin`
5. Start the server: `./startNetworkServer -noSecurityManager`

When everything done well, at the end the _Terminal_ should answer that the Apache Derby server is started and ready to accept the connections.

### Create the batch file to automatize the server startup

Create a file \(right-click in _WebDev_ folder -&gt; New File\) and place the code corresponding with the previous section:

{% code title="derbyStart.sh" %}
```text
#!/bin/bash

echo "Starting Apache Derby"

echo "  Preparing Java"
export JAVA_HOME=/home/userName/WebDev/jdk-11.0.6
PATH=$JAVA_HOME/bin:$PATH

echo "  Preparing Apache Derby"
export DERBY_INSTALL=/bin/userName/WebDev/db-derby-10.15.1.3-bin

echo "  Starting server"
cd $DERBY_INSTALL/bin
./startNetworkServer -noSecurityManager
```
{% endcode %}

Note that under Linux the file cannot be executed easily by double-clicking the icon by default. Firstly, by context menu on the file icon open _Properties,_ and under the tab _Permissions_ check the option to _Allow the file to be executed_. Then, to automatically start the server by double click, set the _File Manager_ to start _.sh_ files automatically - open the menu in any _File Manager_ window, select _Edit_ -&gt; _Preferences_, under tab _Behavior_ in the section _Executable text files_ select the appropriate behavior.

## IntelliJ Idea Instalation

In the content of _Idea_ extracted folder, there is a file called _install-Linux-tar.txt_ describing the Idea startup procedure. It's quite simple.

To start _Idea_ for the first time:

1. Open _Terminal_ in _bin_ subfolder under the _Idea_ extracted folder \(use the right context menu in the folder to open _Terminal_ in the specified location\).
2. Execute starting script `./idea.sh`
3. Continue with the _Idea_ first startup wizard:
   1. Select if some settings from the previous versions should be imported.
   2. Agree License Terms.
   3. Decide if Usage statistics should be shared.
   4. Choose the theme \(dark/light\).
   5. Decide if you would like to create a fast shortcut for Idea \(suggested to keep checked\).
   6. Decide if a launcher script should be created \(not needed\).
   7. Choose default plugins. For high _Idea_ performance, uncheck everything you think you will not need \(anything can be selected later in _Idea_ settings\). Minimal required settings for OOPR2/3 courses are \(if the following list is too long for you, just keep the default settings\):
      * Frameworks: nothing for OOPR2, Spring+Java EE+Hibernate for OOPR3
      * Build Tools: Ant, Maven suggested for OOPR3
      * Web Development: nothing for OOPR2, HTML+JavaScript+CSS for OOPR3 \(at least\)
      * Version Controls: Git+GitHub recommended \(at least\)
      * Test Tools: JUnit
      * Application Servers: nothing for OOPR2, Application Servers View+Jetty for OOPR3
      * Clouds: disable
      * Swing: disable \(keep enabled if you would like develop window-based user interface applications\)
      * Android: disable
      * Database Tools: disable for OOPR2, enable for OOPR3
      * Other Tools: keep Terminal, also UML for OOPR3
      * Plugin Development: Disable
   8. Download featured plugins - none is required, so you can skip this step.
   9. Connect _Idea_ to the appropriate license.

Then, the _Idea_ should correctly.

### Validate correct "Java Console Development" installation

In the next step, validate if _Idea_ is set correctly:

1. Choose "Create new project"
2. Select "Java" application type \(left column\). Select the project SDK \(top row combo-box\). Add \(using "New..." button\) downloaded Java SDK if required. Leave the main window frameworks \(Groovy/Kotlin/...\) unchecked\). Continue using "Next" button.
3. Check "Create project from template" and use "Command Line App"
4. Name the project and update the location if required. Finish the wizard.
5. In the created _Main_ class and its _main\(\)_ method, place some code printing some text: \``System.out.println("Hello, world!");`
6. Start the project using a green arrow and check if the application starts correctly and prints the required output.

### Validate correct "Web-development" installation

In the next step, validate if _Idea_ is set correctly for the web development:

1. Choose a creation of the new project.
2. Select "Java Enterprise" from the left column.
3. Check "Web Application" from the middle area. 
4. **Now the application server must be set**. 
   1. Choose "New" button next to the "Application Server:" line, choose "Jetty Server".
   2. In the opened window, use the icon at the end of the first line to select the folder of the _Jetty_ server in the _WebDev_ folder.
   3. Confirm the following dialog with activated modules.
5. Continue using "Next" button.
6. Choose the appropriate project name and finish the wizard.
7. Start the project using a green arrow and check if the application starts correctly and opens a web browser with some \(more less empty\) content.

{% hint style="info" %}
**Note:** The application served added in step 4 will remain available for all other web projects. This step is not processed for every new web application.
{% endhint %}

### Validate database connection to Apache Derby

This section will validate if the database server is started correctly and if _Idea_ can connect to it.

{% hint style="info" %}
**Note:** The database server must be set and started for this part of the tutorial \(see previous sections\).
{% endhint %}

To check the access to the database from _Idea_:

1. Open _Database_ menu from the right side of the _Idea_ window.
2. Press the small '+' symbol to open the menu.
3. Choose "Data Source =&gt; Apache Derby".
4. Under the "Driver" change the value of "Apache Derby \(Embedded\)" into "Apache Derby \(Remote\)".
5. Press "Download missing driver files" to download required drivers into the _Idea_ environment.
6. Fill the connection settings:
   1. Host: `localhost`
   2. Port: `1527`
   3. User: `sa`\(some random, generally\)
   4. Password: `sa`\(some random, generally\)
   5. Database: `testDB;create=true`
7. Press "Test Connection" button
8. The response should show a green message about successful connection.

