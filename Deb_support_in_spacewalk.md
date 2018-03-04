## Deb support in Spacewalk



In 2010, Lukaš Ďurfina introduced support for Debian in Spacewalk-server. In 2011,
Šimon Lukašík resumed his work on the Debian-client and created Apt-Spacewalk package,
the equivalent of yum-rhn-plugin. Nowadays, Debian clients are able to register with
Spacewalk-server, as well as install and update packages.
## Client packages



For the latest Debian GNU/Linux client packages please refer to [[RegisteringClients]]
## Example of package creation


```
wget http://www.redhat.com/spacewalk/source/0.3/rhn-client-tools-0.4.17-10.fc9.src.rpm
rpm2cpio rhn-client-tools-0.4.17-10.fc9.src.rpm | cpio -idmv --no-absolute-filenames
mv rhn-client-tools-0.4.17.tar.gz mv rhn-client-tools-0.4.17.orig.tar.gz
tar xfz rhn-client-tools-0.4.17.orig.tar.gz
cd rhn-client-tools-0.4.17
export DEBFULLNAME='name'
export DEBEMAIL='e-mail'
dh_make -c gpl
rm debian/*.ex debian/*.EX
```
 * edit files in debian/, in file: dirs will contain lines: usr/sbin, usr/share, usr/share/rhn
 * in control file add
   * add into Build-Depends: python-all-dev, python-support (>= 0.8), gettext, intltool, pychecker
   * add into Depends: ${python:Depends}, gnupg, hal, rhnlib (>= 2.1), python-dbus, rhpl
   * add Provides: ${python:Provides}
   * edit Homepage and Description
 * edit file rules (this is makefile)
   * for example main install command

    `$(MAKE) -f Makefile.rhn-client-tools install  DESTDIR=$(CURDIR)/debian/rhn-client-tools PREFIX=$(CURDIR)/debian/rhn-client-tools`
 * dpkg-buildpackage -rfakeroot
## History



Hi, my name is Lukas Durfina. I am studying in the Faculty of Information Technology,
Brno University of Technology. In my master thesis I am going to work on supporting
"deb" systems/packages for Spacewalk. My technical consultant is Miroslav Suchy,
RHN Satellite Engineer.

On this page I will describe progress of my work.

*December 2008*


I have 2 virtual servers with CentOS. On 1 of them Spacewalk is installed and the second
will be used for testing. I am going to install Debian on a third virtual server
and then I will start work on creating "deb" packages.

*January 2009*


I have now created a virtual Debian server. I started to experiment with Debian packages.
I created a "deb" package for "rhnlib". A Debian repo for getting packages was established.

*August 2009*


"rhnlib" , "rhpl" and "rhn-client-tools" are in the repository. It is possible to register 
a Debian machine using Spacewalk.

*October 2009*


You can use `apt-get install rhn-setup` to install all available packages. These are
"rhnsd", "rhn-client-tools" and "rhncfg". Some of the functions do not work and everything
is in an experimental state, but if you would like to, you can test it and send me feedback.

*March 2010*


Work on "rhnpush" has started. Already available packages: "jabberpy", "osad",
"rhn-client-tools", "rhn-setup", "rhncfg", "rhnlib" and "rhnsd".

*May 2010*


My Thesis has been completed. The most important features that were implemented are: 
registration, deployment of configuration files and running scripts.
Spacewalk can receive a "deb" package, but unfortunately the clients are not able
to get these packages from Spacewalk (this would require some modification
to APT as some plugin or similar).

Text of thesis can be downloaded from http://www.stud.fit.vutbr.cz/~xdurfi00/diplomka/xdurfi00.pdf or http://www.redhat.com/spacewalk/documentation/master-thesis-debian-support.pdf

*April 2011*

Šimon Lukašík has introduced the "Apt-Spacewalk" package -- the equivalent of "yum-rhn-plugin"
on Debian clients. The "rhnlib" and "rhn-client-tools" packages are rebased to the latest versions.



