
[[Image(https://fedorahosted.org/spacewalk/attachment/wiki/ReleaseNotes23/spacewalk_2.3.png?format=raw)]]

= __Spacewalk 2.3 Release Notes__ =

Hello everyone,

We are proudly announcing release of Spacewalk 2.3, a systems management solution.

The download locations are

  * http://yum.spacewalkproject.org/2.3/RHEL/6/
  * http://yum.spacewalkproject.org/2.3/RHEL/7/
  * http://yum.spacewalkproject.org/2.3/Fedora/20/
  * http://yum.spacewalkproject.org/2.3/Fedora/21/

with client repositories under

  * http://yum.spacewalkproject.org/2.3-client/RHEL/5/
  * http://yum.spacewalkproject.org/2.3-client/RHEL/6/
  * http://yum.spacewalkproject.org/2.3-client/RHEL/7/
  * http://yum.spacewalkproject.org/2.3-client/Fedora/20/
  * http://yum.spacewalkproject.org/2.3-client/Fedora/21/


SUSE Linux client packages can be found here

  * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/2.3/openSUSE_13.1/
  * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/2.3/openSUSE_13.2/
  * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/2.3/openSUSE_Tumbleweed/
  * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/2.3/SLE_12/
  * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/2.3/SLE_11_SP3/

For fresh installations, please use steps from

  * https://fedorahosted.org/spacewalk/wiki/HowToInstall

If you plan to upgrade from older release, search no more -- the following page will guide you:

  * http://fedorahosted.org/spacewalk/wiki/HowToUpgrade

== Features & Enhancements in Spacewalk 2.3 ==

  * Spacewalk now supported on RHEL7/CentOS7/Fedora21
  * Spacewalk supports Fedora21 clients
  * Addition of spacewalk-setup-ipa-authentication to make the external authentication setup easier.
    * https://fedorahosted.org/spacewalk/wiki/SpacewalkAndIPA
  * Improved Proxy caching of yum meta-data with addition of If-Modified-Since header support
  * Community contribution to support for using external Oracle 12c. (Please note that this is provided as-is and not tested by the core team.)
  * Continued UI polish and improvements, including standardizing on [https://www.patternfly.org/ Patternfly]
  * Improved and simplified codebase by:
    * Removing all Monitoring-related code
    * Removing all Solaris support
    * Completing the port of the web-UI from Perl to Java
    * Cleaning up and removing large chunks of orphaned code
  * Plenty of small enhancements and fixes
    * spacecmd enhancements:
      * softwarechannel_errata
      * configchannel_sync
      * softwarechannel_sync
      * softwarechannel_removesyncschedule
    * spacewalk-clone-by-date enhancements:
      * added a --dry-run option
      * improved dependency resolution (see [https://bugzilla.redhat.com/show_bug.cgi?id=1123468 1123468])
      * removed asynchronous background cloning - it conflicts (badly) with dependency resolution, see [https://bugzilla.redhat.com/show_bug.cgi?id=1207846 1207846]
    * spacewalk-reports additions:
      * config-files
      * config-files-latest
      * additional data to scap-scan report
    * Added support for xz-compressed repositories
    * Added Korea to list of timezones
    * aarch64 support
  * New API calls:
    * activationkey.clone
    * configchannel.deployAllSystems
    * kickstart.listKickstartableTreeChannels
    * kickstart.profile.getAvailableRepositories
    * kickstart.profile.getRepositories
    * kickstart.profile.getVirtualizationType
    * kickstart.profile.setRepositories
    * kickstart.profile.setVirtualizationType
    * system.unentitle
    * user.setErrataNotifications
  * Removed API calls:
    * proxy.createMonitoringScout
    * satellite.isMonitoringEnabled
    * satellite.isMonitoringEnabledBySystemId

The up-to-date API documentation can be found at http://www.spacewalkproject.org/documentation/api/

== Contributors ==

Our thanks go to the community members who contributed to this release:

* Anastasios Papaioannou                                                                     
* Aron Parsons                                                                               
* Avi Miller                                                                                 
* Bo Maryniuk                                                                                
* Cynthia Sanchez                                                                            
* David Holland                                                                              
* Dimitar Yordanov                                                                           
* Duncan Mac-Vicar P                                                                         
* Flavio Castelli                                                                            
* Gregor Gruener                                                                             
* Ian Forde                                                                                  
* Jan Hutar                                                                                  
* Jan Pazdziora                                                                              
* Jiri Mikulka                                                                               
* Joerg Steffens                                                                             
* Johannes Renner                                                                            
* Kilian Petsch                                                                              
* Lasse Palm                                                                                 
* lbayerlein                                                                                 
* Ludwig                                                                                     
* Lukas Pramuk                                                                               
* Marcelo Moreira de Mello                                                                   
* Martin Seidl                                                                               
* Mathieu Bridon                                                                             
* Michael Calmer 
* Michael Kromer                                                                            
* Michael Mraka                                                                              
* Micha Lenk                                                                                 
* Milan Zazrivec                                                                             
* Miroslav Suchý                                                                             
* Neha Rawat                                                                                 
* Patrick Hurrelmann                                                                         
* Paul Wayper                                                                                
* Pavel Studenik                                                                             
* Peter Gervase                                                                              
* Robert Moser II                                                                            
* Satoru SATOH                                                                               
* Shannon Hughes                                                                             
* Silvio Moioli                                                                              
* Tasos Papaioannou                                                                          
* Tim Speetjens                                                                              
* Tobias D. Oestreicher                                                                      

https://fedorahosted.org/spacewalk/wiki/ContributorList

== Some statistics ==

In Spacewalk 2.3, we've seen

    * 220 bugs fixed
    * 1247 changesets committed
    * 1878 commits done

Github repo for commits since Spacewalk 2.2

    * [https://github.com/spacewalkproject/spacewalk/graphs/contributors?from=2014-07-17&to=2015-03-27&type=c Spacewalk 2.2 to 2.3]

== Spacewalk 2.3 on RHEL 5 (CentOS 5) ==

With the addition of installation-support on RHEL7/CentOS7, Spacewalk is now no longer supported running on RHEL5/CentOS5

== Solaris and Monitoring Support - Removal Notice ==

The Spacewalk team has dropped code for Solaris clients and the Monitoring component of Spacewalk. Anyone currently using either of the capabilities will need to consider alternatives for their needs prior to upgrading to 2.3. 

== User community, reporting issues ==

To reach the user community with questions and ideas, please use
mailing list spacewalk-list@redhat.com. On this list, you can of
course also discuss issues you might find when installing or using
Spacewalk, but please do not be surprised if we ask you to file a bug
at https://bugzilla.redhat.com/enter_bug.cgi?product=Spacewalk with more
details or full logs.

Thank you for using Spacewalk.
