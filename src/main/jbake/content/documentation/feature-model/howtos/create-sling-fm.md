title=Create your own Sling Feature Model for the Kickstart 
type=page
status=published
tags=feature model,sling,kickstart
~~~~~~

### How-To Overview

<div style="background: lightblue;">

* What will you learn: 
	* Creating your own Sling Feature Model with the latest from Sling

* Time: 20 minutes
* Skill Level: Intermediate
* Environment: Windows/Unix

</div>

* Back To: [Feature Model Home](/documentation/feature-model/feature-model-overview.html)

### Prerequisites

In order to follow through this HowTo you need the following on your computer:

* Java 8
* Maven 3
* Command Line with Bash
* Worked through *Get up and running with Sling and the Kickstarter*

### What is the Sling Feature Model

* a [feature model](https://github.com/apache/sling-org-apache-sling-feature/blob/master/readme.md) is a description of all the OSGi dependency and configurations
* feature models can be aggregated into a single feature model
* feature model(s) can be started with the Sling Feature Launcher 
* Sling feature model defines all the dependencies and setup actions to launch Sling

### Explanation on what will happen

Until Sling is fully converted to Feature Models we will use a Provisioning to
Feature Model Converter Plugin that will create a Feature Model from each Provisiong
Model file. Afterwards we will aggregate this into one file: the Sling Feature Model.

### Step 1: Obtain Sling Starter and Kickstart Modules

The Sling 12 Starter is not released yet and so have to obtain the project from its GitHub repository:

    $ cd <project root folder>
    $ git clone https://github.com/apache/sling-org-apache-sling-starter.git
    $ cd sling-org-apache-sling-starter
    $ pwd

The last step is priting the path to the Sling Starter module.

**Note**: to obtain the latest source code from the Kickstart GitHub repository see below in the Addendum. 

The last step is priting the path to the Sling Starter module. Now let's get the
[Released Source of Sling Kickstart](https://repository.apache.org/content/groups/public/org/apache/sling/org.apache.sling.kickstart):

    $ cd <project root folder>
    $ curl https://repository.apache.org/content/groups/public/org/apache/sling/org.apache.sling.kickstart/0.0.4/org.apache.sling.kickstart-0.0.4-source-release.zip \
      > org.apache.sling.kickstart-0.0.4-source-release.zip
    $ jar -xvf org.apache.sling.kickstart-0.0.4-source-release.zip
    $ mv org.apache.sling.kickstart-0.0.4 sling-org-apache-sling-kickstart
    $ cd sling-org-apache-sling-kickstart


### Step 2: Run the Provisioning Model Conversion

The Kickstart module provides an additional Maven POM file to convert the Sling Starter provisioning models
into feature models and then aggregates them into a single feature model:

    $ cd <project root folder>
    $ cd sling-org-apache-sling-kickstart
    $ mvn -f sling-fm-pom.xml install -Dsling.starter.folder=<path to the sling starter module>


After the build ran through successfully you will find the **Sling Feature Model file** in the
folder **target/slingfeature-tmp** as a file **feature-sling12.json**.

### Step 3: Run the newly created Sling Feature Model

Because this Sling Feature Model is not in the default place (src/main/resources) we need to launch
the **Sling Kickstart** with additional options. So we create a new Kickstart Execution folder and
then copy the Feature Model there, build the Kickstart JAR file and copy there as well. Finally we will
launch Sling:

    $ cd <project root folder>
    $ mkdir kickstart-run
    $ cd sling-org-apache-sling-kickstart
    $ cp target/slingfeature-tmp/feature-sling12.json ../kickstart-run
    $ mvn clean install
    $ cp target/org.apache.sling.kickstart-0.0.2.jar ../kickstart-run
    $ cd ../kickstart-run
    $ java -jar org.apache.sling.kickstart-0.0.2.jar -s feature-sling12.json


Kickstart was then launched with the **-s option** which takes the path to the Sling Feature Model file
and uses that one to launch Sling. 

## Mission Accomplished

* Next Up: [Create a Sling Feature Archive](/documentation/feature-model/howtos/create-sling-far.html)
* Back To: [Feature Model Home](/documentation/feature-model/feature-model-overview.html)

## Addendum

To get the Kickstart sources:

    $ cd <project root folder>
    $ git clone https://github.com/apache/sling-org-apache-sling-kickstart.git
    $ cd sling-org-apache-sling-kickstart
