# Debian/Ubuntu Support in Spacewalk 2.7

Significant improvements to Debian/Ubuntu version parsing and matching were made in the Spacewalk 2.7 release.  The majority of the changes were done in Spacewalk server.  However, there is a change that must be applied to client systems as well.  When upgrading from a previous Spacewalk release to Spacewalk 2.7, it is also required that all Debian/Ubuntu channels be cleaned and synced again to import the right versions

There are still a few rough edges that need some development attention:
- The current version comparison logic does not distinct a dot from an hyphen, a tilde or a plus character.
  This leads to some packages wrongly shown as an update for a client in spacewalk when the package is actually a downgrade.
  The client however uses a correct comparison and handles the package upgrade correctly.
- The deb importer does not import all the package header information into the database and the repository-writer will not write the missing information to the repository metadata served by spacewalk.
  This will lead to problems on the client in case of the missing Multi-Arch header: Clients will try to reinstall the same package over and over again when this header is missing.
- A deb repository provided by spacewalk is not GPG signed and thus will not work without disabling secure-apt.
  Spacewalk imports and recreates the repository based on the imported package catalogue, this will destroy the GPG signing of the repository vendor.
----

## Required Client Update
The client needs to be fixed also to report an X instead of a 0 as release if it is empty.

The file /usr/share/rhn/up2date_client/debUtils.py needs the following change:

    -- a/usr/share/rhn/up2date_client/debUtils.py
    +++ b/usr/share/rhn/up2date_client/debUtils.py
    @@ -28,7 +28,7 @@ def verifyPackages(packages):
     
     def parseVRE(version):
         epoch = ''
    -    release = '0'
    +    release = 'X'
         if version.find(':') != -1:
             epoch, version = version.split(':')
         if version.find('-') != -1:


## Clean, Then Sync Channels After Upgrading To Spacewalk 2.7
Spacewalk 2.7 changes the parsing to handle the debian package version components according to definitions from [here](https://www.debian.org/doc/debian-policy/ch-controlfields.html#s-f-Version).  After an update to Spacewalk 2.7 the existing Debian/Ubuntu channels must be cleaned and synced again to import the right versions.

----

### This Page Is A Work In Progress
We welcome any community input to make this page better.  If you would like to contribute to this page, or any other Spacewalk Wiki pages, please submit Pull request to [spacewalkproject/spacewalk-wiki](https://github.com/spacewalkproject/spacewalk-wiki) repository.  More details can be found [here](WikiContribute).

