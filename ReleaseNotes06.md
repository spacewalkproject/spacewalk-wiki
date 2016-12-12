Howdy Spacewalkers,

Spacewalk 0.6 is finally here! In a few hours, Spacewalk 0.6 will be available for installing. With this release, as with previous releases, Spacewalk 0.4 will no longer be available for download.

{{{
   http://spacewalk.redhat.com/yum/0.6/RHEL/5/<arch>/os/
   http://spacewalk.redhat.com/yum/0.6/Fedora/10/<arch>/os/
   http://spacewalk.redhat.com/yum/0.6/Fedora/11/<arch>/os/
}}}

Make sure to read over the installation again:
   http://fedorahosted.org/spacewalk/wiki/HowToInstall#Installation

If you are upgrading from 0.5 to 0.6, please checkout:
   http://fedorahosted.org/spacewalk/wiki/HowToUpgrade

== Features & Enhancements ==
 * Spacewalk 0.6 is available for Fedora 11!

 * support for SHA256 in rpm

 * large (2+GB) rpm support (requires rpm 4.6)

 * ability to import a yum repo into a channel
   https://fedorahosted.org/spacewalk/wiki/UploadFedoraContent

 * script based reporting: http://bit.ly/NyCrj

 * hopefully a better jabberd experience than in Spacewalk 0.5

 * KVM support

 * and more APIs:

   - errata.delete
   - channel.software.uploadPackage
   - monitoring.*
   - org.list*Entitlements
   - packages.getDetails
   - system.config.scheduleImport

== Bugs fixed ==
* This release fixed approximately 100 bugs -
  http://bit.ly/14ahTW

== Known issues ==
* PostgreSQL support does not work, but the infrastructure has been committed. We will need help with moving this forward.

* Fedora GPG key not recognized by rhnPackageKey table, packages will show up as unknown Provider.

* Documentation search does not work, other search are unaffected.

== Community ==
*  We greatly appreciate the contributions the community has made to this release. Thank you very much.

     - Gurjeet Singh
     - Joshua Roys
     - Mark Chappell
     - Maxim Burgerhout
     - Muhammad Farrukh
     - Satoru SATOH
     - Tom Lane
     - Vikram Rai

   http://fedorahosted.org/spacewalk/wiki/ContributorList

Installation Help
   http://fedorahosted.org/spacewalk/wiki/HowToInstall#Installation

---- 
jesus m. rodriguez