title=How to Create a Custom Feature Model Project
type=page
status=published
tags=feature model,sling,feature launcher, kickstarter
~~~~~~

### About this How-To

<div style="background: #cde0ea; padding: 14px; border-left: 10px solid #f9bb00;">

#### What we'll explore: 

* Create a custom Feature Model / Archive project
* Launch the project with Sling as a Feature Archive
* Launch the project with Sling using the Kickstarter

#### What you should know: 

* Skill Level: Intermediate
* Environment: Windows/Unix
* Time: 30 minutes

</div>

* Back To: [Create your own Sling Feature Model for the Kickstarter](/documentation/feature-model/howtos/create-sling-fm.html)
* Back Home: [Feature Model](/documentation/feature-model/feature-model-overview.html)

### Prerequisites

In order to follow this how-to you'll need the following on your computer:

* Java 8
* Maven 3
* Bash shell
* Completed [Create your own Sling Feature Model for the Kickstarter](/documentation/feature-model/howtos/create-sling-fm.html)


### What's a Feature Model project

A Feature Model project is a standard Maven project with the following additional features:

* Support for creating a Feature Archive


### Explanation on what will happen

1. First, we'll start by cloning a Git repository that defines a sample Feature Model project.
2. Second, we'll review the important sections of the POM.
3. Next, we'll build a Feature Archive from source.
4. Then, we'll launch it... TODO
5. Lastly, we'll launch it using the Kickstarter.

### Step 1: Get the sample Feature Model project

<div style="background: #fff3cd; padding: 14px; border-left: 10px solid #ffeeba;">

**TODO:** Create a sample application and host it on GitHub and update the documentation.

</div>

Begin by cloning the sample Feature Model project from GitHub.

    $ curl -L -O http://localhost:8820/documentation/feature-model/howtos/sling-with-custom-project-sample.zip
    $ jar -xf sling-with-custom-project-sample.zip 
    $ cd sling-with-custom-project-sample


### Step 2: Review the project layout

Here's a partial look at the project structure.

    ├── pom.xml
    ├── repository
    └── src
        └── main
            ├── java
            └── resources
                └── SLING-INF

The project is nearly identitcal to a traditional Sling bundle project with a couple of exceptions.

* `repository` - A local Maven repository to simplify this example. It contains the
   Sling Feature Maven Plugin and  Sling 12 Feature Archive dependencies.
* `src/main/resources/SLING-INF` - Folder for Sling content


TODO: continue editing here.

### Step 3: Build Sling Feature Archive

Due to its size the Sling Feature Archive is not included in the project local folder.
In order to make this available please go back to the [Create Sling Feature Archive](create-sling-far.html)
and build the Sling Feature Archive.

### Step 4: Build and Launch

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

### Step 5: Launch with the Kickstarter

The Sling Kickstart JAR file is included in the project-local repository. So we can easily start
it with:

    $ cd <project root folder>
    $ cd sample-feature-archive-project
    $ [IGNORE if already done]: mvn clean install
    $ java -jar \
      repository/org/apache/sling/org.apache.sling.kickstart/0.0.3-SNAPSHOT/org.apache.sling.kickstart-0.0.3-SNAPSHOT.jar \
      -af target/sample-feature-archive-project-1.0.0-samplefararchive.far


Additional project can be added by adding a **-f** option for each additional project.

## Mission Accomplished

* Back To: [Feature Model Home](/documentation/feature-model/feature-model-overview.html)
