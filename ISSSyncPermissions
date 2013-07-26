[[TOC]]

= ISS: Sync channel visibility/ownership =

== Introduction == #intro

This page is in reference to [https://bugzilla.redhat.com/show_bug.cgi?id=977878 Spacewalk BZ 977878]

The goal here is to give Inter-Spacewalk-Sync (ISS) the ability to maintain ownership and visibility of channels when sync'ing from master to slave.  (Currently, it syncs the channels and their content, without maintaining ownership)

The reason this is non-trivial, is because channels are owned by Organizations, and orgs and channels can be created on slaves in addition to whatever is on the master.  This means that the sync-code cannot reliably know how to map orgs-from-master to a matching org on the slave, without some human intervention.

== Overview == #overview

Spacewalk exposes a functionality known as "Inter-Spacewalk-Sync", or ISS.  ISS allows one Spacewalk instance (which we'll call "the Slave") to '''pull content''' from another Spacewalk instance (which we'll call "the Master").  Content, in this context, means "channels, RPMs, and kickstarts".  It does '''not''' mean Spacewalk entities like Server-profiles, Users, or Organizations.

One of the things a Spacewalk Org-Admin can do, is create Custom Channels, to hold content specific to this admin's Org.  The Org-Admin can decide how "visible" these Custom Channels are.  Specifically, a Custom Channel can have one of three possible visibility states:

1. Public - the channel and its contents are visible to any Org that is "trusted by" its owning Org.
1. Private - the channel and its contents are visible '''only''' to the owning Org.
1. Protected - the channel and its contents are visible to some subset of the Orgs that are "trusted by" its owning Org.

Now, Custom Channels are content in the ISS context, and can therefore be pulled from a Master onto a Slave instance.  However, ISS does not synchronize the non-content Org entities - so what happens to the Custom Channel's permissions setup?

Up through Spacewalk 1.9, the answer has been, "custom channels are assigned to the default Org, and are publicly visible to any Org that the default Org trusts".  This is the behavior that the RFE aims to change.

With Spacewalk 2.0, ISS makes it possible to sync the org-trust-hierarchy, and the custom-channel-permissions, in a way that makes it possible for the channels to end up with the same visibility restrictions on the Slave, that they have on the Master.  It accomplishes this goal by exposing whatever Orgs the Master instance is willing to expose, to the receiving Slave instance.  The Spacewalk-Admin on the Slave can then choose to '''map''' the Master Orgs to specific Slave Orgs.  The next satellite-sync can then utilize this information to assign custom-channel-ownership to whichever Slave Org is mapped to a specific Master Org.  It can also map the trust-relationships between the exposed Master Orgs to matching Slave Orgs, rebuilding the equivalent relationships on the Slave.

=== Web User Interface Changes === #webui

A new entry exists in the Admin pages of Spacewalk.  If a user is logged in as a Spacewalk Administrator, and selects "Admin" from the top-menu, they will see a new entry in the left-nav options, "ISS Configuration".  This leads to two main tabs: Master Setup and Slave Setup

==== Master Setup ==== #master

This set of pages is where the admin sets up their Spacewalk instance as a Master.  It includes:

1. Identifying the Slave Spacewalk that the Master is willing to accept connections from.
1. For each Slave so identified, specifying:
  1. Whether this slave is currently Enabled or not (ie, connections will be accepted from it)
  1. Whether we allow all Master Orgs to be exported
  1. The specific set of Master Orgs we wish to export, assuming we choose to limit that list

The Spacewalk Admin can create, list, update, and remove Slave information from this tab.

==== Slave Setup ==== #slave

This set of pages is where the  admin sets up their Spacewalk instance as a Slave.  It includes:

1. Identifying the Masters that this slave has (or is planning to) synchronize content from.
1. For each Master so identified, 
  1. Identifying this master as the default (i.e., used by satellite-sync unless overridden)
  1. Identifying the file-path '''on the Slave's filesystem''' of the CA-CERT controlling HTTPS access to this master (typically /usr/share/rhn/<master>_RHN-ORG-TRUSTED-SSL-CERT)
  1. Showing the list of Master Orgs exported by that Master to this Slave
  1. The mapping between each Master Org, and a Slave (local) Org

=== Configuration (/etc/rhn/rhn.conf) Changes === #config

Prior to this change, a Master instance identified which Slaves it was willing to allow contact from in an entry in /etc/rhn/rhn.conf, allowed_iss_slaves.  Recording that data now lives in the database, and is under the control of the Master Setup pages described above.

Likewise, a Slave instance identified its "default" master with iss_parent, and that parent's CA-CERT with iss_ca_chain.  Those values are now also kept in the database, and are under the control of the Slave Setup pages.

=== Utility Script Changes === #script

The first-time-in setup for ISS, since it now requires mapping master-to-slave Orgs, can be onerous.  To ease that burden, a new tool has been added to the spacewalk-util.rpm, {{{spacewalk-sync-setup}}}.  This tool allows the user to specify a Master and a Slave instance, and uses configuration files to set up the information described in both the Master and Slave Setup sections above.  It will create a set of default configuration files for the user if requested.

Here is the help-output of the new tool: 
{{{
Usage: spacewalk-sync-setup [options]

Options:
  -h, --help            show this help message and exit

  Connections:
    Identify the spacewalk instances we're going to connect to

    --ss=SLAVE, --slave-server=SLAVE
                        name of a slave to connect to.
    --sl=SLAVE_LOGIN, --slave-login=SLAVE_LOGIN
                        A sat-admin login for slave-server
    --sp=SLAVE_PASSWORD, --slave-password=SLAVE_PASSWORD
                        Password for login slave-login on slave-server
    --ms=MASTER, --master-server=MASTER
                        name of a master to connect to.
    --ml=MASTER_LOGIN, --master-login=MASTER_LOGIN
                        A sat-admin login for master-server
    --mp=MASTER_PASSWORD, --master-password=MASTER_PASSWORD
                        Password for login master-login on master-server

  Templates:
    Options for creating initial versions of setup files NOTE: This will
    replace existing machine-specific stanzas with new content

    --cst, --create-slave-template
                        Create/update a setup file containing a stanza for the
                        slave we're pointed at, based on information from the
                        master we're pointed at
    --cmt, --create-master-template
                        Create/update a setup file stanza for the master we're
                        pointed at, based on information from the slave we're
                        pointed at
    --ct, --create-templates
                        Create both a master and a slave setup file, for the
                        master/slave pair we're pointed at

  Setup:
    Specify the setup files we're actually going to apply to a
    slave/master

    --msf=FILE, --master-setup-file=FILE
                        Specify the master-setup-file we should use
    --ssf=FILE, --slave-setup-file=FILE
                        Specify the slave-setup-file we should use

  Action:
    Should we actually affect the specified spacewalk instances?

    --dry-run           Don't actually change anything, but tell us what you
                        would have done
    --apply             make the changes specified by the setup files to the
                        specified spacewalk instances
    --default-ok        Even if I don't explicitly tell you about a master or
                        slave on the command-line, it's ok to try to apply
                        changes to them!

  Utility:
    -d, --debug         Log debugging output
    -q, --quiet         Log only errors

}}}

=== Typical Use-Case Workflow === #workflow

It is likely that the single largest consumer of this new feature, will be customers that have a Master instance with a set of Orgs that are identical to a set of Orgs on the Slave instance that will be synchronizing content from that Master.  Note that the '''names''' are the key thing - because two Spacewalks do not share the same database, and can have had very different things happen to them, the database IDs are not viable to use as identifiers between Spacewalk instances.

In this use-case, there exists a Master instance with Orgs Org1..OrgN, and a Slave instance with Orgs Org1..OrgN.  The Master has, let us assume, three channels, Org1-Public, Org1-Protected, and Org1-Private.  Org1-Protected is visible only to Org1 and Org3.  The goal is to synchronize the Slave to the Master, and end up with the same visibility.

With the code published as part of Spacewalk2.0, the customer that has access to the Spacewalk Admin role on both Master and Slave would issue the following command on any machine that can access the public XMLRPC API of both instances:

{{{
spacewalk-sync-setup --ms=<Master FQDN> --ml=<Master Sat-Admin login> --mp=<Master Sat_Admin password> \
                     --ss=<Slave FQDN>  --sl=<Slave  Sat-Admin login> --sp=<Slave  Sat-Admin password> \
                     --create-templates
                     --apply
}}}

Output would look something like the following:
{{{
INFO: Connecting to <admin>@<master-fqdn>
INFO: Connecting to <admin>@<slave-fqdn>
INFO: Generating master-setup file $HOME/.spacewalk-sync-setup/master.txt
INFO: Generating slave-setup file $HOME/.spacewalk-sync-setup/slave.txt
INFO: Applying master-setup $HOME/.spacewalk-sync-setup/master.txt
INFO: Applying slave-setup $HOME/.spacewalk-sync-setup/slave.txt
}}}

At this point, the customer could then issue the satellite-sync command for each of their customer channels:
{{{
satellite-sync -c Org1-Public -c Org1-Protected -c Org1-Private
}}}

and at the end of the sync, see the channels correctly synchronized, with a trust-hierarchy and channel-permissions setup matching that on the Master.

== Technical Discussion == #tech-disc

Channel ownership/visibility is controlled per-Org.  Orgs are identified by WEB_CUSTOMER.ID.  Slaves may have their own entries into WEB_CUSTOMER, with IDs that overlap the Master's WEB_CUSTOMER.

Because WEB_CUSTOMER.IDs can overlap, there has to be a way to let the Administrator of the Slave know what Orgs might be sync'd from Master, and to map those Orgs to existing Orgs on the Slave.  This is the {{{rhnIssMasterOrgs}}} table.

Slaves can, over time, be sync'd to more than one Master.  Because of this, there must be a way to identify orgs-from-Master by *which* Master they're from.  This is the {{{rhnIssMaster}}} table.

Masters may not wish to export all their Orgs to all of their Slaves.  There should be a way for the Administrator of a Master to specify which Orgs they wish to export, to which of their several/many Slaves.  This is the {{{rhnIssSlaveOrgs}}} table.

For {{{rhnIssSlaveOrgs}}} to make sense, the Master has to "know about" the slaves it syncs to as DB-ids.  In addition, the Administrator needs to be able to set more global information - is a given Slave enabled to be sync'd to at all, and do we always export all Orgs to a given Slave.  This is the {{{rhnIssSlave}}} table.
