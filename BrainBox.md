# Brainbox



This is just a page to collect brainwaves/ideas/evil-thoughts for Spacewalk.
## Satellite Features

 * Inter-satellite-sync 

   * Activations Keys, Kickstart profiles & Solaris content
 * debuginfo sync - enable sync of debuginfo packages via satellite-sync. (Or should this be under Repo and RPM Handling?)
## Spacewalk Features

### User Features


 * Enable "super-admins" to select the current working organization from a drop down menu, instead of being forced to logout and login as a specific user for that organization.

    * ![Alt](images/action-add.gif?raw=True) Seconded!
    * ![Alt](images/action-add.gif?raw=True) Thirded! -- Phil
 * [Support User](Features_SupportUser)
 * API-only user, with access to only specific API's.  Or a way to login to the API from a client without having to store username/password in cleartext.
 * User Permissions - Discussion around implementing fine grained user permissions.
    * ![Alt](images/action-add.gif?raw=True) Seconded! -- Phil
    * ![Alt](images/action-add.gif?raw=True) Thirded! -- Jose
 * PAM authentication to be extended so that local GIDs can be mapped to organisations. --Phil
### Deployment

 * Use-case deployment - make initial enable & deployment of the following simple 

   * Remote Commands, Configuration Management, Monitoring
   * Clustered/HA profiles? -- Phil

 * gPXE to compliment Cobbler?
   * Something like boot.kernel.org that could be built on the Spacewalk server via some simple scripts.  Would end up creating a menu from existing kickstarts perhaps?  Or just leaving it entirely to Cobbler?
### Database

 * [PostgreSQL Support](PostgreSQL) - content sync, rhnpush, registration and yum operations work

 * make db-control usable with Oracle XE
 * [MySQL as database backend](MySQL_support)
   * ![Alt](images/action-del.gif?raw=True) honestly this doesn't make much sense until PostgreSQL is done.
   * ![Alt](images/action-del.gif?raw=True) There is no PL/SQL support in MySQL so this would require a complete rewrite of the database schema among alot of other things so its just not practical - prmarino1
 * allow system.scheduleScriptRun to work with scripts that have been saved in the database. This way scripts can be re-used and later maybe even attached to triggers like configchannel actions.
