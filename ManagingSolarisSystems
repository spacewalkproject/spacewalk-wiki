'''''DEPRECATED, NO LONGER USED'''''

[[PageOutline]]

= __Managing Solaris Systems__ =
We haven't gotten a solaris build environment up yet for spacewalk, so until we do all files you need can be found here: http://spacewalk.redhat.com/solaris/ 
In each tarball is a Readme file which should go over what you need to do to install the client side packages within solaris.
In order to push patches and patch clusters, you first have to convert them to Mpm files.  Here is the general order of doing things:

== Channel Setup Channel ==
 1.  Create a channel with a solaris arch type on the server

== Provisioning Clients ==
== Registering Clients ==

== Pushing Packages to Clients Pushing ==
 2.  On the Spacewalk Server run: 'solaris2mpm Recommended.zip'  where Recommended.zip is your patch cluster or patch. This will create a bunch of mpm files.
 3.  Now push the mpms into your channel:  'rhnpush --server=http://SW_FQDN/APP --channel=CHANNEL_LABEL  *.mpm'  Where SW_FQDN is the FQDN of the server, and CHANNEL_LABEL is the label of the channel you made in step 1.

== Managing Client Configurations ==

== Related Documentation ==
 * [http://spacewalk.redhat.com/solaris/]