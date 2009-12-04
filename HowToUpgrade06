= Spacewalk Upgrade Instructions =

These upgrade instructions apply to Spacewalk 0.5 running on Red Hat Enterprise Linux 5 Server or CentOS 5.

Spacewalk upgrade from version 0.5 to 0.6 involves several basic steps:
  1.  Configuration files and Spacewalk database backup
  2.  Preparation for jabberd ver. 2.2.8
  3.  Package upgrade
  4.  Schema upgrade
  5.  Upgrade of Spacewalk configuration
  6.  Restart Spacewalk
  7.  Update of monitoring setup
  8.  Update Spacewalk configuration files
  9.  Restart Spacewalk
  10.  Rebuild search indexes

----


= Assumptions =

  * For RHEL or CentOS, you will need the Base and EPEL repositories enabled for dependencies.
  * You had set up your yum repository configuration to point to spacewalk 0.6 repo. Ordinarily, your yum repo configuration might look like:


{{{
[spacewalk]
name=Spacewalk
baseurl=http://spacewalk.redhat.com/yum/0.6/RHEL/5/$basearch/os/
gpgkey=http://spacewalk.redhat.com/yum/RPM-GPG-KEY-spacewalk
enabled=1
gpgcheck=1
}}}


=  Database and configuration backup  =


  *  For existing configuration files, create a backup of everything under /etc/sysconfig/rhn /etc/rhn and /etc/jabberd
  *  Backup your ssl build directory, ordinarily /root/ssl-build
  *  For instructions on how to create a backup of your existing Spacewalk database consult either Oracle documentation or contact your DBA


= Preparation for jabberd ver. 2.2.8 =

Stop Spacewalk:

{{{
# /usr/sbin/rhn-satellite stop
}}}

At this point, all jabberd processes (c2s, s2s, sm, router) need to be stopped.

Clean jabberd database:

{{{
# rm -f /var/lib/jabberd/db/*
}}}

Upgrade jabberd:

{{{
# yum upgrade jabberd
}}}

At this point, jabberd ver. 2.2.8 should be installed on your system.

For jabberd, osa-dispatcher and osad clients to work correctly, SSL certificate used by jabberd services needs to have correct
user & group ownership (user & group == jabber).

Check user and group ownership of file containing SSL certificate and private key used by jabberd services for setting up encrypted channels:

{{{
# ls -l /etc/pki/spacewalk/jabberd/server.pem
}}}

This will either say "jabber" :

{{{
# ls -l /etc/pki/spacewalk/jabberd/server.pem 
-rw------- 1 jabber jabber 7244 Aug  6 16:53 /etc/pki/spacewalk/jabberd/server.pem
}}}

or it will say "jabberd" :

{{{
# ls -l /etc/pki/spacewalk/jabberd/server.pem 
-rw------- 1 jabberd jabberd 7244 Aug  7 10:20 /etc/pki/spacewalk/jabberd/server.pem
}}}

If the user and group ownership says "jabber", you can safely skip rest of this step and continue with a step named "Package upgrade" below.

If the user and group ownership says "jabberd", you need to do following steps:

As root run:

{{{
# getent passwd jabberd && userdel -r jabberd
}}}

As root, run upgrade-cert.sh script attached to this page:

{{{
# ./upgrade-cert.sh
}}}

This script will try to re-create new rpm package containing your original SSL certificates 
and instruct you to do these additional steps that you are required to do:

  *  Uninstall original package containing your certificate files

  *  Install updated package with your original certificate files

  *  Create a backup of rpm files that were created

  *  Remove temporary content created in previous steps.

One last time, make sure SSL certificate file used by jabberd services is now user & group owned by "jabber" :

{{{
# ls -l /etc/pki/spacewalk/jabberd/server.pem 
-rw------- 1 jabber jabber 7244 Aug  7 11:14 /etc/pki/spacewalk/jabberd/server.pem
}}}

= Package upgrade =

Before the actual package upgrade, run following command:

{{{
# semanage port -d -t cobbler_port_t -p tcp 25152
}}}

Perform package upgrade using yum:

{{{
# yum upgrade
}}}

During the upgrade, you may notice messages printed to the terminal when installing oracle-instantclient-selinux and spacewalk-selinux.
These messages are produced by restorecon and do not pose any harm.
On the contrary, these messages indicate relabeling of file system objects that is required for correct Spacewalk and SELinux symbiosis.

=  Schema upgrade  =

Make sure your Spacewalk server is down:

{{{
# /usr/sbin/rhn-satellite status
}}}

