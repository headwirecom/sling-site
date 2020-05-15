title=Get up and running with Sling and the Kickstart
type=page
status=published
tags=feature model,sling,kickstart
~~~~~~

### How-To Overview

<div style="background: lightblue;">

* What will you learn: 
	* We are starting up Apache Sling with the Kickstarter to explore launching
	with feature models

* Time: 15 minutes
* Skill Level: Beginner
* Environment: Windows/Unix
</div>

* Back To: [Feature Model Home](/documentation/feature-model/feature-model-overview.html)

### Prerequisites

In order to follow through this HowTo you need the following on your computer:

* Java 8
* Command Line with Bash

### What is the Kickstarter

* wrapper around fetature launcher exposing the same types of arguments as the uickstart
* bundles a sling12 feature module
* uses the maven repo to download feature models and artefacts to start sling

### Explanation on what will happen

The [Sling Kickstart](https://github.com/apache/sling-org-apache-sling-kickstart) uses the
Feature Model Launcher to run a Sling instance for you, sets up a control port to manage
the instance and provides default values so that Sling starts just fine without any parameters.

The Feature Launcher will then donwload all the necessary dependencies and install them
into a Felix container.

### Step 1: Download the Kickstart JAR File

The Sling Kickstart Project JAR file can be downloaded here:
[Sling Kickstart on Apache](https://repository.apache.org/content/groups/public/org/apache/sling/org.apache.sling.kickstart/)
Select the latest version (like 0.0.1-SNAPSHOT) folder and then the Kickstart Jar file
 and download it.
From now on we are referencing the file's version (the part between org.apache.sling.kickstart-
and .jar) as &lt;VERSION>.

### Step 2: Create your Project  Folder

Copy the file into a folder of your choice and double click the jar file
or go to a shell and navigate to that folder

	$ cd <project root folder>

### Step 3: Run Sling with the Kickstarter

Make sure nothing is running on port 8080 as this port will be taken by sling by default. 

We can run the Kickstarter by just executing the JAR file:

	$ java -jar org.apache.sling.kickstart-<VERSION>.jar

Head over to [Sling Home Page](http://localhost:8080/). You will see the regular sling
startup screen until the server is ready. Once ready you'll see the Welcome screen

![Sling Home](sling.home.in.browser.png)

### Step 4: Start using Sling

Click **Login** link and log in with **admin/admin** and then click on **browse
Content** to bring up Composum to see the JCR node tree.


### Step 5: Check the status of the running Sling Instance

.. other shell, execute jar with status

To get a status on the Sling service
then do:

	$ java -jar org.apache.sling.kickstart-<VERSION>.jar status


Which should return:

	/127.0.0.1:52516>status
	/127.0.0.1:52516<OK
	Sent 'status' to /127.0.0.1:52481: OK
	Terminate VM, status: 0


### Step 6: Shut down Sling

use the stop call in the jar

To stop it do:

	$ java -jar org.apache.sling.kickstart-<VERSION>.jar stop


This will then show the status of the process and unix will also print then
termination of the process:


	/127.0.0.1:52520>stop
	/127.0.0.1:52520<OK
	Stop Application
	Sent 'stop' to /127.0.0.1:52481: OK
	Terminate VM, status: 0
	mac:sling-kickstart-run schaefa$ [INFO] Framework stopped

	[1]+  Done                    java -jar org.apache.sling.kickstart-<VERSION>.jar start


Alternative: We can stop Sling by hitting **Ctrl-C** on the command line to exit the process. Make sure your Sling process is gone by validating you dont get a response with your browser on port localhost:8080 anymore

## Mission Accomplished

* Next Up: [Build your own Sling Feature Model](/documentation/feature-model/howtos/create-sling-fm.html)
* Back To: [Feature Model Home](/documentation/feature-model/feature-model-overview.html)

### Kickstart Launch options

Finally let's have a look at the launch options:

	$ java -jar org.apache.sling.kickstart-<VERSION>.jar -h


This will print this:

	Usage: java -jar <Sling Kickstarter JAR File> [-hnv] [-a=<address>]
	                                              [-c=<slingHome>] [-f=<logFile>]
	                                              [-i=<launcherHome>]
	                                              [-j=<controlAddress>]
	                                              [-l=<logLevel>] [-p=<port>]
	                                              [-r=<contextPath>]
	                                              [-s=<mainFeatureFile>]
	                                              [-af=<additionalFeatureFile>]...
	                                              [-D=<String=String>]... [COMMAND]
	Apache Sling Kickstart
	      [COMMAND]             Optional Command for Server Instance Interaction, can be
	                              one of: 'start', 'stop', 'status' or 'threads'
	  -a, --address=<address>   the interface to bind to (use 0.0.0.0 for any)
	      -af, --additionalFeature=<additionalFeatureFile>
	                            additional feature files
	  -c, --slingHome=<slingHome>
	                            the sling context directory (default sling)
	  -D, --define=<String=String>
	                            sets property n to value v. Make sure to use this option
	                              *after* the jar filename. The JVM also has a -D option
	                              which has a different meaning
	  -f, --logFile=<logFile>   the log file, "-" for stdout (default logs/error.log)
	  -h, --help                Display the usage message.
	  -i, --launcherHome=<launcherHome>
	                            the launcher home directory (default launcher)
	  -j, --control=<controlAddress>
	                            host and port to use for control connection in the
	                              format '[host:]port' (default 127.0.0.1:0)
	  -l, --logLevel=<logLevel> the initial loglevel (0..4, FATAL, ERROR, WARN, INFO,
	                              DEBUG)
	  -n, --noShutdownHook      don't install the shutdown hook
	  -p, --port=<port>         the port to listen to (default 8080)
	  -r, --context=<contextPath>
	                            the root servlet context path for the http service
	                              (default is /)
	  -s, --mainFeature=<mainFeatureFile>
	                            main feature file (file path or URL) replacing the
	                              provided Sling Feature File
	  -v, --verbose             the feature launcher is verbose on launch
	Copyright(c) 2020 The Apache Software Foundation.


Most of the options are the same as for the **Sling Starter** project with these
two additional options:

* **-s**: allows to specify your own Sling Feature Model / Archive
* **-af**: allows to add additional Feature Model / Archives (repeat for each feature file)

## Addendum

### Build from Source

How to build the Sling Kickstart Project from the source is document in
the [Kickstart's Readme file](https://github.com/apache/sling-org-apache-sling-kickstart/blob/master/Readme.md)

### Run as a Background Process / Service

The Kickstart Project can launch Sling as a background process also known as service with the
**start** command:

    $ java -jar org.apache.sling.kickstart-<VERSION>.jar start


If this is done from a Terminal / Shell then the Kickstart project needs to be placed into
the background. In Unix this is done with appending a '&':

    $ java -jar org.apache.sling.kickstart-<VERSION>.jar start &


Because these process are not directly accessible the admin can use the **status** and
**stop** command as mentioned in **Step 6 and 7**.