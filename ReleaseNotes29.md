
![Alt](images/28release.png?raw=True)
# __Spacewalk 2.8 Release Notes__



Hello everyone,

We are proudly announcing release of Spacewalk 2.9, a systems management solution.

Spacewalk 2.9 could be installed on

  * RHEL 6
  * RHEL 7
  * Fedora 27
  * Fedora 28
  * Fedora 29
  
The download location is 
  * https://copr.fedorainfracloud.org/coprs/g/spacewalkproject/spacewalk-2.9/

with client repositories under
  * https://copr.fedorainfracloud.org/coprs/g/spacewalkproject/spacewalk-2.9-client/


SUSE Linux client packages can be found here
  * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/2.9/


For fresh installations, please use steps from

  * https://github.com/spacewalkproject/spacewalk/wiki/HowToInstall 

If you plan to upgrade from older release, search no more -- the following page will guide you:

  * https://github.com/spacewalkproject/spacewalk/wiki/HowToUpgrade

## Features & Enhancements in Spacewalk 2.9

  * Spacewalk now installable on Fedora 29
  * Spacewalk supports Fedora 29 and Fedora rawhide clients
  * All packages and repositories are exclusively in https://copr.fedorainfracloud.org/
  * Spacewalk server is now capable of syncing and distributing of Red Hat Enterprise Linux 8 Beta content
  * Number of bugfixes
  * Updated API calls:
    * errata.create/setDetails - add possibility to manage severities
 
The up-to-date API documentation can be found at http://spacewalkproject.github.io/documentation/api/2.9/
  

## Contributors

Our thanks go to the community members who contributed to this release:

* Chris Bajumpaa
* Neal Gompa
* Robert Paschedag
* Angelo Lisco

see [Contributor List](ContributorList) for all contributors list

## Some statistics

In Spacewalk 2.9, we've seen

    * 41 major bugs fixed
    * 318 changesets committed
    * 555 commits done

Github repo for commits since Spacewalk 2.8

* [Spacewalk 2.8 to 2.9](https://github.com/spacewalkproject/spacewalk/graphs/contributors?from=2018-04-16&to=2019-01-14&type=c)

## User community, reporting issues



To reach the user community with questions and ideas, please use
mailing list spacewalk-list@redhat.com. On this list, you can of
course also discuss issues you might find when installing or using
Spacewalk, but please do not be surprised if we ask you to file a bug
at https://bugzilla.redhat.com/enter_bug.cgi?product=Spacewalk with more
details or full logs.

Thank you for using Spacewalk.
