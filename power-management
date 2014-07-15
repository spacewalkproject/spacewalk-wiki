= Power Management =
Spacewalk 2.2 incorporates power management functionality that is provided by Cobbler and has been made part of the Spacewalk webui. This page will give an overview of power management functionality, for other documentation see the [https://fedorahosted.org/cobbler/wiki/PowerManagement Cobbler documentation].

== What is it? ==
Cobbler power management allows you to control the power state (power on, power off, reboot) physical machines that are managed by a power management systems. Power management systems are commonly found in datacenters managing rack-mounted servers, but may also include network-attached battery backups or virtualization software. Many such power management systems exist, Cobbler interacts with them through various translator scripts known as "fence agents". The fence agents are not installed on Spacewalk by default and must be manually installed if you want to use power management functionality.

Fence agents are available in the base operating system, try installing fence-agents-all on Fedora 19/20 or fence-agents on RHEL 6:
{{{
# On Fedora:
yum install fence-agents-all
}}}
{{{
# On RHEL 6:
yum install fence-agents
}}}
There is no fence-agents rpm available for RHEL 5, so power management functionality is not supported for Spacewalk instances running on RHEL 5.

== Power Management Types ==
Spacewalk is configured by default to use the IPMI power management system, but there are many others in existence. Fence agents generally have a name like "fence_impilan", which is the agent for IPMI. That is the only solution that has been tested and is supported, but others may work just fine. To enable Spacewalk to use other power management systems you can add an extra line in /etc/rhn/rhn.conf with a comma separated list of the fence agents you wish to enable, minus the "fence_" part. For example:
{{{
java.power_management.types = ipmilan,apc
}}}
followed by a 'rhn-satellite restart' would enable both the IPMI and APC fence agents. It is assumed that you know or are able to find the appropriate fence agent for your power management setup.

== Power Management Configuration / Usage ==
There is a new page in the system details pages of the Spacewalk UI that allows you to configure and use the power management system. Go to Systems -> <system> -> Provisioning -> Power Management. From here you can enter the IP address / hostname of the power management server, the username / password needed to log into it, and the identifier for this system in the power management system. Once configured you should be able to power on / off the system at will. There are also new pages in the Provisioning section of the System Set Manager that allow you to configure / manage multiple servers at a time.

Please note that not even all IPMI setups are supported. The fence_ipmilan agent uses the lan protocol, so setups that require the lanplus protocol (for example) will not function correctly.

== Security Warning ==
Any configuration you save on the Power Management page is saved in Cobbler, and is available to any unauthenticated network user that can see your Spacewalk server. This is a known weakness in Cobbler's power management system. For mitigation strategies see the [https://fedorahosted.org/cobbler/wiki/PowerManagement#Important:SecurityImplications Cobbler documentation].