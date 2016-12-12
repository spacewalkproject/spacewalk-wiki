
= __Spacewalk 1.7 Release Notes__ =

Hello friends of the walks in space,

we are happy to announce the release 1.7 of Spacewalk, a systems management solution.

The download locations are

  * http://spacewalk.redhat.com/yum/1.7/RHEL/5/$basearch/ 
  * http://spacewalk.redhat.com/yum/1.7/RHEL/6/$basearch/ 
  * http://spacewalk.redhat.com/yum/1.7/Fedora/15/x86_64/ 
  * http://spacewalk.redhat.com/yum/1.7/Fedora/16/x86_64/ 

with client repositories under 

  * http://spacewalk.redhat.com/yum/1.7-client
  * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/1.7/openSUSE_11.4/
  * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/1.7/openSUSE_12.1/
  * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/1.7/openSUSE_Factory/
  * http://miroslav.suchy.cz/spacewalk/debian

For fresh installations, please use steps from

  * https://fedorahosted.org/spacewalk/wiki/HowToInstall 

If you plan to upgrade from older release, the following page
can be helpful:

  * http://fedorahosted.org/spacewalk/wiki/HowToUpgrade 

== Features & Enhancements in Spacewalk 1.7 ==

  * Monitoring is now working on the [wiki:PostgreSQL] database backend, making both database backends feature-comparable
  * [wiki:Scap OpenSCAP] integration
  * Fully asynchronous clearing of orphaned monitoring data
  * Stock rhnpush is able to push debian packages
  * New spacewalk-clone-by-date tool
  * Numerous spacecmd enhancements
  * Improved messages printed when certificate activation fails
  * Modified API calls:
     * channel.software.getDetails returns empty strings instead of omitting attributes
     * configchannel.createOrUpdatePath, createOrUpdateSymlink, getEncodedFileRevision, getFileRevision, getFileRevisions, lookupFileInfo, system.config.createOrUpdatePath, createOrUpdateSymlink, lookupFileInfo, and system.provisioning.snapshot.listSnapshotConfigFiles now return contents_enc64 field which identifies whether contents is Base64-encoded
     * system.comparePackages and system.comparePackageProfile return package_arch value
     * system.provisionVirtualGuest default values now match those on the WebUI
  * New API calls:
     * configchannel.getEncodedFileRevision
     * system.scap.scheduleXccdfScan

The up-to-date API documentation can be found at http://spacewalk.redhat.com/documentation/api/

== Contributors ==

Our thanks go to the community members who contributed to this release: 

  * Aron Parsons
  * Jiří Mikulka
  * Joerg Steffens
  * Joshua Roys
  * Michael Calmer
  * Pierre Casenove
  * Simon Lukasik
  * Speagle, Andy
  * Steven Hardy

https://fedorahosted.org/spacewalk/wiki/ContributorList

You can view the history of Spacewalk from its start to 1.7 at

   http://www.youtube.com/watch?v=jsx6cGmBtzg

== Some statistics ==

In Spacewalk 1.7, we've seen

    * 49  bugs fixed 
    * 1246 changesets committed 
    * 1581 commits done 

== Reporting errors ==

To report issues with Spacewalk, please use mailing list spacewalk-list@redhat.com to reach the user community. We might ask you to file bugzilla at http://bugzilla.redhat.com/ with more details or full logs. From now on, errors related to PostgreSQL database backend are considered bug defects vs. missing capabilities.

Thank you for using Spacewalk.

