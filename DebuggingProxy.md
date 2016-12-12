== Debugging Proxy ==

A few pointers for issues you might run into with Proxy:

Useful log files:
 * /var/log/httpd/* - Apache logs
 * /var/log/squid/* - Squid logs
 * /var/log/rhn/rhn_proxy_broker.log - Proxy Broker Server
 * /var/log/rhn/rhn_proxy_redirect.log - SSL Redirect Server

Config files:
 * /etc/httpd/conf.d/*.conf
 * /etc/rhn/rhn.conf
 * /etc/squid/squid.conf
 * /etc/sysconfig/rhn/up2date

You can raise the debug level in /etc/rhn/rhn.conf to increase logging to the Proxy Broker and SSL Redirect logs - set to a value between 0 (default) and 9 (loads of debugging info).

=== Common Errors ===

When starting up Squid: "Aborted $SQUID $SQUID_OPTS >>/var/log/squid/squid.out     FATAL: Could not determine fully qualified hostname"[[BR]]
Set visible_hostname in /etc/squid/squid.conf - see [http://kbase.redhat.com/faq/FAQ_48_9716.shtm]

----

If after running configure-proxy.sh your clients receive a "Spacewalk Proxy service not enabled for server profile: 'proxy_fqdn' Error Class Code: 1002", try rerunning rhn-proxy-activate as below, then verify that the systemid of your new proxy server as specified in /etc/sysconfig/rhn/systemid matches the row in rhnproxyinfo in your database:

{{{
rhn-proxy-activate --server=<fqdn-of-your-master-here>
}}}

{{{
[you@asatproxyserver ~]# grep ID- /etc/sysconfig/rhn/systemid 
<value><string>ID-1000036838</string></value>
[you@asatmasterserver ~]# spacewalk-sql -i
psql (8.4.20)
SSL connection (cipher: DHE-RSA-AES256-SHA, bits: 256)
Type "help" for help.

rhnschema=# select * from rhnproxyinfo;
 server_id  | proxy_evr_id 
------------+--------------
 1000036838 |        17467
(1 row)
}}}