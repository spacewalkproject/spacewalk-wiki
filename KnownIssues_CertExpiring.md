= Certificate Expiring =
If you are receiving this message via the Spacewalk web interface the explanation can be found by reading this [https://www.redhat.com/archives/spacewalk-list/2015-July/msg00036.html message] from the mailing list.

To resolve the issue, install the newer Spacewalk certificate, if you have already upgraded to Spacewalk 2.2 or newer, run this command (service/server restart is '''not''' required) 

{{{ 
#!sh 
$ sudo rhn-satellite-activate --rhn-cert /usr/share/spacewalk/setup/spacewalk-public.cert --disconnected 
}}} 

If you are still running an older version before Spacewalk 2.2, download the certificate and install (again, no need to restart the server/service) 

{{{ 
#!sh 
$ wget 'https://raw.githubusercontent.com/spacewalkproject/spacewalk/master/branding/setup/spacewalk-public.cert' -O spacewalk-public.cert 
$ sudo rhn-satellite-activate --rhn-cert spacewalk-public.cert --disconnected 
}}}