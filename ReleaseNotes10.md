[[PageOutline]]

= __Release Notes 1.0__ =
Heya Spacewalkers,

Spacewalk 1.0 has been released!

Server:
 * http://spacewalk.redhat.com/yum/1.0/RHEL/5/<arch>/
 * http://spacewalk.redhat.com/yum/1.0/Fedora/11/<arch>/
 * http://spacewalk.redhat.com/yum/1.0/Fedora/12/<arch>/

Client:
 * http://spacewalk.redhat.com/yum/1.0-client/RHEL/5/<arch>/
 * http://spacewalk.redhat.com/yum/1.0-client/Fedora/11/<arch>/
 * http://spacewalk.redhat.com/yum/1.0-client/Fedora/12/<arch>/

Make sure to read over the installation again:

 * http://fedorahosted.org/spacewalk/wiki/HowToInstall

If you are upgrading from older release, please checkout:

 * http://fedorahosted.org/spacewalk/wiki/HowToUpgrade

== Features & Enhancements ==
 * RHN Client code improvements. Now using python-ugdev and dropped HAL. 
 * [wiki:SpacewalkHostnameRename] script for re-configuration of Spacewalk server, in case its hostname or IP has changed
 * Many [wiki:ApiAdditions] as well as improvements to existing API calls. 
 * RHN Proxy now uses mod_wsgi
 * Tomcat 6 for Fedora 12
 * Delete Systems script 
 * Many bugfixes and general code clean up to support 1.0 release

== Known issues ==
 * Provider GPG key is not sometimes recognized and packages shows up as unknown Provider.
 * PostgreSQL support still does not work. We will need help with moving this forward. 
 * Documentation search does not work, other search are unaffected. 
 
== Community ==
We greatly appreciate the contributions the community has made to this release. Thank you very much.
 * Colin Coe
 * James Hogarth
 * Jason Lawer
 * Joshua Roys
 * Lukas Durfina
 * Simon Lukasik

 * [wiki:ContributorList]

== Installation Help ==
 * [wiki:HowToInstall]