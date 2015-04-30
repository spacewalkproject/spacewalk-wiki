= Upload content to Spacewalk =

In order to use Spacewalk, you need to populate it with content.  There are three ways to do this:
 1. spacewalk-repo-sync (For syncing yum repos)
 2. rhnpush (For simply pushing a rpms directly to the spacewalk server)
 3. satellite-sync (For syncing channels/packages from another Spacewalk)

Before doing either of these things, you will need to create a custom software channel within spacewalk.


== Create Channel ==

The easiest way to create a channel is through. the WebUI.  Simply log in and navigate to the "Channels"->"Manage Software Channels"-> "Create"  page.  


Alternatively you can use {{{spacewalk-common-channels}}} script from {{{spacewalk-utils}}} package to create a predefined set of channel from the command line. It also creates desired activation keys and definitions for external repositories. 

For example, to create i386 and x86_64 channels for CentOS 5, activation keys and their spacewalk client subchannels:
 {{{/usr/bin/spacewalk-common-channels -v -u admin -p pass -a i386,x86_64 -k unlimited 'centos5*' 'spacewalk-nightly-client*'}}}

Usage:
{{{
# spacewalk-common-channels -h
Usage: spacewalk-common-channels [options] <channel1 glob> [<channel2 glob> ... ]

Options:
  -c CONFIG, --config=CONFIG
                        configuration file
  -u USER, --user=USER  username
  -p PASSWORD, --password=PASSWORD
                        password
  -s SERVER, --server=SERVER
                        your spacewalk server
  -k KEY_LIMIT, --keys=KEY_LIMIT
                        activation key usage limit - 'unlimited' or number
                        (default: options is not set and activation keys are
                        not created at all)
  -n, --dry-run         perform a trial run with no changes made
  -a ARCHS, --archs=ARCHS
                        list of architectures
  -v, --verbose         verbose
  -l, --list            print list of available channels
  -h, --help            show this help message and exit


Examples:

Create Fedora 12 channel, its child channels and activation key limited to 10 servers:
    /usr/bin/spacewalk-common-channels -u admin -p pass -k 10 'fedora12*'

Create Centos 5 with child channels only on x86_64:
    /usr/bin/spacewalk-common-channels -u admin -p pass -a x86_64 'centos5*'

Create only Centos 4 base channels for intel archs:
    /usr/bin/spacewalk-common-channels -u admin -p pass -a i386,x86_64 'centos4'

Create Spacewalk client child channel for every (suitable) defined base channel:
    /usr/bin/spacewalk-common-channels -u admin -p pass 'spacewalk-client*'

Create everything as well as unlimited activation key for every channel:
    /usr/bin/spacewalk-common-channels -u admin -p pass -k unlimited '*'

}}}




== Repo-sync ==

Intruduced in Spacewalk 0.6, repo sync was enhanced in Spacewalk 1.1. For version 1.1 and newer proceed as following :

Goto Channels -> Manage Software Channels -> Manage Repositories -> create new repository

[[Image(spacewalk_reposync_v1_2_scaled.jpeg)]]

After creating the repository, you need to link it to one or more Software Channels. Goto: Channels -> Manage Software Channels -> Choose the channel to be linked -> Repositories -> Select the repositories to be linked to the channel -> Update Repositories. Now you can sync the repository clicking on the sync tab. Click on sync now or schedule a sync. If you used {{{spacewalk-common-channels}}} it has already setup repositories for you.

Alternatively you can start a sync of a yum repository defined in the web ui by command line:

{{{
spacewalk-repo-sync --channel CHANNEL_LABEL 
}}}

You can use any repo url that is supported by yum (http://, file://, etc...).  If running the command line directly, you must be the root user.

For more information also see
{{{
man 8 spacewalk-repo-sync
}}}

If, when doing a spacewalk-repo-sync, you get a "yum.Errors.NoMoreMirrorsRepoError" error then you need to install python-hashlib.

The logs are stored in /var/log/rhn/reposync/

== rhnpush ==

If the packages you are wanting to push to the Spacewalk server are not in a yum repository, you can push them directly using the rhnpush tool.


For example, to upload a directory of packages called 'dir_of_packges' to the channel 'my-custom-channel':
{{{
rhnpush -v --channel=my-custom-channel --server=http://localhost/APP --dir=dir_of_packages
}}}

Note: Make sure {{{/var/satellite}}} exists on the Spacewalk server and has owner:group {{{apache}}} before pushing. The -v option causes progress information to be printed, e.g.:

{{{
# rhnpush -v --channel=rhel-x86_64-server-5 --server=http://localhost/APP --dir=update
Connecting to http://localhost/APP
Package /root/update/xorg-x11-drv-vga-4.1.0-2.1.x86_64.rpm Not Found on RHN Server -- Uploading
Uploading package /root/update/xorg-x11-drv-vga-4.1.0-2.1.x86_64.rpm
Using POST request
Package /root/update/gnome-python2-gconf-2.16.0-1.fc6.x86_64.rpm Not Found on RHN Server -- Uploading
Uploading package /root/update/gnome-python2-gconf-2.16.0-1.fc6.x86_64.rpm
Using POST request
}}}

Note: I found that the above failed on Fedora 9 Everything.  My work around was
{{{
find . -name "*rpm" | xargs rhnpush --channel=fedora-9-i386 --server=http://localhost/APP -v --tolerant -u spacewalk -p spacewalk
}}}

Note: The directory you pass as --dir=suchandsuch HAS TO contain the RPMs. rhnpush won't descend into directories to find RPMs.
      Also, I had to put the -v at the end of the group of parameters.
        --onekopaka

Note: If you are running rhnpush from a box that is NOT your spacewalk server, use the correct hostname in the --server param

== satellite-sync ==

You have to configure "inter satellite sync" (aka ISS) on your source (master) Spacewalk and target (slave) one.