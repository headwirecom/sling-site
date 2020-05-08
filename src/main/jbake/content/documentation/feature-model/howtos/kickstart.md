title=Get up and running with Sling and the Kickstarter 
type=page
status=published
tags=feature model,sling,kickstart
~~~~~~

<div style="background: lightblue;">

* What will you learn: 
	* We are starting up Apache Sling with the Kickstarter to explore launching
	with feature models

* Time: 15 minutes
* Skill Level: Beginner
* Environment: Windows/Unix

</div>

[Back to the Feature Model Home](/documentation/feature-model/feature-model-overview.md)

### Prerequisites

In order to follow through this HowTo you need the following on your computer:

* Java 8
* Maven 3 (is maven really necessary or do we only need the .m2/repo folder? what happens without maven?)
* Command Line with Bash

### What is the Kickstarter

* wrapper around fetature launcher exposing the same types of arguments as the uickstart
* bundles a sling12 feature module
* uses the maven repo to download feature models and artefacts to start sling

### Explanation on what will happen

The Kickstarer uses the Feature Model Launcher to run a sling instance for you (what's the difference - document here?)

consults feature model, fetches all files from maven to run sling

### Step 1: Download the Kickstart JAR File

The Sling Kickstart Project JAR file can be downloaded here:
[Sling Kickstart Snapshots](https://repository.apache.org/content/groups/snapshots/org/apache/sling/org.apache.sling.kickstart/0.0.1-SNAPSHOT/)
Select the latest version (jar file) and download it.

### Step 2: Create your Project  Folder

Copy the file into a folder of your choice and double click the jar file
or go to a shell and navigate to that folder

	$ cd <project root folder>

### Step 3: Run Sling with the Kickstarter

Make sure nothing is running on port 8080 as this port will be taken by sling by default. 

We can run the Kickstarter by just executing the JAR file:

	$ java -jar org.apache.sling.kickstart-0.0.1-SNAPSHOT.jar

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

	$ java -jar org.apache.sling.kickstart-0.0.1-SNAPSHOT.jar status


Which should return:

	/127.0.0.1:52516>status
	/127.0.0.1:52516<OK
	Sent 'status' to /127.0.0.1:52481: OK
	Terminate VM, status: 0


### Step 5: Shut down Sling

use the stop call in the jar

To stop it do:

	$ java -jar org.apache.sling.kickstart-0.0.1-SNAPSHOT.jar stop


This will then show the status of the process and unix will also print then
termination of the process:


	/127.0.0.1:52520>stop
	/127.0.0.1:52520<OK
	Stop Application
	Sent 'stop' to /127.0.0.1:52481: OK
	Terminate VM, status: 0
	mac:sling-kickstart-run schaefa$ [INFO] Framework stopped

	[1]+  Done                    java -jar org.apache.sling.kickstart-0.0.1-SNAPSHOT.jar start


Alternative: We can stop Sling by hitting **Ctrl-C** on the command line to exit the process. Make sure your Sling process is gone by validating you dont get a response with your browser on port localhost:8080 anymore

## Mission Accomplished

next up: build your own feature model project and run it with the kickstarter 

### Kickstart Launch options

Finally let's have a look at the launch options:

	$ java -jar org.apache.sling.kickstart-0.0.1-SNAPSHOT.jar -h


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

### Conclusion

This was s short introduction into the **Sling Kickstart** project to launch Sling
on your local computer to check it out, develop of test Sling applications.

[Back to the Feature Model Home](/documentation/feature-model/feature-model-overview.md)


>> isnt that documented in the readme of the kickstarter? 

## Addendum: Build from Source

To build the Kickstart project from code you need to clone and build both the
Sling Kickstart Maven Plugin as well as the Sling Kickstart project.

### Clone and Build Kickstart Maven Plugin

First we need to build the Maven plugin because it is needed to run the IT test
in the Kickstart Project. So we clone it from GitHub:

	$ cd <project root folder>
	$ git clone git@github.com:apache/sling-kickstart-maven-plugin.git
	$ cd sling-kickstart-maven-plugin


Now we can build the project:

	$ mvn clean install


### Clone and Build Kickstart Project

Now we can clone the Kickstart Project:


	$ git clone git@github.com:apache/sling-org-apache-sling-kickstart.git
	$ cd sling-org-apache-sling-kickstart


Now we are ready to build it and then finally run Sling.

	mvn clean install


This will build the project but also run Sling as IT to run the Smoke IT test to
make sure it is working.
At the end you will find the file **org.apache.sling.kickstart-0.0.1-SNAPSHOT.jar**
in the **target** folder of our project. We could run it there but that would wipe
Sling away whenever we clean the project.
To avoid this will go back to the project root folder and create folder **sling-kickstart-run**
folder next to folder **sling-org-apache-sling-kickstart** and copy the JAR file
there:

	cd ..
	mkdir sling-kickstart-run
	cd sling-kickstart-run
	cp ../sling-org-apache-sling-kickstart/target/org.apache.sling.kickstart-0.0.1-SNAPSHOT.jar .
