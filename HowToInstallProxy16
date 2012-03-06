= Spacewalk Proxy Installation Instructions =

'''These instructions are for Spacewalk Proxy 0.2 onwards'''

If you're looking for instructions for the RHN Proxy product see the [http://docs.redhat.com/docs/en-US/Red_Hat_Network_Satellite/index.html Red Hat Network Satellite documentation].

== Prerequisites ==

  * Disable SELinux or install spacewalk-proxy-selinux
  * Around 6GB storage per distribution under /var/spool/squid (or whereever you want your Squid cache to be)
  * Outbound open ports 80, 443, 4545 (only if you want to enable monitoring) and 5269
  * Inbound open ports 80, 443 and 5222
  * An upstream RHN Satellite server with an available Proxy entitlement or a Spacewalk server
  * Machine where you will install Spacewalk Proxy must be registered against Spacewalk Server, which you will proxy.
  * A provisioning entitlement for the Proxy server
  * Enable EPEL yum repository

== Repository ==

RPM downloads of the project are available through yum repositories:

  * http://spacewalk.redhat.com/yum
or
  * nightly builds repo
(choose one of them)

==== Spacewalk-repo ====
To use the official repository install spacewalk-repo RPM:

Red Hat Enterprise Linux 5, CentOS
{{{
rpm -Uvh http://spacewalk.redhat.com/yum/1.6/RHEL/5/x86_64/spacewalk-repo-1.6-1.el5.noarch.rpm
}}}
Red Hat Enterprise Linux 6
{{{
rpm -Uvh http://spacewalk.redhat.com/yum/1.6/RHEL/6/x86_64/spacewalk-repo-1.6-1.el6.noarch.rpm
}}}
Fedora 15
{{{
rpm -Uvh http://spacewalk.redhat.com/yum/1.6/Fedora/15/x86_64/spacewalk-repo-1.6-1.fc14.noarch.rpm
}}}
Fedora 16
{{{
rpm -Uvh http://spacewalk.redhat.com/yum/1.6/Fedora/16/x86_64/spacewalk-repo-1.6-1.fc15.noarch.rpm
}}}

==== Nightly builds ====

If you want to use the nightly builds, add a .repo file pointing to the nightly repository:

{{{
cat > /etc/yum.repos.d/spacewalk.repo << 'EOF'
[spacewalk]
name=Spacewalk
# RHEL 5 / CentOS 5
baseurl=http://spacewalk.redhat.com/yum/nightly/RHEL/5/$basearch/
# RHEL 6
baseurl=http://spacewalk.redhat.com/yum/nightly/RHEL/6/$basearch/
# Fedora 15
#baseurl=http://spacewalk.redhat.com/yum/nightly/Fedora/15/$basearch/
# Fedora 16
#baseurl=http://spacewalk.redhat.com/yum/nightly/Fedora/16/$basearch/
gpgkey=http://spacewalk.redhat.com/yum/RPM-GPG-KEY-spacewalk-2010
enabled=1
gpgcheck=0
EOF
}}}

== Installation ==

Ensure your machine is registered in Spacewalk and has a provisioning entitlement. Then just ask yum to install the application:

{{{
yum install spacewalk-proxy-installer
}}}

This will pull down and install the set of RPMs required for the installer to run. Below is an example of the set of packages pulled in:

{{{
========================================================================================================================
 Package                                 Arch                 Version                     Repository               Size
========================================================================================================================
Installing:
 spacewalk-proxy-installer               noarch               0.5.25-1.el5                spacewalk                40 k
Installing for dependencies:
 rhncfg                                  noarch               5.9.5-1.el5                 spacewalk                58 k
 rhncfg-actions                          noarch               5.9.5-1.el5                 spacewalk                28 k
 rhncfg-client                           noarch               5.9.5-1.el5                 spacewalk                24 k
 rhncfg-management                       noarch               5.9.5-1.el5                 spacewalk                33 k
}}}

If this is the first time installing an RPM from the Spacewalk repo, yum will prompt you to install the GPG key:

{{{
Importing GPG key 0x430A1C35 "Spacewalk <spacewalk-devel@redhat.com>" from http://spacewalk.redhat.com/yum/RPM-GPG-KEY-spacewalk
Is this ok [y/N]: y
}}}

Then you need to configure the proxy. Run:

{{{
configure-proxy.sh
}}}

and follow the prompts. This will download and configure the nessecery packages. Once installed use /usr/sbin/rhn-proxy to stop and start the service.

You should now be able to navigate to the hostname of your proxy in a web browser and see the Spacewalk Proxy page. If you see an error here there's likely something wrong with your install.

  * If all seems well you can now try [RegisteringClients Registering Clients]
  * If you're having problems check [DebuggingProxy Debugging Proxy]

== Automated Installation ==

The configure-proxy.sh install script supports an answer file to allow you to preanswer the questions. For the full list of variables see "man configure-proxy.sh".

{{{
configure-proxy.sh --answerfile=proxyanswers.txt
}}}

proxyanswers.txt:

{{{
VERSION="1.6"
RHN_PARENT="spacewalk.example.com"
TRACEBACK_EMAIL="admin@example.com"
USE_SSL="Y"
CA_CHAIN="/usr/share/rhn/RHN-ORG-TRUSTED-SSL-CERT"
HTTP_PROXY=
SSL_ORG="Example Org"
SSL_ORGUNIT="proxy1.example.com"
SSL_COMMON="proxy1.example.com"
SSL_CITY="New York"
SSL_STATE="New York"
SSL_COUNTRY="US"
SSL_EMAIL="admin@example.com"
INSTALL_MONITORING="n"
POPULATE_CONFIG_CHANNEL="n"
}}}

As this example doesn't answer all the questions (e.g. SSL_PASSWORD) you'd still be prompted for that value. If you know that you've answered all questions then you can run with --non-interactive.