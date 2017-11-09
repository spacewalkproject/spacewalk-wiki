## Introducing Spacewalk and Cobbler



Version 0.4 of Spacewalk adds significantly enhanced operating system installation capabilities by bundling the [Cobbler](https://cobbler.github.io/) Linux installation server.

_Note:  There are a lot of power features documented on the Cobbler project page, see https://cobbler.github.io/ for those._

New features in 0.4 include:

 * Installing and manage qemu/KVM based virtual machines and Xen fullvirt installs (previously, only Xen paravirt was supported)
 * Installing guests to LVM backed storage
 * Creating guests with multiple disks
 * Creating guests with multiple virtual NICs on multiple bridges
 * First-class support for Fedora and CentOS in addition to RHEL
 * Evolving support for Debian, SuSE, and Ubuntu (limited exposure in Spacewalk)
 * Automatic PXE setup with auto-generated PXE menus for physical deployments
 * Integrated DHCP and DNS management (can be enabled, but is not surfaced in Spacewalk)
 * Powerful kickstart file templating using Cheetah (see https://fedorahosted.org/cobbler/wiki/KickstartTemplating and https://fedorahosted.org/cobbler/wiki/KickstartSnippets)
 * But wait, there's more! (see [Cobbler Wiki](https://github.com/cobbler/cobbler/wiki))
## Quick Start Guide for Spacewalk



<ol>
    <li> Create a Distro</li>
    <ol type="a">
        <li>Initial steps</li>
        <ol>
            <li>Find some distribution isos and mount them.</li>
            <ol type="a">
                <li>mkdir -p /var/iso-images</li>
                <li>downloadFedora-9-i386-DVD.iso for example to that directory</li>
                <li>mkdir -p /var/distro-trees/Fedora-9</li>
                <li>mount -oloop /var/iso-images/Fedora-9-i386-DVD.iso /var/distro-trees/Fedora-9</li>
                <ul>
                    <li>You may find it useful to include an entry in /etc/fstab to ensure this mount persists across reboots
                    </li>
                </ul>
            </ol>
            <ol>
                <li>Create a Spacewalk channel for the distribution (if you have one set up already, go to the next step)
                </li>
                <ol type="a">
                    <li>In the Spacewalk GUI, select "Channels|Manage Software Channels"</li>
                    <ol type="a">
                        <li>select "+ create new channel"</li>
                    </ol>
                    <li>Fill in the required fields as described - name: "Fedora 9" with a label "fedora-9"</li>
                    <li>Populate the channel with packages using rhnpush</li>
                    <ul>
                        <li>`rhnpush --server localhost -u admin -p spacewalk --channel fedora-9
                            /var/distro-trees/Fedora-9/Packages/*.rpm`
                        </li>
                        <li>Note that this step takes some time. We're working on the speed issue for the next
                            release.
                        </li>
                    </ul>
                </ol>
            </ol>
            <li>Having made the Spacewalk base channel, you must now create a child channel for the kickstart helper
                tools
            </li>
            <ol type="a">
                <li>in the GUI, select "Channels|Manage Software Channels" and select "+ create new channel"</li>
                <li>Fill in the required fields as described - call it "Fedora 9 with Tools" and label
                    "fedora-9-with-tools"
                </li>
                <li>Download the following packages from the client repo here http://spacewalk.redhat.com/yum/:</li>
                <ul>
                    <li>pyOpenSSL (in rawhide for F11)</li>
                    <li>rhnlib (in rawhide for F11)</li>
                    <li>libxml2-python (in rawhide for F11)</li>
                    <li>spacewalk-koan*</li>
                </ul>
                <li>Populate your new child channel with these rpms: `rhnpush --server localhost -u admin -p spacewalk --channel fedora-9-with-tools /path/to/the/helper/rpms/(ASTERISK).rpm`
                </li>
                <li>You're now ready to create a distribution.</li>
            </ol>
        </ol>
        <li>Select "Systems|Kickstart|Distributions" and select "+ create new distribution"</li>
        <ol>
            <li>On the "Create Kickstart Distribution" page, fill in the items as described</li>
            <ol type="a">
                <li>"Distribution label" in this case might be "Fedora-9"</li>
                <li>"Tree path" should be "/var/distro-trees/Fedora-9"</li>
                <li>"Base channel" should be "Fedora 9 with Tools"</li>
                <li>"Installer generation" should be Fedora 9 in this case.</li>
                <li>Once you've pressed the "Create Kickstart Distribution" button you'll see a table that includes your
                    new
                    entry.
                </li>
                <li>If you run `cobbler distro list` at your spacewalk system's command line, you'll now see these
                    entries in
                    addition to any others you may have already defined. This will show that things were created
                    successfully.
                    These are named automatically by Spacewalk using the pattern
                    &lt;spacewalk distribution tag&gt;:&lt;organization ID&gt;:&lt;organization name&gt;. You may rename them in Cobbler if you like.
                </li>
                <ul>
                    <li>Fedora-9:1:Spacewalk-Public-Cert</li>
                    <li>Fedora-9:xen:1:Spacewalk-Public-Cert</li>
                </ul>
            </ol>
        </ol>
    </ol>
    <li>Having created your kickstartable distribution, you can now create a new Spacewalk kickstart profile:</li>
    <ol type="a">
        <li>select "Systems|Kickstart|Profiles" and then select "+ create new kickstart profile"</li>
        <li>Give your new profile a label - in this case "Fedora-9-No-Tools"</li>
        <li>Select the "base channel" you created earlier</li>
        <li>Select the "kickstartable tree" you created earlier</li>
        <li>Select the "virtualization type" you intend the target machine to utilize. (xen paravirt or qemu/KVM)</li>
        <li>Click "Next", and on the next screen just accept the "Default Download Location" and click "Next"</li>
        <li>Give your new machine a root password, and then click "Finish"</li>
        <li>If you run `cobbler profile list` you'll now see this entry in addition to those you've already defined:
        </li>
        <ul>
            <li>Fedora-9:1:Spacewalk-Public-Cert</li>
        </ul>
    </ol>
    <li>Use Spacewalk to kickstart a system to a given profile</li>
    <ol type="a">
        <li>Kickstarting physical machines</li>
        <ol>
            <li>You can kickstart physical machines with spacewalk:</li>
            <ol type="a">
                <li>On your network</li>
                <ol>
                    <li>Setup a dhcp server that sets the ''next-server'' variable to point to your spacewalk machine
                    </li>
                    <li>make sure your spacewalk machine address can be found in DNS.</li>
                </ol>
                <li>On the spacewalk server</li>
                <ul>
                    <li>edit /etc/xinetd.d/tftp and change the value of "disabled" to "no".</li>
                    <li>generally, running `cobbler check` is an excellent way to see if your install server
                        configuration has
                        any problems like this that should be addressed.
                    </li>
                    <li>in particular, make sure that TFTP is unblocked in your firewall rules.</li>
                </ul>
                <li>Some lab setup instructions that cover the same information can be found at TODO: link
                </li>
            </ol>
            <li>Bare metal machines can be kickstarted in one of two ways:</li>
            <ol type="a">
                <li>By profile (interacting with the PXE menu)</li>
                <ol>
                    <li>Connect a machine to the network, having already ensured DHCP and next-server are configured
                    </li>
                    <li>Power on the machine.</li>
                    <ol type="a">
                        <li>A disk with no OS will roll over to PXE boot after searching through available disks</li>
                        <li>Alternatively, set the machine to PXE first in the BIOS order.</li>
                    </ol>
                    <li>Choose what profile you want to install from the PXE menu.</li>
                    <li>The box will finish installation automatically</li>
                </ol>
                <li>By system record</li>
                <ol>
                    <li>From a prompt on your spacewalk server: `cobbler system add --name
                        <nameOfYourSystem> --mac
                            <mac addr of netboot interface> --profile <a profile from cobbler profile list'>`.
                    </li>
                    <li>Boot the machine, ideally set so it PXE's first in the BIOS order. The install will continue
                        without any
                        user interaction.
                    </li>
                    <li>To reinstall the machine at any time, `cobbler system edit --name=foo --netboot-enabled=1` and
                        cycle the
                        system power. Cobbler contains power management features if you want to use them.
                    </li>
                </ol>
                <li>Reinstalling or upgrading machines that already have an OS on them</li>
            </ol>
            <li>With koan: `koan --replace-self --server=cobbler.example.org [[--profile=profile-name]]
                [[--system=system-name]]` and then reboot
            </li>
            <li>Note that upgrades require an upgrade kickstart, and this is an operation that will destroy the existing
                system in most cases
            </li>
        </ol>
    </ol>
</ol>

Virtual machines:

Koan can be run on any Spacewalk managed machine to install new VMs.
```shell
    man koan # read the instructions for --virt
```

If you use profiles set up in Spacewalk, virtual machines will be registered to the Spacewalk server automatically.

* For various other tips and tricks see  https://cobbler.github.io, in particular, you will probably be interested in the kickstart templating features under "User Documentation".
## How Cobbler and Spacewalk are linked internally



(This is development information only and should probably be a separate topic.)

Spacewalk and Cobbler integration is bi-directionally synced: relevant elements of object definition - distros, profiles, and systems - are stored across both system's databases.  Spacewalk's awareness of these objects is both by keeping records for itself that contain information about UIDs for each cobbler object.

Object definition is done over an XMLRPC-based protocol. As far as configuration is concerned in XMLRPC communication between Spacewalk and Cobbler, with one exception (Cobbler back-authentication to Spacewalk), Spacewalk is always the "client", and the client presents UIDs generated by Cobbler as the primary key for the various object types being discussed in XMLRPC conversations.

This allows someone to mostly interact with Spacewalk or Cobbler directly as they see fit.  (For instance, it allows usage of features available in Cobbler but not in Spacewalk, and for the most part Spacewalk does not need to know... there are some technicalities in this WRT spacewalk system records, for instance, Cobbler does more with networking ATM than Spacewalk is aware of.  This should change in a future release.)

Every time Spacewalk asks Cobbler to do something via XMLRPC, Cobbler makes an XMLRPC call *back* to Spacewalk to authenticate the tokens passed into it.  A few settings in Cobbler are required to set this up.

 * redhat_management_server - this is the hostname (preferable) or ip address of the Spacewalk system
 * redhat_management_type - in the case of Spacewalk, this will be set to "site", signifying that the Cobbler instance is running on either an RHN Satellite or a Spacewalk instance.

The call sequence is

        spacewalk_url = "https://%s/rpc/api" % server
        client = xmlrpclib.Server(spacewalk_url, verbose=0)
        valid = client.auth.checkAuthToken(username,password)

with *username* and *password* coming from [[PARTHA]].  This call is made when obtaining a cobbler token for interaction with Cobbler.  Once a token is established this callback does
not occur again.

The following describes in some detail the interplay between the two systems in regards Distros, Profiles, and Systems.  Bear in mind however that Cobbler holds a fair number of relevant default values from its own installation.  Spacewalk only sets Cobbler parameters to the extent required to cause Cobbler to work with Spacewalk - there's much more going on with Cobbler that this document won't cover.  If you need more detail, _man cobbler_ and see  https://cobbler.github.io

 * When you create a *Distribution* in the Spacewalk GUI, Spacewalk makes one ore more distros in Cobbler, depending on the distribution:
    * Two XMLRPC calls are made to Cobbler to create two versions of a distro: one with a Xen kernel, one without.  The call passes:
        * http_server and http_port set to point to the spacewalk/cobbler server's apache instance
        * distro_name set to <Spacewalk Distribution Label>:<spacewalk organization id>:<spacewalk organization name>
        * ks_meta with the key-value pair "tree"=><absolute path to distro tree in web space on the sw/cobbler server>
        * initrd set to the absolute path to the initrd for the distribution in question - calculated by spacewalk
        * kernel set to the absolute path to the kernel for the distribution in question - calculated by spacewalk
    * When Cobbler processes the creation request, it generates UIDs for the Xen and non-Xen distros.   These UIDs form the tie that binds Spacewalk's *Distribution* to Cobbler's *distro* - as long as the two exist, these UIDs serve to uniquely identify each distro.
    * On the Spacewalk side, the Cobbler distro UIDs are stored together with a Spacewalk Organization ID, the distribution's label text, the absolute path to the distribution tree's root (which, in the case of a Spacewalk managed distribution, is a web-space path to Spacewalk package management logic - _not_ a plain old directory of files on disk), the channel id in which the distribution's  packages reside, an RHNKSTREETYPE reference indicating that the distribution is managed by Spacewalk, and an RHNINSTALLTYPE reference indicating more specifically the type of installation tree the distro uses (e.g. RHEL_5 or Fedora_10).
    * Note that if you're managing the distro entirely from Spacewalk, the fact of the existence of the _two_ distros (Xen and non) in Cobbler will be transparent to you.  If you happen to select Xen virtualization, the Xen version gets used, else the other.
 * When you create a *Kickstart Profile* in the Spacewalk GUI, you make a corresponding *profile* in Cobbler:
  * One XMLRPC call is made to Cobbler to create a *profile*.  The call passes:
      * Profile name
      * Distro name
      * Virtualization type
      * Organization ID (in ks_meta as org)
      * VM install location
      * virtual bridge name
      * Kickstart kernel path
      * Kernel options for the kickstart operation itself
      * Kernel options the machine should boot with after it's installed
      * Redhat management key
      * If there is virtualization, then also:
          * Amount of ram vm should get
          * Number of cpus vm should get
          * Size of disk vm should get
  * On the Spacewalk side, an RHNKSDATA record is created that records the following facts:
      * The kickstart type - normally wizard, unless you upload your own ks file - see below
      * The spacewalk organization ID
      * The profile's label text
      * Any comments associated with the profile
      * Whether or not the profile is active
      * Whether or not the kickstart process should write output of custom post scripts to /root/ks-post.log
      * Whether or not the kickstart process should write output of pre- scripts to /root/ks-pre.log
      * Whether or not the kickstart process should write ks.cfg and all %include fragments to /root/
      * The Cobbler UID for the profile
      * Any additional arguments you might wish to pass to the anaconda (kickstart) kernel
      * Any additional arguments you might wish to pass to the installed kernel
      * Whether or not this profile should be considered the default one for the organization
 * When and why we create cobbler system records.
     * Note: currently Spacewalk needs to learn more about multiple interfaces in Cobbler, and when they exist, network objects (should be a Trac on this)
     * Spacewalk also needs to know about how to tolerate already existing cobbler system records in it's registration engine (should be a Trac on this)
 * What happens when the user creates cobbler objects and when they show up in Spacewalk, and where these operations are limited
 * taskomatic's job in syncing changes from spacewalk to cobbler
 * taskomatic's job in syncing changes from cobbler to cobbler


## Cobbler Tips And Tricks
See  https://cobbler.github.io for lots of them -- this section can be updated later with pointers to some of the more popular items.
