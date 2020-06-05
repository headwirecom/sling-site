title=How to Start Sling with the Kickstarter
type=page
status=published
tags=feature model,sling,kickstart
~~~~~~

### About this How-To

<div style="background: #cde0ea; padding: 14px; border-left: 10px solid #f9bb00;">

#### What we'll explore: 

* We'll start Sling with the Kickstarter and explore the Feature Model

#### What you should know: 

* Skill Level: Beginner
* Environment: Windows/Unix
* Time: 15 minutes

</div>

Back To: [Feature Model Home](/documentation/feature-model/feature-model-overview.html)

### Prerequisites

In order to follow this how-to you'll need the following on your computer:

* Java 8
* Bash shell

### What's the Kickstarter

Prior to the Kickstarter, the Sling application was assembled into an uber JAR using the Provisioning Model. 
The JAR file was fairly large in size and weighed in at ~70MB. If the Sling application required a new bundle,
the uber JAR would have to be rebuilt.

The Kickstarter provides a method to start Sling using a new application packaging/assembly approach known as
the _Feature Model_.  By default, the Kickstarter is configured with a minimum set of feature definitions to allow 
for a light weight Sling instance (~3MB).  If additional customizations to your Sling application are required, simply 
define additional features based on your requirements. Any additional features will then be pulled from a Maven repository.

### How does the Kickstarter work

The [Kickstarter](https://github.com/apache/sling-org-apache-sling-kickstart) uses the Feature Model Launcher to 
start a Sling instance. It sets up a control port to manage the instance and provides default values to start Sling.
The Feature Launcher then donwloads the necessary dependencies and installs them into the OSGi container. 

Let's try this out!

### Step 1: Download the Kickstarter 

<div style="background: #cde0ea; padding: 14px; border-left: 10px solid #f9bb00;">

At the time of this writing, the latest Kickstarter version was`0.0.2`. Adjust the commands below to reflect the 
version you downloaded.

</div>

Visit the [Apache Sling Downloads](https://sling.apache.org/downloads.cgi) page and download the _Kickstart Project_
bundle. 

Then, create a working directory for the Kickstarter and copy the bundle to this location. You can name this 
directory anything you like.

    $ mkdir kickstarter
    $ cd kickstarter
    $ cp /some/download/path/org.apache.sling.kickstart-0.0.2.jar .

### Step 2: Start Sling with the Kickstarter

<div style="background: #cde0ea; padding: 14px; border-left: 10px solid #f9bb00;">

 Make sure nothing is listening on port 8080 as this port will be used by Sling.

</div>. 

Start the Kickstarter.

    $ java -jar org.apache.sling.kickstart-0.0.2.jar

Next, open a browser and visit [http://localhost:8080/](http://localhost:8080/).

<div style="background: #cde0ea; padding: 14px; border-left: 10px solid #f9bb00; margin-bottom: 1em;">

- The Kickstarter will take some time to start the first time since the Feature Model needs to populate your local
  Maven repository with any missing artifacts. 
- If you run into any issues, try re-running the Kickstarter with **-v** option.

</div>

### Step 3: Start using Sling

Click the **Login** link and log in with **admin/admin**. 


### Step 4: Check the status of Sling

Open a new terminal window and navigate to the same Kickstarter working directory that
was used to start Sling.

Now, run the Kickstarter JAR again with the **status** command to view the current
status of your Sling instance.

    $ java -jar org.apache.sling.kickstart-0.0.2.jar status


If your Sling instance is running, you should see output similar to this:

    /127.0.0.1:52516>status
    /127.0.0.1:52516<OK
    Sent 'status' to /127.0.0.1:52481: OK
    Terminate VM, status: 0

If your sling instance is not running, you should see:

    No Apache Sling running at /127.0.0.1:52244
    Terminate VM, status: 3

### Step 5: Stop Sling with the Kickstater

Run the Kickstarter JAR again and specify the **stop** command.

    $ java -jar org.apache.sling.kickstart-0.0.2.jar stop

Alternatively, you can stop Sling by hitting **<Ctrl+C>**.

## Mission Accomplished


<div style="background: #cde0ea; padding: 14px; border-left: 10px solid #f9bb00; margin-bottom: 1em;">

#### What we learned: 

* We successully started Sling with the Kickstarter and had our first
  glimpse of the Feature Model.

</div>

Did we succeed in making you more curious about the world of Feature Models? 
The next how-to in this series dives into Sling Feature Models a bit more to help
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
