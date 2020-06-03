title=Feature Model
type=page
status=published
tags=feature model,osgi,project,guide,howtos
~~~~~~

## A How-To Guide to Feature Models

The Sling Feature Model[1] is a replacement for the Sling Provisioning Model. In general, both models accomplish the same goal
of defining and assembling an OSGi-based application or part of an application. However, the Feature Model provides a more
robust method of application definition and assembly that isn't tied to Sling. It specifies a number of new concepts that
address end-to-end application packaging that include support for bundles, configuration, framework properties, capabilities,
requirements and custom artifacts. 

### Key Concepts

* [Features](https://github.com/apache/sling-org-apache-sling-feature/blob/master/docs/features.md) - The central entity of the 
   Feature Model used to logically package and group metadata, configuration, bundles and 
   extensions that represent a particular piece of system functionality. 
* [Feature Extension](https://github.com/apache/sling-org-apache-sling-feature/blob/master/docs/extensions.md) - An extension
   point allowing users to customize the Feature Model to support new capabilities. 
* [Feature Archives](https://github.com/apache/sling-org-apache-sling-feature/blob/master/docs/feature-archives.md) - Packages one
   or more features and dependencies to simplify the distribution of a complete application.
* [Feature Reference Files](https://github.com/apache/sling-org-apache-sling-feature/blob/master/docs/feature-ref-files.md) - A
   text descriptor file with a list of features.
* [Feature Aggregation](https://github.com/apache/sling-org-apache-sling-feature/blob/master/docs/aggregation.md) - GG: Need some help here

## Exploring Feature Models by Example

* [How To Start Sling with the Kickstarter](/documentation/feature-model/howtos/kickstart.html)
* [Deep Dive into Creating a Sling Feature Model](/documentation/feature-model/howtos/create-sling-fm.html)
* [How To Create a Sling Feature Archive](/documentation/feature-model/howtos/create-sling-far.html)
* [How to Create a Sling Composite Node Store](/documentation/feature-model/howtos/create-sling-composite.html)
* [How to Launcher Sling with the Feature Launcher](/documentation/feature-model/howtos/start-sling-feature-launcher.html)
* [Create a Custom Feature Project and Launch it](/documentation/feature-model/howtos/sling-with-custom-project.html)

## Related Advanced Reading

Want to know more? Have a look at the readme files of the projects making up the feature model support in our ecosystem.

* [1] [Feature Model](https://github.com/apache/sling-org-apache-sling-feature/blob/master/readme.md)
* [Feature Docs](https://github.com/apache/sling-org-apache-sling-feature/blob/master/docs/features.md)
* [Feature Extensions](https://github.com/apache/sling-org-apache-sling-feature/blob/master/docs/extensions.md)
* [Feature Archive](https://github.com/apache/sling-org-apache-sling-feature/blob/master/docs/feature-archives.md)
* [Feature References](https://github.com/apache/sling-org-apache-sling-feature/blob/master/docs/feature-ref-files.md)
* [Feature Aggregation](https://github.com/apache/sling-org-apache-sling-feature/blob/master/docs/aggregation.md)
* [SlingFeature Maven Plugin](https://github.com/apache/sling-slingfeature-maven-plugin)
* [Feature Launcher](https://github.com/apache/sling-org-apache-sling-feature-launcher)
* [Kickstart Launcher](https://github.com/apache/sling-org-apache-sling-kickstart/blob/master/Readme.md)
