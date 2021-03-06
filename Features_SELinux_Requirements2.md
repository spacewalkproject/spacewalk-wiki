# SELinux Requirements



Revised by Jan Pazdziora, January 16, 2009.
## Overview



Security-enhanced Linux (SELinux) adds type enforcement rules to the standard Linux distro that will allow Spacewalk and Proxy to take advantage of more secure access control. SELinux adopts a mandatory access control mechanism that is embedded in the kernel and checks for allowed operations after standard Linux discretionary access controls are checked. SELinux access control is based on security contexts associated with the various resources such as files, processes, and ports. For example, a specific Spacewalk or Proxy process would need both write permission and write access before writing to a file. The type enforcement rules are contained within a security policy that can be installed on a Linux server. 
## Business Justification



 * By the very nature of SELinux, creating a custom policy for Spacewalk will increase the security of Spacewalk and Proxy server. 
 * Attract new users. GSS estimates that roughly 20% of the current Satellite customer install base would like to have a SELinux supported and would also bring more interest, especially in the government sector.
 * We ship it, we support it. Reinforces Red Hat's position on using and supporting its own products. 
 * We have a previous initial attempt at a SELinux policy for Satellite and Proxy 4.x on Red Hat Enterprise Linux 4 AS online in the Red Hat Knowledgebase. This policy has not been maintained since release, but can be used for idea of some code location changes needed in current Spacewalk code. 
  * Satellite changes - http://kbase.redhat.com/faq/FAQ_49_6086.shtm
  * Satellite rules - http://kbase.redhat.com/faq/FAQ_49_6089.shtm
  * Proxy changes and rules - http://kbase.redhat.com/faq/FAQ_48_6088.shtm
## Deployment



 * Spacewalk SELinux policy module will support the Targeted SELinux policy and require the selinux-policy-targeted rpm as a prerequisite to Spacewalk installation
 * Proxy SELinux policy module will support the Targeted SELinux policy and require the selinux-policy-targeted rpm as a prerequisite to Proxy installation
 * Spacewalk SELinux policy module will be deployed in RPM format and named spacewalk-selinux
 * Other -selinux packages might be needed for optional parts of Spacewalk server, or when it will make sense to break the SELinux modules into multiple pieces.
 * Proxy SELinux policy module will be deployed in RPM format and named spacewalk-proxy-selinux 
 * Spacewalk SELinux policy module will be supported on Enterprise Linux 5 systems and Fedora (whichever version is current at time of feature development work ending)
 * Proxy SELinux policy module will be supported on Enterprise Linux 5 systems and Fedora (Whichever version is current at time of feature development work ending)
 * Spacewalk and Proxy installation will support both SELinux states: Enforcing and Permissive
### Differences from previous version of Requirements



 * Package names changed (dropped the rhn- prefix).
 * More packages possible.
 * No support for EL 4.
 * No support for Disabled.
## Security Goal

This section will describe at a high level what security precautions will be implemented for Proxy and Spacewalk. Some considerations are:


 * Service Confinement - make sure all Spacewalk and Proxy services have the minimum amount of access required to function properly 
 * System Protection - protect the system from Spacewalk and Proxy services to remove any possible exploitations 
 * Configuration files - protect config files from domains that do not need access 
 * Domain types - each Proxy and Spacewalk process will need one or more domain types
 * Entrypoints - need to have at least one entrypoint executable file type for each of the domains 
 * Application resources - need to have one more more types for the resources controlled by Proxy and Spacewalk. Examples are temp files, config files, sockets, log files, web files, pid files, etc) 
 * Network access - what network interfaces will Proxy and Spacewalk services be allowed access? Which ports can Spacewalk and Proxy use? DNS name resolution, etc. 
## The investigation and development work



[[Features_SELinuxNotes]]