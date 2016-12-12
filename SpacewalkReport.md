spacewalk-report is a tool that is included as part of the Spacewalk distribution. It outputs useful information in a CSV format.

= To install =
To install spacewalk-report:
{{{
yum -y install spacewalk-report
}}}

= Using spacewalk-report =
Run spacewalk-reports at the command line to list available reports.

{{{
$ spacewalk-report
channel-packages
channels
custom-info
entitlements
errata-channels
errata-list
errata-list-all
errata-systems
inactive-systems
inventory
kickstartable-trees
packages-updates-all
packages-updates-newest
scap-scan
scap-scan-results
system-currency
system-groups
system-groups-keys
system-groups-systems
system-groups-users
system-history
system-history-channels
system-history-configuration
system-history-entitlements
system-history-errata
system-history-kickstart
system-history-packages
system-history-scap
system-packages-installed
users
users-systems
}}}

Then, just run spacewalk-report <report-name> to show the results of a report.

{{{

$ spacewalk-report channels
channel_label,channel_name,number_of_packages
centos-4-i386-base,z CentOS 4 i386 Base,1598
centos-4-x86-64-base,z CentOS 4 x86_64 Base,1843
centos5-base-i386,Centos 5 base i386,5542
centos5-base-x86_64,Centos 5 base x86_64,8694
centos5-centosplus-i386,CentOS 5 CentOSPlus - i386,55
centos5-centosplus-x86_64,CentOS 5 CentOSPlus - x86_64,121
}}}