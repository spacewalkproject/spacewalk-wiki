# *User Documentation*

In this section you can find user documentation for Spacewalk.  Are you a developer? You may also be interested in the [Developer Documentation](DeveloperDocs) and [Source/RPM Download Information](DownloadIt).


Is there documentation missing or you simply would like to see? Is there something you would like to understand better? Please feel free to let us know using the [mailing list](https://fedorahosted.org/spacewalk/wiki#Communication) or look for us on #spacewalk on irc.freenode.net.

Do you know the answer to something and think others might find it useful? Feel free to add your own docs and/or additions, it is a wiki you know.

----

----
## __Getting Started__

### Introduction


  * [Preface](Preface)

  * [Conventions](Conventions)
### Installation

  * [How to install](HowToInstall)

    * [Setting up Spacewalk repo](HowToInstall)
    * [Additional repos & packages](HowToInstall)
    * [Oracle Pre-Requisites](HowToInstall)
    * [Conflicting Packages](HowToInstall)
    * [Installing Spacewalk](HowToInstall)
    * [Configuring Spacewalk](HowToInstall)
    * [Managing Spacewalk](HowToInstall)
    * [Known Issues](HowToInstall)
  * [Oracle XE Setup](OracleXeSetup) -- How to get Oracle XE running if you do not already have Oracle.
  * [Configuring redundant Satellites](http://www.redhat.com/f/pdf/rhn/satellite_redundancy.pdf).
  * [Spanish Translation](http://wiki.woop.es/Instalacion_Spacewalk)
### Post-Installation Guides

  * [Backup Spacewalk](SpacewalkBackup) -- How to backup a Spacewalk server

  * [Entitlement Certs ](CertCreation) -- Create and activate your own certs for Spacewalk
  * [Upload  Content](UploadFedoraContent) -- Upload RHEL, CENT, or Fedora content (or any other rpms)
  * [Registering Clients](RegisteringClients) -- How to register Fedora / Cent OS clients to Spacewalk.
  * [How to install osad](OSADSetup) -- How to use osad to force scheduled actions to run immediately on Spacewalk clients
  * [How to install Proxy](HowToInstallProxy) -- How to download and install Spacewalk Proxy.
  * [Implement monitoring via SSH](http://www.redhat.com/docs/manuals/satellite/Red_Hat_Network_Satellite-5.2.0/html/Reference_Guide/s2-mon-rhnmd-sshd.html) -- Instead of RHNMD/port 4545
  * [Implement PAM (LDAP, Kerberos) authentication](https://access.redhat.com/knowledge/docs/en-US/Red_Hat_Network_Satellite/5.5/html/Installation_Guide/sect-Installation_Guide-Maintenance-Implementing_PAM_Authentication.html)  -- For the spacewalk web portal.
### Core Guides

  * [Multiple Organizations](MultiOrgGuide) -- A guide to setting up multiple organisations

  * [General Configuration Management](ConfigurationManagement)
  * [Managing RHEL Systems](ManagingRHELSystems)
  * [Managing Fedora Systems](ManagingFedoraSystems)
    * [Channel Setup](ManagingFedoraSystems)
    * [Provisioning](ManagingFedoraSystems)
    * [Registering](ManagingFedoraSystems)
    * [Pushing Packages](ManagingFedoraSystems)
    * [Fedora Specific Configuration Management](ManagingFedoraSystems)
    * [Related Documentation](ManagingFedoraSystems)
  * [Managing CentOS Systems](ManagingCentOSSystems) 
  * [Managing Solaris Systems](ManagingSolarisSystems) -- Registering Solaris clients and uploading content.
    * [Channel Setup](ManagingSolarisSystems)
    * [Provisioning](ManagingSolarisSystems)
    * [Registering](ManagingSolarisSystems)
    * [Pushing Packages](ManagingSolarisSystems)
    * [Solaris Specific Configuration Management](ManagingSolarisSystems)
    * [Related Documentation](ManagingSolarisSystems)
  * [Managing Debian Systems](ManagingDebianSystems)
----
## __General Documentation__

### Advanced Guides


  * [How to Kickstart with Spacewalk and Cobbler](HowToKickstartCobbler) -- Spacewalk and Cobbler together: How to use the new integration released in Spacewalk 0.4

  * [Spacewalk on Virtual Machine Procedure](Spacewalkonvm) -- A brain/screendump of installing a Fedora 10, kvm install of a spacewalk server (under construction!)
  * [Using Spacewalk to review Linux audits](AuditReviewing) -- How to setup Spacewalk to search and review Linux audits
  * [[spacecmd]] -- command-line interface to Satellite and Spacewalk servers
  * [Debian Support](Deb_support_in_spacewalk) -- Debian Support
  * [Software Crash Reporting](HowToUseCrashReporting) -- How to use software crash reporting functionality in Spacewalk (Spacewalk & abrt)
### Ongoing Maintenance and Administration

  * [How to upgrade](HowToUpgrade) -- How to upgrade an existing Spacewalk installation.

  * [Debugging Proxy](DebuggingProxy) -- Tip on solving problems with Spacewalk Proxy.
  * [Host name Rename](SpacewalkHostnameRename) -- How to rename the server running spacewalk.
### In-depth Guides

  * [osad and osa-dispatcher](OsadHowTo) -- How does osad / osa-dispatcher / jabberd work

  * [Red Hat Network Satellite docs](http://www.redhat.com/docs/manuals/satellite/) -- These are the official docs for the production product.  Over time we will have some docs more closely related to upstream here.
  * [Debian Support Thesis](http://www.stud.fit.vutbr.cz/~xdurfi00/diplomka/xdurfi00.pdf) which can also be downloaded [here](http://www.redhat.com/spacewalk/documentation/master-thesis-debian-support.pdf).
### Core API Reference

  * [Core API Guide](ApiDocs) -- Learn how to use our XMLRPC API to control your Spacewalk server.

  * [API Guide for Perl](SpacewalkApiPerlGuide) -- A user contributed guide to using the Spacewalk API in Perl.
  * [Developer documentation](DeveloperDocs) -- Learn how to work on and build Spacewalk, and how Spacewalk is put together. 
----
## __Other Documentation__

### Up/Down-stream Documentation


  * The upstream, [Official Red Hat Network Satellite Documentation](http://www.redhat.com/docs/manuals/satellite/)

    * [The official Red Hat Satellite Installation Section](http://www.redhat.com/docs/en-US/Red_Hat_Network_Satellite/5.3/Installation_Guide/html/index.html)
    * [The official Red Hat Proxy Installation Section](http://www.redhat.com/docs/en-US/Red_Hat_Network_Satellite/5.3/Proxy_Installation_Guide/index.html)
    * [ISS Best Practices Whitepaper](http://www.redhat.com/pdf/ISS_Best_Practices_Whitepaper.pdf)
  * The downstream, [CentOS](http://centos.org/) Project Documentation
    * [The CentOS Project's Package Management/Spacewalk HowTo](http://wiki.centos.org/HowTos/PackageManagement/Spacewalk)
  * J. Tanner's notes about RHN Satellite
    * http://tannerjc.net/wiki/index.php?title=Satellite
### Related Technology Resources

  * [The RPM Homepage](http://www.rpm.org/)

    * [Wikipedia article on RPM](http://en.wikipedia.org/wiki/RPM_Package_Manager)
    * [The Fedora RPM guide](http://docs.fedoraproject.org/drafts/rpm-guide-en/index.html)
  * [The Fedora 12 Deployment guide](http://docs.fedoraproject.org/deployment-guide/f12/en-US/html/pt-pkg-management.html)
  * [The YUM Homepage](http://yum.baseurl.org/)
    * [Wikipedia article on YUM](http://en.wikipedia.org/wiki/Yellow_Dog_Updater_Modified)
    * [Useful article about YUM](http://prefetch.net/articles/yum.html)
----
## __Frequently Asked Questions__

### [FAQ](SpacewalkFaq)


  * [General](SpacewalkFaq)

  * [What Is Spacewalk?](SpacewalkFaq)
  * [How long has Spacewalk been around?](SpacewalkFaq)
### [Installation](SpacewalkFaq)

  * [Is a Satellite/Spacewalk certificate required to use Spacewalk?](SpacewalkFaq)

### [Architecture](SpacewalkFaq)

  * [Any plans for supporting other databases?](SpacewalkFaq)

  * [What if I don't have Oracle?](SpacewalkFaq)
  * [Perl, Python, and Java OH MY!](SpacewalkFaq)
  * [Why are there two XML-RPC API endpoints?](SpacewalkFaq)
  * [What languages can I use to interact with the XML-RPC APIs?](SpacewalkFaq)
  * [What OSes are supported?](SpacewalkFaq)
### [The Future](SpacewalkFaq)

  * [What about integrating technology X?](SpacewalkFaq)

  * [What kind of community contributions are you interested in?](SpacewalkFaq)
----
## __Known Issues & Current Limitations__

### Installation Issues


=== GUI Issues ===

  * ["Certificate Expiring" messages](KnownIssues_CertExpiring) -- What to do when such a message appears in the GUI.
### Configuration Issues

  * [Multibyte (UTF8) input limitations](MultibyteSpacewalk)

### Networking Issues

  * [Jabber & OSAD](JabberAndOSAD)

### Database Issues

  * [Oracle XE](OracleXeSetup) Issues

  * [Postgresql](PostgreSQL) Support
  * [Jabber Berkeley Database](JabberDatabase) Maintenance
----
## __Contact__

 * [Communication](https://fedorahosted.org/spacewalk/wiki#Communication)

 * [Bugzilla](https://fedorahosted.org/spacewalk/#WanttoFileaBugRFE)
----
## __Appendices__

### Features


  * [Feature Requests/BrainBox](BrainBox)

  * [Road Map](https://fedorahosted.org/spacewalk/roadmap)
### Installation Guides

  * [for 2.7](HowToInstall)
  * [for 2.6](HowToInstall26)
  * [for 2.5](HowToInstall25)
  * [for 2.4](HowToInstall24)
  * [for 2.3](HowToInstall23)
  * [for 2.2](HowToInstall22)
  * [for 2.1](HowToInstall21)
  * [for 2.0](HowToInstall20)
  * [for 1.9](HowToInstall19)
  * [for 1.8](HowToInstall18)
  * [for 1.7](HowToInstall17)
  * [for 1.6](HowToInstall16)
  * [for 1.5](HowToInstall15)
  * [for 1.4](HowToInstall14)
  * [for 1.3](HowToInstall13)
  * [for 1.2](HowToInstall12)
  * [for 1.1](HowToInstall11)
  * [for 1.0](HowToInstall10)
  * [for 0.8](HowToInstall08)
  * [for 0.7](HowToInstall07)
### Upgrade Notes

Upgrade guides, sequential, stepped upgrades are recommended to cover multiple update levels, i.e. to get from 0.6 to 0.8 please upgrade via 0.7 (and we recommend testing at each intermediate level as you proceed) rather than trying to go directly from 0.6 to 0.8. Jumping levels is deeply discouraged with the exception of version 0.9 which was never released.

  * [From 2.6 to 2.7](HowToUpgrade)
  * [From 2.5 to 2.6](HowToUpgrade26)
  * [From 2.4 to 2.5](HowToUpgrade25)
  * [From 2.3 to 2.4](HowToUpgrade24)
  * [From 2.2 to 2.3](HowToUpgrade23)
  * [From 2.1 to 2.2](HowToUpgrade22)
  * [From 2.0 to 2.1](HowToUpgrade21)
  * [From 1.9 to 2.0](HowToUpgrade20)
  * [From 1.8 to 1.9](HowToUpgrade19)
  * [From 1.7 to 1.8](HowToUpgrade18)
  * [From 1.6 to 1.7](HowToUpgrade17)
  * [From 1.5 to 1.6](HowToUpgrade16)
  * [From 1.4 to 1.5](HowToUpgrade15)
  * [From 1.3 to 1.4](HowToUpgrade14)
  * [From 1.2 to 1.3](HowToUpgrade13)
  * [From 1.1 to 1.2](HowToUpgrade12)
  * [From 1.0 to 1.1](HowToUpgrade11)
  * [From 0.8 to 1.0](HowToUpgrade10)
  * [From 0.7 to 0.8](HowToUpgrade08)
  * [From 0.6 to 0.7](HowToUpgrade07)
  * [From 0.5 to 0.6](HowToUpgrade06)

If you are looking for instructions on how to upgrade the operating system underlying your Spacewalk without upgrading your Spacewalk, follow the instructions described in HowToUpgradeOperatingSystem.
### Release Notes

Release notes for each version:

  * [for 2.7](ReleaseNotes27)
  * [for 2.6](ReleaseNotes26)
  * [for 2.5](ReleaseNotes25)
  * [for 2.4](ReleaseNotes24)
  * [for 2.3](ReleaseNotes23)
  * [for 2.2](ReleaseNotes22)
  * [for 2.1](ReleaseNotes21)
  * [for 2.0](ReleaseNotes20)
  * [for 1.9](ReleaseNotes19)
  * [for 1.8](ReleaseNotes18)
  * [for 1.7](ReleaseNotes17)
  * [for 1.6](ReleaseNotes16)
  * [for 1.5](ReleaseNotes15)
  * [for 1.4](ReleaseNotes14)
  * [for 1.3](ReleaseNotes13)
  * [for 1.2](ReleaseNotes12)
  * [for 1.1](ReleaseNotes11)
  * [for 1.0](ReleaseNotes10)
  * [for 0.8](ReleaseNotes08)
  * [for 0.7](ReleaseNotes07)
  * [for 0.6](ReleaseNotes06)
  * [for 0.5](ReleaseNotes05)
### Wiki

  * [Snippets and Templates](WikiSnippetsAndTemplates)

  * [Wiki Page Index](WikiPageIndex)
  * [Wiki Macro List](WikiMacroList)
  * [Meta-Wiki Page](Meta-WikiPage)
     * [User Documentation Requests](Meta-WikiPage)
     * [Documentation Marked as Work in Progress/Todo List](Meta-WikiPage)
     * [User Docs Road Map](Meta-WikiPage)
### Licensing and Copyright

  * [License](source__LICENSE)

  * Copyright