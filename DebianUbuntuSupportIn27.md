# Debian/Ubuntu Support in Spacewalk 2.7

Significant improvements to Debian/Ubuntu version parsing and matching were made in the Spacewalk 2.7 release.  The majority of the changes were done in Spacewalk server.  However, there is a change that must be applied to client systems as well.  When upgrading from a previous Spacewalk release to Spacewalk 2.7, it is also required that all Debian/Ubuntu channels be cleaned and synced again to import the right versions

----

## Required Client Update
The client needs to be fixed also to report an X instead of a 0 as release if it is empty.

The file /usr/share/rhn/up2date_client/debUtils.py needs the following change:
{{{
-- a/client/rhel/rhn-client-tools/src/up2date_client/debUtils.py
+++ b/client/rhel/rhn-client-tools/src/up2date_client/debUtils.py
@@ -28,7 +28,7 @@ def verifyPackages(packages):
 
 def parseVRE(version):
     epoch = ''
-    release = '0'
+    release = 'X'
     if version.find(':') != -1:
         epoch, version = version.split(':')
}}}


## Clean, Then Sync Channels After Upgrading To Spacewalk 2.7
Spacewalk 2.7 changes the parsing to handle the debian package version components according to definitions from here: https://www.debian.org/doc/debian-policy/ch-controlfields.html#s-f-Version
After an update to Spacewalk 2.7 the existing Debian/Ubuntu channels must be cleaned and synced again to import the right versions.


