= Oracle 11g Express Edition Setup =

== Limitations ==

 * will use maximum of 1 GB RAM
 * 11 GB user data
 * may be installed on a multiple CPU server, but may only be executed on one processor in any server
 * one database
 * x86_64 only

The full license agreement is available via the download page linked below.

== Download ==

=== Server ===

You can download Oracle 11g Express Edition (XE) Server from Oracle's [http://www.oracle.com/technetwork/products/express-edition/downloads/index.html Oracle Database Express Edition 11g Release 2 Download page]. You will need an Oracle OTN account to download the rpms and you will need to accept the license agreements. Download the "Oracle Database Express Edition 11g Release 2 for Linux x64"

 * oracle-xe-11.2.0-1.0.x86_64.rpm

=== Client ===

You will also need the Oracle Instant Client rpm from [http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html Instant Client Downloads for Linux x86-64 page]. Download the following two rpms:

 * oracle-instantclient11.2-basic-11.2.0.4.0-1.x86_64.rpm
 * oracle-instantclient11.2-sqlplus-11.2.0.4.0-1.x86_64.rpm

== Before installing Oracle XE ==

You must run the following two commands before starting the installation to create the oracle user uid below 500 (below 1000 on Fedoras), to be able to run Oracle XE with SELinux enforcing:

{{{
# /usr/sbin/groupadd -r dba
# /usr/sbin/useradd -r -M -g dba -d /u01/app/oracle -s /bin/bash oracle
}}}

Check if you have swap > 1GB because Oracle XE RPM will need it. You can create temporary one (or grow existing) with

{{{
# dd if=/dev/zero of=/var/swapfile bs=1M count=1100
# mkswap /var/swapfile
# swapon /var/swapfile
}}}

Ensure your hostname is resolvable either with by DNS or an entry in /etc/hosts. Otherwise the configuration of the Oracle Net Listener will fail.

== Install ==

=== Make sure bc is installed ===

The '''bc''' is a dependency of oracle-xe which is not resolved automatically. Therefore, run:

{{{
# yum -y install bc net-tools
}}}

=== Oracle XE itself ===

{{{
# yum -y localinstall --nogpgcheck oracle-xe-11.2.0-1.0.x86_64.rpm
}}}

The '''--nogpgcheck''' is required because the Oracle RPMs are not signed.

=== Oracle Instant Client ===

{{{
# yum -y localinstall --nogpgcheck oracle-instantclient11.2-basic*.rpm oracle-instantclient11.2-sqlplus*.rpm
}}}

  * '''WARNING:''' Don't run {{{oracle_env.sh}}} or {{{oracle_env.csh}}} in your root environment settings. Instant Client stuff works fine without it. It's useful only for command line XE server management under {{{oracle}}} user. 

=== Oracle compatibility library ===

To install oracle-lib-compat, you'll need to have the Spacewalk repo configured, see HowToInstall for installation directions. Run:

{{{
# yum -y install oracle-lib-compat
}}}

This will likely pull in {{{compat-libstdc++-33}}}.

== Install SELinux Policy ==

If you are running with SELinux enabled then install the following packages from the Spacewalk repo before running {{{oracle-xe configure}}}:

{{{
# yum -y install oracle-xe-selinux oracle-instantclient-selinux oracle-instantclient-sqlplus-selinux
}}}

This will likely pull in {{{oracle-nofcontext-selinux}}} as well.

== Server Setup ==

Configure the Oracle XE database by running

{{{
# cd / && /etc/init.d/oracle-xe configure
}}}

Here are some sane values for the configure:

{{{
 HTTP port for Oracle Application Express: 9055
 Database listener port: 1521
 Password for SYS/SYSTEM: <password>
 Start at boot: y
}}}

Note that the port 8080 is used by tomcat so you cannot accept the default for the Oracle Application Express. Also, the SELinux policy module assumes that the Application Express port is 9055, if you want different value, you need to define it for {{{oracle_port_t}}} via {{{semanage}}}.

Note that Oracle does not like certain characters in your password, but will not tell you until you can't login as sys later on.

=== Rerunning the configure ===

If you find out you are not happy with the configuration, remove the oracle-xe rpm and start anew.

== Test your connection with sqlplus ==

Replace <password> with the password provided on /etc/init.d/oracle-xe configure

Run
{{{
sqlplus 'sys/<password>@//localhost/XE as sysdba'
}}} 
 
You should see something like
 
{{{
SQL*Plus: Release 11.2.0.3.0 Production on Tue Oct 9 14:31:40 2012

Copyright (c) 1982, 2011, Oracle.  All rights reserved.


Connected to:
Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production

SQL> 
}}} 

== Create the spacewalk database user ==

Replace <password> with the password provided on /etc/init.d/oracle-xe configure

{{{
# sqlplus 'sys/<password>@//localhost/XE as sysdba'
SQL> create user spacewalk identified by spacewalk default tablespace users;
SQL> grant dba to spacewalk;
SQL> grant create table to spacewalk;
SQL> grant create trigger to spacewalk;
SQL> quit
}}}

Test that

{{{
# sqlplus spacewalk/spacewalk@//localhost/XE
}}}

works.

== Additional Oracle configuration ==

Oracle XE is initially configured to grant 40 connections to its database.  Spacewalk needs more than this default configuration.  We recommend increasing this setting to 400. 

Oracle XE has a bug that causes an Internal Server Error (500) in Spacewalk when viewing Systems -> Pick a System -> Software -> Packages -> Install New Package
(URL: https://spacewalk.example.com/rhn/systems/details/packages/InstallPackages.do?sid=**********) or when running
{{{rhnreg_ks}}} to register a client.

In order to avoid these two problems we need to increase the max processes in Oracle and to 
 
{{{ 
# sqlplus spacewalk/spacewalk@//localhost/XE
SQL> alter system set processes = 400 scope=spfile; 
SQL> quit  
# /sbin/service oracle-xe restart 
}}}

You might want to install 'rlwrap' to make your sqlplus more usable. {{{rlwrap}}} will retain 
a [http://en.wikipedia.org/wiki/Curses_(programming_library) 'curses'] like behavior with sqlplus such as maintaining sql call history, etc.

{{{
yum install rlwrap
}}}
 
You can then do  

{{{ 
rlwrap sqlplus spacewalk/spacewalk@//localhost/XE
}}}  
 
== Troubleshooting ==

 * If you get errors such as: 

{{{
   ORA-01654: unable to extend index SPACEWALK.RHN_PACKAGE_FILE_CID_PID_IDX by 5 in tablespace SPACEWALK
}}}

 or anything from Oracle complaing that it can't '''extend''' an index, a table, etc.. you should run:

{{{
  SQL> ALTER TABLESPACE system ADD DATAFILE '/u01/app/oracle/oradata/XE/system_02.dbf' size 200m;
  SQL> ALTER TABLESPACE users ADD DATAFILE '/u01/app/oracle/oradata/XE/users_02.dbf' size 200m;
  SQL> ALTER TABLESPACE undo ADD DATAFILE '/u01/app/oracle/oradata/XE/undo_02.dbf' size 200m;
}}}

 Quit sqlplus and restart spacewalk and oracle-xe (may not require a restart)

 * If you run out of space in Oracle XE you can try and reclaim some space by shrinking your storage:

 Administration > Storage > Compact Storage in XE webUI (http://your.satellite.tld:9055/apex/) .

== Misc links ==

Continue installing Spacewalk: [HowToInstall#InstallingSpacewalk HowToInstall > InstallingSpacewalk]
