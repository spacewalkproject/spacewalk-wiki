[[PageOutline]]

= Adding revisioning to kickstart pre/post scripts =
== Rational ==
Allow Spacewalk/RHN Satellite admins to view previous kickstart pre/post script contents.  Ideally this would give similar functionality to that provided by the managed config files.
== Requirements ==
 * Not block out other potential features (i.e. re-ordering of pre/post scripts)
 * Have minimal impact on other Spacewalk/RHN Satellite sub-systems (think API calls)
== Nice to have features ==
 * Be able to use macros, a la managed config files.
== High level events ==
=== Create script ===
 * Pretty much un-changed from now (except for table layouts).
=== Update script ===
 * A new row should be created for each edit but the 'created' column should be the same as the that of the original copy of the script.
=== Delete script ===
 * A delete of the script should result in all copies of the script being deleted.
=== Delete script revision ===
 * Should only delete that script revision, nothing more.
== Lower level stuff ==
=== DB Changes ===
==== rhnKSData ====
===== Remove columns: =====
 * POST
 * PRE
 * NOCHROOT_POST
==== rhnKickstartScript ====
Add columns
 * CURRENT_REVISION
===== Remove columns =====
 * SCRIPT_TYPE
 * CHROOT
 * INTERPRETER
 * DATA
=== New tables ===
==== rhnKickstartScriptContent ====
 * ID (PK)
 * TYPE
 * INTERPRETER
 * DATA
 * KS_SCRIPT_ID (ID from rhnKickstartScript)
 * MODIFIED
== Proposed table layout for comment ==

[[Image(kss_revisioning.jpg)]] 