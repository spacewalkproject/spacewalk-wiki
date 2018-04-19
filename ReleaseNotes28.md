
![Alt](images/28release_new.png?raw=True)
# __Spacewalk 2.8 Release Notes__



Hello everyone,

We are proudly announcing release of Spacewalk 2.8, a systems management solution.

Spacewalk 2.8 could be installed on
  * RHEL 6
  * RHEL 7
  * Fedora 26
  * Fedora 27
  * Fedora 28 (tested on Beta)
  
The download location is 
  * https://copr.fedorainfracloud.org/coprs/g/spacewalkproject/spacewalk-2.8/

with client repositories under
  * https://copr.fedorainfracloud.org/coprs/g/spacewalkproject/spacewalk-2.8-client/


SUSE Linux client packages can be found here
  * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/2.8/


For fresh installations, please use steps from

  * https://github.com/spacewalkproject/spacewalk/wiki/HowToInstall 

If you plan to upgrade from older release, search no more -- the following page will guide you:

  * https://github.com/spacewalkproject/spacewalk/wiki/HowToUpgrade

## Features & Enhancements in Spacewalk 2.8

  * Spacewalk now installable on Fedora 27 and has been tested on Fedora 28 Beta as well
  * Spacewalk supports Fedora 27 and Fedora 28 clients
  * All packages and repositories are exclusively in https://copr.fedorainfracloud.org/
  * https://spacewalkproject.github.io/ has been completely redesigned
  * Python 2 packages are no longer needed on systems with Python 3 as default
  * Spacewalk server is now capable of syncing and distributing of Fedora modularity (modules.yaml) files.
  * PostgreSQL 10 is now supported
  * Package dwr updated to version 3.0.2 fixing security vulnerabilities
  * It's now possible to manage errata severities via Spacewalk server
  * Several bugfixes
  * Updated API calls:
    * errata.create/setDetails - add possibility to manage severities
    * system.schedulePackageRemoveByNevra - support removal of packages which are not in database
  

The up-to-date API documentation can be found at http://spacewalkproject.github.io/documentation/api/2.8/

## Contributors


Our thanks go to the community members who contributed to this release:

* Amit Upadhye
* Andrew Dahms
* Ben Roberts
* Candace
* Candace Sheremeta
* Christian Lanig
* Dario Leidi
* Dario Maiocchi
* Frantisek Kobzik
* Jakub Jankowski
* Johannes Renner
* Kevin Walter
* Laurence Rochfort
* Marcelo Moreira de Mello
* Mattias Giese
* Michael Calmer
* Michele Bologna
* Miroslav Suchý
* Pavel Studenik
* Robert Paschedag
* Silvio Moioli
* Thomas Blanchard
* Thomas Mueller
* angystardust
* bhavesh_bharadiya
* ranmses
* suzuki

see [Contributor List](ContributorList) for all contributors list

## Some statistics

In Spacewalk 2.8, we've seen

    * 85 major bugs fixed
    * 641 changesets committed
    * 1146 commits done

Github repo for commits since Spacewalk 2.7

* [Spacewalk 2.7 to 2.8](https://github.com/spacewalkproject/spacewalk/graphs/contributors?from=2017-08-17&to=2018-04-16&type=c)

## User community, reporting issues



To reach the user community with questions and ideas, please use
mailing list spacewalk-list@redhat.com. On this list, you can of
course also discuss issues you might find when installing or using
Spacewalk, but please do not be surprised if we ask you to file a bug
at https://bugzilla.redhat.com/enter_bug.cgi?product=Spacewalk with more
details or full logs.

Thank you for using Spacewalk.
