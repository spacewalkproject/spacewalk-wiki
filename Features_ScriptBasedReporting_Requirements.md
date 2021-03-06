# Script based reporting: Requirements

## Content of the feature




 * Canned, but open source, scripts for some key, highly desired reports
 * Entitlement / subscriptions report
 * Inventory report
 * Software / errata report
 * FISMA (federally mandated) report
 * Event reporting
## Detailed Requirements

### Scripted subscription and entitlement canned report




This is a report users might run every month or every year in order to determine charge-backs to different groups/customers using the Spacewalk and to better grasp real costs of Spacewalk managed systems.
#### Goal



Determination of usage of consumed product denominated by subscriptions and entitlements.
#### Preconditions (assumptions)



Spacewalk is installed and all manageable/purchased systems are connected and managed by Spacewalk. Organizations are defined and systems are allocated appropriately.
#### Main Flow / Description



 1. User (or delegate) changes to general script directory on the Spacewalk server
 2. User executes script
 3. Based on script parameters, a CSV is generated that delineates per organization per group subscription and entitlement report of systems registered to the Spacewalk server. Report contains enough breakdown of the subscriptions and entitlements to validate all discrete factors in what defines a subscription or entitlement. This should be more of a summarization of subscriptions and entitlements.
 4. Report also contains initiation date of consumption of those subscriptions and entitlements.
 5. Report also contains expiration information of those subscriptions and entitlements.
 6. Report also contains dependent relationships between subscriptions and entitlements.
 7. Layered entitlements need to be represented.
 8. TODO: example report.
#### Alternative flows

(Alternative flows due to exceptions or alternate course of action described in the following section.)


 1. n/a
#### Post Conditions



State is unchanged. This is a read-only operation.
### Scripted inventory canned report



This is a report users might run every month or every year in order to better understand the hardware or identified base objects (example, hardware) that are being managed.
#### Goal



Generate a report broken down by physical and virtual hardware and network information. Also included will be some basic information surrounding check-in dates, entitlements and package installation status. As for entitlement/subscription information, think entitlement report, but per system.
#### Preconditions (assumptions)



Spacewalk is installed and all manageable/purchased systems are connected and managed by Spacewalk. Organizations are defined and systems are allocated appropriately.
#### Main Flow / Description



 1. User (or delegate) changes to general script directory on the Spacewalk server
 2. User executes script
 3. Based on script parameters, a CSV is generated that delineates per organization per group report of managed systems registered to the Spacewalk server.
 4. Contained within the report:
    1. Profile name
    2. hostname
    3. IP address
    4. Registered by (if avail)
    5. Registration time
    6. Last checkin time
    7. kernel version (X getRunningKernel)
    8. How many packages out of date
    9. How many errata are out of date
    10. Software channels
    11. Configuration channels
    12. Entitlements (software + system)
    13. System group memberships
    14. System organization memberships
    15. Virtual host association
    16. Architecture 
    17. Hardware info: sockets/cores, network info...
 5. TODO: example report.
#### Alternative flows



(Alternative flows due to exceptions or alternate course of action described in the following section.)

 1. n/a
#### Post Conditions



State is unchanged. This is a read-only operation.
### Scripted Basic FISMA Report



This is a report specific to users interested in FISMA type reports, but useful to all. This report will result in aiding users to meet reporting requirements surrounding software installed with a focus on security.
#### Additional information:



http://www.sans.org/cag/print.php
Twenty Most Important Controls and Metrics for Effective Cyber Defense and Continuous FISMA Compliance
#### Goal



Generate a report in compliance with the basic requirements of security software installation needs.
#### Preconditions (assumptions)



Spacewalk is installed and all manageable/purchased systems are connected and managed by Spacewalk. Organizations are defined and systems are allocated appropriately.
#### Limitations



FISMA has a configuration tracking component that is not a part of this feature for this release.
#### Main Flow / Description



 1. User (or delegate) changes to general script directory on the RHN Satellite
 2. User executes script
 3. Based on script parameters, a set of CSVs are generated that...
   1. Per organization user role rights and ability information.
   2. Delineate per organization / group report of managed systems registered to the RHN Satellite.
 4. User based report
   1. TODO: further breakdown needed - Organization admins, Group admins, Channel admins, Configuration admins, per system access, etc.
 5. Inventory report
   1. See above
 6. Software inventory report - errata out of compliance information
   1. CVE
   2. Advisory
   3. Severity
   4. Synopsis
   5. Systems affected
     1. Profile name
     2. hostname
     3. IP address
 7. TODO: example reports
#### Alternative flows



(Alternative flows due to exceptions or alternate course of action described in the following section.)

1.n/a
#### Post Conditions



State is unchanged. This is a read-only operation.
### Report on events applied to a system (bugzilla 156311)



This is a "should have" requirement.

No details.
## Specification



See [[Features_ScriptBasedReporting_Spec]] for specification based on these requirements.
## Progress



See [[Features_ScriptBasedReporting]] for the overall status of the feature.

