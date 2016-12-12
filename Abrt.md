= ABRT =

[https://fedorahosted.org/abrt/ ABRT] is a tool that helps users to detect defects in applications and to create a bug report with all information needed by a maintainer to fix it. It uses plugin system to extend its functionality.

== Spacewalk & ABRT ==

The main purpose of this plug-in is to send number
of crashes that happened on a given machine back to the
spacewalk server and gain attention
of the sysadmin, who should investigate these
crashes and/or configure ABRT reporting so these
issues doesn't go unnoticed.

After installing 
{{{spacewalk-abrt}}} package {{{rhn-check}}} will automatically start reporting
ABRT information back to the server.



Currently (with Spacewalk 1.7 nightly), 
the number of crashes is shown only on
system detail page ({{{systems/details/Overview.do}}})
under ''Application crashes'' header. If there are
no ABRT data available, it prompts the user for
installing {{{spacewalk-abrt}}} package.

Later, this feature can be extended to
- display data on summary ({{{YourRhn.do}}}) page;
- collect more information about crashes.

Following issues were identified recently:
 - if the {{{spacewalk-abrt}}} package is removed from
 the system, data on the server remain unchanged,
 showing inaccurate information;
 - there's no way to dismiss this information;
 - time of the last crash should be present.