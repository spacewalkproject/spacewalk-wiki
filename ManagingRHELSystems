== Managing Configuration Files ==
This section details managing configuration files in the spacewal web
interface.  Configuration files can be managed on both a per system
configuration level and a central configuration channel level.  Per system
configuration files always override central channel configuration files, keep
that in mind when deploying configuration files.  Additionally, a system may
be subscribed to more than one centrally managed configuration channels.  When
this is the case, the administrator should ensure that proper channel
hierarchy is maintained.  Channels with lower number rank have higher priority
than channels with a higher number rank, ie Rank 1 files take priority over
Rank 2 files.
=== Systems Configuration Management ===
This section covers managing files on a particular system.  It assumes that
you have selected a system to manage files on by logging in to spacewalk,
choosing {{{Systems}}} from the top menu bar, selecting a particular system to
manage, then choosing {{{Configuration}}} located next to {{{Software}}}.

This view shows how many files are configured for this system and actions
available.  Additionally it shows recent configuration events relevant to this
system such as deploying files or comparing files.

==== System Configuration Channels ====
To view what configuration channels the system is subscribed to choose
{{{Manage Configuration Channels}}}.  This view lists what channels the system
is subscribed to along with the rank and the number of deployable files.  You
can add channels by choosing {{{Subscribe to Channels}}} and then selecting
which channels you want to add to the system.  Once you click {{{Continue}}}
you will be taken to the {{{View/Modify Rankings}}} screen to order the
priority of the configuration channels.  Your channels will not be saved until
you choose {{{Update Channel Rankings}}}.

To remove a configuration channel from the system, check the checkbox next to
the configuration channel(s) you want to remove and then click
{{{Unsubscribe}}}.  This action does not require confirmation so make sure you
double check your selection before clicking {{{Unsubscribe}}}.

Choosing {{{View/Modify Files}}} allows you to view the configuration overview
for the system.  It lists the files that are centrally managed on the system
and whether there are any system specific files that override a centrally
managed file.
=== Centrally Managed Configuration Channels ===
 1. To create a centrally managed configuration channel, log in as a spacewalk administrator and select the Configuration tab from the top menu bar.
 1. Select Configuration Channels
   * You will be presented with a list of the current Centrally Managed Configuration Channels.
   * From this screen you can {{{create new config channel}}} or view information about the previously configured channels.
==== Creating a Centrally Managed Configuration Channel ====
 * Choose {{{create new config channel}}} and then fill in a {{{Name}}}, {{{Label}}}, and {{{Description}}}.
==== Managing a Central Configuration Channel ====
Choosing a configuration channel on the {{{Configuration Channels}}} listing page takes you to the {{{Overview}}} page for the configuration channel.  This page shows the basic information about the channel and various actions that are available such as deploying the configurationt files or comparing files or creating new files.

To see what files are in a configuration channel, choose the {{{List/Remove
Files}}} link from the header.  This view shows you the files that are
contained in the configuration channel along with their revision and when they
were last modified.  Choosing {{{[View]}}} next to a file takes you to the
file details page which lists the information about the file and the contents
(if it is not a directory or binary file).  From this page you may modify the
properties of the file such as the ownership or file permissions.  If you make
any modifications to the file such as modifying the contents or the
permissions, you must click on {{{Update Configuration File}}} or your changes
will be discarded.

If you would like to upload a new version of the configuration file you may
use the {{{Upload New Revision of File}}} section and upload the file from
your computer.  Be sure to click on {{{Upload File}}} after you have selected
the file to upload by clicking on {{{Browse...}}}.
