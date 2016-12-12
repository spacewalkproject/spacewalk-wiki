[[PageOutline]]

= __Release Notes 0.8__ =
Heya Spacewalkers,

Spacewalk 0.8 has been released!

Server:
 * http://spacewalk.redhat.com/yum/0.8/RHEL/5/<arch>/
 * http://spacewalk.redhat.com/yum/0.8/Fedora/11/<arch>/
 * http://spacewalk.redhat.com/yum/0.8/Fedora/12/<arch>/

Client:
 * http://spacewalk.redhat.com/yum/0.8-client/RHEL/5/<arch>/
 * http://spacewalk.redhat.com/yum/0.8-client/Fedora/11/<arch>/
 * http://spacewalk.redhat.com/yum/0.8-client/Fedora/12/<arch>/

Make sure to read over the installation again:

 * http://fedorahosted.org/spacewalk/wiki/HowToInstall

If you are upgrading from older release, please checkout:

 * http://fedorahosted.org/spacewalk/wiki/HowToUpgrade

== Features & Enhancements ==
 * SHA256 Feature - support for packages with checksums other than MD5. Spacewalk server and proxy can serve and accept rpms with SHA256 digests, show the checksums in webUI and accept them in API calls.
 * Moved from mod_python to mod_wsgi on Fedora 11 and Fedora 12, which promises better performance.
 * Improved performance of System Set Management when working with thousands of packages.
 * Improved performance of API.
 * Fixed number of bugs on Fedora 12 both on client and server side.
 * Improved kickstarts and kickstating via proxy.
 * Improved client hardware info reporting.
 * Basic support for cobbler 2.0 version.

...and many bugfixes.

== Known issues ==
 * PostgreSQL support still does not work. We will need help with moving this forward.
 * Provider GPG key is not sometimes recognized and packages shows up as unknown Provider.
 * Documentation search does not work, other search are unaffected.
 * Cobbler 2.0 is known to work, but full tests haven't been done yet.

== Community ==
We greatly appreciate the contributions the community has made to this release. Thank you very much.
 * Andy Speagle
 * David Nutter
 * Flavio Leitner
 * Joshua Roys

 * [wiki:ContributorList]

== Installation Help ==
 * [wiki:HowToInstall]