
== Deb support in Spacewalk ==

In 2010, Lukaš Ďurfina has introduced a support of Debian in Spacewalk server. In 2011, Šimon Lukašík reasumed his work
and created Apt-Spacewalk package, the equivalent of yum-rhn-plugin for Debian clients. Nowadays, Debian clients are
able to register with Spacewalk server, install, and update packages.

== Client packages ==

For the latest Debian GNU/Linux client packages please refer to [wiki:RegisteringClients#Debian]

== Example of package creation ==

 * wget http://www.redhat.com/spacewalk/source/0.3/rhn-client-tools-0.4.17-10.fc9.src.rpm
 * rpm2cpio rhn-client-tools-0.4.17-10.fc9.src.rpm | cpio -idmv --no-absolute-filenames
 * mv rhn-client-tools-0.4.17.tar.gz mv rhn-client-tools-0.4.17.orig.tar.gz
 * tar xfz rhn-client-tools-0.4.17.orig.tar.gz
 * cd rhn-client-tools-0.4.17
 * export DEBFULLNAME='name'
 * export DEBEMAIL='e-mail'
 * dh_make -c gpl
 * rm debian/*.ex debian/*.EX
 * edit files in debian/, in file: dirs will contain lines: usr/sbin, usr/share, usr/share/rhn
 * in control file add
   * add into Build-Depends: python-all-dev, python-support (>= 0.8), gettext, intltool, pychecker
   * add into Depends: ${python:Depends}, gnupg, hal, rhnlib (>= 2.1), python-dbus, rhpl
   * add Provides: ${python:Provides}
   * edit Homepage and Description
 * edit file rules (this is makefile)
   * for example main install command
{{{
$(MAKE) -f Makefile.rhn-client-tools install  DESTDIR=$(CURDIR)/debian/rhn-client-tools PREFIX=$(CURDIR)/debian/rhn-client-tools
}}}
 * dpkg-buildpackage -rfakeroot


== History ==

Hi, my name is Lukas Durfina. I am studying on Faculty of Information Technology,
Brno University of Technology. In my master thesis I am going to work on support
deb systems/packages in spacewalk. My technical consultant is Miroslav Suchy,
RHN Satellite Engineer.

On this page I would describe progress of my work.

'''December 2008'''[[BR]]

I have 2 virtual servers with CentOS. On 1 of them spacewalk is installed, second
will be for testing. Now I am going to install debian on third virtual server
and then I will start work on creating deb packages.

'''January 2009'''[[BR]]

Now I have virtual Debian server. I started to experiment with debian packages.
I created deb package for rhnlib. Debian repo for getting packages was established.

'''August 2009'''[[BR]]

Rhnlib, rhpl and rhn-client-tools are in repository. It is possible to register
with debian machine in spacewalk.

'''October 2009'''[[BR]]

You can use apt-get install rhn-setup to install all available packages. There are
rhnsd, rhn-client-tools, rhncfg. Some of functions do not work and everything is in
experimental state, but if you like it, try it and send me feedback.

'''March 2010'''[[BR]]

Work on rhnpush has started. Already available packages: jabberpy, osad,
rhn-client-tools, rhn-setup, rhncfg, rhnlib, rhnsd.

'''May 2010'''[[BR]]

Thesis is done. The most important features as registration, deployment of configuration
files and running scritps were implemented. Spacewalk can received DEB package, but
unfortunately the clients are not able to get these packages from Spacewalk (this would
require some modification to APT as some plugin or similar).

Text of thesis can be downloaded from http://www.stud.fit.vutbr.cz/~xdurfi00/diplomka/xdurfi00.pdf or http://www.redhat.com/spacewalk/documentation/master-thesis-debian-support.pdf

'''April 2011'''[[BR]]
Šimon Lukašík introduces Apt-Spacewalk package -- the equivalent of yum-rhn-plugin
on Debian clients. Rhnlib and rhn-client-tools packages are rebased to the latest versions.



