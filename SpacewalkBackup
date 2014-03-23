[[TOC]]

= How to backup Spacewalk =


== Directories ==

 * '''/var/satellite'''  contains all the pushed packages
 * '''/var/lib/cobbler'''  all the cobbler configuration
 * '''/etc'''
 * '''/root/ssl-build''' with the package containing your SSL certificate
 * '''/home/nocpulse'''
 * '''/var/lib/rhn/''' which contains all the kickstart files



== Oracle XE 10g backup ==

=== Setting up archivelog for online backup ===

Make sure nothing is connected to the Oracle database:

{{{
# spacewalk-service stop
}}}

{{{
bash-3.2# su oracle
bash-3.2$ . /usr/lib/oracle/xe/app/oracle/product/10.2.0/server/bin/oracle_env.sh
bash-3.2$ sqlplus /nolog
 connect / as sysdba
 shutdown immediate
 startup mount
 alter database archivelog;
 alter database open;
 SELECT LOG_MODE FROM SYS.V$DATABASE;
 quit

You should get:
 SQL> SELECT LOG_MODE FROM SYS.V$DATABASE;
 
 LOG_MODE
 ------------
 ARCHIVELOG
 --
}}}

Make sure Oracle XE restart works fine:
{{{
# service oracle-xe restart
}}}

=== Run (online) backup ===

The backup script can be found at {{{/usr/lib/oracle/xe/app/oracle/product/10.2.0/server/config/scripts/backup.sh}}}.
Please note that you have to run it under the '''oracle''' system user.

{{{
bash-3.2$ HOME=/tmp /usr/lib/oracle/xe/app/oracle/product/10.2.0/server/config/scripts/backup.sh
Doing online backup of the database.
Backup of the database succeeded.
Log file is at /tmp/oxe_backup_current.log.
}}}

Location of the backup files: /usr/lib/oracle/xe/app/oracle/flash_recovery_area/XE/

=== Restore backup ===

The restore script can be found at {{{/usr/lib/oracle/xe/app/oracle/product/10.2.0/server/config/scripts/restore.sh}}}.
Please note that you have to run it under the '''oracle''' system user.

{{{
-bash-3.2$ HOME=/tmp /usr/lib/oracle/xe/app/oracle/product/10.2.0/server/config/scripts/restore.sh 
This operation will shut down and restore the database. Are you sure [Y/N]?y
Restore in progress...
Restore of the database succeeded.
Log file is at /tmp/oxe_restore.log.
Press ENTER key to exit
}}}

Do one more Oracle XE server restart:

{{{
# service oracle-xe restart
}}}

== Postgres backup and restore ==

=== Creating the backup ===

Ensure that spacewalk is not using the postgres database:
{{{
# spacewalk-service stop
}}}

This procedure assumes that spacewalk is the only application using this postgres database.

Create a full postgres backup.
{{{
# su - postgres -c  'pg_dumpall > /var/lib/pgsql/backups/full_postgres_backup-`date +%Y%m%d`.sql'
}}}

Don't forget to restart once the sql dump has finished.
{{{
# spacewalk-service start
}}}


=== Restoring from the backup ===

This assumes that you'd be happy to drop all databases and install from the backup perform with a pg_dumpall command.

Stop spacewalk from accessing the postgres database.
{{{
# spacewalk-service stop
}}}

Switch to postgres user.
{{{
# su - postgres
}}}

Define some temp variables, adjust appropriately as per your setup.
{{{
# SPACEWALK_DB_NAME=spacewalkdb
# SPACEWALK_DB_BACKUPFILE=/var/lib/pgsql/backups/full_postgres_backup-20140323.sql
}}}

Use temp variables from above to drop the existing database and restore from the backup, then exit from postgres user's shell.
{{{
# dropdb $SPACEWALK_DB_NAME
# createdb $SPACEWALK_DB_NAME
# psql -e -d $SPACEWALK_DB_NAME -f $SPACEWALK_DB_BACKUPFILE
# exit
}}}

Don't forget to restart once you've restored the database.
{{{
# spacewalk-service start
}}}

== Links ==
 * [http://www.markcallen.com/oracle/oracle-xe-backup]
 * [http://www.mail-archive.com/spacewalk-list@redhat.com/msg02732.html]