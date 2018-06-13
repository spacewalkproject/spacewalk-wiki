# How to create a test rpm with scratch build

If you need to build a test rpm to check your changes (commited into git) then scratch build is what you want.

First of all you need an srpm of with your changes

        cd <package dir>
        tito build --test --srpm

This will create package.src.rpm with the latest commited changes for you.
Then there are three options where to run the scratch build:

## COPR

If you have a Fedora account (aka FAS) log into https://copr.fedorainfracloud.org/ and create new
project (click 'New Project'). Decide for which operating system(s) you will do scratch builds (e.g.
Fedora 25 / x86_64) and then fill in

        Project Name:  spacewalk-scratch
        Description:   My Spacewalk scratch builds
        Chroots:       [x] fedora-25-x86_64
        External Repositories: copr://@spacewalkproject/nightly
        Other Options: [x] Project will not be listed on home page

and click 'Create'. Then you can create a new build, go to 

* 'Builds' tab > 'New Build' > 'Upload SRPM' > 'Browse'

select your package.src.rpm, click 'Build'.
Binary packages will be found via

        https://copr.fedorainfracloud.org/coprs/<username>/spacewalk-scratch/builds/

### Copr-cli

Once you create spacewalk-scratch project you can also use copr-cli to submit build and download
build results (setup cli - https://developer.fedoraproject.org/deployment/copr/copr-cli.html)

        copr-cli build spacewalk-scratch package.src.rpm

Download build results (including logs)

        copr-cli download-build <build id>


### Mock

Third (and mostly the fastest) way to run a scratch build is locally on your workstation using mock.

Install mock; on Fedora

        dnf install mock

or on RHEL / CentOS 7 (it's in EPEL repo)

        rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
        yum install mock

Configure build root (e.g. Fedora 25 / x86_64)

        mkdir ~/.mock
        cp /etc/mock/fedora-25-x86_64.cfg ~/.mock/spacewalk-fedora-25-x86_64.cfg
        # rename buildroot to spacewalk-fedora-25-x86_64
        sed -i "/config_opts\['root'\]/ s/= '/= 'spacewalk-/" ~/.mock/spacewalk-fedora-25-x86_64.cfg
        # add spacewalk nightly repo
        sed -i '$ s,""",\n[group_spacewalkproject-nightly]\nname=Copr repo for nightly owned by @spacewalkproject\nbaseurl=https://copr-be.cloud.fedoraproject.org/results/@spacewalkproject/nightly/fedora-$releasever-$basearch/\ngpgcheck=0\nenabled=1\n""",' ~/.mock/spacewalk-fedora-25-x86_64.cfg

and now you can run local scratch build (they run in chroot /var/lib/mock/spacewalk-fedora-25-x86_64
and do not mess your workstation OS)

        mock -r ~/.mock/spacewalk-fedora-25-x86_64.cfg --rebuild package.src.rpm

Build results (including logs) will be in

        /var/lib/mock/spacewalk-fedora-25-x86_64/result/


> Note: If you want to scratch build RHEL 6 or RHEL 7 packages in COPR you have to add java to 'External repositories' (in addition to spacewalk nightly):  
> RHEL 7:  
>   https://copr-be.cloud.fedoraproject.org/results/@spacewalkproject/java-packages/epel-7-x86_64/  
> RHEL 6:  
>   https://copr-be.cloud.fedoraproject.org/results/@spacewalkproject/java-packages/epel-7-x86_64/  
>   https://copr-be.cloud.fedoraproject.org/results/@spacewalkproject/epel6-addons/epel-6-x86_64/    
> For RHEL 6 or RHEL 7 scratch builds in `mock` you have to add the above java repos to your mock config file.
