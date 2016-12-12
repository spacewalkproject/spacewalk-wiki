== Inter Spacewalk Sync Spec ==

=== Goal === 

To be able to synchronize content of spacewalk to another directly without need to dump it to disk.

For now, when user have two spacewalk and want to synchronize them, they have to export data to disk, copy it to second spacewalk and synchronize spacewalk from dump on disk. This procedure is painfully slow, because we export and copy whole content, instead of just diff between two spacewalk. 

The Aim here is to sync content between spacewalks, where slave communicated with the xmlrpc and db layers of the master spacewalks to export content and import it back to the slave. We don't anticipate activating this slave against the master. The slaves will be either disconnected or activated against hosted. 

=== ISS Best Practices Whitepaper ===

See http://www.redhat.com/pdf/ISS_Best_Practices_Whitepaper.pdf

=== Additional Content Sync(Future Feature): ===

We need to be able to sync down content other than channel content such as kickstart profiles, user profiles and activation keys.

==== Activation keys: ==== 

* Using Current xmlWire setup:

* Add export activation key to dump/auth_dump handlers on sat-export.

* Add in activation key containers to the sync_handlers.

* The exporter on the master spacewalk exports the activation keys from the db into an xml stream and passes it to satelite-sync.

* Satellite-sync gets this and imports it to its db based on --to-org info or defaults to org-1

* Caching as usual implemented

* With this we should be able to sync ativation keys from master sat as 
  satellite-sync --parent-sat=<sat> --type=activationkeys --orgid=2

'''''Total Dev estimate: 80 hours'''''

Note: Flags/variables are so named in this document just to give an idea. They might change without notice at implementation time based on our mood.
