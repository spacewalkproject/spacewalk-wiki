# __Spacewalk 1.1 Release Notes__

Hello everyone,


Spacewalk 1.1 has been released!

Server:
 * http://spacewalk.redhat.com/yum/1.1/RHEL/5/<arch>/
 * http://spacewalk.redhat.com/yum/1.1/Fedora/12/<arch>/
 * http://spacewalk.redhat.com/yum/1.1/Fedora/13/<arch>/

Client:
 * http://spacewalk.redhat.com/yum/1.1-client/RHEL/5/<arch>/
 * http://spacewalk.redhat.com/yum/1.1-client/Fedora/12/<arch>/
 * http://spacewalk.redhat.com/yum/1.1-client/Fedora/13/<arch>/

For new installations, consult:
 * https://fedorahosted.org/spacewalk/wiki/HowToInstall

For upgrades, consult:
 * https://fedorahosted.org/spacewalk/wiki/HowToUpgrade
## Features and enhancements

 * First Spacewalk release built in a [publicly available build system](http://koji.spacewalkproject.org/koji).

 * Spacewalk 1.1 runs on Fedora 13
 * Introduction of [spacecmd](https://fedorahosted.org/spacewalk/wiki/spacecmd), a command line interface to Spacewalk.
 * Support for [synchronization of comps files](https://fedorahosted.org/spacewalk/wiki/Features/CompsSyncing).
 * support for staging content - ability to have all updates pulled off Spacewalk onto registered systems prior to the start of maintenance window
 * support for [eliminating orphaned (duplicate) profiles](https://fedorahosted.org/spacewalk/wiki/DuplicateProfiles).
 * new API calls:
  * channel.software.getChannelLastBuildById
  * configchannel.listSubscribedSystems
  * kickstart.profile.downloadRenderedKickstart
  * org.setSoftwareFlexEntitlements
  * schedule.rescheduleActions
  * system.convertToFlexEntitlement
  * system.deletePackageProfile
  * system.deleteSystem
  * system.listDuplicatesByHostname
  * system.listDuplicatesByIp
  * system.listDuplicatesByMac
  * system.listEligibleFlexGuests
  * system.listFlexGuests
  * system.listLatestAvailablePackage
  * system.listPackageProfiles
  * systemgroup.scheduleApplyErrataToActive
 * localization updates
## Known issues



 * Wrong tomcat6 directory permissions on Fedora 13
  * https://bugzilla.redhat.com/show_bug.cgi?id=574593
  * https://bugzilla.redhat.com/show_bug.cgi?id=586364
  * https://bugzilla.redhat.com/show_bug.cgi?id=605335
 * workaround:
{{{ 
chmod g+w /var/log/tomcat6 /etc/tomcat6/Catalina/localhost /var/cache/tomcat6 /var/cache/tomcat6/temp /var/cache/tomcat6/work
}}}

 * cobbler - related SELinux denials on Fedora 12 and Fedora 13:
  * https://bugzilla.redhat.com/show_bug.cgi?id=620503
  * https://bugzilla.redhat.com/show_bug.cgi?id=621095
  * solution: install updated selinux-policy-targeted as noted in the above bugs

 * Deprecation warning during osa-dispatcher start on Fedora 12 and Fedora 13:
  * https://bugzilla.redhat.com/show_bug.cgi?id=621204
  * https://bugzilla.redhat.com/show_bug.cgi?id=621206

 * Documentation search does not work, other searches are unaffected
## Contributors

Thank you goes out to the following people who contributed to Spacewalk 1.1 release:


 * Aron Parsons
 * Colin Coe
 * James Hogarth
 * Joshua Roys
 * Lukas Durfina
 * Maxim Burgerhout
 * Paul Morgan
 * Satoru SATOH

Regards
 -- Milan Zazrivec