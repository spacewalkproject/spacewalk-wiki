[Tasks Page](PostgresTasks)

*Note*: ~~Views~~ - are no longer used and do not need to be migrated.

This table __is__ ordered by dependency so working _top down_ will probably work best.
### Views To Migrate




|  *View*  |  *Status*  |  *Assigned*  |  *Notes*  |
| --- | --- | --- | --- | --- |
| rhnOrgChannelFamilyPermissions |  done  |  Gurjeet  |   |  |
| rhn_contact_monitoring |  done  |  Gurjeet  |   |  |
| rhn_customer_monitoring |  done  |  Gurjeet  |   |  |
| rhn_host_monitoring |  done  |  Gurjeet  |   |  |
| rhnActionOverview |  done  |  Gurjeet  |  Had Oracle style join syntax  |
| rhnOrgChannelTreeView |  done  |  Gurjeet  |   |  |
| rhnSharedChannelView |  done  |  Gurjeet  |   |  |
| rhnSharedChannelTreeView |  done  |  Gurjeet  |   |  |
| rhnAvailableChannels |  done  |  Gurjeet  |   |  |
| rhnCDDevice |  done  |  Gurjeet  |   |  |
| rhnChannelFamilyOverview |  done  |  Gurjeet  |  Was using NVL(); replaced with COALESCE()  |
| rhnChannelFamilyPermissions |  done  |  Gurjeet  |   |  |
| rhnServerEntitlementView |  done  |  Gurjeet  |   |  |
| rhnChannelFamilyServerPhysical |  done  |  Gurjeet  |   |  |
| rhnChannelFamilyServers |  done  |  Gurjeet  |   |  |
| rhnChannelFamilyServerVirtual |  done  |  Gurjeet  |   |  |
| rhnChannelNewestPackageView |  done  |  Gurjeet  |   |  |
| rhnChannelPackageOverview |  done  |  Gurjeet  |   |  |
| rhnChannelPermissions |  done  |  Gurjeet  |   |  |
| rhnChannelTreeView |  done  |  Gurjeet  |   |  |
| rhnDemoOrgs |  done  |  Gurjeet  |   |  |
| rhnEntitledServers |  done  |  Gurjeet  |   |  |
| rhnHistoryView_refresh |  done  |  Gurjeet  |   |  |
| rhnHistoryView_packages |  done  |  Gurjeet  |   |  |
| rhnHistoryView_errata |  done  |  Gurjeet  |   |  |
| rhnHistoryView |  done  |  Gurjeet  |  File contains functions  |
| rhnHWDevice |  done  |  Gurjeet  |   |  |
| rhnOrgErrata |  done  |  Gurjeet  |   |  |
| rhnOrgPackageOverview |  done  |  Gurjeet  |   |  |
| rhnPaidOrgs |  done  |  Gurjeet  |  Was using the reserved word 'do' as table alias; Was using NVL(); replaced with COALESCE()  |
| rhnWebContactDisabled |  done  |  Gurjeet  |   |  |
| rhnPrivateErrataMail |  done  |  Gurjeet  |   |  |
| rhnServerEntitlementPhysical |  done  |  Gurjeet  |   |  |
| rhnServerEntitlementVirtual |  done  |  Gurjeet  |   |  |
| rhnServerErrataTypeView |  done  |  Gurjeet  |   |  |
| rhnServerFeaturesView |  done  |  Gurjeet  |   |  |
| rhnServerGroupMembership |  done  |  Gurjeet  |  Had Oracle style join syntax  |
| rhnUserManagedServerGroups |  done  |  Gurjeet  |   |  |
| rhnServerGroupOverview |  done  |  Gurjeet  |   |  |
| rhnServerGroupOVLiteHelper |  done  |  Gurjeet  |   |  |
| rhnServerNeededPackageView |  done  |  Gurjeet  |   |  |
| rhnServerOutdatedPackages |  done  |  Gurjeet  |  Uses EVR_T, and uses Oracle style outer join syntax  |
| rhnVisibleServerGroupMembers |  done  |  Gurjeet  |   |  |
| rhnServerOverview |  done  |  Gurjeet  |  Was using NVL(); replaced with COALESCE()  |
| rhnStorageDevice |  done  |  Gurjeet  |   |  |
| rhnUserActionOverview |  done  |  Gurjeet  |  Was using DECODE(), replaced with appropriate COALESCE()  |
| rhnUserAppletOverview |  done  |  Gurjeet  |   |  |
| rhnUserChannelFamilyPerms |  done  |  Gurjeet  |   |  |
| rhnUserChannelTreeView |  done  |  Gurjeet  |   |  |
| rhnUserAvailableChannels |  done  |  Gurjeet  |  commented in main.sql since uses rhn_channel package's member function  |
| rhnUserChannel |  done  |  Gurjeet  |   |  |
| rhnUserGroupMembership |  done  |  Gurjeet  |  Had Oracle style join syntax  |
| rhnUserServerPermsDupes |  done  |  Gurjeet  |   |  |
| rhnVisibleServerGroup |  done  |  Gurjeet  |   |  |
| rhnUserTypeBase |  done  |  Gurjeet  |   |  |
| ~~rhnUserTypeArray~~ |  done  |  Gurjeet  |  Removed; since rhnUserTypeCommaView includes the required functionality  |
| rhnUserTypeCommaView |  done  |  Gurjeet  |   |  |
| rhnUsersInOrgOverview |  done  | Gurjeet   |  Was using NVL(); replaced with COALESCE()  |
| rhnVisServerGroupMembership |  done  |  Gurjeet  |  Had Oracle style join syntax  |
| rhnVisServerGroupOverviewLite |  done  |  Gurjeet  |   |  |
| rhnVisServerGroupOverview |  done  |  Gurjeet  |   |  |
| rhnWebContactEnabled |  done  |  Gurjeet  |   |  |
