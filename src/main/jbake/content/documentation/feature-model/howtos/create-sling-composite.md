title=Create a Sling with Composite Nodestore 
type=page
status=published
tags=feature model,sling,kickstart
~~~~~~

### How-To Overview

<div style="background: lightblue;">

* What will you learn: 
	* Creating your own Sling Instance with a Composite Nodestore

* Time: 30 minutes
* Skill Level: Intermediate
* Environment: Windows/Unix

</div>

* Back To: [Feature Model Home](/documentation/feature-model/feature-model-overview.html)

### Prerequisites

In order to follow through this HowTo you need the following on your computer:

* Java 8
* Maven 3
* Command Line with Bash
* Worked through *Get up and running with Sling and the Kickstart*

### What is the Composite Nodestore

A Composite Nodestore is a virtual nodestore that is composed of multiple nodestores acting as
one. Each of the nodestore has a part of the nodestore like a mount in a Unix file system. One
of these nodestore can be made read-only.

### Why Using a Composite Nodestore

One of the problems with working with Sling is that during development changes and deployments
can conflict or changes can be lost making Sling unstable.
A solution is to make static part of Sling read-only like libs and apps and only dynamic parts like
content. For that the JCR nodestore is split into two - one for read-only content, the other for
the dynamice content - and them combined as Composite Nodestore which act like one.
The difficult part is that during the deployment of the artifacts the static nodestore needs to
be writable in order to install and configure the artiacts and after the installation / setup that
nodestore is then made read-only.
 
### Explanation on what will happen

First we will start Sling with just a single, regular Nodestore that will become the read-only Nodstore
later. After Sling stabilizes we will shut it down, create a link of the created segment store
as **segmentstore** and then launch Sling once more with initial Nodestore as read-only and the
new created **global** nodestore as writable. 
A Feature Archive is a fully self contained ZIP file with the Feature Model file and all its dependencies.
We will create such an Archive and the launch it. The startup will be significally faster is the Kickstart
project / Feature Launcher does not have to download the dependencies.

### Step 1: Obtain Kickstart Module

**Note**: if both modules were already obtained by the previous (Create Sling Feature Model / Archive) then
this entire step can be skipped.

We will obtain the released source from the Apache Repository. If you want to get the latest then have a
look at the Addendum on how to clone the GitHub repository instead.

