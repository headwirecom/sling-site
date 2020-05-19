title=Start Sling with a Custom Project
type=page
status=published
tags=feature model,sling,feature launcher
~~~~~~

### How-To Overview

<div style="background: lightblue;">

* What will you learn:
	* Creating a Custom Feature Model / Archive Project
    * Launch it with Sling Feature Archive
    * Launch it with Sling Kickstart

* Time: 15 minutes
* Skill Level: Beginner
* Environment: Windows/Unix
</div>

* Back To: [Feature Model Home](/documentation/feature-model/feature-model-overview.html)

### Prerequisites

In order to follow through this HowTo you need the following on your computer:

* Java 8
* Maven 3
* Command Line with Bash

### Custom Feature Model Project

This is an introduction on how to create a Project in the age of Feature Models. 

### Explanation on what will happen

A Feature Model can be created from a **Maven POM** file and then be used to either
launch it or create a Feature Archive and the launch that one.
We are going to create a Sling Project, create a Bundle and then aggregate this into
a Feature Archive. Finally we will launch it together with Sling.

### Step 1: Create a Feature Model Project

The source code to this project can be [downloaded here](sample-feature-archive-project-1.0.0.zip).
It is a ZIP file so just uncompress it and have a look at it. These are the main parts:

* a Bundle POM file
* a **src/main/java** folder for the Java sources
* a **src/resources/SLING-INF** folder for the Sling content
* a **repository** folder for project-local Maven content

This is a straight forward Sling Content Bundle where the code is in its regular place
but the content is going into the resource folder. Because the folder will go into the
bundle it is suggested to name it **SLING-INF** (here) or **SLING-CONTENT** or so.
**Attention**: when the bundle is installed the **SLING-INF** folder is falling away but
the name might help when introspecting the bundle ZIP file.

### Step 2: Use of Project Local Repository

In order to speed things up this project comes with a **project-local** repository for
both the Slingfeature Maven Plugin (using unreleased plugin) and the Sling 12 Feature
Archive.
A **project-local** is a **Maven Repository** inside the project and better than like
**system dependencies** and dependencies can be easily copied over from your local (.m2)
folder into your project-local folder.
Adding this to your project is pretty simple:

    <repositories>
       <repository>
          <id>project.local</id>
          <name>project</name>
          <url>file:${maven.multiModuleProjectDirectory}/repository</url>
       </repository>
    </repositories>
    <pluginRepositories>
      <pluginRepository>
         <id>project.local</id>
         <name>project</name>
         <url>file:${maven.multiModuleProjectDirectory}/repository</url>
      </pluginRepository>
    </pluginRepositories>


**Attention**: the **url** path needs to be relative to the POM that contains the
dependencies. So if your project-local repository is defined in a parent POM but then
used in a child POM the folder found by **file:${maven.multiModuleProjectDirectory}**
is your current folder so the repository is in the parent folder then the path
needs to go up a directory with **..**.

**Note**: if the project-local repository contains plugins then the **&lt;pluginRepositories/>**
must be provided otherwise it can be omitted. Also if only plugins are provided then
the **&lt;repositories/>** can be omitted.

### Step 3: Build and Launch

Build and launching is just a matter of running the Maven build:

    $ cd <project root folder>
    $ mkdir sample-feature-archive-project
    $ cd sample-feature-archive-project
    $ jar -xvf <downloaded project ZIP file>
    $ mvn clean install -P launch 


Sling comes up and you can [login here](http://localhost:8080/) using the default *admin/admin*.
The click on the [System Console Link](http://localhost:8080/system/console/bundles) which will
bring you to the OSGi Bundles list. Click on the **filter box** and enter *sample* and hit enter.
You will see that your project bundles was successfully installed:

![Installed Sample FAR Bundle](sample.far.project.bundles.png)

Now go back to the [Sling Starter Page](http://localhost:8080) and click on
[Browse Content](http://localhost:8080/bin/browser.html) to have a look at what the
**Sample Content Bundle** installed:

![Sample FAR Project Content](sample.far.project.jcr.content.png)

**Note**: there is not limit to add additional custom projects. The only thing that needs to be
done is to build the custom projects first and then add the Maven Id to the **featureArchiveIds**
in the Slingfeature Launcher.

### Step 4: Launch with the Kickstarter

The Sling Kickstart JAR file is included in the project-local repository. So we can easily start
it with:

    $ cd <project root folder>
    $ cd sample-feature-archive-project
    $ [IGNORE if already done]: mvn clean install
    $ java -jar \
      repository/org/apache/sling/org.apache.sling.kickstart/0.0.3-SNAPSHOT/org.apache.sling.kickstart-0.0.3-SNAPSHOT.jar \
      -s repository/org/apache/sling/org.apache.sling.feature.archive.starter/0.0.2/org.apache.sling.feature.archive.starter-0.0.2-sling12archive.far \
      -f target/sample-feature-archive-project-1.0.0-SNAPSHOT-samplefararchive.far


Additional project can be added by adding a **-f** option for each additional project.

## Mission Accomplished

* Next Up: [Custom Feature Project with Sling](/documentation/feature-model/howtos/sling-with-custom-project.html)
* Back To: [Feature Model Home](/documentation/feature-model/feature-model-overview.html)
