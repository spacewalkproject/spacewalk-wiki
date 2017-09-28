# Ubuntu Channel Instructions
These are upgrade instructions for rebuilding Ubuntu Channels when updating Spacewalk 2.6 to Spacewalk 2.7

If you have channels for Ubuntu releases and packages, you need to clear them out and reload them to recreate the package index entries in the database and gain the benefits of the PR500 fixes.

----
## Clear all existing channels, repository, and packages for Ubuntu releases

Execute the following commands to get a list of all your channels and repositories. Identify which channels and repos are related to ubuntu releases

    # spacecmd softwarechannel_list
    # spacecmd repo_list    

Execute the following commands for each Ubuntu channel and repo. Delete all the channels first and then the repos.

    # spacecmd softwarechannel_delete [channel_name] 
    # spacecmd repo_delete [repo_name] 

Once all the Ubuntu channels and repos are deleted, you can recycle the packages with the commands

    # spacecmd package_removeorphans 
    # spacewalk-data-fsck -r -S -C -O

This process takes quite a while to complete.

To rebuild the channels you can execute the following commands. Add or remove commands depending on whether you use packages from universe or multiverse repositories

    # spacecmd softwarechannel_create ' -n "Ubuntu 14.04 (x86_64)" -l "trusty" -a amd64-deb -c sha256 -p "" '
    # spacecmd softwarechannel_create ' -n "trusty-backports" -l "trusty-backports" -a amd64-deb -c sha256 -p "trusty" '
    # spacecmd softwarechannel_create ' -n "trusty-security" -l "trusty-security" -a amd64-deb -c sha256 -p "trusty" '
    # spacecmd softwarechannel_create ' -n "trusty-security-universe" -l "trusty-security-universe" -a amd64-deb -c sha256 -p "trusty" '
    # spacecmd softwarechannel_create ' -n "trusty-spacewalk-client" -l "trusty-spacewalk-client" -a amd64-deb -c sha256 -p "trusty" '
    # spacecmd softwarechannel_create ' -n "trusty-updates" -l "trusty-updates" -a amd64-deb -c sha256 -p "trusty" '
    # spacecmd softwarechannel_create ' -n "trusty-updates-universe" -l "trusty-updates-universe" -a amd64-deb -c sha256 -p "trusty" '
    # spacecmd softwarechannel_create ' -n "Ubuntu 16.04 (x86_64)" -l "xenial" -a amd64-deb -c sha256 -p "" '
    # spacecmd softwarechannel_create ' -n "xenial-backports" -l "xenial-backports" -a amd64-deb -c sha256 -p "xenial"'
    # spacecmd softwarechannel_create ' -n "xenial-security" -l "xenial-security" -a amd64-deb -c sha256 -p "xenial"'
    # spacecmd softwarechannel_create ' -n "xenial-security-universe" -l "xenial-security-universe" -a amd64-deb -c sha256 -p "xenial"'
    # spacecmd softwarechannel_create ' -n "xenial-updates" -l "xenial-updates" -a amd64-deb -c sha256 -p "xenial" '
    # spacecmd softwarechannel_create ' -n "xenial-updates-universe" -l "xenial-updates-universe" -a amd64-deb -c sha256 -p "xenial" '

