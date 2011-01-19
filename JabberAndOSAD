= Jabber and OSAD client connection issues =

== Connection Errors ==
{{{
1.  Verify jabberd and osa-dispatcher are running:
   a)  service jabberd status
   b) service osa-dispatcher status

2.  verify in /etc/rhn/rhn.conf 
 * server.jabber_server = FQDN
 * osa-dispatcher.jabber_server = FQDN
 * osa-dispatcher.jabber_username = rhn-dispatcher-sat
 * osa-dispatcher.jabber_password = rhn-dispatcher-RANDOM_STRING
 * osa-dispatcher.osa_ssl_cert = /var/www/html/pub/RHN-ORG-TRUSTED-SSL-CERT

Where FQDN is your fully qualified domain name and NOT your shortname (i.e.  spacewalk.domain.com)

3.   verify in /etc/jabberd:
 
  within c2s.xml you should have:

  <local>
      <id>FQDN</id>

and within sm.xml you should have:
   <sm>
      <id>FQDN</id>

where FQDN is the same as step 2.

4.  Within /etc/jabberd/server.pem, verify that the same FQDN is present here.
  
}}}

== Lost Connections ==

- Client connections aren't reconnecting if the connection is lost.

- A client's 'netstat -an |grep 5222' command saying "ESTABLISHED" even though there is no connection on the spacewalk server.

   -  Usually happens if there are firewalls with session timeout's


=== Symptom Description ===
We were seeing a 104 connections connected with local port 5222 (jabber) to our server, whenever we reboot it the connections dropped back to 3 (local connections). It seems that the client connections aren't reconnecting if the connection is lost.
Setting debug to 10 on the client showed nothing in the logs when it stops responding (nothing in "osa-dispatcher" log on server either). We noticed that on the client side that netstat shows an "ESTABLISHED" connection to the server whilst on the actual server it does not show any corresponding connection from the client.
We then found that our firewalls have session time-outs, thus the jabber connections were timing out. This also causes the strange behaviour of a clients 'netstat -an |grep 5222' command saying "ESTABLISHED" even though there is no connection on the spacewalk server.


=== Fix ===
After doing the following, we saw no further issues with time-outs.


The below jabber settings are all explained in the .xml configs under /etc/jabberd/.

Essentially, it will send a "whitespace" character as a keepalive. (Every 2 minutes with the below settings.)

This may be verified with tcpdump.

Of course depending on your firewall time-out settings, you may need to tweak this a bit.



==== On the spacewalk server ====


1. Set the timeout intervals for jabber daemon

{{{
sed -i 's/<interval>.*/<interval>120<\/interval>/' /etc/jabberd/*.xml*
sed -i 's/<keepalive>.*/<keepalive>120<\/keepalive>/' /etc/jabberd/*.xml*
sed -i 's/<idle>.*/<idle>200<\/idle>/' /etc/jabberd/*.xml*
}}}

2. Restart spacewalk
{{{
/usr/sbin/spacewalk-service restart
}}}

3. Create cronjob in root's crontab that reinitializes the jabber DB.
{{{
SAMPLE
# Restart jabber everyday @6am
00 06 * * *  /sbin/service jabberd stop ; /sbin/service osa-dispatcher stop ; rm -Rf /var/lib/jabberd/db/* ; /sbin/service jabberd start ; /sbin/service osa-dispatcher start
}}}


4. Actually run the command now.

{{{
/sbin/service jabberd stop ; /sbin/service osa-dispatcher stop ; rm -Rf /var/lib/jabberd/db/* ; /sbin/service jabberd start ; /sbin/service osa-dispatcher start
}}}



==== On spacewalk clients ====

1. Create cronjob in root's crontab that restarts osad every 4 hours or so.

(I also disable rhnsd to alleviate issues with that daemon not checking in.)

{{{
SAMPLE
# Check in with spacewalk and restart osad every ~4 hours
0 */3 * * *  sleep `expr ${RANDOM:0:4} / 2` ; /sbin/service osad restart ; /usr/sbin/rhn_check ; /usr/sbin/rhn-profile-sync
}}}

2. Restart OSAD on all clients.
{{{
/sbin/service osad restart
}}}





== Results ==

I now have no issues whatsoever with getting spacewalk clients to pickup scheduled actions within 5 minutes.

Currently working with 235 clients.

Hope this helps someone else.

-- Josh Mullis (CCI - Atlanta)