We contain the [Release Source of Sling Kickstart](https://repository.apache.org/content/groups/public/org/apache/sling/org.apache.sling.kickstart):

    $ cd <project root folder>
    $ curl https://repository.apache.org/content/groups/public/org/apache/sling/org.apache.sling.kickstart/0.0.4/org.apache.sling.kickstart-0.0.4-source-release.zip \
      > org.apache.sling.kickstart-0.0.4-source-release.zip
    $ jar -xvf org.apache.sling.kickstart-0.0.4-source-release.zip
    $ mv org.apache.sling.kickstart-0.0.4 sling-org-apache-sling-kickstart
    $ cd sling-org-apache-sling-kickstart


### Step 2: Build the Kickstart

We still need to build the Kickstart project as we need the launching ability:

    $ mvn clean install

### Step 3: Run the Initialization Script

First We need to start Sling now as we would normally do (seed mode):

    $ sh bin/create_seed_fm.sh


This will start Sling with these two Feature Models:

1. sling-fm-two-headed.json: Sling Feature Model without a Nodestore
2. feature-two-headed-seed.json: Feature Model with a single Nodestore

When Sling is donw with the startup shut it down.

### Step 4: Run Sling with a Composite Nodstore

Now we are ready to launch Sling for public usage:

    $ sh bin/run_composite_fm.sh

This will first create a link of the first nodestore to the expected name
of **segmentstore**. Then it will make that nodestore read-only. Finally
it will create a **global** nodestore for the content.

### Step 5: Test Expected Functionality

To make sure that the first nodestore is read-only but content is still
editable we log into Sling and then use Composum to add properties and
see what happens.

**1.** Log Into [Sling](http://localhost:8080)

![Sling Logged In](sling-logged-in-starter.png)

**2.** Click on [Browse Content](http://localhost:8080/bin/browser.html)
 
![Sling Composum Landing](sling-composum-landing.png)

**3.** Click on [Slingshot Node in Content](http://localhost:8080/bin/browser.html/content/slingshot)

![Sling Composum Slingshot Content](sling-content-slingshot-selected.png)

**4.** Click on the **Plus** sign to the right over the Properties to add a new one

![Sling Composum Content Add Prop](sling-content-slingshot-add-prop.png)

**5.** Fill in a name and a value and safe it and it is properly added

![Sling Composum Content Prop Added](sling-content-slingshot-prop-added.png)

**6.** Click on [Slingshot Node in Apps](http://localhost:8080/bin/browser.html/apps/slingshot)

![Sling Composum Slingshot Apps](sling-apps-slingshot-selected.png)

**7.** Click on the **Plus** sign to the right over the Properties to add a new one

![Sling Composum Apps Add Prop](sling-apps-slingshot-add-prop.png)

**8.** Fill in a name and a value and safe it and it will error out

![Sling Composum Content Prop Add Failed](sling-apps-slingshot-prop-add-failed.png)


### Step 6: Update your Project

The next step is to being able to upgrade your project when you release new software.
To showcase how this works without having to code everything you can
[download a sample project here](sling-composite-nodestore-project.zip).
Uncompress and install:

    $ cd <project root folder>
    $ mkdir sling-composite-nodestore-project
    $ cd sling-composite-nodestore-project
    $ jar -xvf <download folder>/sling-composite-nodestore-project.zip


The project contains these pieces:

* Two versions of the same code with slight changes
* Scripts to launch Sling Composite Nodestore
* Project-local Repository

An **Update Cycle** is done like this:

* Build the samples
* Start the Sling Seed with Sample v1.0.0 and then the Composite Nodestore
* Restart the Sling Seed with Sample v1.0.1 and then the Composite Nodestore
* Check the changes and the read-only state

#### Build the Samples

As of now the samples need to be built from their folders:

    $ cd <project root folder>
    $ cd sling-composite-nodestore-project
    $ sh bin/build-samples.sh

#### Start Sling with Initial Version 1.0.0

This is a regular launch of Sling Composite Nodestore together with the
Sample project version 1.0.0:

    $ cd <project root folder>
    $ cd sling-composite-nodestore-project
    $ sh bin/create_seed_fm.sh
    [stop sling after fully started]
    $ sh bin/run_composite_fm.sh

After Sling is up and running go to the [Sling Starter Page](http://localhost:8080) and
login. Then click on the [Browse Content link](http://localhost:8080/bin/browser.html) and
have a look at **/content** and **/apps**.

**Note**: this time when Sling is restart into the Composite Nodestore mode
the **launcher** (Sling code base) is deleted before the launch.

#### Start Sling with the Updated Version 1.0.1

To Update Sling we need to launch it first in the **Seed Mode** so that
the read-only nodestore can be updated before starting the **Composite
Nodestore**:

    [Shutdown Sling]
    $ sh bin/create_updated_seed_fm.sh
    [stop sling after fully started]
    $ sh bin/run_updated_composite_fm.sh

### Check the Update

We will check the changes made by the Update of the Sample and also to make
sure that the /libs and /apps folders are still read-only.

These are updates to look for:

**Changes in Nodes**:

* /content/testContentUI/test - added version to description
* /apps/testAppsUI/install - added version to description
* /

**New Nodes**:

* /apps/testAppsAllMer - folder and sub nodes added
* /apps/testContentAllMer - folder and sub nodes added

**Removed Nodes**:

* /apps/testToBe/removed - nodes and parent node are removed during upgrade
* /content/testToBe/staying - nodes stays even though it was removed from project    

**Attention**: a project normally would not contain content except for an initial
seed and so it is not expected that content would be deleted. This is only here
to proof that fact.

## Mission Accomplished

* Next Up: [Launch Sling with Feature Launcher](/documentation/feature-model/howtos/start-sling-feature-launcher.html)
* Back To: [Feature Model Home](/documentation/feature-model/feature-model-overview.html)

## Addendum

### Scripts

The build and launch scripts are configurable in the **bin/setenv.sh** bash script.
Just adjust the variables there to your environment and you are good to go.

**Attention**: these scripts are geared towards a launch of an intial custom project and
then doing an update of that initial project. They can be adjusted by additional additional
projects, feature models or archives but that is not provided in the scripts.

### Nodestore Introspection

It is interesting to inspect the nodestore Sling is using to see what actually happened.
So we are going to shutdown Sling and then start the nodestore viewer with the path to
the nodestore:

    $ <shut down Sling>
    $ sh bin/explore_repo.sh sling/sling-composite/repository-libs/segmentstore


This will bring up a Java UI where the user can view the nodes. Keep in mind that children
of a node are only shown if the node is selected. So we click on **root**, then **libs**,
**content** and finally **content/slingshot** and we see:

![Content Slingshot in Nodestore Viewer](nodestore-viewer-content-slingshot.png)

**Note**: the node /content/slingshot is not containing the node **test-content1** we created
while running Sling.

Now why is this?

Well, when we started Sling the first time to create the **seed** we only created one
nodestore, then one we look at here. Even though we only need it later to service /apps
and /libs Sling will install everything into that nodestore including /content. 
When we start up Sling for the second time Sling will install the /content once more but this
time into the editable nodestore. 

It is left to the user to explore that nodestore and see the changes made by themselves.