[[TOC]]

= Setup of the PostgreSQL database =

== Manual PostgreSQL setup ==

You should have PostgreSQL server running somewhere. Let's assume you will run the server on the same machine as Spacewalk itself:

{{{
yum install -y 'postgresql-server > 8.4'
# we do this to get postgresql84-server on RHEL 5

# to use pltcl on RHEL5 install:
yum install -y postgresql84-pltcl
# to use pltcl on RHEL6 and Fedoras:
yum install -y postgresql-pltcl

chkconfig postgresql on
# on Fedoras, run:
postgresql-setup initdb
# everywhere else run:
service postgresql initdb

service postgresql start
}}}

Create database, user, and plpgsql language there:

{{{
su - postgres -c 'PGPASSWORD=spacepw; createdb -E UTF8 spaceschema ; createlang plpgsql spaceschema ; createlang pltclu spaceschema ; yes $PGPASSWORD | createuser -P -sDR spaceuser'
}}}

Configure the user to use md5 password to connect to that database. Put the lines like following to
{{{/var/lib/pgsql/data/pg_hba.conf}}}. Avoid the common pitfall: '''Make sure you put them *before* those existing lines that are for {{{all}}}.'''.

{{{
local spaceschema spaceuser md5
host  spaceschema spaceuser 127.0.0.1/8 md5
host  spaceschema spaceuser ::1/128 md5
local spaceschema postgres  ident
}}}

Then reload PostgreSQL:

{{{
service postgresql reload
}}}

and test the connection:

{{{
PGPASSWORD=spacepw psql -a -U spaceuser spaceschema
PGPASSWORD=spacepw psql -h localhost -a -U spaceuser spaceschema
}}}

Please note that you will want the TCP connection allowed as well because the JDBC driver cannot use the Unix domain socket. So even if the Python and Perl stack will use the Unix domain socket if you do not specify the hostname, JDBC will still go to localhost via TCP.

If you want to have the database on a separate box you will likely need to change this line in pg_hba.conf from 127.0.0.1/8 to something else:

{{{
host  spaceschema spaceuser 127.0.0.1/8 md5
}}}

and also to add line to /var/lib/pgsql/data/postgresql.conf 

{{{
listen_addresses = '*'
}}}

=== The contrib package ===

If your PostgreSQL server is installed on different machine than where you setup your Spacewalk server, please make sure the {{{postgresql-contrib >= 8.4}}} (or postgresql84-contrib on RHEL 5) is installed on the PostgreSQL server.

== Setup with spacewalk-setup-postgresql ==

You may setup your PostgreSQL server for use with Spacewalk with '''spacewalk-setup-postgresql''' utility.

=== PostgreSQL database and Spacewalk on one machine === #pg-single-machine

Assuming you have your yum repo configuration for Spacewalk in place, these are the instructions needed to configure PostgreSQL server to be used on the same machine as your Spacewalk.

1. Install '''spacewalk-setup-postgresql''' package.

{{{
# yum install spacewalk-setup-postgresql
}}}

2. Run spacewalk-setup-postgresql to configure the PostgreSQL server. You will need the following data to configure the database server:
  * database name
  * database username
  * database user password

{{{
# DBNAME=yourdbname
# DBUSER=yourdbuser
# DBPASSWORD=youruserpassword
# spacewalk-setup-postgresql create \
                             --db $DBNAME \
                             --user $DBUSER \
                             --password $DBPASSWORD
}}}

=== PostgreSQL database and Spacewalk on different machines === #pg-standalone

Assuming you have your yum repo configuration for Spacewalk in place, these are the instructions needed to configure your PostgreSQL database for a multi-server installation (database and Spacewalk on different machines).

1. Install '''spacewalk-setup-postgresql''' package.

{{{
# yum install spacewalk-setup-postgresql
}}}

2. Run spacewalk-setup-postgresql to configure the PostgreSQL server. You will need the following data to configure the database server:
  * database name
  * database username
  * database user password
  * list of local addresses that the DB server will listen on (can be configured to listen on all addresses)
  * list of remote addresses to allow the connection from (presumably the IP address of your Spacewalk server)

{{{
# DBNAME=yourdbname
# DBUSER=yourdbuser
# DBPASSWORD=youruserpassword
# spacewalk-setup-postgresql create \
                             --db $DBNAME \
                             --user $DBUSER \
                             --password $DBPASSWORD \
                             --standalone
}}}

3. Make sure the firewall is configured correctly for a remote DB access. Normally, this would involve opening the TCP connection to port 5432.


= Tune up PostgreSQL =

Tune up PostgreSQL's performance by running pgtune:

{{{
yum install pgtune
pgtune --type=web -c 600 -i /var/lib/pgsql/data/postgresql.conf >/tmp/pgtune.conf
# Review the changes by
diff -u /var/lib/pgsql/data/postgresql.conf /tmp/pgtune.conf
mv /var/lib/pgsql/data/postgresql.conf{,.bak}
cp /tmp/pgtune.conf /var/lib/pgsql/data/postgresql.conf
service postgresql restart
}}}

or at least increase maximal number of connections to 600:
{{{
echo max_connections = 600 >>/var/lib/pgsql/data/postgresql.conf
}}}