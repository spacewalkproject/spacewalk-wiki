# Multiple Organizations High Level Specification

## Introduction




The multiple organization support feature in Spacewalk allows you to partition your Spacewalk server into different organizations that each have their own set of entitlements, systems, and content that other organizations on the Spacewalk server cannot access. This document describes the high level design overview for this feature. 
## DB Schema

### rhnTrustedOrgs




rhnTrustedOrgs will handle the trusted organizations that Spacewalk Admin will setup among the various organizations that are defined. Note this does not mean the orgs will trust each other, rather it merely gives the Org Admins of those Organizations the opportunity to share channel content and system migrations if they wish. 

        org_id          number
                        constraint rhn_trusted_orgs_oid_nn not null
                        constraint rhn_trusted_orgs_oid_fk
                                references web_customer(id)
                                on delete cascade,
        org_trust_id    number
                        constraint rhn_trusted_orgs_otid_nn not null
                        constraint rhn_trusted_orgs_otid_fk
                                references web_customer(id),
                                on delete cascade,
        created         date default (sysdate)
                        constraint rhn_trusted_orgs_created_nn not null,
        modified        date default (sysdate)
                        constraint rhn_trusted_orgs_modified_nn not null,
### rhnAllowTrust



This table will keep data that allows Org admin to enable the sharing of channel content and/or system migrations

        org_id          number
                        constraint rhn_allow_trust_oid_nn not null
                        constraint rhn_allow_trust_oid_fk
                                references web_customer(id)
                                on delete cascade,
        channel_flag    char(1) default('N')
                        constraint rhn_allow_trust_channelflg_nn not null
                        constraint rhn_allow_trust_channelflg_ck
                                check (channel_flag in ('N','Y')),
        migration_flag  char(1) default('N')
                        constraint rhn_allow_trust_migrationflg_nn not null
                        constraint rhn_allow_trust_migrationflg_ck
                                check (migration_flag in ('N','Y')),
        created         date default (sysdate)
                        constraint rhn_allow_trust_created_nn not null,
        modified        date default (sysdate)
                        constraint rhn_allow_trust_modified_nn not null
### rhnChannelTrust



This table will keep track of which orgs have access to a specific orgs shared channels

        channel_id      number
                        constraint rhn_channel_trust_cid_nn not null
                        constraint rhn_channel_trust_cid_fk
                                references rhnChannel(id)
                                on delete cascade,
        org_trust_id    number
                        constraint rhn_channel_trust_otid_nn not null
                        constraint rhn_channel_trust_otid_fk
                                references web_customer(id),
                                on delete cascade,
        created         date default (sysdate)
                        constraint rhn_channel_trust_created_nn not null,
        modified        date default (sysdate)
                        constraint rhn_channel_trust_modified_nn not null,
### rhnChannel



This table will require an additional column to handle if the Org Admin will allow orgs from its Trusted Ring to have access to Channel Content, 

 * public: char(1) default('N'), check public in ('Y', 'N')
## New & Existing Pages



