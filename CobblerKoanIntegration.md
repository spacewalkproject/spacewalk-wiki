== Goal ==

Why should we integrate cobbler into Spacewalk:

 * Keeping our virt stuff updated: 
  * xen installs using virtinst library (same as virt manager), remove legacy Xen-specific code
  * qemu/KVM
  * vmware installs (evolving)
  * lvm backed guests
  * multiple disks for VMs
  * multiple NICs for VMs
 * Other OS support: 
  * First class:
   * Fedora
   * CentOS 
  * Second class (no repo support, kickstart templating and PXE still works)
   * Debian / Ubuntu
   * SuSE
 * Additional rollout options
  * auto-managed PXE tree with auto-generated/auto-updated menus: Ability to reinstall hardware via PXE (do not need "good" OS)
 * LiveCD/DeadCD support for bare metal rollout where customer has no PXE
 * DHCP and DNS management
 * Templating of our files (especially kickstarts) -- less kickstarts to manage
 * able to do bare-metal deployments on multiple subnets
 * replication between Cobbler servers (multi-datacenter) (evolving)
 * integrated image replication/cloning (future?) for Windows/other environs
 * power management (future?)
 * for more see: https://fedorahosted.org/cobbler

== Requirements ==

1. Installation of Spacewalk will include an installation of Cobbler

1.1  The spacewalk-setup program will configure Cobbler to work with Spacewalk

2. Cobbler will be able to be invoked from the command line on the Spacewalk server

3. Clients with koan installed will be able to be invoked against the Spacewalk server:
   
  {{{koan --virt --profile=f9-xen-i386 --nogfx --server=spacewalk.example.com --virt-name=test-f9}}}

4. Cobbler and koan CLI and server (but not Cobbler WUI) will be supported by Red Hat once this feature is available within a Satellite release.  --TBD

5. Spacewalk will synchronize creation/updates/deletes to each Kickstart Profile with a Cobbler profile

5.1. Changes to Cobbler profiles from the cobbler CLI will be synchronized to the Spacewalk Kickstart Profiles

6. Cobbler will fetch Kickstart files from Spacewalk

 -- upgrade path? 

7. Systems using Cobbler and Koan for provisioning will consume a Provisioning entitlement upon registration to the Spacewalk server.

8. A Spacewalk server will be supported and able to function as a PXE server for use with Provisioning if so configured. 

8.1. A Spacewalk server will be be supported able to function as a DHCP/DNS server for use with Provisioning if so configured. 

8.2 The spacewalk-setup 
 
 -- do we need a webui for this?
 -- 

9. Cobbler will be able to use the Kickstart Profiles contained within Spacewalk to provide to Anaconda.

10. Spacewalk will offer a new advanced Kickstart GUI page that will allow a user to hand-edit the kickstart file and send it down to Cobbler for synchronization.

11. Spacewalk's virtualization provisioning feature will utilize Cobbler and Koan to create new virtual guests.

12. Spacewalk's virtualization provisioning feature will be expanded to include more options to include LVM backed guest options, qeumu and KVM options, multiple NICs per VM and multiple disks for VMs.

13. Spacewalk will be able to provision CentOS and Fedora virtual and non virtual machines.

== RHN Satellite Specific Requirements  ==

1. Cobbler will be shipped in the rhn-satellite channel -- TBD

2. Koan will be shipped in the rhn-tools channels -- TBD

3. Satellite upgrades --> sync all existing ks profiles to cobbler

4. Satellite will only support provisioning and virtualization management features that are supported in Red Hat Enterprise Linux.

== Specification ==

=== Command Line ===

We want to allow users to continue to use Cobbler's CLI from the server side.
In other words, edits from WebUI should not mean CLI usage is disallowed, i.e. CLI has more "knobs" exposed.
Spacewalk can be "read-modify-write" into Cobbler DB using Cobbler API calls.

=== Content ===

 * Make cobbler know how to represent a repository that it is not mirroring (DONE -- mpd)
 * yum-rhn-plugin - (??? what does this mean -- mpd )

=== Client ===
 * calling out to koan from rhnlib
 * kickstart progress monitoring?
  * cobbler has some monitoring via a post-install trigger.  Can we leverage that? -- mpd

=== Server ===
 * cobbler server running on Spacewalk
  * installer should configure /etc/cobbler/settings, /etc/cobbler/modules.conf
 * synchronization of Spacewalk Kickstart Profiles into Cobbler Profiles
 * synchronization of Cobbler profiles into Spacewalk Kickstart Profiles

