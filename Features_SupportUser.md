
    #!html
    <div style="background-color:#2b299e; color:white; background-image:url('/spacewalk/attachment/wiki/WikiSnippetsAndTemplates/workinprogress.png?format=raw'); background-repeat:no-repeat; padding-left:7em;background-position:1em;" >
       <h2>Work in Progress</h2>
       <div style="padding-bottom:0.5em;" >
           <p>This is page in currently in a draft state.</p>
       </div>
    </div>
# Support User

### REVIEWED BY:

## Overview

Support for users with read-only access has been a common request of Satellite customers. After several customer discussions we found that this requirement more often than not boils down to a desire for a support user who can log in and perform some simple searches on systems, users, and organizations. As such we decided to pursue this path by implementing a new tab in the satellite UI exposing a simple search interface to users who have the appropriate role.

## Requirements



[Requirements Discussion](SupportUserRequirementsDiscussion)

 1. A new user role that corresponds to the support user role.
  * A user may be assigned to the support user role in the same way as any other role is assigned. The role may be assigned in conjunction with other roles as well. For example, a channel administrator may be granted the support user role in order to be able to access the support user specific functionality (searching, auditing, etc.)
  * Permissions to assign the support user role to a user:
   * Satellite/Spacewalk admin can give to anyone.
   * Org admins can give to users in their org.
 1. Users in the support user role have access to a listing of all systems across all orgs the user is granted visibility into.
 1. Users in the support user role have access to new search functionality for their needs.
   * Support user searches must be able to search across multiple organizations (all must be trusted by the home org of the user).
    * The user may be granted access to only a subset of trusted orgs (somewhat similar to protected access with channels).
     * Removing a trust relationship must propagate to these subsets.
    * The user must be able to select a subset of _their_ allowed organizations at the time of search.
   * For initial release this interface will allow only for searching of *systems* by:
    * Hostname
    * System ID
    * Custom Field
    * Package Installed
   * Search results must provide a link to existing SDC pages.
    * In the case that the user would not normally have access to the system (i.e. the system is not part of the user's assigned system groups), the SDC pages will be rendered as read only.
   * Search results will indicate the following as a summary of systems that match (this may change to simply be all of the columns listed on the Systems > Systems pages).
    * System Name
    * Status
    * Organization
    * Errata Updates Available
    * Package Updates Available
 1. New support user searching functionality will be provided through the XMLRPC API layer.
  * Ability to assign a user the support role.
  * Ability to set the org visibility for a user (i.e. all trusted orgs or a subset).
  * Ability to manipulate the subset of orgs for a user.
  * Ability to indicate org boundaries for the search must be supported.
  * Each of the search criteria above must be supported.
  * The return of these search APIs should match what is returned for existing search APIs.
## Other Feature Impact

APIs - Existing APIs check for the existence of a particular role, so there won't be a need to revisit all of the existing APIs to prevent the support user role from having access. New APIs will be added according to the requirements above, which means the addition of QA unit tests as well.


Search Server - There may be need to enhance the search server's abilities, depending on the final decision on what sort of searches are supported. See the table below for more information on what is currently supported by the search server.

Schema - Small addition to the prepopulated schema data to add the support user role (both for new installations and upgrades)
## Proposed Implementation

The implementation focuses around creating a new "Support User" role and an associated new section of the UI specifically for its tasks. The APIs will also be added to support the new features, utilizing existing APIs where applicable (i.e. addRole v. assignSupportUserRole).

### Support User Role

 1. Database changes (both new DB installation and upgrade scripts)

  * Add support user role to the database (rhnUserGroupType)
  * Add table to track what orgs a support user is granted access to
 1. Add support user role to user details screen.
  * Satellite/Spacewalk admin can give to anyone.
  * Org admins can give to users in their org.
 1. Add ability to select the orgs for the user from two options:
  * For users with the support user role, add a new subtab on the user details page for Support User. This subtab will let the admin select the org visibility for the user from the following options:
   1. All trusted orgs. This option will use the current list of trusted orgs. In other words, when managing trust relationships there won't be a need to edit all support users assigned to this option to add in the new changes.
   1. Subset of orgs. The list of orgs selectable is taken from the current list of trusted orgs for the user's home org. Display the orgs in a table with checkboxes to indicate inclusion of the org in the user's permissions (see the Organizations subtab for a protected channel as a model).
 1. Hook into the trust relationship code to make sure that removing a trust relationship removes that org from any support users that were explicitly assigned the removed org as part of a previous subset configuration.
### Support User Tab

 1. Add a new top level tab (same level as Overview, Systems, etc.) for users with the support user role (similar to how the Admin tab only appears for the satellite admin). The tab text will read "Support Tools" (jdob - This was taken from a tab in hosted that provides similar functionality, though I'm not 100% in love with the name)

 1. Left navigation while on this tab will have the following entries:
  * Systems - Leads to the Systems List page described below.
  * Search Systems - Leads to the System Search Page described below.
### Systems List

 1. This page is somewhat similar to the Systems > Systems page. It is meant to be a summary of *all* systems the user has access to (i.e. crossing org boundaries). Ultimately, this may make similar distinctions such as All v. Out of Date.

### System Search Page

 1. The system search page will follow the model in the current Systems > Advanced Search page (/rhn/systems/Search.do). The page will display the following sections:

  * Search For - Text field allowing the user to enter the search criteria values
  * Field to Search - Drop-down providing the following options:
   * Hostname
   * System ID
   * Custom Field
   * Package Installed
  * Where to Search - The user selects one of two options. These options will be presented similar to the Channels > Package Search page (/rhn/channels/software/Search.do).
   * All trusted organizations the user has access to (default)
   * Specific organizations - The user will be able to select one or more of their allowed trusted organizations (similar to the specific architecture selection in Package Search)
  * Invert Results - If checked, results that *do not match* the search criteria will be displayed (functionality currently exists on Systems > Advanced Search).
### System Search Results

 1. The results of a search will be displayed beneath the form used to enter the criteria, keeping consistent with our search model elsewhere in the UI.

  * The criteria from the previous search will be populated into the form.
 1. The data displayed for each system returned by the search will adhere to the requirements listed above, with the following notes:
  * System Name - The name itself will be a link to the system's SDC page.
 1. The results will be displayed using ListTag 3.0 with the following options:
  * Pagination
  * Display X items per page
  * Filter by system name
 1. The results should be sortable on the following columns:
  * System Name
### APIs

 1. Create support user API handler offering the same functionality as is available in the UI.

 1. Keep in mind to lay the groundwork for community involvement in expanding future support user searches.
#### Role APIs

We can leverage the existing user.addRole API to assign the support user. However, we will still need to add an API for the configuration of which orgs the user will have access to. APIs will be added for retrieving the org visibility for a user (i.e. "all" or "subset") and manipulating the org subset in the latter case.

#### Search APIs

Looking at the system.search API package, it looks like each criteria has its own API call. There doesn't seem to be a specific reason for doing so, though I'm not sure a single call with a flag indicating the criteria type is necessarily better since it requires the caller to make sure to use the correct flag for indicating the criteria. I'm going to continue to follow the existing model for consistency unless there is a compelling argument to switch to a single call. - jdob, Jun 23, 2009


The APIs will be put into a new package: support.search.system

Following that model, the following APIs should be added:
 1. hostname
 1. id
 1. customField
 1. installedPackageName

The return from these calls should also follow the existing model for searching for systems. The following is taken from the API docs for calls in the system.search package as a reference:

array:
 * struct - system
  * int "id"
  * string "name"
  * dateTime.iso8601 "last_checkin" - Last time server successfully checked in
  * string "hostname"
  * string "ip"
  * string "hw_description" - hw description if not null
  * string "hw_device_id" - hw device id if not null
  * string "hw_vendor_id" - hw vendor id if not null
  * string "hw_driver" - hw driver if not null
### Search Server

Below is a listing of the different types of searching proposed for the support user and if they are *currently* (as of Spacewalk 0.6) supported in the search server. A star indicates the criteria should be available in this iteration.



|     |  Type of Search  | Search Criteria  |  Supported in Search-Server  |  Comments  |
| --- | --- | --- | --- | --- | --- |
|  *  | Systems  |  hostname  |  yes  |   |  |
|  *  | Systems  |  system id  |  yes  |   |  |
|  *  | Systems  |  custom info  |  yes  |   |  |
|     | Systems  |  running kernel  |  yes  |   |  |
|  *  | Systems  |  package installed  |  yes  |   |  |
|     | Systems  |  last checkin  |  yes  |   |  |
|     | Errata  |  CVE ID   |  yes  |   |  |
|     | Errata  |  RHBA/RHSA/RHEA ID  |  yes  |
|     | Errata  |  Package Name  |  yes  |   |  |
|     | Errata  |  Issue Date  |  yes  |   |  |
|     | Errata  |  Severity (Bug | Enhancement | Security (Critical, Moderate, Important)  |  no  |   |  |
|     | Software Channels  |  *  |  no  |   |  |
|     | ORG  |  *  |  no  |   |  |
|     | Entitlements  |  *  |  no  |   |  |
|     | User  |  *  |  no  |   |  |

To reiterate, for criteria that are already supported, there are no changes necessary to the search server. Any sort of org filtering is already done on the web UI side currently, so in our implementation we will be working with unfiltered data and be able to apply any org boundaries we choose to.
### Known Issues


### Future Enhancements

The following search abilities have been requested:


 1. Systems
  1. Search By:
   1. hostname 
   1. system id  
   1. custom info field 
   1. running kernel (or package installed)  
  1. View:
   1. running kernel
   1. last check in   
   1. last booted
   1. available updates and errata
   1. applied errata
   1. subscribed channels
   1. full hardware profile   
   1. scheduled action status
   1. action history
   1. system owner / contact info
 1. Errata
  1. Search By:
   1. CVE ID     (Supported in Search-Server)
   1. RHBA/RHSA/RHEA ID   (Supported in Search-Server)
   1. Package Name   (Supported in Search-Server)
   1. Issue Date   (Supported in Search-Server)
   1. Severity (Bug | Enhancement | Security (Critical, Moderate, Important)   
  1. View:
   1. Normal errata details
   1. Systems still affected
   1. Systems with errata applied. (including date)
 1. Software Channels
  1. Search By:
   1. Browsing was requested, not sure if this affects our plans.
  1. View:
   1. List of packages.
   1. List of subscribed systems.
   1. Errata issued (with issued/cloned date)
 1. Organizations
  1. Search By:
   1. Org ID
   1. Org name
   1. Browsing requested.
  1. View
   1. System/software entitlement usage.
 1. Entitlements
  1. Search By:
   1. Browsing requested.
  1. View
   1. Allocation to organizations.
 1. User
  1. Search By:
   1. email address
   1. real name
   1. user id
   1. login name
  1. View:
   1. user details
 1. Activation Keys (look up for user?)
## Mock Ups

Screenshot taken from Support User tab on RHN Hosted:


![Alt](images/hosted-support-user-tab.png?raw=True)

Search Form Mockup:

[[50%)](Image(Supportuser-searchform_whiteboard.png.jpeg,)]

Search Results Mockup:

[[50%)](Image(Supportuser-searchresults_whiteboard.png.jpeg,)]
## Tasks



_TODO: Revisit this after Requirements/Implementation discussions._

Confidence: Values 1-5, where 1 is very unsure of the estimate and 5 is extremely confident it will be met.

Role Creation
|| *Description* || *Estimate* || *Confidence* ||
|| Add support user role to database (install and update scripts) || 1h || 5 ||
|| Make support user role available to UI for users with appropriate permissions || 12h || 4 ||
|| Add org selection page (all or subset, subset selection) || 28h || 4 ||
|| Add hook to trust relationship code to propagate trust removals to user subsets || 4h || 4 ||
*Total: 6d*

Support User Tab
|| *Description* || *Estimate* || *Confidence* ||
|| Add support tab to UI header || 2h || 3 ||
|| Add left navigation elements for the tab. || 2h || 4 ||
*Total: .5d*

Systems List
|| *Description* || *Estimate* || *Confidence* ||
|| Implement page || 8h || 3 ||
*Total: 1d* _This one is a bit fuzzy since we haven't fully flushed out what capabilities it will provide._

System Search Page
|| *Description* || *Estimate* || *Confidence* ||
|| Implement page with search fields and calls to search server || 24h || 3 ||
*Total: 3d*

System Search Results
|| *Description* || *Estimate* || *Confidence* ||
|| Implement page || 16h || 3 ||
*Total: 2d*

APIs
|| *Description* || *Estimate* || *Confidence* ||
|| Verify existing addRole API allows the support user role to be assigned || 1h || 4 ||
|| Add API handler for support.search.system package || 2h || 4 ||
|| Add hostname call (includes QA API test) || 4h || 4 ||
|| Add id call (includes QA API test) || 4h || 4 ||
|| Add customField call (includes QA API test) || 4h || 4 ||
|| Add installedPackageName call (includes QA API test) || 4h || 4 ||
|| Add listSystems call (includes QA API test) || 4h || 4 ||
*Total: 3d*

Read Only Features
|| *Description* || *Estimate* || *Confidence* ||
|| Investigate abilities of no-role user to ensure they are acceptable || 8h || 4 ||
*Total: 1d*
_This investigation may yield small to medium sized tasks relating to existing pages (i.e. what to do about SDC pages for systems in non-home orgs._

Adding up the estimated totals from above, the total time is *17 days* _(Note: this value is still very much a work in progress, this is just a first pass)_
## Test Notes

High level testing areas:

 * Test both new installs and upgrades to make sure support user role is correctly added to database
 * Ability to create support users and see the appropriate support user UI functionality
 * Support user searching works correctly
 * New support user APIs work correctly
## Risks/Concerns

The read only investigation may potentially produce a number of tasks for modifications to existing pages.

## Additional Resources

[Original Wiki Discussion](SupportUserArchive)