Make sure your oracle server is running (likely it isn't running since the earlier 'rhn-satellite stop' command stops the oracle-xe processes). For oracle-xe that would be:

{{{
# service oracle-xe status
}}}

If Oracle isn't running, start it

{{{
# service oracle-xe start
}}}
Run spacewalk-schema-upgrade script to upgrade database schema:

{{{
# /usr/bin/spacewalk-schema-upgrade
}}}


Log files from schema upgrade are being put into /var/log/spacewalk/schema-upgrade

=  Upgrade of Spacewalk configuration  =

For Spacewalk 0.6, it is required to deploy configuration for cobbler and update configuration of apache SSL virtual host. Both steps can be accomplished by running:

{{{
# spacewalk-setup --disconnected --upgrade
}}}

You'll be asked by spacewalk-setup to provide your database connection information.  This is the same information you provided to HowToInstall. Following this spacewalk-setup will ask you whether you wish to
have your apache ssl configuration (/etc/httpd/conf.d/ssl.conf) updated by spacewalk-setup. You may choose to setup your ssl.conf manually, in which
case you have to make sure you have the following directives present in your /etc/httpd/conf.d/ssl.conf:

{{{
# cat /etc/httpd/conf.d/ssl.conf
...
<VirtualHost _default_:443>
...
SSLCertificateFile /etc/pki/tls/certs/spacewalk.crt
SSLCertificateKeyFile /etc/pki/tls/private/spacewalk.key
RewriteEngine on
RewriteOptions inherit
SSLProxyEngine on
<IfModule mod_jk.c>
    JkMountCopy On
</IfModule>
...
</VirtualHost>
...
}}}

= Restart Spacewalk = 

{{{
# /usr/sbin/rhn-satellite start
}}}

= Update of monitoring setup =

Recent Spacewalk installations may have set several important monitoring configuration values incorrectly. To correct these, run the following
as root (note that you should run this command regardless of your current monitoring status; even on installations that never had monitoring
enabled, running this script will have no ill effect):

{{{
# /usr/share/spacewalk/setup/upgrade/rhn-update-monitoring.pl
}}}

You may now choose to activate monitoring or both monitoring & monitoring scout on your Spacewalk.

If you had monitoring enabled previously and wish to re-enable it now (without having to do so in the web ui), run as root:

{{{
# /usr/share/spacewalk/setup/upgrade/rhn-enable-monitoring.pl
}}}

If you'd like to re-enable both monitoring and monitoring scout on your Spacewalk (without having to do so in the web ui), instead run as root

{{{
# /usr/share/spacewalk/setup/upgrade/rhn-enable-monitoring.pl --enable-scout
}}}

= Update Spacewalk configuration files =

Restore some of the custom values you might have set previously in /etc/rhn/rhn.conf from the backup of your configuration files, such as:

  *  debug = 3
  *  pam_auth_service = rhn-satellite
  *  default_db = user/password@sid

=  Restart Spacewalk  =

If all of the above steps completed successfully, you can start your upgraded Spacewalk server. With Spacewalk 0.3, rhn-satellite init script
was decommissioned and all Spacewalk services are being started individually. If you need to restart your Spacewalk using one command,
use /usr/sbin/rhn-satellite script.

{{{
# /usr/sbin/rhn-satellite restart
}}}

= Rebuild search indexes =

After a successful upgrade, it is recommended to rebuild search indexes used by Spacewalk search engine (rhn-search service).
This can be accomplished by the following:

{{{
# /etc/init.d/rhn-search cleanindex
}}} 


----
What's below are additional steps that were required for Spacewalk 0.4 to Spacewalk 0.5 upgrades. These steps are no longer required for
Spacewalk 0.5 to Spacewalk 0.6 and can be safely ignored.

= Update of Jabber SSL certificate location =

With jabberd-2.2.5 package present in EPEL, it is necessary to update location of SSL certificate used by jabberd service: /etc/jabberd/server.pem
This change is required to avoid file conflict that would otherwise occur during package upgrade: the PEM file in question is owned by both
new jabberd from EPEL and another package from your earlier Spacewalk installation created at setup time 
(by spacewalk-setup / rhn-ssl-tool).

To update the location, follow these steps: 

Setup your yum configuration to point to Spacewalk 0.5 repo:

{{{
[spacewalk]
name=Spacewalk
baseurl=http://spacewalk.redhat.com/yum/0.5/RHEL/5/$basearch/os
gpgkey=http://spacewalk.redhat.com/yum/RPM-GPG-KEY-spacewalk
enabled=1
gpgcheck=1
}}}

Stop your Spacewalk server. To accomplish this, execute following command as root:

{{{
# /usr/sbin/rhn-satellite stop
}}}

Update spacewalk-certs-tools package:

{{{
# yum upgrade spacewalk-certs-tools
}}}


As root, run upgrade-cert.sh script attached to this page:
{{{
# ./upgrade-certs.sh
}}}

This script will try to re-create new rpm package containing your original SSL certificates with server.pem file put
into a new location and instruct you to do these additional steps that you are required to do:

  *  Uninstall original package containing your certificate files

  *  Install updated package with your original certificate files

  *  Create a backup of rpm files that were created

  *  Remove temporary content created in previous steps.


----
What's bellow are additional steps that were required for Spacewalk 0.2 to Spacewalk 0.3 upgrades. These steps are no longer required for
Spacewalk 0.4 to Spacewalk 0.5 upgrades and can be safely ignored.

=  Activate Spacewalk services  =

With release of 0.3, spacewalk services (rhn-search, satellite-httpd, taskomatic, ...) are no longer started from /etc/init.d/rhn-satellite,
every service has its own init.d script with chkconfig entries.

When doing a new Spacewalk 0.3 installation, all the services are being activated by spacewalk-setup. When upgrading existing Spacewalk 0.2
installation, you have to activate all these services manually:

{{{
# for service in jabberd osa-dispatcher taskomatic tomcat5 satellite-httpd Monitoring MonitoringScout rhn-search; do
    if [ -e "/etc/init.d/$service" ]; then
        /sbin/chkconfig --level 345 $service on
    fi
done
}}}


---

What's bellow are additional steps that were required for Spacewalk 0.1 to Spacewalk 0.2 upgrades. These steps are no longer required for
Spacewalk 0.2 to Spacewalk 0.3 upgrades and can be safely ignored.

=  Package paths upgrade =

Spacewalk ver. 0.2 introduced new layout of /var/satellite directory (change coming with multiple distribution support). To migrate
existing packages into new locations, use update-packages script (spacewalk-backend-satellite-tools) with a valid database connection string.

{{{
# /usr/bin/update-packages --db=spacewalk/spacewalk@xe
}}}


=  Update configuration  =


If you're upgrading Spacewalk 0.1 to version 0.2, add the following line into /etc/rhn/rhn.conf and /etc/rhn/default/rhn_web.conf:


{{{
web.product_name = Spacewalk
}}}