The design of the following new pages can be found on the [User Interface](Features/MultiOrg2/TrustRelationships) mockups and the [Content Sharing](Features/MultiOrg2/ContentSharing) mockups. High Level descriptions of the widgets can be found on the mockup pages. A diagram representing the new and existing pages can be found at [[https://fedorahosted.org/spacewalk/wiki/Features/MultiOrg2]].
### Organizations

Location: /rhn/admin/multiorg/Organizations.do (Existing)


ACL: Spacewalk Admin
### Trust Orgs

Location: /rhn/admin/multiorg/TrustOrgs.do?oid=X (New)


ACL: Spacewalk Admin
### Trust Ring Remove Confirm

Location: /rhn/admin/multiorg/TrustOrgDelete.do?oid=X (New)


ACL: Spacewalk Admin
### Trusted Organizations

The set of orgs available are determined by the set defined by the spacewalk admin. This page allows enable/disable for channel content and 

system migrations.
Location: /rhn/trust/Trusts.do

ACL: Org Admin
### Trusted Organizations Removal

Location: /rhn/trust/RemoveTrusts.do


ACL: Org Admin
### Channel Details

Location: /network/software/channels/manage/edit.pxt?cid=101 (Existing)


ACL: Org Admin
### Channel Organization

Channel organization page that allows a Org Admin to give access to specific Organizations in addition to the Org Trust Page. 


Location: /rhn/channels/manage/ChannelTrust.do?cid=101 (New?)

ACL: Org Admin
## System Migration

### Implementation Notes




 * Register a system under Org-1. 
 * Create Org-3 with user scoobert
 * Update rhnServer as:

    update rhnServer set org_id =: new_org where id = 1000010000 
         and org_id = (select org_id from web_contact where login = 'admin');

 * If both the origination and destination Org each have only 1 Org Admin defined, update the existing row in the rhnUserServerPerms associated with the system to reflect the id of the Org Admin of the destination Org:

     update rhnUserServerPerms 
         set user_id =: new_user_id where user_id = 1
         and server_id = 1000010000;   
 * However, if either the origination or destination Org or both have more than 1 Org Admin defined, remove all rows associated with the server from the rhnUserServerPerms. (Rule: this assumes that a system may not exist in more than 1 org.)

    delete from rhnUserServerPerms where server_id = 1000010000;
 * And insert a row for each of the Org Admins defined in the destination Org (i.e. Org-3) associating it to the system being migrated 

    insert into rhnuserserverperms(user_id, server_id)
         select ugm.user_id, 1000010000
            FROM rhnUserGroupMembers ugm
            WHERE ugm.user_group_id = (SELECT id
               FROM rhnUserGroup
               WHERE org_id =: new_org
               AND group_type = (SELECT id FROM
                  rhnUserGroupType WHERE label = 'org_admin'));
 * The server registered under Org-1 should disappear and show up under Org-3 
 * As Org-3 user, schedule an action and let client pick it up
 * The client should now be able to see actions from new Org
 * Observations from above test .. 

     - server shows up in SystemList page in new org
     - channel associations still remain intact
     - Hardware profile migrated
     - Server History remains intact
     - cleint still checks back in with new org
     - Entitlements need to be reallocated
### rhn-server-migrate

rhn-server-migrate is a new command line script that will migrate a system. Details are below: 

 * script to migrate a given server from one org to another 
 * Usage: 

    rhn-server-migrate [options] 
    options:
     -v, --verbose         Increase verbosity
     -uUSERNAME, --username=USERNAME
                           Satellite Admin username
     -pPASSWORD, --password=PASSWORD
                           Satellite Admin password
     --satellite=SATELLITE
                           Satellite server to run migrate on
     --list-systems=LIST_SYSTEMS
                            Available Systems by org. Usage:
                           --list-systems=<org_id>
     --serverId=SERVERID   Server to migrate
     --org-id=ORG_ID       Destination Org ID
     --csv=CSV             CSV File to process
     -h, --help            show this help message and exit

    * List available systems for a given org
       $ rhn-server-migrate --satellite=rlx-2-22.rhndev.redhat.com  --list-systems=1
         Red Hat Network username: admin
         Red Hat Network password:
       Available Systems for Org-ID: 1 
       ------------------------------------
        Server-ID      Server-Name         
       ------------------------------------
        1000010002   rlx-2-24.rhndev.redhat.com 
        1000010003   rlx-2-24.rhndev.redhat.com 
       --------------------------------------------

    * Check the migration without commit, thsi rollsback after success
        $ rhn-server-migrate --satellite=rlx-2-22.rhndev.redhat.com --serverId=1000010002 --org-id=1
         Red Hat Network username: admin
         Red Hat Network password:
         Migration : OK

    * Batch migrate using CSV (TODO)
       * The csv format: serverId,to_org
      $ rhn-server-migrate --satellite=rlx-2-22.rhndev.redhat.com --csv=servers.csv
       Red Hat Network username: admin
       Red Hat Network password:
       Migration : OK
## API Calls

A full list of API calls can be found on the [API](ApiAdditions) wiki. 

### Implementation Notes



 * The abbreviation "org" will be used in place of "organization" for all method and argument names.
 * orgId will be used as the standard means of identifying an org in API call arguments. Org name, which is unique, was considered but ruled out due to the fact that it's intended as a display name and not a label. 
 * Channel family entitlements will be referred to by channelFamilyName. This is returned from the existing satellite.listEntitlements call. 
 * Any set*Entitlements functions should reject org 1. This org behaves as the default container and it's allocation originates from the satellite certificate. Unused entitlements in this org are available to be given to other orgs, and removing entitlements from an org automatically assigns them back to org 1. 
 * Most existing API calls will be ok as-is. The user logged into the API session is used for credentials and any operation they perform *should* be specific to their own org. Thus most existing API calls should not need to be overloaded to accept an orgId.
 * NULL entitlement counts returned from the database (representing unlimited) will have to be represented as -1 values in API calls as null is not supported in standard xmlrpc. 
 * Calls which list entitlements will not include entries that have none allocated. Eg. When listing allocation for an entitlement, only orgs which have been given the entitlement will be present in the list.
 * Private channel families will not appear in the list entitlement calls or be accepted by the set entitlement calls. This matches the UI functionality as private channel families are really only relevant to the org they're created for. 
 * A -1 entitlement count signifies an unlimited allocation of an entitlement. (i.e. custom channel families, etc) This is represented as NULL in the database, but this xmlrpc has no null values so we must resort to using -1. This situation is rare if even possible, NULL max member counts seems to only occur for private channel families, which are not supported in list/set entitlement calls. (see previous point) 
 * implement a config option/flag to disable if a customer needs at some point in the future 
### Structs



 * Org: org_id, org_name, active_users (int), systems (int), system_groups (int), activation_keys (int), kickstart_profiles (int), configuration_channels (int). This data matches the details screen for an org in the UI.  
 * User: login, login_uc, name, email, org_admin (boolean). (Slightly different than the User struct used elsewhere in the API, may be able to just add to existing serializer) 