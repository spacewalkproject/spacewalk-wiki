[[PageOutline]]

= __Configuration Management__ =

== Features ==
 * File Support:
  * Binary Files
  * Text files
  * Directories
  * Sym-links

 * File Attribute support:
  * User/Group ownership
  * Mode Permission
  * SELinux permissions

== Getting started ==
This assumes you have pushed the Spacewalk client repository to your Spacewalk server and subscribed a server to it.
 * Install the client utilities:
{{{
yum install rhncfg*
}}}
 * Enable config management:
{{{
rhn-actions-control --enable-all
}}}
 * Within the web GUI create a configuration channel and add files.
 * Subscribe the client to the configuration channel.
 * Either schedule a config deploy from the webUI or run 'rhncfg-client get' on the client.

== Client utility functions ==
 * rhncfg-client -- Meant for managing the configuration files in relation to what is on the client.
  * diff
  * get
  * list
  * elist
  * channels
  * verify

 * rhncfg-manager -- Meant for managing the configuration files on the server.
  * add
  * create-channel
  * diff
  * diff-revisions
  * download-channel
  * get
  * list
  * list-channels
  * remove
  * remove-channel
  * revisions
  * update
  * upload-channel

== FAQ ==
1. Can I automatically restart a service after a configuration file is deployed?
   No, you can schedule a separate remote command though.  Hopefully this is coming soon.

2. Can I upload files to the local sandbox or override list from the client (not to a configuration channel)?
   No, you can only upload to a specified configuration channel from the client.