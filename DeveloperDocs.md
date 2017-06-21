# *Developer Documentation*

This section contains information about developer topics.  You may also be interested in [User Documentation](UserDocs) and [downloads](DownloadIt).


----

----
## __Design docs__

 * [[Architecture]] -- a quick overview of the various components in Spacewalk

 * [[JavaDesign]] -- design of the Java web UI.
 * [Navi](NavigationSystem) -- our navigation system -- understand our own home grown navigation system used throughout the app
 * [[ListTag]] -- used for creating lists within the java pages   
 * [[AclDesign]] -- detailed explanation of ACLs
 * Database
   * [Development Guide](DatabaseGuide)
 * [JavaDoc](http://www.redhat.com/spacewalk/documentation/javadoc/) -- Documentation of Java code
 * [[PythonDocumentation]] -- Documentation of python code
 * [[XmlrpcHandlers]] -- understand how to deal with XMLRPC handlers
 * [[UnitTests]] -- how to run them
 * [[CodeAnalysis]] -- a document going through the java packages describing what they contain and what work is needed.
 * [CodeAnalysis](https://www.ohloh.net/p/spacewalk/analyses/latest) -- Analyze of code on Ohloh.net
 * [[TaskoMatic]] -- Document outlining what taskomatic is and what each task is used for.
 * [[CobblerIntegration]] - How spacewalk and cobbler interact
 * [Proxy](proxy) - How Proxy works
----
## __Working with the code__

 * [Git Guide](GitGuide) -- how to work with Git, the source control of choice for Spacewalk

 * [Git stats](https://github.com/spacewalkproject/spacewalk/graphs/contributors) -- boring statistics extracted from our git repo.
 * [Setting up the Development environment](DevelopmentWorkstationSetup)
 * [[EclipseSetup]] -- How to get eclipse running to edit/compile/junit the spacewalk-java code.
 * [[PatchProcess]] -- how to go about creating a new patch and submitting it to the mailing list
 * [[BugHandling]] -- how to commit a fix and what to do with bugzillas.
 * [[ChangingSchema]] -- if you need to change the Spacewalk DB schema read this
 * [[CodingConventions]] -- coding style and conventions used in the project
 * [Tracing a page](TracingaPage) -- Follow code flow from the webUI down to the DB layer
 * [[AddingPagesOverview]] -- Adding a page to the Java stack
 * [[Branching]] -- how branches are organized in the git repo.
 * [[LocalIvyRepo]] -- how to create a local ivy repository
 * [[CustomSerializer]] -- how to create a custom serializer
 * [[LogDriverSetup]] -- how to enable the log4j logdriver utility to see SQL output and params used
 * [[HowToScratchBuild]] -- how to create a test rpm with scratch build

----
## __Releasing the code__

 * [[ReleaseProcess]] -- Process for building RPMs

 * [List of packages in releases](http://miroslav.suchy.cz/spacewalk/packages-overview/) -- including packages already in Fedora. Contains version of each package for each distribution.
 * [Koji](http://koji.spacewalkproject.org/koji/) -- Current build status.
----
## __Artwork__

 * [Artwork and Design resources](ArtAndDesignStuff)

----
## __Misc__

 * [Brainstorming](BrainBox)

 * [Contributor List](ContributorList)
 * [Projects](Projects) - libraries, which may be useful for others as well and for which we are upstream.