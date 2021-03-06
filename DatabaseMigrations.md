# Migrating Spacewalk database backend between Oracle and PostgreSQL



If you have an existing Spacewalk 1.3+ installation with Oracle backend, you can migrate it to use the PostgreSQL database server instead.

If you have an existing Spacewalk 2.3+ installation with Oracle or PostgreSQL backend, you can migrate it to use the another Oracle or PostgreSQL database server instead.
## Prepare existing Spacewalk server for migration



Make sure your Spacewalk server is down:


    spacewalk-service stop

Make sure your database server you are migrating from is running. If your old database server is *Embedded PostgreSQL*, then run:


    db-control start

 * If you are using other than embedded database and you don't have access to database machine, ask your Database Administrator.

Upgrade your existing schema.

 * When migrating the data between Oracle and PostgreSQL database schemas, you will use a dump of the data from the old database server and import it to a fresh database schema. Therefore, you need the new schema to be of the same version as the old one. To insure this, you will need to upgrade the Spacewalk and the old schema to the latest version, to match the schema that you will have in newly installed database server.

 * You accomplish this by executing the following as root:


    spacewalk-schema-upgrade

 * Log files from schema upgrade will be put into */var/log/spacewalk/schema-upgrade*.

Make sure _spacewalk-utils_ package is installed:


    yum install spacewalk-utils
## Create database dump



You will dump data from your old database with _spacewalk-dump-schema_ script. Data are stored in text file(s) but format depends on desired target database.

 * Make sure you have enough disk space for the dump.
### Migrating to Oracle



 * File for each table and binary data type is created.

Run following statement as root:


    spacewalk-dump-schema --to=oracle > migrate-to-oracle.sql

 * This will connect to database specified in */etc/rhn/rhn.conf*, dumps content of tables into */tmp/dumped-tables* and create migration script in actual directory.

 * If you want to dump table content into different directory, you can set it with parameter *--dump-dir*.
### Migrating to PostgreSQL



 * Only one file is created.

Run following statement as root:


    spacewalk-dump-schema --to=postgresql > migrate-to-postgresql.sql

If your original oracle database was in AL32UTF8 encoding, re-encode it:

    mv migrate-to-postgresql.sql migrate-to-postgresql.sql.al32utf8
    iconv -c -t utf-8 migrate-to-postgresql.sql.al32utf8 > migrate-to-postgresql.sql
    
 * This will connect to database specified in */etc/rhn/rhn.conf*, dumps all content into file *migrate-to-postgresql.sql*.


### Additional parameters



If you want dump other database than one specified in */etc/rhn/rhn.conf*, you can override it with parameters.

 * Connection to PostgreSQL database you can override with parameters:


    --from=postgresql --db=DBNAME --host=HOSTNAME --port=PORT --user=USERNAME --password=PASSWORD

 * Connection to Oracle database you can override with parameters:


    --db=SID --user=USERNAME --password=PASSWORD
## Prepare the new database



At this point, you can stop the old database server.

You need to replace some database specific packages if you are migrating between Oracle and PostgreSQL.

 * If you are migrating from PostgreSQL to Oracle, run:
  * Note: Package *spacewalk-setup-postgresql* is present only on Embedded PostgreSQL installations.


    yum remove -y spacewalk-postgresql spacewalk-setup-postgresql && yum install -y spacewalk-oracle && yum remove -y spacewalk-java-postgresql spacewalk-backend-sql-postgresql

 * If you are migrating from Oracle to PostgreSQL, run:


    yum remove -y spacewalk-oracle && yum install -y spacewalk-postgresql && yum remove -y spacewalk-java-oracle spacewalk-backend-sql-oracle

Install and start the new database server. For Embedded PostgreSQL you can run:


    yum install spacewalk-setup-postgresql
    spacewalk-setup --db-only

 * For installation of other than Embedded PostgreSQL database contact your Database Administrator. Then just populate the new database with Spacewalk schema.

 * To populate External Oracle run:


    spacewalk-setup --db-only --external-oracle

 * To populate External PostgreSQL run:


    spacewalk-setup --db-only --external-postgresql

 * Note that database population will not work if External Oracle is running on unstandard port and you have enabled _SELinux_ on Spacewalk server.
## Load dump into new database



 * Make sure you have enough disk space for database import.

 * If you are migrating to *External Oracle*, make sure your database has _TEMP_, _UNDO_ and _DATA_ tablespaces big enough.

 * If you are migrating to *External Oracle* on different machine, you need to copy dumped tables there, e.g.:


    scp -r /tmp/dumped-tables oracle-machine-hostname:/tmp

Load into established database:


    spacewalk-sql -i < migrate-to-?.sql

 * Note that you may need to change _SELinux_ context of the migration script to be able to load it into Oracle:


    semanage fcontext -a -t oracle_sqlplus_exec_t /root/migrate-to-oracle.sql
    restorecon -v /root/migrate-to-oracle.sql

 * Similarly, you may need to change _SELinux_ context of dumped tables:


    semanage fcontext -a -t oracle_tmp_t "/tmp/dumped-tables(/.*)?"
    restorecon -R -v /tmp/dumped-tables/

Now you can run Spacewalk with new database:


    spacewalk-service start
