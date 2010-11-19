[[PageOutline]]

= __Spacewalk 1.2 Release Notes__ =

Hello world,

Spacewalk 1.2 is now available for download from

        * http://spacewalk.redhat.com/yum/1.2/RHEL/5/$basearch/
        * http://spacewalk.redhat.com/yum/1.2/Fedora/12/$basearch/
        * http://spacewalk.redhat.com/yum/1.2/Fedora/13/$basearch/
        * http://spacewalk.redhat.com/yum/1.2/Fedora/14/$basearch/

depending on your operating system, with client repositories under

        * http://spacewalk.redhat.com/yum/1.2-client/

Check the installation steps at

        * [wiki:HowToInstall]

or if you will upgrade from older release, consult

        * [wiki:HowToUpgrade]

== Features & Enhancements in Spacewalk 1.2 ==

        * Support for re-provisioning of FV Xen & KVM guests was added.
        * Large rpm support fixes.
        * Fixes to spacewalk-repo-sync.
        * Staging content (prefetching content to clients in advance)
          made more solid.
        * Multiple WebUI pages were ported from Perl to Java.
        * Localization extended.
        * PostgreSQL database backend can be used for limited operation.
        * Spacewalk 1.2 runs on Fedora 14.
        * Updated and new API calls:
                * configchannel.lookupFileInfo
                * configchannel.createOrUpdateSymlink
                * channel.software.createRepo
                * channel.software.associateRepo
                * channel.software.getRepoDetails
                * channel.software.updateRepo
                * channel.software.updateRepoUrl
                * channel.software.updateRepoLabel
                * channel.software.disassociateRepo
                * channel.software.removeRepo
                * channel.software.listUserRepos
                * channel.software.removeErrata
                * satellite.isMonitoringEnabled
                * satellite.isMonitoringEnabledBySystemId
                * schedule.archiveActions
                * system.getSystemCurrencyMultipliers
                * system.getSystemCurrencyScores
                * channel output now shows all associated repos

Spacewalk 1.2 is the last version to run on Fedora 12.

== Some notes about the PostgreSQL support ==

We've been fixing bits and pieces that prevented Spacewalk to use
PostgreSQL database server to store the data. Currently known to work are:

         * content sync with satellite-sync and spacewalk-repo-sync;
         * rhnpush;
         * client registration;
         * yum operations.

So it is possible to use Spacewalk on PostgreSQL to manage the rpm
packages on your machines. For the above mentioned areas, please use
bugzilla to report the bugs just like you would normally do.

The PostgreSQL port is however far from complete, so sooner than later
you will encouncer weird behavior or Internal Server Error, especially
if you use functionality not listed above. For that we will appreciate
patch rather than bugzilla report -- we know these do not work and we
need help. If you are ready to code, please check

        * [wiki:PostgreSQL]
        * [wiki:PostgreSQLPortingGuide]

== Community contributors ==

We thank the community members who contributed to this release:

        * Ali Yousefi Sabzevar
        * Aron Parsons
        * Colin Coe
        * Francesco Tombolini
        * Jaswinder Singh Phulewala
        * Joshua Roys
        * !KrishnaBabu Krothapalli
        * Luc de Louw
        * Manoj Kumar Giri
        * Timo Trinks
        * Yulia Poyarkova

with special kudos to Colin Coe for his Java porting and API work.

 * [wiki:ContributorList]

Yours,

-- [[BR]]
Jan Pazdziora [[BR]]
Principal Software Engineer, Satellite Engineering, Red Hat
