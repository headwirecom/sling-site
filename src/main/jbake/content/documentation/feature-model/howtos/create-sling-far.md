title=Create your own Sling Archive for the Kickstart 
type=page
status=published
tags=feature model,sling,kickstart
~~~~~~

<div style="background: lightblue;">

* What will you learn: 
	* Creating your own Sling Feature Archive with the latest from Sling

* Time: 20 minutes
* Skill Level: Intermediate
* Environment: Windows/Unix

</div>

[Back to the Feature Model Home](/documentation/feature-model/feature-model-overview.md)

### Prerequisites

In order to follow through this HowTo you need the following on your computer:

* Java 8
* Maven 3
* Command Line with Bash
* Worked through *Create your own Sling Feature Model for the Kickstart*

### What is the Sling Feature Archive

* a feature archive (FAR) is a ZIP file containing the feature model file together with all dependencies

### Explanation on what will happen

A Feature Archive is a fully self contained ZIP file with the Feature Model file and all its dependencies.
We will create such an Archive and the launch it. The startup will be significally faster is the Kickstart
project / Feature Launcher does not have to download the dependencies.

### Temporary Actions until Feature Archive is Released

The Feature Archives are on the bleeding edge requiring the user to clone, build and install under
development Sling Modules and need to adjust the versions inside the Sling Kickstart POM.
So these are the steps necessary:

1. Clone, build and install sling-org-apache-sling-feature-launcher version 1.1.3-SNAPSHOT
2. Clone, build and install sling-org-apache-sling-feature-extension-content version 1.0.7-SNAPSHOT
3. Clone, build and install sling-slingfeature-maven-plugin version 1.3.1-SNAPSHOT
4. Adjust the version in the pom.xml as well as the sling-fm-pom.xml accordingly (set inside the &lt;properties> section)

### Step 1: Adjust the Kickstart POM File

We will use the Kickstart project as the base for creating the Feature Archive as it has already in
place to create the Sling FM. To avoid issues with Git we will create a copy and then remove .git folder
just to be on the safe side.

    $ cd <project root folder>
    $ cp -R sling-org-apache-sling-kickstart kickstart-with-far
    $ cd kickstart-with-far
    $ rm -rf ./target ./.git


Now we can adjust the POM file by adding the goal **attach-featurearchives** of the
**Slingfeature Maven Plugin**. The classifier is the one set on the FAR and the
**includeClassifier** is the classifier of the Feature Models to be included into the FAR.

1. Open sling-fm-pom.xml file in text editor
2. Search for **slingfeature-maven-plugin** execution with id **aggregate-base-sling-feature**
3. Add the execution snippet below after the execution we found

FAR execution snippet:

    <execution>
        <id>create-sling-feature-archives</id>
        <goals>
            <goal>attach-featurearchives</goal>
        </goals>
        <configuration>
            <archives>
                <archive>
                    <classifier>sling12archive</classifier>
                    <includeClassifier>sling12</includeClassifier>
                </archive>
            </archives>
        </configuration>
    </execution>


### Step 2: Build the Feature Archive

The build is straight forward from here:

1. Execute the build with: `mvn clean install -f sling-fm-pom.xml -Dsling.starter.folder=<path to the sling starter module>`
2. The **target** folder now contains the file: **org.apache.sling.kickstart.conversion-&ltVERSION>-sling12archive.far**

**Note**: the build could fail due to IANAL plugin reports an issue of missing legal files but only after the
Feature Archive is built so we can ignore that for now.

### Step 3: Launch the Feature Archive

The most part was already done in the previous Howto. If you have not done that see below:

    $ cd <project root folder>
    $ cd sling-org-apache-sling-kickstart
    $ cp target/org.apache.sling.kickstart.conversion-&ltVERSION>-sling12archive.far ../kickstart-run


These are the actions to execute if the previous Howto was not done:

    $ cd <project root folder>
    $ mkdir kickstart-run
    $ cd sling-org-apache-sling-kickstart
    $ cp target/org.apache.sling.kickstart.conversion-&ltVERSION>-sling12archive.far ../kickstart-run
    $ mvn clean install
    $ cp target/org.apache.sling.kickstart-<VERSION>.jar ../kickstart-run


Finally we can startup Sling:

    $ cd ../kickstart-run
    $ java -jar org.apache.sling.kickstart-<VERSION>.jar -s org.apache.sling.kickstart.conversion-&ltVERSION>-sling12archive.far


This will bring up Sling in all its glory.

## Mission Accomplished

next up: create a feature archive (FAR) 

## Addendum

### Feature Archive Structure

As mentioned above the Feature Archive (FAR) is a ZIP file with a META-INF/MANIFEST.MF file.
That file declares the Feature Model file this Archive is based one in the
**Feature-Archive-Contents** property as an Artifact Id
(groupId:artiactId:type:classifier:version). This allows the launcher to find the
Feature Model in the Archive as the structure of the Feature Archive is more or less
the same as a local Maven Repository. 
