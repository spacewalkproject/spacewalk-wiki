# Proxy Pre-Cache



When using Spacewalk Proxy 2.2 connected to an upstream Spacewalk 2.2 instance Proxy now has the ability to pre-cache or mirror RPMs so that they will not need to be downloaded from the upsteam Spacewalk server. Clients requesting the RPMs will receive them immediately from Proxy, instead of waiting for Proxy to download them from Spacewalk first and then serve them to the client. Proxy recognizes RPM requests from yum as well as anaconda (kickstarts / provisioning). Much of this information is also available in the rhn_package_manager man page:


    man rhn_package_manager

This may be particularly useful if the network connection to Spacewalk is slow and/or bandwidth is precious. RPMs may be loaded into Proxy's cache manually using rhn_package_manager, or automatically by putting an rsync command into a cron job.
## Manually Through rhn_package_manager

Spacewalk Proxy and rhn_package_manager have been updated to avoid unwanted cache collisions, which is what made this feature possible but should be invisible to the user. The previously-existing rhn_package_manager option --copyonly can be used to populate the cache, and an alias to that option has been added with the more user-friendly name --cache-locally. The only other major visible change to rhn_package_manager is that it now knows how to read and import packages from a channel export, which could for example be created on the Spacewalk server using the rhn-satellite-exporter utility. This is in addition to the other methods that rhn_package_manager can use to import RPMs, like the --dir option for importing all RPMs in a directory or a list of RPMs supplied on the command line.


As an example, if you had a channel export from the Spacewalk server that contained all the channels that Spacewalk contains mounted on /mnt/export and you wanted to only cache RPMs that are in the my-channel-1 channel, you could do so by executing:

    rhn_package_manager --cache-locally --from-export /mnt/export --channel my-channel-1

Importing all the RPMs that the channel export contains can be done by simply leaving off the --channel option:

    rhn_package_manager --cache-locally --from-export /mnt/export

If the channel export is spread across multiple ISO images it is not necessary to re-combine them locally on the Proxy before running rhn_package_manager, simply mount them one at a time and run the same command on each.
## Automatic Updates With RSync

Populating the initial clone may be convenient and fast through a channel export or directory with RPMs in it, but it would be inconvenient to have to create a new channel export or directory containing new RPMs every time the Spacewalk server is updated with new content. To that end, it is possible to set up a cron job or similar with a simple rsync command that will download any updated RPMs to the Proxy cache, as long as it is desired that *all* RPMs on the Spacewalk instance should be cached on the Proxy. The cron job / rsync will need to run as root on the Proxy, but can log in to Spacewalk as any user that has read access to the RPM store (should be all users).


First you must set up ssh keypairs such that root on the Proxy can ssh to $user on the Spacewalk server without typing a password. Note: this unfortunately means that you cannot have a password on this private key. Ensure that you keep it safe!

For example, as root on the Proxy:

    ssh-keygen -t rsa -N '' -f /root/.ssh/id_dsa
    ssh-copy-id -i /root/.ssh/id_dsa.pub user@spacewalk.example.com # You'll have to type your password after this
    # You should then be able to do (without typing in any password):
    ssh user@spacewalk.example.com

If the above does not work then debug your ssh configuration and make it work.

Once that is working add a cron job with an appropriate rsync command. For example, as root on the Proxy: 

    crontab -e
    1 3 * * * /usr/bin/rsync -r user@spacewalk.example.com:/var/satellite/redhat/*/ /var/spool/rhn-proxy/rhn
    # write and quit with :wq

The above command will at 3:01 AM every day run the rsync command to download any RPMs that are on the Spacewalk server into the Proxy cache. /var/satellite is the default Spacewalk root directory, but it is configurable so if you've changed it on your Spacewalk installation you'll need to adjust this command appropriately.