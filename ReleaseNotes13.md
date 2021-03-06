# __Spacewalk 1.3 Release Notes__



Hello world,

Spacewalk 1.3 is now available for download from

    * http://spacewalk.redhat.com/yum/1.3/RHEL/5/$basearch/
    * http://spacewalk.redhat.com/yum/1.3/RHEL/6/$basearch/
    * http://spacewalk.redhat.com/yum/1.3/Fedora/13/$basearch/
    * http://spacewalk.redhat.com/yum/1.3/Fedora/14/$basearch/

depending on your operating system, with client repositories under

    * http://spacewalk.redhat.com/yum/1.3-client/

Check the installation steps at

    * [[HowToInstall]]

or if you will upgrade from older release, consult

    * [[HowToUpgrade]]
## Features & Enhancements in Spacewalk 1.3



        * Spacewalk can now be installed and run on RHEL 6
        * Server side support for apt-get
        * Spacewalk on PostgreSQL can be upgraded from older versions
        * Spacewalk on Oracle now uses Oracle InstantClient version 11g
        * Spacewalk Proxy works with Spacewalk on PostgreSQL
    * tnsnames.ora file no longer needed
        * New API calls:
        * system.getUuid
        * system.search.uuid
        * errata.publishAccordingToParents
    * Extended API calls:
        * system.listPackages by id
        * channel.software.getDetails,
          channel.software.listChildren,
          kickstart.listKickstartableChannels,
          system.listSubscribedChildChannels,
          system.getSubscribedBaseChannel by last_modified
## Some notes about apt-get plug-in



Spacewalk 1.3 contains server support for apt-get plug-in.
An experimental apt-get plug-in client package is available at
http://miroslav.suchy.cz/spacewalk/debian/
Feel free to try it and report any issues.
This plug-in package will be enhanced during the Spacewalk 1.4 cycle.

For more information check:
    * [[RegisteringClients]]
## Some notes about the PostgreSQL support



PostgreSQL support is limited similar to Spacewalk 1.2.

    * [[PostgreSQL]]
## Community contributors



We thank the community members who contributed to this release:

    * Ani Peter
    * Aron Parsons
    * Colin Coe
    * Luc de Louw
    * Johannes Renner
    * John van Zantvoort
    * Marcelo Moreira de Mello
    * Paresh Mutha
    * Simon Lukasik

    * [[ContributorList]]
## Bug fixes and commits

In Spacewalk 1.3 there were:

 * 123 bugs solved
 * 964 changesets committed
 * 1275 commits done






Yours, 

Tomas Lestach 

RHN Satellite Engineering, Red Hat