### General

 * [General Improvements](GeneralImprovements)

 * HelpSystem to make help a bit easier to work with.
 * Asset identification
 * Integration with [JasperReports](http://www.jasperforge.org/jaspersoft/opensource/business_intelligence/jasperreports/)
   * ![Alt](images/action-del.gif?raw=True) not sure how important this is to Spacewalk. -- jmrodri
 * Ability to control activation keys with editor and comments -- Antoine
 * Allow user-defined types for custom info items -- Antoine
### Repo and RPM Handling

 * DeltaRpmSupport

    * ![Alt](images/action-add.gif?raw=True) Seconded! -- Phil
 * [SRPM Sync](SRPMSyncSpec) to enable sync of source RPM content via satellite-sync.
 * New page for signing and uploading your own packages.  A browse button for selecting the rpm you want to upload, then a multi-select list of channels you want to upload to. The rpm would be automatically signed by your rpm key specified in /root/.rpmmacros .  --Josh
 * Default base channel for non-Redhat supplied channels. --heydrick [bugzilla 559370](https://bugzilla.redhat.com/show_bug.cgi?id=559370)
 * Provide a UI for folks to edit/create comps files and associated package/group entries for custom content to use with yum client
 * easier support for importing content from multiple distributions and managing those distributions in parallel 
   * ? Is this the NEVRA issue? or something else? -- jmrodri
 * integration with koji -- this is building the packages in koji which will occur when we submit to Fedora.
   * ![Alt](images/action-del.gif?raw=True)/![Alt](images/action-add.gif?raw=True) I'm not sure what this does for us? unless it's the ability to get koji content into Spacewalk after building? If that's the case then I'm ok with it. -- jmrodri
   * Koji can push packages to a repo directory once their built anyway? Just run a standard, local, flat file repo fully of SRPMS, build in koji and push back to arch specific directories before sync'ing into spacewalk locally? 
 * [add support for deb packages/Debian](Deb_support_in_spacewalk)
   * ![Alt](images/action-add.gif?raw=True) this would be useful -- jmrodri
 * add support for external repositories so that Spacewalk doesn't have to have a local copy of all RPMs, just retrieves the headers in the same way yum does and can direct subscribed clients to download directly from the 3rd party repo. (Proxy already does this. What is the use case that YUM covers differently?)
 * Provide a tree structure with a depth greater than 1 for the Software Channel Subscriptions
   * ![Alt](images/action-del.gif?raw=True) What does having a depth greater than 1 buy us? I'd rather remove the hieararchy altogether and go with something more like tags ala [flickr](http://www.flickr.com/help/tags/). -- jmrodri
 * Provide a way to add arbitrary metadata to any given repository. Like modifyrepo does in createrepo.
   * ![Alt](images/action-add.gif?raw=True) seems generally useful
 * download packages using torrent protocol
   * ![Alt](images/action-del.gif?raw=True) Not going to happen. What does this mean? yum / up2date can't use torrents. I don't think this feature makes sense. -- jmrodri
   * I thought about something similar to [DebTorrent](http://wiki.debian.org/DebTorrent) -- msuchy
   * There's a proposal over here http://software-libre.rudd-o.com/Proposal_for_Yum_updates_via_torrent, is it a case that it's not taken off because no one offers a tracker? If Spacewalk were to have an "enable tracker server" that latched onto another port would it not help get things rolling (even if there wasn't a yum-plugin for it even)?
 * enable package/package group selection via a GUI which shows available package/package groups for that distribution
   * ![Alt](images/action-add.gif?raw=True)/![Alt](images/action-del.gif?raw=True) this would be along the lines of using tags to group packages. I'm not in favour of bolting this functionality onto the existing model, but replace it with tags. -- jmrodri
 * enable uploads of (single or max 5)RPMS into the system from the web page, so a user does not have to have a login on the console of the server - Antoine
   * ![Alt](images/action-add.gif?raw=True)  this is an excellent idea. we can do what numerous photo sites do with uploading. -- jmrodri
 * Allow a channel to be available as a raw yum repo.  This would work for building tools like mock.  I often have to keep rpms around in 2 spots so I can perform mock biulds.  Please, please fix this. --stahnma
  * ![Alt](images/action-add.gif?raw=True) Seconded, it would also make migration, manual changes easier? An export as yum repo hierarchy button somewhere? and would help with Koji integration. -- Phil
 * Spacewalk can serve metalink file with list of Spacewalk Proxies, from which can download package too. --msuchy
### Configuration Management

 * [Per Channel Vars for Config Channels](Per_Channel_Vars)

    * ![Alt](images/action-add.gif?raw=True) Seconded -- Jose
 * Associate a config file with a remote command to allow e.g. dependent service restart after config file deployment. It should be possible to define pre or post deployment scripts.
    * ![Alt](images/action-add.gif?raw=True) Seconded...but this should include the ability to tie scripts with RPM and/or erratum deployment as well
 * upgrade configuration management engine? Integrate with puppet or cfengine, let's avoid enhancing our current implementation.
   * ![Alt](images/action-add.gif?raw=True) to integrate with puppet -- jmrodri
   * ![Alt](images/action-add.gif?raw=True) to integrate with puppet -- Phil
   * ![Alt](images/action-del.gif?raw=True) [Why not to use a configuration management engine](Why_not_to_use_a_configuration_management_engine) -- jvzantvoort(jydawg)
   * ![Alt](images/action-del.gif?raw=True) I think spacewalk has a great foundation for config management and should be expanded.  No other tool has the ability to control access to files, thus preventing accidents.
     * ![Alt](images/action-add.gif?raw=True) Use spacewalk as AAA/a wrapper to Func? Func can be extremely dangerous, not that I nearly blew an entire DC away with it once or anything.
   * ![Alt](images/action-add.gif?raw=True) Ansible integration?
### Monitoring / Orchestration

 * [func](http://fedorahosted.org/func) instead of osad? What are the advantages/disadvantages? Is it possible?

    * Start looking at func as possible future alternative to osad, or AMQP (with osad/osa-dispatcher) [bugzilla 453459](https://bugzilla.redhat.com/show_bug.cgi?id=453459)
    * half of an answer:  func can change, and it's probably possible :) Let's start a discussion on the ML. Do we have a page about OSAD here? -- mpd
    * ![Alt](images/action-add.gif?raw=True) probably should be done considering the recent jabberd trouble we've had. -- jmrodri
 * replace ssh probes in monitoring with regular scripts from remote action (will imply usage of osad).
 * upgrade monitoring engine?
   * ![Alt](images/action-del.gif?raw=True) let's not UPGRADE it, let's integrate with something else. -- jmrodri
   * ![Alt](images/action-del.gif?raw=True) Agreed, could there be some integration with Nagios or something similar to NPC (http://trac2.assembla.com/npc/) -- Phil
   * ![Alt](images/action-del.gif?raw=True) +1 this idea.
   * integration with rancid (http://www.shrubbery.net/rancid/) (suggested by Alticon-Brian in IRC)
     * ![Alt](images/action-del.gif?raw=True) I don't think this buys us anything, should be part of the integration with a different monitoring system. -- jmrodri
 * Addition of a GSSAPI/Kerberos authentication method in place of the RHNSD and SSH method for remote client access. Instead of providing the identity file and a user for SSHing (shared keys are banned and impractical in my environment) I'd like to use my existing automaton user and distribute his key tab so that a simple kinit would allow password-less SSH to client machines. AKA Func these days --Phil
### Provisioning

 * add support for Jumpstarting Solaris

 * Automatic change logging of Kickstart profiles - Starting with kickstart pre/post revisioning.  https://fedorahosted.org/spacewalk/wiki/KickstartScriptRevisioning details coec's current thinking on the matter.
 * Expand macros to have conditional logic and loops in config files using custom system info or other values stored in satellite/spacewalk.  {| if rhn.system.custom_info(foo) == 'bar' display 'foo' otherwise display 'not found' |}.  
 * Allow postinstall actions for the server. E.g. force software and configuration updates, set custom variables, send mail.
  * ![Alt](images/action-del.gif?raw=True) - can be already done using System details -> Software -> Install -> select some package -> Install -> Run command
  * I mean, postinstall of a server, not a package. the '%post' part of a kickstart if you will.
  * ![Alt](images/action-add.gif?raw=True) +1 for Postinstall of server.  Have done this many times by writing large series of scripts which are then deployed by Config Channels via Activation Keys.  Then builds up to an init.d type of environment where all scripts in this deployed directory are run.  It works, but it's fiddly.
### Errata

 * Make it possible to subscribe to Errata and create the errata to subscribe to -- red (Done? -- Phil)

  * ![Alt](images/action-add.gif?raw=True)/![Alt](images/action-del.gif?raw=True) Lots of work, but also great benefit -- red
 * ...or maybe create an option to create stupid standard errata (like one for each update/rpm without any text, just to allow the pushing of updates to clients) automatically -- red
  * ![Alt](images/action-add.gif?raw=True)/![Alt](images/action-del.gif?raw=True) Fine enough to at least enable the pushing to the clients, but creating errata just for that?! -- red
 * ...or make it possible to push updates/rpm to the clients instead of errata only -- red
  * ![Alt](images/action-add.gif?raw=True) Enables pushing of updates to clients without anyone having to write errata - might just suffice for lots of use cases -- red
### Other

 * [IPv6](https://bugzilla.redhat.com/show_bug.cgi?id=487666)

   * note that Oracle libraries use gethostbyaddr and friends, so may not be usable under IPv6
   * ![Alt](images/action-del.gif?raw=True) when IPv6 becomes the std, then we can spend the time doing it. I keep seeing comments about things not working with IPv6 but who's using it now? -- jmrodri
 * [Containers in Spacewalk](Container_support)
   * ![Alt](images/action-add.gif?raw=True) I like this idea, it's something we've needed for some time. -- jmrodri
 * Replace the current "Custom Build Content Management System" with one that is Open Source (GPL'd) like [Alfresco](http://www.alfresco.com).  This could replace the legacy PERL/RPC scheme with a more current Javascript/REST scheme.  Alfresco is willing to contribute to this development as there is a need for an Alfresco "Network" and we don't want to reinvent the wheel.  If you would like to contact me: lee.faus at alfresco dot com.
   * ![Alt](images/action-del.gif?raw=True) This request doesn't make much sense.  What Alfresco 'manages' is much more suited to documents than packages. Sure packages can be considered content, but I think the workflows are much different. -- jmrodri
 * finish migrating [perl pages](RemainingPxtPages) to Java, and remove the perl stack entirely.
   * ![Alt](images/action-add.gif?raw=True) this really has been the piece that has held this project behind. We REALLY need to just finish this up and kill off the perl stack (web side) -- jmrodri
 * Ability to save arbitrary script within satellite for reuse (be able to save a script you use a lot) -- Antoine
 * Manage chkconfig via a kewl webinterface on the satellite -- Antoine
   * ![Alt](images/action-del.gif?raw=True) - Not a "spacewalk" function, use Webmin for that sort of thing (if you really want). -- Phil
 * Manage OpenVZ Containers
  * ![Alt](images/action-add.gif?raw=True) Seconded. -- Phil
 * Add NIM on Linux support, based largely around straight TFTP, a Network Installation Manager is used to boot, update and manage AIX servers, IBM have already packaged NIMOL in PRM form for their engineers to use on Linux laptops at client sites, unsure as to feasibility of managing spot sources and LPP in spacewalk? See links...
  * http://www-01.ibm.com/support/docview.wss?uid=isg3T1010948
  * http://www.ibm.com/developerworks/forums/thread.jspa?messageID=13894772
  * http://publib.boulder.ibm.com/infocenter/pseries/v5r3/index.jsp?topic=/com.ibm.aix.howtos/doc/howto/sw_installation_maintenance.htm
  * http://www.redbooks.ibm.com/redpapers/pdfs/redp4194.pdf
  * http://publib.boulder.ibm.com/infocenter/pseries/v5r3/topic/com.ibm.aix.cmds/doc/aixcmds4/nimol_install.htm?resultof=%22%6e%69%6d%6f%6c%22%20
  * http://dag.wieers.com/howto/bits/aix-lpp.php --Phil
## General Project

 * streamline development process to encourage more contributions, see [The Architecture of Participation: ...](http://209.85.215.104/search?q=cache:0hNSibSYPmYJ:www.people.hbs.edu/cbaldwin/DR2/BaldwinArchPartAll.pdf+%22architecture+of+participation%22+harvard&hl=en&ct=clnk&cd=1&gl=us&client=firefox-a)

   * ![Alt](images/action-del.gif?raw=True)/![Alt](images/action-add.gif?raw=True) what are some of the hurdles today that concern people? I am all for making things easier, but I don't know what the problems are with this? Spacewalk is NOT a small project so it's not as easy as {git clone ... ; ./build all}. But I think we've done a fine job with making it much better than it was before by basing it on an installed version. -- jmrodri
 * decompose Spacewalk into an aggregation of OSS projects, composed of separate subprojects that can be used by other projects
   * ![Alt](images/action-add.gif?raw=True) cobbler integration is a step in this direction. Though we need a bigger architectural plan to make this work. -- jmrodri
 * continually ask the question: what use cases are new and not covered by the app because of previous datacenter focus, if any?  Where can it and it's subprojects grow?
 * Integrate Clickheat as a method for determining how the UI is used.  Determine some (optional) method to "phone home" data that can be used to aggregate usage data for future UI improvements.
   * http://www.labsmedia.com/clickheat/index.html --cswiii
## Feature/Function Integration

 * Integration with some sort of FIM such as [AIDE](http://en.wikipedia.org/wiki/AIDE_(software)), such features as exporting the compiled database back to the spacewalk server and comparison of databases and file hashes between Groups/SSMs. I have clusters of machines that need to be kept consistent, which is harder than it sounds when some are taken out of service for developers or tweaked for special purposes. Plus security obviously. --Phil

 * See above about NRPE and NPC but integration with Cacti and Nagios perhaps? I know Redhat has "func" but they aren't quite the same and Nagios + Cacti + NPC is becoming an almost de-facto standard in quite a few places I've worked/consulted. --Phil
 * implement native [systemd](http://freedesktop.org/wiki/Software/systemd/) services.
 * integration with [OpenSCAP](http://www.open-scap.org/page/Main_Page).