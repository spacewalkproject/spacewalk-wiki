= Errata & CVE Enhancements =

== Requirements ==

=== Errata Overview page ===

Security Errata, CVE's - allow for sorting/filtering/searching of Errata & CVE's 
* Errata browsing page, to view security errata

Currently on rhn/systems/details/ErrataList.do we have a drop down menu box to filter errata by type:

 * All
 * Non Critical
 * Bug Fix Advisory
 * Product Enhancement Advisory
 * Security Advisory

We need to provide a similar capacity for Errata Overview page to see only Security, Bug Fix and Product Enhancement Errata for rhn/errata/Overview.do

This could be left side tab menu for 'Security', etc or a drop-down menu. 

=== Advisory column ===

* For top level Errata Overview page, beyond allowing for filter/display of only Security Errata, within the UI add and render a column for the associated CVE's to the Errata 

NOTES 
* Errata Search allows to search by CVE's
* CVE's are listed within the content/text of Security Errata - rhn/errata/details/Details.do
* Currently do not expose CVE's for Errata in the SDC (per system) Errata pages - this change is NOT to expose the CVE's as we will in top level Errata Overview page rhn/systems/details/ErrataList.do

== Specification ==

=== Errata Overview page ===

After discussions, it was agreed to allow advanced errata filtering. The user shall be able to filter errata according to relevancy (whether listed errata affect registered systems) and according to errata type at the same time.

Left side menu already allows filtering according to the relevancy.

So there's a need to create an extra tab menu for all overview errata pages - with errata type items - All Errata / Bug Errata / Enhancement Errata / Security Errata - what ensures additional errata filtering according to the type.
Affected errata pages are:

 * rhn/errata/Overview.do
 * rhn/errata/RelevantErrata.do
 * rhn/errata/AllErrata.do

=== Advisory column ===

Need to get all CVEs associated to a single errata and display them within a special column on errata/*!SecurityErrata.do pages.

== Progress ==
 * 2009-11-09 implementation done
 * 2009-11-11 code enhancements
 * 2009-11-13 enhancement errata created