Next create the repos, substituting in the URL a mirror site which provides low latency and high bandwidth access from your location
    
    # spacecmd repo_create ' -n "External - Ubuntu 14.04 backports x86_64" -u "http://<preferredarchive.ubuntu.com>/ubuntu/dists/trusty-backports/main/binary-amd64/" -t deb'
    # spacecmd repo_create ' -n "External - Ubuntu 14.04 security x86_64" -u "http://<preferredarchive.ubuntu.com>/ubuntu/dists/trusty-security/main/binary-amd64/" -t deb'
    # spacecmd repo_create ' -n "External - Ubuntu 14.04 universe security x86_64" -u "http://<preferredarchive.ubuntu.com>/ubuntu/dists/trusty-security/universe/binary-amd64/" -t deb'
    # spacecmd repo_create ' -n "External - Ubuntu 14.04 updates x86_64" -u "http://<preferredarchive.ubuntu.com>/ubuntu/dists/trusty-updates/main/binary-amd64/" -t deb'
    # spacecmd repo_create ' -n "External - Ubuntu 14.04 universe updates x86_64" -u "http://<preferredarchive.ubuntu.com>/ubuntu/dists/trusty-updates/universe/binary-amd64/" -t deb'
    # spacecmd repo_create ' -n "External - Ubuntu 14.04 x86_64" -u "http://<preferredarchive.ubuntu.com>/ubuntu/dists/trusty/main/binary-amd64/" -t deb'
    # spacecmd repo_create ' -n "External - Ubuntu 16.04 backports x86_64" -u "http://<preferredarchive.ubuntu.com>/ubuntu/dists/xenial-backports/main/binary-amd64/" -t deb'
    # spacecmd repo_create ' -n "External - Ubuntu 16.04 security x86_64" -u "http://<preferredarchive.ubuntu.com>/ubuntu/dists/xenial-security/main/binary-amd64/" -t deb'
    # spacecmd repo_create ' -n "External - Ubuntu 16.04 universe security x86_64" -u "http://<preferredarchive.ubuntu.com>/ubuntu/dists/xenial-security/universe/binary-amd64/" -t deb'
    # spacecmd repo_create ' -n "External - Ubuntu 16.04 updates x86_64" -u "http://<preferredarchive.ubuntu.com>/ubuntu/dists/xenial-updates/main/binary-amd64/" -t deb'
    # spacecmd repo_create ' -n "External - Ubuntu 16.04 universe updates x86_64" -u "http://<preferredarchive.ubuntu.com>/ubuntu/dists/xenial-updates/universe/binary-amd64/" -t deb'
    # spacecmd repo_create ' -n "External - Ubuntu 16.04 x86_64" -u "http://<preferredarchive.ubuntu.com>/ubuntu/dists/xenial/main/binary-amd64/" -t deb'
    # spacecmd softwarechannel_addrepo '"trusty" "External - Ubuntu 14.04 x86_64"'
    # spacecmd softwarechannel_addrepo '"trusty-backports" "External - Ubuntu 14.04 backports x86_64"'
    # spacecmd softwarechannel_addrepo '"trusty-security" "External - Ubuntu 14.04 security x86_64"'
    # spacecmd softwarechannel_addrepo '"trusty-security-universe" "External - Ubuntu 14.04 universe security x86_64"'
    # spacecmd softwarechannel_addrepo '"trusty-updates" "External - Ubuntu 14.04 updates x86_64"'
    # spacecmd softwarechannel_addrepo '"trusty-updates-universe" "External - Ubuntu 14.04 universe updates x86_64"'
    # spacecmd softwarechannel_addrepo '"xenial" "External - Ubuntu 16.04 x86_64"'
    # spacecmd softwarechannel_addrepo '"xenial-backports" "External - Ubuntu 16.04 backports x86_64"'
    # spacecmd softwarechannel_addrepo '"xenial-security" "External - Ubuntu 16.04 security x86_64"'
    # spacecmd softwarechannel_addrepo '"xenial-security-universe" "External - Ubuntu 16.04 universe security x86_64"'
    # spacecmd softwarechannel_addrepo '"xenial-updates" "External - Ubuntu 16.04 updates x86_64"'
    # spacecmd softwarechannel_addrepo '"xenial-updates-universe" "External - Ubuntu 16.04 universe updates x86_64"'

Execute these commands to set up the sync schedule, adjusting the frequency as appropriate for your organization.

    # spacecmd softwarechannel_setsyncschedule '"trusty" 0 5 1 8 * ?'
    # spacecmd softwarechannel_setsyncschedule '"trusty-backports" 0 15 1 8 * ?'
    # spacecmd softwarechannel_setsyncschedule '"trusty-security" 0 10 1 8 * ?'
    # spacecmd softwarechannel_setsyncschedule '"trusty-security-universe" 0 16 1 8 * ?'
    # spacecmd softwarechannel_setsyncschedule '"trusty-updates" 0 20 1 8 * ?'
    # spacecmd softwarechannel_setsyncschedule '"trusty-updates-universe" 0 24 1 8 * ?'
    # spacecmd softwarechannel_setsyncschedule '"xenial" 0 30 1 8 * ?'
    # spacecmd softwarechannel_setsyncschedule '"xenial-backports" 0 40 1 8 * ?'
    # spacecmd softwarechannel_setsyncschedule '"xenial-security" 0 35 1 8 * ?'
    # spacecmd softwarechannel_setsyncschedule '"xenial-security-universe" 0 41 1 8 * ?'
    # spacecmd softwarechannel_setsyncschedule '"xenial-updates" 0 50 1 8 * ?'
    # spacecmd softwarechannel_setsyncschedule '"xenial-updates-universe" 0 55 1 8 * ?'

Finally you can trigger an initial synchronization of packages for each channel by executing the commands

    # spacecmd softwarechannel_syncrepos '"trusty"'
    # spacecmd softwarechannel_syncrepos '"trusty-backports"'
    # spacecmd softwarechannel_syncrepos '"trusty-security"'
    # spacecmd softwarechannel_syncrepos '"trusty-security-universe"'
    # spacecmd softwarechannel_syncrepos '"trusty-updates"'
    # spacecmd softwarechannel_syncrepos '"trusty-updates-universe"'
    # spacecmd softwarechannel_syncrepos '"xenial"'
    # spacecmd softwarechannel_syncrepos '"xenial-backports"'
    # spacecmd softwarechannel_syncrepos '"xenial-security"'
    # spacecmd softwarechannel_syncrepos '"xenial-security-universe"'
    # spacecmd softwarechannel_syncrepos '"xenial-updates"'
    # spacecmd softwarechannel_syncrepos '"xenial-updates-universe"'



