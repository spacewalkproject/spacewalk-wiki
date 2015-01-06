= How to setup Spacewalk with PostgreSQL database over SSL

There are two possible ways to get Spacewalk + PostgreSQL database
to communicate over SSL:

  * New Spacewalk + PostgreSQL database installation with SSL
    (Follow [#NewSpacewalk New Spacewalk] instructions).
  * Reconfigure existing Spacewalk to start using SSL
    (Follow [#ExistingSpacewalk Existing Spacewalk] instructions).

== New Spacewalk

  * Install PostgreSQL database.
  * Follow steps in [#EnablingSSLonPostgreSQLdatabase Enabling SSL on PostgreSQL database].
  * Follow [#Installation Installation].
  * When using command line utilities follow [#Command-lineutilities Command-line utilities].

== Existing Spacewalk

  * Stop Spacewalk server.
  * Follow steps in [#EnablingSSLonPostgreSQLdatabase Enabling SSL on PostgreSQL database].
  * Follow steps in [#EnablingSSLonSpacewalk Enabling SSL on Spacewalk].
  * When using command line utilities follow [#Command-lineutilities Command-line utilities].
  * Start Spacewalk server.

== Enabling SSL on PostgreSQL database

In `/var/lib/pgsql/data`:

  * Create `server.crt` -- signed certificate.
  * Create `server.key` -- private key for certificate (`chmod 0400`).
  * Edit `postgresql.conf` -- add `ssl=on` directive.
    * Related directives are `ssl_ciphers`, `ssl_renegotiation_limit`.
      Consult your security people for tweaking those.
  * Edit `pg_hba.conf` -- Change lines allowing remote hosts (Spacewalk)
    to start with `hostssl` instead of `host`. Example of change:
        {{{
        --- old/pg_hba.conf
        +++ new/pg_hba.conf
        @@...@@
         local rhnsat rhnsat md5
         host  rhnsat rhnsat 127.0.0.1/8 md5
         host  rhnsat rhnsat ::1/128 md5
         local rhnsat postgres ident map=usermap
         local postgres postgres ident map=usermap
         
        -host   rhnsat rhnsat 192.0.2.5/32 md5
        +hostssl   rhnsat rhnsat 192.0.2.5/32 md5
        }}}
  * Make sure keys are owned by postgres and have correct permissions.

        {{{
        # chown postgres:postgres /var/lib/pgsql/data/server.{key,crt}
        # chmod 0400 /var/lib/pgsql/data/server.key
        }}}

Restart database:

    {{{
    # service postgresql restart
    }}}

=== Debugging

Some usefull information that can help you in case of issues
is in directory `/var/lib/pgsql/data/pg_log`.

=== Example self-signed certificate

    {{{
    # openssl req -new -text -out server.req
    # openssl rsa -in privkey.pem -out server.key
    # rm privkey.pem
    # openssl req -x509 -in server.req -text -key server.key -out server.crt
    # cp server.{key,crt} /var/lib/pgsql/data
    # chown postgres: /var/lib/pgsql/data/server.{key,crt}
    # chmod og-rwx /var/lib/pgsql/data/server.key
    }}}

== Installation

    Follow usual installation guide, but use `--external-postgresql-over-ssl`
    option in addition to `--external-postgresql`. In addition to usual stuff
    you will be prompted for location of file containing certificate of
    trusted certification authority. Installator will make sure certificate
    will get into `~/.postgresql/root.crt`, `/etc/rhn/postgresql-db-root-ca.cert`,
    and `/etc/rhn/javatruststore.jks`. It will also change permissions, do restorecon,
    and set `db_ssl_enabled = 1` in `rhn.conf`.

    {{{
    spacewalk-setup --disconnected --external-postgresql --external-postgresql-over-ssl
    }}}

== Command-line utilities

Tools that use rhn.conf should be all ok after following steps in
[#EnablingSSLonSpacewalk Enabling SSL on Spacewalk]. However, some
(e.g. installation) do not. For those you can use procedure described
here.

Command-line utilities that connect directly to database use `libpq`
(psql, python based, Perl based). To employ full potential of SSL
(perform verification of server identity on top of encryption), it is
needed (and strongly recommended) to tell `libpq` so. Without following
modification library will detect that server is willing to connect
only via SSL and act appropriately but without verifying certificates,
thus making connection vulnerable.

You need certificate of trusted certification authority (in case of
self signed certificate it would be `/var/lib/pgsql/data/server.crt`
from database server) in `~/.postgresql/root.crt` (path can be changed
with `PGSSLROOTCERT` environment variable). Then you can run installer
with `PGSSLMODE` environment variable set to `verify-full`. Example of
calling `spacewalk-sql` would be

    {{{
    # PGSSLMODE=verify-full spacewalk-sql -i
    }}}

or you can export `PGSSLMODE` variable

    {{{
    # export PGSSLMODE=verify-full
    # spacewalk-sql -i
    }}}

== Enabling SSL on Spacewalk

  * First, you need spacewalk nightly.
  * You need certificate of trusted certification authority in
    `/etc/rhn/postgresql-db-root-ca.cert`. (Path can be changed by
    option `db_sslrootcert` in `rhn.conf`.)
  * Set `db_ssl_enabled` to property to `1` in `/etc/rhn/rhn.conf`:

        {{{
        db_ssl_enabled = 1
        }}}

  * Follow steps in [#Java Java] subsection.
  * Restore selinux context for new files.

        {{{
        # restorecon -R -F -v /etc/rhn/
        }}}

Most database-aware components in Spacewalk should work with this.

=== Java

Java-based components (Tomcat, Taskomatic, and Rhn-search) require
certificates to be stored in !TrustStore. Its default location
`/etc/rhn/javatruststore.jks` can be overwritten by `java.ssl_truststore`
option in `rhn.conf`.

!TrustStore is manipulated by `keytool`. It expects key to be in DER
format for import. !TrustSore needs password for creation/manipulation,
not for access though. Follows example of importing authority certificate.

    {{{
    # openssl x509 -in /etc/rhn/postgresql-db-root-ca.cert -out server.der -outform der
    # keytool -keystore /etc/rhn/javatruststore.jks -alias postgresql -import -file server.der
    # rm server.der
    }}}

For SSL-related troubleshooting one can temporarily add
`-Djavax.net.debug=ssl` to `JAVA_OPTS`, for example in
`/etc/sysconfig/tomcat6`.

==== Change password to keystore

    {{{
    # keytool -storepasswd -keystore /path/to/keystore.file
    }}}