=== Things we need from cobbler ===
 * virt provision time system overrides
  * Basically Spacewalk sets virt-ram on a system-by-system basis, where as Cobbler prefered profile-by-profile.  Easy enough to add.
  * DONE -- mpd
 * Ability to block for virt installs 
  * post-install trigger for success/registration may be enough?
  * perhaps we can make rhnreg_ks be smart enough to be this? 
 * xmlrpc api vs python lib direct 
  * Cobbler is written in it's own API (api.py), remote.py offers 90% functionality, might need some extensions
 * need to verify koan --replace-self is happy on RHEL3 (it's supposed to be), and also make it work on RHEL2.1

=== Phase I (prototype/investigation) ===

Investigation phase:

 * get rhn client talking to koan to initiate virt installs and re-installs
 * setup action types and ability for Spacewalk to schedule reinstalls and virt installs.
 * write API so we can schedule cobbler style installs
 * get cobbler running on a Spacewalk
 * use xmlrpc to utilize above setup to test doing a cobbler KS from Spacewalk


=== Phase II (real integration) ===

 * Integrate Spacewalk's Kickstart UI with cobbler:  Keep our existing database tables but keep our Kickstart Profiles in sync with cobbler.  CRUD operations will trickle down to cobbler
  * I think we can likely construct a Spacewalk "wizard.ks" template for using Cobbler to generate the existing Spacewalk profiles into Cobbler form, but we want to explore that further...  Cobbler uses Cheetah for templating which essentially allows for anything Pythonic to be included.
 * Offer GUI with 'advanced' option to show a textbox with existing rendering of Spacewalk's Kickstart profile that can be edited then stored down into cobbler.
 * replace client side kickstart and virtualization code with full integration with cobbler vs the rhn-kickstart and rhn-kickstart-vitualization packages.

=== Phase III (replacement of Spacewalk's kickstart persistence) ===

Phase III is where we build on the work from the previous phases to fully integrate and replace Spacewalk's kickstart/virt code with cobbler/koan.  This Phase will be evaluated as necessary or not depending on the success of the previous Phases.

 * Remove Spacewalk's rhnKickstart* database representation of the kickstart profile.  Move to storing it all in cobbler
 * Build advanced GUI to expose all of what Cobbler can do within our GUI (livecd, pxe, etc..).
  * Note: I think we get the live CD and PXE menus for free in phase II. (mpd)

=== Potentially most interesting things to thing about for Phase III ===

(from mdehaan)

 * Getting Spacewalk to manage DHCP and BIND entries for new systems (optional)
 * Fedora and CentOS support (including ability to rsync the trees in cobbler)
 * KVM
 * LVM backed virtual guests
 * Expose the "netboot-enabled flag" and look into integrating power management

== Estimates ==

||  '''Task''' || '''Target Release''' ||  '''Estimate (days)''' || '''Developer''' || '''Status''' || '''Notes''' ||
||  ----   ||  ||  ||  ||  ||  ||
||  Installer: Modify spacewalk rpm to depend on cobbler || 0.3 || 1 || mmccune || '''DONE''' ||  ||
||  Installer: Modify installer to configure Cobbler || 0.3 || 2 || paji || '''DONE'''  ||  ||
||  ----   ||  ||  ||  ||  ||  ||
||  Koan: Verify and test koan clients calling cobbler on spacewalk server || 0.4 || 2 || mmccune || '''DONE''' ||  ||
||  Koan: Modify rhn-kickstart to call koan when kickstarting machine || 0.4 || 4 || mmccune || '''DONE''' ||  ||
||  ----   ||  ||  ||  ||  ||  ||
||  Synchronization: Develop method for Spacewalk server calling Cobbler (xmlrpc) || 0.4 || 3 || mmccune || '''DONE''' ||  ||
||  Synchronization: Develop method for Cobbler calling Spacewalk (xmlrpc) || 0.4 || 3 || mmccune ||'''DONE'''  ||  ||
||  Synchronization: Kickstart Profile creation in Spacewalk creates cobbler profile || 0.4 || 5 || paji || '''DONE'''  ||  ||
||  Synchronization: Kickstart Profile edit in Spacewalk edits cobbler profile || 0.4 || 5 || paji || '''DONE''' ||  ||
||  Synchronization: Kickstart Profile delete in Spacewalk deletes cobbler profile || 0.4 || 1 || paji || '''DONE''' ||  ||
||  Synchronization: Kickstart Distribution creation in Spacewalk creates cobbler profile || 0.4 || 5 || paji || '''DONE''' ||  ||
||  Synchronization: Kickstart Distribution edit in Spacewalk edits cobbler profile || 0.4 || 5 || paji || '''DONE''' ||  ||
||  Synchronization: Kickstart Distribution delete in Spacewalk deletes cobbler profile || 0.4 || 1 || paji || '''DONE''' ||  ||
||  Synchronization: Satellite sync creates cobbler distros when trees are synced || 0.4 || 3 || paji || '''DONE''' ||  ||
||  Synchronization: Create a cli utility that allows users to sync CLI originated CRUD of profiles and distributions to spacewalk on demand || 0.4 || 15 || dparker || '''DONE''' || This is a pretty big change from the initial approach, which was to use cobbler triggers.  The problem with that approach is that one gets into an issue of circularity: if spacewalk is pushing changes to cobbler, one does not want cobbler trying to sync changes back to spacewalk.  And vis-versa.  This turns out to be pretty ugly on a number of levels. This new approach provides the CLI user with a CLI program that synchronizes state between cobbler and spacewalk on demand.  It avoids the considerable overhead that would be incurred if this were done from within spacewalk/java due to extensive shelling and text parsing.  This instead uses the cobbler api in its native namespace and XMLRPC calls to spacewalk.  The downside is the complexity of differential analysis involved.  ||
|| Synchronization: Need api call that returns kickstart tree install type (e.g. fedora_9, etc) for a given ks tree || 0.4 || 1 || mmccune || '''DONE''' || Required so that cobbler updates to a ks tree definition can "carry" the install type into the editTree call ||
|| Synchronization: Need api that returns distro install type list || 0.4 || 1 || mmccune || '''DONE''' || ||
|| Synchronization: Need api that returns a list of profile(s) associated with a given kickstart tree || 0.4 || 1 || mmccune || '''DONE''' || ||
|| Synchronization: deleteTree() api call needs option to force profile deletion || 0.4 || 1 || mccune || '''DONE''' || ||
|| Synchronization: renameDistro() api call doesn't work - appears to attempt copy/delete (and fails) || 0.4 || 1 || mmccune || '''DONE''' || ||
||  ----   ||  ||  ||  ||  ||  ||
||  Upgrades: All pre-existing Kickstart Profiles/Distributions will be populated into Cobbler upon upgrade from Satellite 5.1.0 or earlier. || 0.4 || 5 || paji || '''DONE''' ||  ||
||  ----   ||  ||  ||  ||  ||  ||
||  Kickstarts: Remove tinyurl usage from Spacewalk   . || 0.4 || 3 || mmccune || '''DONE''' || Note:  Didn't remove it entirely. Just made the viewable URLs in the kickstart file usable and shortened the standard non tiny URLs from /kickstart -> /ks ||
||  Kickstarts: Document how to use Spacewalk content for kickstarting without need for tinyurl. || 0.4 || 1 || mmccune ||  ||  ||
||  Kickstarts: Port remaining Perl Kickstart Sniglet handlers || 0.4 || 4 || mmccune || '''CANCELED''' || Lower priority task is canceled unless we have time at the end of the schedule  ||
||  ----   ||  ||  ||  ||  ||  ||
||  Cobbler: All Cobbler Profiles created by Spacewalk KS GUI/API will be setup to fetch their kickstart.cfg files from Spacewalk  . || 0.4 || 3 || mmccune || '''DONE''' ||  ||
||  Cobbler: Manually setup Cobbler as PXE server on spacewalk . || 0.4 || 3 || dparker || '''DONE''' ||  ||
||  Cobbler: Manually setup Cobbler as DHCP server on spacewalk . || 0.4 || 3 || dparker || '''DONE''' ||  ||
||  ----   ||  ||  ||  ||  ||  ||
||  GUI: Build GUI in Spacewalk Tools to configure Spacewalk as PXE server || 0.4 || 3 || paji || '''CANCELLED''' || Level of effort and benefit of offering these GUI screens was determined to be not worth the development cost ||
||  GUI: Build GUI in Spacewalk Tools to configure Spacewalk as DHCP server || 0.4 || 3 || paji || '''CANCELLED''' || Level of effort and benefit of offering these GUI screens was determined to be not worth the development cost ||
||  GUI: Modify Kickstart Profile creation to have option for 'advanced' mode where user pastes in their own kickstart.cfg file || 0.4 || 5 || paji || '''DONE''' ||  ||
||  GUI: Build input form for manual paste of kickstart.cfg || 0.4 || 5 || paji || '''DONE''' || ||
||  GUI: Modify kickstart schedule and status pages to work with file based KS || 0.4 || 5 || paji || '''DONE''' || ||
||  ----   ||  ||  ||  ||  ||  ||
||  Database: Modify rhnKSData and associated tables to be able to store an actual file || 0.4 || 3 || paji || '''DONE''' ||  ||
||  Backend: Verify modification to rhnKSData and associated tables will not effect backend code || 0.4 || 3 || mmccune || '''DONE''' ||  ||
||  API: Modify kickstart.importFile to use new database storage mechanism  || 0.4 || 3 || paji || '''DONE'''  ||  ||
||  Kickstart File Render: Kickstart rendering to support new 'advanced' mode || 0.4 || 3 || paji || '''DONE''' ||  ||
||  ----   ||  ||  ||  ||  ||  ||
||  Entitlements: One-time Activation Keys created for a Kickstart Session will automatically have a Provisioning Entitlement added to them || 0.4 || 1 || bbuckingham || '''DONE''' || * See KickstartScheduleCommand.createKickstartActivationKey().  We need to auto-add the Provisioning entitlement to this activation key we create for the kickstart.[[BR]]* Updated so that kickstart profiles' one-time AK will always be provided provisioning entitlement.  Commit: e49be340662997ec9e376b5258bb71ab7dd13a15||
||  ----   ||  ||  ||  ||  ||  ||
||  Virtualization: Database modifications to offer more virtualization options for a Kickstart Profile (disk, cpu, lvm, etc..) || 0.4 || 2 || mmccune ||  ||  ||
||  Virtualization: GUI to support new virtualization options for Kickstart Profiles || 0.4 || 3 || mmccune ||  ||  ||
||  Virtualization: Provisioning displaying expanded options list for guest creation and allowing overrides || 0.4 || 3 || mmccune ||  ||  ||
||  General: Genericify the xmlrpc layer code to proper cobbler profile  and distro objects in to a separate jar and provide it as a patch to the cobbler upstream.  || 0.4+ || 3 +|| paji/mmccune/dparker ||  ||  ||

'''Total Days: ~105'''

== Non-Cobbler But Slightly Related Interesting Things ==

 * put Virt-viewer applet in Spacewalk virt UI (something we wanted to do in VF at one point)

== Random technical notes ==

 * when looking at cobbler, you are most interested in the devel branch ATM (no longer really applies, I'd code against master -- mpd)
 * Cobbler's command line is written against the Cobbler api, see "api.py"
 * Cobbler's WUI is written against it's XMLRPC API, as is koan, see ""remote.py" -- for examples "remote.py", "services.py"
 * probably best to use the XMLRPC API to allow for maximum cross-language support
 * multiple authentication options to webui, simplest is creating a key in a digest file, also does kerberos/LDAP.
 * for example of misc. cobbler triggers, templating examples, kickstart snippets, and other customization, see the Cobbler Wiki

== Installation Issues =

 * Cobbler uses /etc/httpd/conf.d/cobbler.conf and we are using /etc/httpd/conf configs that need to be linked together
 * This means requests to http://host.example.com/cobbler/ will 404


== Notes from June 26 ==

 * Consensus:  Most important/interesting features to add first are:

    * Baremetal deployment: whole lot of rhel, very fast.  DNS/DHCP
    * Do we care about other virt types: xen-full virt support.  PXE support: rhel cant do it (yet).  KVM, rhel can't do it.  LVM backed virt storage.
    * Are we going to provide a way to configure and turn on baremetal DHCP and/or DNS support in the GUI?
       * or does the installer just ask this question while generating /etc/cobbler/settings  and /etc/cobbler/modules.conf ?
    * Build ISO stuff

== Development ==

  * BZ Bug for tracking: https://bugzilla.redhat.com/show_bug.cgi?id=461162
  * We are doing our work in a public branch in the git repo in the origin/spacewalk-cobbler branch:
    
    git checkout --track -b spacewalk-cobbler origin/spacewalk-cobbler

== Integration Testing Tasks ==

Our first pass of integration testing brought forward some critical issues with the Cobbler-Spacewalk integration.  below is the list of outstanding issues:



||  '''Task''' || '''Target Release''' ||  '''Estimate (days)''' || '''Developer''' || '''Status''' || '''Notes''' ||
||  ----   ||  ||  ||  ||  ||  ||
||  Synchronization: Write Taskomatic task to pull cobbler CLI changes into Spacewalk || 0.4 || 4 || jsherril || '''DONE''' ||  ||
||  Synchronization: For each system being provisioned have spacewalk create cobbler system profile || 0.4 || 2 || mmccune || '''DONE''' ||  ||
||  Kickstart Files: Modify Spacewalk to write kickstart config files to disk so we can properly use Cobbler's cheetah templating system|| 0.4 || 2 || mmccune || '''DONE'''  ||  ||
||  Client: kernel override options not being passed to koan || 0.4 || 2 || dparker || '''DONE''' ||  ||
||  GUI: System Provisioning Schedule page needs to show "cobbler only" profiles || 0.4 || 2 || paji || '''DONE''' || This will allow users to provision systems using Cobbler profiles that are NOT synced to Spacewalk.  Allows for greater flexibility. ||
||  ----   ||  ||  ||  ||  ||  ||
||  Cobbler Install: Satellite does not package it's own defaults file for the installer || 0.4 || 0.5 || paji || '''DONE''' ||  ||
||  Cobbler Install: Cleanup defaults || 0.4 || 1 || paji || '''DONE''' || * remove the message about Ctrl+C as it will break the installer * DHCP answer file choice should be "N" * DHCP answer file choice should be "N" * interactive DHCP options should be "no/isc/dnsmasq" (default no) * interactive DNS options should be "no/BIND/dnsmasq" (default no) * mcc says remove extra blank spaces * XMLRPC should always be set on, do not ask for spacewalk as it will break things, just remove this question * Disable TFTP by default ||
||  Spacewalk Setup: Get rid of satellite-httpd and move to httpd/conf.d || 0.4 || 3 || dparker || '''DONE''' ||  ||
||  ----   ||  ||  ||  ||  ||  ||
||  Cobbler:  make a "--check-less-stuff" option for spacewalk to use from it's installer, don't mention reposync package for instance || 0.4 || 1 || mpd || '''DONE''' ||  ||
||  Cobbler:  move password from templates to settings file and check against that instead || 0.4 || 1 || mpd || '''DONE''' ||  ||
||  ----   ||  ||  ||  ||  ||  ||
||  Synchronization:  spacewalk must synchronize (new and existing) cobbler system records || 0.4 || 4 || mmccune ||  ||  ||
||  Synchronization:  spacewalk must create system records in cobbler when a system is granted provisioning. || 0.4 || 2 || mmccune ||  ||  ||
||  Synchronization:  spacewalk preserve all cobbler variables and not overwrite items in cobbler objects || 0.4 || 2 || mmccune ||  ||  ||
||  Synchronization:  Whitepaper for QA || 0.4 || 1 || jsherrill ||  || (Justin/MPD) to discuss synchronization workflow/limitations from user expectation perspective and document in whitepaper for use by Q/A  and development.  ||
||  ----   ||  ||  ||  ||  ||  ||
||  Create Distro GUI: Surface fields for ksmeta and kernel options || 0.4 || 5 || jsherill || '''DONE''' || IMO: Not critical for 0.4 but definitely a nice to have  ||
||  Create Distro GUI:  Location of kernel and initrd needs to be documented as the filesystem path that contains the kernel+initrd, update GUI || 0.4 || 1 || jsherill || '''DONE''' || Required ||
||  Create Distro GUI: Fix double slash in kernel/initrd path || 0.4 || 0.5 || jsherill || '''DONE''' ||  ||
||  ----   ||  ||  ||  ||  ||  ||
|| Templating: In kickstart profile GUI: Add cobbler ksmeta fields || 0.4 || 5 || jsherill || '''DONE''' ||  ||
|| Templating: ship a sample spacewalk template and install it into /var/lib/cobbler/kickstarts || 0.4 || 1 || dparker || '''DONE'''  ||  ||
|| Templating:  We need to create a snippet that includes spacewalk registration code || 0.4 || 1 || dparker || '''DONE''' ||  ||
|| Templating: We need to create a snippet that contains spacewalk "media_url" logic, clarify and document any special media URLs || 0.4 || 1 || mmccune ||  ||  ||
|| Packaging: Saving kickstarts in /etc/rhn is not as LSB friendly as it could be as these aren't config files.    Use /var/lib/rhn/kickstarts or similar for user data.  || 0.4 || 0 || dparker || '''DONE'''  ||  ||
|| Templating: Spacewalk should install snippet for file preservation feature in /var/lib/cobbler/snippets, called spacewalk_file_preservation || 0.4 || 0.5 || dparker || '''DONE'''  ||  ||
|| Templating: Need page for storing/viewing cobbler system variables and allow them to be edited and stored in a cobbler system including the Netboot enabled flag. || 0.4 || 3 || mmccune ||  || This will take some time and is one of the larger time sinks ||
||  ----   ||  ||  ||  ||  ||  ||
|| Virt: Remove vmware || 0.4 || 0.5 || dparker || '''DONE''' ||  ||
|| Virt: For Satellite we need to only show xenpv || 0.4 || 2 || mmccune ||  || Not required for 0.4, more for Satellite ||
|| Virt: In kickstart profile GUI: Add cobbler ksmeta fields || 0.4 || 5 || jsherill || '''DONE''' ||  ||
|| Virt: Fix virt choices || 0.4 || 0.5 || dparker || '''DONE''' || The valid virt choices are xenpv, xenfv, qemu, auto.... auto is not recommended and we should default to qemu (qemu will use /dev/kvm if avail) for spacewalk, prob xenpv for Sat ||
|| Virt: Satellite really should delete storage when guests are deleted || 0.5 || 5 || jsherill ||  || Not for 0.4 ||
|| Virt: needs to support saving all of these options also in the profile and inheriting the values || 0.4 || 2 || mmccune || '''ACTIVE''' || ||
||  ----   ||  ||  ||  ||  ||  ||
|| MISC: We talked earlier about having a "cobbler buildiso" button in the GUI to generate the ISO to a pub URL to help out folks that don't have PXE setups.  We should do this at least for a later release. || 0.5 || 5 || none ||  || Not for 0.4 ||
||  ----   ||  ||  ||  ||  ||  ||
|| Docs: Document spacewalk setup that it cannot be used with Apache vhosts|| 0.4 || 1 || jha ||  ||  ||
|| Docs: ow to use the cobbler command line to make changes || 0.4 || 1 || jha ||  ||  ||
|| Docs: what is supported and what's not || 0.4 || 1 || jha ||  ||  ||
|| Docs: cobbler setup Q&A || 0.4 || 1 || jha ||  ||  ||
|| Docs: BIOS options || 0.4 || 1 || jha ||  ||  ||
|| Docs: list of new features || 0.4 || 1 || jha ||  ||  ||

== Post Implementation Details ==

After implementation of the Cobbler-Spacewalk integration we wanted to document some of the important changes that affect Spacewalk.

* Kickstart trees must be stored locally:  We used to support the ability to point your kickstart distribution at an external location such as http://someserver.example.com/repo/rhel5-i38/.  We now no longer support this option as all kickstart trees must be stored on the Spacewalk server.

* Kickstart distribution URLs have changed:  Previous releases had http://spacewalk.example.com/kickstart/dist/ks-tree-label as the base for their --url stanzas.  This has been shortened a bit to: http://spacewalk.example.com/ks/dist/ks-tree-label

* All kickstart profiles are available for viewing using the koan tool.  In previous releases the only way to know what Kickstart Profiles were available for use in bare metal installs was to login and list all the kickstart profiles.  Now users are able to run the 'koan --server=spacewalk.example.com --list=profiles' to view all the kickstart profiles available for use.

For example, if you have a Spacewalk instance setup with 2 organizations and 2 kickstart profiles, 1 in org 1 and 1 in org 2 koan will display and make available both kickstart profiles:

# koan --server=spacewalk.example.com --list=profiles
rhel5-i386-pv:2:org2
rhel5-i386-pv:1:Spacewalk-Public-Cert

the naming convention is <profile-name>-<orgid>-<orgname>

* Likewise if you are using cobbler to manage a PXE host all the kickstart profiles will show up in the PXE menu when booting.

* Previously in order to manipulate 'kickstart' objects in Spacewalk required  access to the Spacewalk GUI or API with a username and password.  You are now able to also from the Spacewalk server's shell directly manipulate Kickstart Distributions and Profiles.  This means that someone with root access to your Spacewalk box is able to create/update/delete kickstart distributions and profiles.  




