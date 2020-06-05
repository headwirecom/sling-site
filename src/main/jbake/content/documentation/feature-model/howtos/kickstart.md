title=How to Start Sling with the Kickstarter
type=page
status=published
tags=feature model,sling,kickstart
~~~~~~

**RR: note** we are a bit all over the place with kickstart, kickstarter, kickstart launcher, etc. We probably have to find some common language about that, maybe
use 'Kickstart Launcher (kickstarter)' first and then continue using kickstarter
as that may be the easier name to use throughout the doc.  

### About this How-To

<div style="background: #cde0ea; padding: 14px; border-left: 10px solid #f9bb00;">

#### What we will explore: 

* We are starting up Apache Sling with the Kickstarter to explore launching
  with feature models

#### What you should know: 

* Skill Level: Beginner
* Environment: Windows/Unix
* Time: 15 minutes

</div>

Back To: [Feature Model Home](/documentation/feature-model/feature-model-overview.html)

### Prerequisites

In order to follow through this HowTo you need the following on your computer:

* Java 8
* Command Line with Bash

### What is the Kickstarter

Traditionally Sling has been started with the rather larger Sling Starter JAR file clocking in at about 70MB for Sling-11 - the starter is built with the provisioning model. 

Kickstart provides the same command line options available through the launcher
to start Sling but relies on a feture model. The initial download for the kickstarter clocks in at about 3MB. Kickstart itself relies on the feature launcher, a more 
barebones tool to launch with a feature model.

Kickstart comes bundled with a basic feature model to start Apache Sling. You can also
provide your own feature model for Sling and provide a set of additional feature models to run your application on top of Sling.

While Kickstarting Apache Sling all necessary artefacts are fetched from your local and the public maven repository. 

### Explanation on what will happen

[Sling Kickstart](https://github.com/apache/sling-org-apache-sling-kickstart) uses the
Feature Model Launcher to run a Sling instance for you, sets up a control port to manage
the instance and provides default values so that Sling starts just fine without any parameters.
The Feature Launcher will then donwload all the necessary dependencies and install them
into a Felix container. 

Let's go try this out!

### Step 1: Download the Kickstart JAR File

**RR: question:** should we not point them to https://sling.apache.org/downloads.cgi and have them download the latest release for kickstart? 

The Sling Kickstart Project JAR file can be downloaded here:

	$ cd <project root folder>
    $ mkdir kickstart
    $ cd kickstart
    $ curl https://repository.apache.org/content/groups/public/org/apache/sling/org.apache.sling.kickstart/0.0.2/org.apache.sling.kickstart-0.0.2.jar \
      > org.apache.sling.kickstart-0.0.2.jar


### Step 2: Run Sling with the Kickstarter

Make sure nothing is running on port 8080 as this port will be taken by sling by default. 

We can run the Kickstarter by just executing the JAR file:

	$ java -jar org.apache.sling.kickstart-0.0.2.jar

Head over to [Sling Home Page](http://localhost:8080/). You will see the regular sling
startup screen until the server is ready. Once ready you'll see the Welcome screen, 

<div style="background: #cde0ea; padding: 14px; border-left: 10px solid #f9bb00; margin-bottom: 1em;">

- The first time startup with kickstarter may take a bit longer as your local maven repository
may not contain the artefacts defined in the feature model. 
- If you think something is not
right you may want to try again and use the **-v** option to get a verbose output from
the kickstarter process.

</div>

![Sling Home](sling.home.in.browser.png)

### Step 3: Start using Sling

Click **Login** link and log in with **admin/admin** and then click on **Browse
Content** to bring up Composum to see the JCR node tree.


### Step 4: Check the status of the running Sling Instance

You can always check the status of a running sling instance by executing the kickstart
jar file with the command **status** in the folder where you started your instance.

To get a status on the Sling service open up an additional terminal window in the same
folder where you placed your kickstart jar file in Step 1 and then execute the **status** command:

	$ java -jar org.apache.sling.kickstart-0.0.2.jar status


This should return something similar to these lines:

	/127.0.0.1:52516>status
	/127.0.0.1:52516<OK
	Sent 'status' to /127.0.0.1:52481: OK
	Terminate VM, status: 0

If your sling instance is not running you should see

	No Apache Sling running at /127.0.0.1:52244
	Terminate VM, status: 3

### Step 5: Shut down Sling

use the **stop** comand in the jar

To stop it do:

	$ java -jar org.apache.sling.kickstart-0.0.2.jar stop


This will then show the status of the process. 
On unix systems you are also informed about the termination of the process in the
terminal window where you started sling:

	/127.0.0.1:52520>stop
	/127.0.0.1:52520<OK
	Stop Application
	Sent 'stop' to /127.0.0.1:52481: OK
	Terminate VM, status: 0
	mac:sling-kickstart-run schaefa$ [INFO] Framework stopped

	[1]+  Done                    java -jar org.apache.sling.kickstart-0.0.2.jar start

Alternative: We can stop Sling by hitting **Ctrl-C** on the command line to exit the process. Make sure your Sling process is gone by validating you dont get a response with your browser on port localhost:8080 anymore

## Mission Accomplished


<div style="background: #cde0ea; padding: 14px; border-left: 10px solid #f9bb00; margin-bottom: 1em;">

#### What we learned: 

* We successully started up Apache Sling with the Kickstarter and had our first
  brush up with feature models

</div>

Did we succeed in making you more curious about the world of feature models? 
The next how to in this series dives into Sling Feature Models a bit more to help
you with that. You can also read a bit more about the Kickstarter on this page if you
feel you need a bit more information.

* Next Up: [Build your own Sling Feature Model](/documentation/feature-model/howtos/create-sling-fm.html)
* Back To: [Feature Model Home](/documentation/feature-model/feature-model-overview.html)

## A couple additional things to explore

### Kickstart Launch options

Finally let's have a look at the launch options:

	$ java -jar org.apache.sling.kickstart-0.0.2.jar -h


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

### Run as a Background Process / Service

**RR: question** what's the real difference between using start and using no command?

The Kickstart Project can launch Sling as a background process also known as service with the
**start** command:

    $ java -jar org.apache.sling.kickstart-0.0.2.jar start


If this is done from a Terminal / Shell then the Kickstart project needs to be placed into
the background. In Unix this is done with appending a '&':

    $ java -jar org.apache.sling.kickstart-0.0.2.jar start &


Because these process are not directly accessible the admin can use the **status** and
**stop** command as mentioned in **Step 6 and 7**.

### Build from Source

How to build the Sling Kickstart Project from source is document in
the [Kickstart's Readme file](https://github.com/apache/sling-org-apache-sling-kickstart/blob/master/Readme.md)
