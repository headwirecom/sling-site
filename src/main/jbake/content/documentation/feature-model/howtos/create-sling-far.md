title=How To Create a Feature Archive
type=page
status=published
tags=feature model,sling,kickstarter,feature archive
~~~~~~

### About this How-To

<div style="background: #cde0ea; padding: 14px; border-left: 10px solid #f9bb00;">

#### What we'll explore: 

* Create a custom Sling Feature Archive using the latest version of Sling

#### What you should know: 

* Skill Level: Intermediate
* Environment: Windows/Unix
* Time: 20 minutes

</div>

Back To: [Create your own Sling Feature Model for the Kickstarter](/documentation/feature-model/howtos/create-sling-fm.html)

### Prerequisites

In order to follow this how-to you'll need the following on your computer:

* Java 8
* Maven 3
* Bash shell
* Completed [Create your own Sling Feature Model for the Kickstarter](/documentation/feature-model/howtos/create-sling-fm.html)


### What's the Sling Feature Archive

A Feature Archive (FAR) is a ZIP file containing the Feature Model along with its dependencies.

### Explanation on what will happen

A Feature Archive is a fully self contained ZIP file with the Feature Model file and all its dependencies.
We will create such an Archive and the launch it. The startup will be significantly faster is the Kickstart
project / Feature Launcher does not have to download the dependencies.

### Step 1: Obtain Sling Starter and Kickstart Modules

**Note**: if both modules were already obtained by the previous (Create Sling Feature Model) then this entire
step can be skipped.


The Sling 12 Starter is not released yet and so have to obtain the project from its GitHub repository:

    $ cd <project root folder>
    $ git clone https://github.com/apache/sling-org-apache-sling-starter.git
    $ cd sling-org-apache-sling-starter
    $ pwd

The last step is printing the path to the Sling Starter module.

**Note**: to obtain the latest source code from the Kickstart GitHub repository see below in the Addendum. 

Now let's get the [Source Release of Sling Kickstart](https://repository.apache.org/content/groups/public/org/apache/sling/org.apache.sling.kickstart):

    $ cd <project root folder>
    $ curl https://repository.apache.org/content/groups/public/org/apache/sling/org.apache.sling.kickstart/0.0.4/org.apache.sling.kickstart-0.0.4-source-release.zip \
      > org.apache.sling.kickstart-0.0.4-source-release.zip
    $ jar -xvf org.apache.sling.kickstart-0.0.4-source-release.zip
    $ mv org.apache.sling.kickstart-0.0.4 sling-org-apache-sling-kickstart
    $ cd sling-org-apache-sling-kickstart


### Step 2: Build the Feature Archive

The build is straight forward from here:

    $ mvn -f sling-fm-pom.xml install -Dsling.starter.folder=<path to the sling starter module> -P create-far
    $ ls target/*.far

You should see a file listed with the name **org.apache.sling.kickstart.conversion-0.0.4-sling12archive.far**
which is the Sling Feature Archive.

### Step 3: Launch the Feature Archive

The most part was already done in the previous how-to. If you have not done that see below:

    $ cd <project root folder>
    $ cd sling-org-apache-sling-kickstart
    $ cp target/org.apache.sling.kickstart.conversion-0.0.4-sling12archive.far ../kickstart-run


These are the actions to execute if the previous how-to (Create Sling Feature Model) was not done:

    $ cd <project root folder>
    $ mkdir kickstart-run
    $ cd sling-org-apache-sling-kickstart
    $ cp target/org.apache.sling.kickstart.conversion-0.0.4-sling12archive.far ../kickstart-run
    $ mvn clean install
    $ cp target/org.apache.sling.kickstart-0.0.4.jar ../kickstart-run


Finally we can startup Sling:

    $ cd ../kickstart-run
    $ java -jar org.apache.sling.kickstart-0.0.4.jar -s org.apache.sling.kickstart.conversion-0.0.4-sling12archive.far


This will bring up Sling in all its glory.

## Mission Accomplished

* Next Up: [Create a Sling Composite Nodestore](/documentation/feature-model/howtos/create-sling-composite.html)
* Back To: [Feature Model Home](/documentation/feature-model/feature-model-overview.html)

## Addendum

### Obtain Sources from GitHub repositories:

To get the Kickstart sources:

    $ cd <project root folder>
    $ git clone https://github.com/apache/sling-org-apache-sling-kickstart.git
    $ cd sling-org-apache-sling-kickstart

**Attention**: If the repository is already cloned (you will find a **.git** folder in the project folder)
then you can **omit 'git clone'** and change into the project folder and do a **git pull** to get the latest
code changes if desired.

### Temporary Actions until Feature Archive is Released

This how-to assumes that the modules to handle a Feature Archive is released. Until this is done you need
to execute these actions in order to work through this how-to:

1. Clone, build and install sling-org-apache-sling-feature-launcher version 1.1.3-SNAPSHOT
2. Clone, build and install sling-org-apache-sling-feature-extension-content version 1.0.7-SNAPSHOT
3. Clone, build and install sling-slingfeature-maven-plugin version 1.3.1-SNAPSHOT
4. Adjust the version in the pom.xml as well as the sling-fm-pom.xml accordingly (set inside the &lt;properties> section)


### Feature Archive Structure

As mentioned above the Feature Archive (FAR) is a ZIP file with a META-INF/MANIFEST.MF file.
That file declares the Feature Model file this Archive is based one in the
**Feature-Archive-Contents** property as an Artifact Id
(groupId:artiactId:type:classifier:version). This allows the launcher to find the
Feature Model in the Archive as the structure of the Feature Archive is more or less
the same as a local Maven Repository. 
