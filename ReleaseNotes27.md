
![Alt](images/27release.png?raw=True)
# __Spacewalk 2.7 Release Notes__



Hello everyone,

We are proudly announcing release of Spacewalk 2.7, a systems management solution.

The download locations are

  * http://yum.spacewalkproject.org/2.7/RHEL/6/
  * http://yum.spacewalkproject.org/2.7/RHEL/7/
  * http://yum.spacewalkproject.org/2.7/Fedora/24/
  * http://yum.spacewalkproject.org/2.7/Fedora/25/
  * http://yum.spacewalkproject.org/2.7/Fedora/26/

with client repositories under

  * http://yum.spacewalkproject.org/2.7-client/RHEL/5/
  * http://yum.spacewalkproject.org/2.7-client/RHEL/6/
  * http://yum.spacewalkproject.org/2.7-client/RHEL/7/
  * http://yum.spacewalkproject.org/2.7-client/Fedora/24/
  * http://yum.spacewalkproject.org/2.7-client/Fedora/25/
  * http://yum.spacewalkproject.org/2.7-client/Fedora/26/


SUSE Linux client packages can be found here

  * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/2.7/SLE_12_SP2/
  * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/2.7/SLE_12_SP3/
  * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/2.7/openSUSE_Leap_42.2/
  * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/2.7/openSUSE_Leap_42.3/
  * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/2.7/openSUSE_Tumbleweed/


For fresh installations, please use steps from

  * https://fedorahosted.org/spacewalk/wiki/HowToInstall

If you plan to upgrade from older release, search no more -- the following page will guide you:

  * http://fedorahosted.org/spacewalk/wiki/HowToUpgrade

## Features & Enhancements in Spacewalk 2.7

  * Spacewalk now supported on Fedora 25 and Fedora 26
  * Spacewalk supports Fedora 25 and Fedora 26 clients
  * Improved Debian/Ubuntu version parsing and matching
  * Spacewalk wiki now hosted on GitHub
  * New utility to monitor what taskomatic daemon is doing - taskotop is part of spacewalk-utils package
  * jabberd, which support OSAD, now uses sqlite database for improved reliability
  * jpackage libraries/packages replace with standard ones
  * Improved kickstart profile support
  * New API Calls:
    * channel.listManageableChannels
    * proxy.createMonitoringScout
    * satellite.isMonitoringEnabled
    * satellite.isMonitoringEnabledBySystemId
    * schedule.failSystemAction

The up-to-date API documentation can be found at http://www.spacewalkproject.org/documentation/api/2.7/

## Contributors


Our thanks go to the community members who contributed to this release:

 * Avi Miller
 * Can Bulut Bayburt
 * Candace Sheremeta
 * Chris Gavin
 * Dario Leidi
 * Duncan Mac-Vicar P
 * Frantisek Kobzik
 * Ilya Gorbunov
 * Jakub Jankowski
 * Kenny Tordeurs
 * Laurence Rochfort
 * Marc A. Dahlhaus
 * Marcelo Moreira de Mello
 * Martin Matuska
 * Martin Seidl
 * Michael Calmer
 * Michele Bologna
 * Miroslav Suchý
 * Pablo Suárez Hernández
 * Patrick Hurrelmann
 * Shannon Hughes
 * Silvio Moioli
 * Valérian Beaudoin

see [Contributor List](ContributorList) for all contributors list

## Some statistics

In Spacewalk 2.7, we've seen

    * 219 major bugs fixed
    * 880 changesets committed
    * 1463 commits done

Github repo for commits since Spacewalk 2.5

    * [Spacewalk 2.6 to 2.7](https://github.com/spacewalkproject/spacewalk/graphs/contributors?from=2016-11-18&to=2017-08-17&type=c)
## User community, reporting issues



To reach the user community with questions and ideas, please use
mailing list spacewalk-list@redhat.com. On this list, you can of
course also discuss issues you might find when installing or using
Spacewalk, but please do not be surprised if we ask you to file a bug
at https://bugzilla.redhat.com/enter_bug.cgi?product=Spacewalk with more
details or full logs.

Thank you for using Spacewalk.