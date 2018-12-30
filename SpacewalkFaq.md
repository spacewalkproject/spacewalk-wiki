# Frequently Asked Questions

## General


### What is Spacewalk

That's the first thing posted here:  See https://github.com/spacewalkproject/spacewalk :)


### How long has Spacewalk been around?

In 2000, Red Hat launched a hosted update service called *Red Hat Network (RHN)*.

It wasn't long though, before Red Hat customers began clamoring for an on-premise point-of-presence solution. In 2001, *Red Hat Network Proxy Server* was launched to serve this need, helping to bridge network layers and scale infrastucture beyond a few managed servers.

Though popular as a product, RHN Proxy Server was not sufficient for real application life cycle or more complex system management. *Red Hat Network Satellite Server* was released for the first time in April 2002 to serve these needs. RHN Satellite Server took off and now is used to manage servers performing a miriad of workloads -- from military applications to banking and financial services, to airlines, to ecommerce behomoths, to retail, to manufacturing plants. RHN Satellite is used everywhere.

Viewed as a service, RHN Satellite and Proxy Servers languished as proprietary offerings for some time. Finally, in June 2008, the open source, community-drive project, *Spacewalk*, was born and remains the upstream project for the very popular and successful Red Hat Network Satellite and Proxy Server family of products.

On April 30th, 2010, Spacewalk v1.0 was released, achieving an important milestone for this project.

On November 19th, 2010, Spacewalk v1.2 was released which was the first release installable and usable with PostgreSQL database backend (albeit with limited feature set), making Spacewalk a full open-source solution.

On March 7th, 2012, Spacewalk v1.7 was released, making both database backends feature-comparable.
### Who designed Spacewalk logo? Can I get hi-res picture?



See [Logo](Logo) page.
## Installation

### Is a Satellite/Spacewalk certificate required to use Spacewalk?


Traditionally for Red Hat Satellite a Satellite certificate was required to use the product and listed what channel families and entitlements (and how many) the customer had purchased.  Spacewalk still uses a certificate, but it is provided for free and activated upon installation.  You shouldn't ever have to worry about it expiring or about running out of entitlements.  

### Help - Spacewalk requires Oracle 11, while OracleXE is only 10



Spacewalk only requires Oracle InstantClient 11g, not server 11g. Instant Client is just client libraries and it is completely different thing from database itself. You can use Oracle InstantClient 11g to connect to Oracle Database 11g or 10g or Oracle XE (10g).
### Should I give the PostgreSQL version a try?



Yes.
## Problems?

### I got Internal Server Error. Please help!




Investigate logs on Spacewalk Server:


    tail -f /var/log/rhn/* /var/log/httpd/* /var/log/tomcat*/*

Do not panic. Try to interpret those errors and if you could not solve it yourself, then send those errors (just them, not whole files) to mailing list.
## Spacewalk Architecture

### Why do you use Oracle?  Any plans for supporting other databases?




Besides Oracle database backend, Spacewalk can be used with [[PostgreSQL]] database server.
### What if I don't have Oracle?



You can use [Oracle Express Edition](OracleXeSetup), or [[PostgreSQL]] database server.
### Perl, Python, and Java OH MY!
  


"Why do you use 3 different languages in your product?"   The original WebUI for the Spacewalk code base was purely Perl.  As time went on scalability issues came in to play, which was worrisome because developers who knew (and wanted to develop in) Perl were somewhat hard to come by.  This was to be solved by migrating to Java.  Sadly migrating to Java took longer to accomplish than anticipated and thus is not complete.  All new development is done in either Java or Python, we are only maintaining the Perl at this point in hopes of migrating it all to Java.  We continually migrate pages to Java as new features need to be added to the pages, but it is not a top priority.
### Why are there two XML-RPC API endpoints?



Originally there was only a backend/business-logic API (within the application's Python layer) making the Satellite server accessible to clients via XML-PRC.  Documents were not published for this API and it wasn't intended to be used by anything but the pre-built client utilities (rhn_register, etc..).  The frontend/WebUI API (within the application's WebUI Java/Perl layer) was created so that users could write scripts and programs to interact with the Satellite and perform the same actions available within the UI.  This was done in the Java side of things since all of the manager code is already in Java and is well documented.
### What languages can I use to interact with the XML-RPC APIs?



Any language that has XML-RPC bindings can be used to interact with the Spacewalk/Satellite APIs. XML-RPC is a common RPC standard for which most languages have bindings. So, you can exploit the available APIs with Python, TCL, Perl, or Ruby... or even C++ and .Net if you so desire. Many of our examples will be written in Python since it is the simplest and easiest to understand, but don't let that stop you from writing your API scripts in the language of your choosing.
### What OSes are supported?



Currently, Spacewalk server can be run on Fedora and RHEL (and its derivatives like CentOS). It can manage the same operating systems, plus there is limited support for Debian-based clients and SuSE-based client.
## The Future
 

### What about integrating technology X?



We are looking to include "best of breed" open source technology where possible and this is one of our major priorities. One of the things we did was integration of [Cobbler](_cobbler_) for provisioning in Spacewalk/Satellite.  We are also going to be looking at upgrading other components over time as well.  Input is always welcome.  We are also looking to break Spacewalk down into smaller more consumable subprojects to make them usable by other projects and tools.   See also [the roadmap](_spacewalk_roadmap).  If there is something you are interested in, the best thing to do would be to bring it up on the lists.
### What kind of community contributions are you interested in?



Everything!  Ideas, feedback, testing, documentation fixes, and most importantly code patches are all welcome.  If there is something you don't like or would like to see improved, let us know, ideally with a patch.  See the [Communication](_spacewalk_) section on the bottom page for some initial pointers. Some things might be relatively complex for newcomers feature wise but we're trying to simplify and document things as much as possible to make things easy to dive in and get involved with.  If you see an area where the documentation is lacking, remember that this is a Wiki, so you can help expand our documentation sections as well.
