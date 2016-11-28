= Spacewalk Proxy Installation Instructions =

'''These instructions are for Spacewalk Proxy 0.2 onwards'''

If you're looking for instructions for the Red Hat Satellite Proxy product see the [https://access.redhat.com/documentation/en-US/Red_Hat_Satellite/ Red Hat Satellite documentation].

For more information on what Spacewalk is or for developer information see [wiki:proxy this page].

 * For Spacewalk proxy installation for Spacewalk 2.5 see [wiki:HowToInstallProxy25]
 * For Spacewalk proxy installation for Spacewalk 2.4 see [wiki:HowToInstallProxy24]
 * For Spacewalk proxy installation for Spacewalk 2.3 see [wiki:HowToInstallProxy23]

== Prerequisites ==

  * Around 6GB storage per distribution under /var/spool/squid (or wherever you want your Squid cache to be)
  * Outbound open ports 80, 443, 4545 (only if you want to enable monitoring) and 5269
  * Inbound open ports 80, 443 and 5222
  * An upstream RHN Satellite server with an available Proxy entitlement or a Spacewalk server
  * Machine, where you will install Spacewalk Proxy, must be registered against Spacewalk Server, which you will proxy.
  * A provisioning entitlement for the Proxy server
  * Enable EPEL yum repository

== Repository ==

RPM downloads of the project are available through yum repositories. Set up your yum to point to Spacewalk 2.4 repositories (including *-client). For the repo setup specifics, see HowToInstall#SettingupSpacewalkrepo. 

== Installation ==

Ensure your machine is registered in Spacewalk and has a provisioning entitlement. Then just ask yum to install the application:

{{{
yum -y install spacewalk-proxy-selinux spacewalk-proxy-installer
}}}

This will pull down and install the set of RPMs required for the installer to run.:


If this is the first time installing an RPM from the Spacewalk repo, yum will prompt you to install the GPG key:

Then you need to configure the proxy. Run:

{{{
configure-proxy.sh
}}}

and follow the prompts. This will download and configure the necessary packages. Once installed use /usr/sbin/rhn-proxy to stop and start the service.

You should now be able to navigate to the hostname of your proxy in a web browser and see the Spacewalk Proxy page. If you see an error here there's likely something wrong with your install.

  * If all seems well you can now try [RegisteringClients Registering Clients]
  * If you're having problems check [DebuggingProxy Debugging Proxy]

== Automated Installation ==

The configure-proxy.sh install script supports an answer file to allow you to preanswer the questions. For the full list of variables see "man configure-proxy.sh".

{{{
configure-proxy.sh --answer-file=proxyanswers.txt
}}}

proxyanswers.txt:

{{{
VERSION="2.6"
RHN_PARENT="spacewalk.example.com"
TRACEBACK_EMAIL="admin@example.com"
USE_SSL="Y"
CA_CHAIN="/usr/share/rhn/RHN-ORG-TRUSTED-SSL-CERT"
HTTP_PROXY=
SSL_CNAME_ASK=''
SSL_ORG="Example Org"
SSL_ORGUNIT="proxy1.example.com"
SSL_COMMON="proxy1.example.com"
SSL_CITY="New York"
SSL_STATE="New York"
SSL_COUNTRY="US"
SSL_EMAIL="admin@example.com"
POPULATE_CONFIG_CHANNEL="n"
}}}

As this example doesn't answer all the questions (e.g. SSL_PASSWORD) you'd still be prompted for that value. If you know that you've answered all questions then you can run with --non-interactive.