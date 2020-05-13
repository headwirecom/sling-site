title=Create a Sling with Composite Nodestore 
type=page
status=published
tags=feature model,sling,kickstart
~~~~~~

### How-To Overview

<div style="background: lightblue;">

* What will you learn: 
	* Creating your own Sling Instance with a Composite Nodestore

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

### Step 1: Checkout the Branch

Here the Sling Kickstart is still used but the code is in another branc so we need to check that one out:

    $ cd <project root folder>
    $ cd sling-org-apche-sling-kickstart
    $ git checkout feature/composite-node-store
    $ git branch


The last action is just there to confirm that the branch indeed switched.

### Step 2: Build the Kickstart

We still need to build the Kickstart project as we need the launching ability:

    $ mvn clean install

### Step 3: Run the Initialization Script

First check that there is no **sling** or **launcher** folder. If there is copy them
away or delete them alltogether.
 
We need to start Sling now as we would normally do:

    $ ./create_seed_fm.sh


This will start Sling with these two Feature Models:

1. sling_fm.json: Sling Feature Model without a Nodestore
2. feature-two-headed-seed.json: Feature Model with a single Nodestore

When Sling is down startup up shut it down.

### Step 4: Run Sling with a Composite Nodstore

Now we are ready to launch Sling for public usage:

    $ ./run_composite_fm.sh

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


## Mission Accomplished

* Back To: [Feature Model Home](/documentation/feature-model/feature-model-overview.html)

## Addendum

### Nodestore Introspection

It is interesting to inspect the nodestore Sling is using to see what actually happened.
So we are going to shutdown Sling and then start the nodestore viewer with the path to
the nodestore:

    $ <shut down Sling>
    $ ./explore_repo.sh sling/sling-composite/repository-libs/segmentstore


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