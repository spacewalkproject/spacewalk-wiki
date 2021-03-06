# Spacewalk Upgrade Instructions



These are upgrade instruction for upgrading Spacewalk 2.0 to Spacewalk 2.1

These upgrade instruction apply to Spacewalk installations meeting the following criteria:

  *  Spacewalk 2.0 running on Red Hat Enterprise Linux 5 Server, 6 Server or CentOS/Scientific Linux or Fedora 19/20.
  *  Your Spacewalk uses either of Oracle 10g (including XE) / Oracle 11g / PostgreSQL 8.4+ as a database backend.
  *  In most cases it's possible to perform Package upgrade and Schema upgrade steps from any previous version to the latest one directly (e.g. from 1.6 to 2.1). Make sure you have a valid backup in case anything will go wrong.
## Archive of older upgrade instructions



 * Spacewalk 1.9 to 2.0 upgrade instructions, are available at [[HowToUpgrade20]]
 * Spacewalk 1.8 to 1.9 upgrade instructions, are available at [[HowToUpgrade19]]
 * Spacewalk 1.7 to 1.8 upgrade instructions, are available at [[HowToUpgrade18]]
 * Spacewalk 1.6 to 1.7 upgrade instructions, are available at [[HowToUpgrade17]]
 * Spacewalk 1.5 to 1.6 upgrade instructions, are available at [[HowToUpgrade16]]

----
## Assumptions



  * For RHEL or CentOS, you will need the base and EPEL repositories enabled for dependencies.
  * For Fedora, your Fedora yum repositories are setup properly.
  * You had set up your yum to point to Spacewalk 2.1 repository. For the repo setup specifics, see HowToInstall#SettingupSpacewalkrepo.
    * In particular, for Fedora, make sure your jpackage repo is setup properly: https://fedorahosted.org/spacewalk/wiki/HowToInstall#Jpackagerepository
## Database and configuration backup




  *  For existing configuration files, create a backup of everything under /etc/sysconfig/rhn /etc/rhn and /etc/jabberd
  *  Backup your SSL build directory, ordinarily /root/ssl-build
  *  For instructions on how to create a backup of your existing Spacewalk database consult either Oracle / PostgreSQL documentation or contact your DBA
## Package upgrade



Perform package upgrade using yum:

    # yum upgrade

During the upgrade, you may notice messages printed to the terminal when installing oracle-instantclient-selinux and spacewalk-selinux. These messages are produced by restorecon and do not pose any harm.

Check any `.rpmnew`/`.rpmsave` files that were created during the upgrade for your configuration files and make sure changes to your configuration files are preserved while new content from the distribution files is carried over.

    # yum install rpmconf
    # rpmconf -a
## Schema upgrade



Make sure your Spacewalk server is down:


    # /usr/sbin/spacewalk-service status

Do a backup of your database.

If you are running PostgreSQL database backend check whether you have created language pltclu:


    su - postgres -c 'PGPASSWORD=spacepw; createlang pltclu spaceschema ;'

If you are running Oracle database backend grant these rights to your database user:

Update spacewalk database user - replace <spacewalk> with your database user name 

{{{ 
# sqlplus 'sys/<password>@//localhost/XE as sysdba' 
SQL> grant create table to <spacewalk>; 
SQL> grant create trigger to <spacewalk>; 
SQL> quit 
}}} 

Make sure your database server is running. Run spacewalk-schema-upgrade script to upgrade database schema:


    # /usr/bin/spacewalk-schema-upgrade

Important notes:

  * The above command will inform you whether or not was the schema upgrade successful.
    * Log files from schema upgrade are being put into /var/log/spacewalk/schema-upgrade.
    * Should the schema upgrade fail, investigate, restore from backup, fix the cause (for example, if it failed because of insufficient space in tablespace, extend the tablespaces) and rerun spacewalk-schema-upgrade.
## Upgrade of Spacewalk configuration



  1. Use spacewalk-setup to upgrade Spacewalk configuration. Run as root:


    # spacewalk-setup --disconnected --external-$DB --upgrade

  where `DB` can  be either `oracle` or `postgresql`.

  2. Restore some of the custom values you might have set previously in /etc/rhn/rhn.conf from the backup of your configuration files, such as:

  *  debug = 3
  *  pam_auth_service = rhn-satellite

  3. Enable (or re-enable) monitoring (and monitoring scout).

  * If you wish to enable (or re-enable) monitoring without enabling monitoring scout, run the following command as root:


    # /usr/share/spacewalk/setup/upgrade/rhn-enable-monitoring.pl

  * If you wish to enable (or re-enable) both monitoring and monitoring scout, run the following command:


    # /usr/share/spacewalk/setup/upgrade/rhn-enable-monitoring.pl --enable-scout
## Restart Spacewalk




    # /usr/sbin/spacewalk-service start