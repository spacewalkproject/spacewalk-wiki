= Script based reporting =

== Requirements ==

[wiki:Features/ScriptBasedReporting/Requirements]

== Specification ==

[wiki:Features/ScriptBasedReporting/Spec]

== Progress ==

=== Script and the inventory report ===

2009-07-24: spacewalk-backend 0.6.24-1 contains /usr/bin/spacewalk-report and the inventory report. 

2009-12-14: spacewalk-reports 0.7.0-1 -- the feature is now in separate package, moved away from spacewalk-backend.

[wiki:Features/ScriptBasedReporting/Inventory]

=== Subscription and entitlement report ===

2009-08-05: spacewalk-backend 0.6.29-1 contains the subscription and entitlement report. 

2009-12-14: spacewalk-backend 0.8.10-1 adds column total to the report.

2009-12-14: spacewalk-reports 0.7.0-1 -- the feature is now in separate package, moved away from spacewalk-backend.

[wiki:Features/ScriptBasedReporting/Entitlements]

=== Basic FISMA Report: Software inventory report - errata out of compliance information ===

2009-10-23: spacewalk-backend 0.7.9-1 contains two errata reports. 

2009-10-30: spacewalk-backend 0.7.11-1 adds column type to the errata-list report.

2009-12-14: spacewalk-reports 0.7.0-1 -- the feature is now in separate package, moved away from spacewalk-backend.

[wiki:Features/ScriptBasedReporting/Erratas]

=== Documentation ===

2009-10-26: spacewalk-backend 0.7.10-1 contains spacewalk-report which supports the --info options, and all four existing reports now have documentation.

2009-11-19: spacewalk-backend 0.7.15-1 dropped the usage of report-specific options; instead of --list-fields --info, use --list-fields-info.

2009-12-14: spacewalk-reports 0.7.0-1 -- the feature is now in separate package, moved away from spacewalk-backend.

=== Basic FISMA Report: user report ===

2009-11-18: spacewalk-backend 0.7.14-1 contains "users" and "users-systems" reports.

2009-12-14: spacewalk-reports 0.7.0-1 -- the feature is now in separate package, moved away from spacewalk-backend.

[wiki:Features/ScriptBasedReporting/Users]
