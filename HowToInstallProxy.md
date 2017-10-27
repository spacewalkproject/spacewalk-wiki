# Spacewalk Proxy Installation Instructions

If you're looking for instructions for the Red Hat Satellite Proxy product see the [Red Hat Satellite documentation](https://access.redhat.com/documentation/en-us/red_hat_satellite/).

For more information on what Spacewalk Proxy is see [[proxy]] page.

 * For Spacewalk proxy installation for Spacewalk 2.6 see [[HowToInstallProxy26]]
 * For Spacewalk proxy installation for Spacewalk 2.5 see [[HowToInstallProxy25]]
 * For Spacewalk proxy installation for Spacewalk 2.4 see [[HowToInstallProxy24]]
 * For Spacewalk proxy installation for Spacewalk 2.3 see [[HowToInstallProxy23]]

## Prerequisites

  * Around 6GB storage per distribution under /var/spool/squid (or wherever you want your Squid cache to be)
  * Outbound open ports 80, 443, 4545 (only if you want to enable monitoring) and 5269
  * Inbound open ports 80, 443 and 5222
  * An upstream RHN Satellite server with an available Proxy entitlement or a Spacewalk server
  * Machine, where you will install Spacewalk Proxy, must be registered against Spacewalk Server, which you will proxy.
  * A provisioning entitlement for the Proxy server
  * Enable EPEL yum repository

## Repository

RPM downloads of the project are available through yum repositories. Set up your yum to point to Spacewalk 2.7 repositories (including *-client). For the repo setup specifics, see [[HowToInstall]] and [[RegisteringClients]].. 
## Installation



Ensure your machine is registered in Spacewalk and has a provisioning entitlement. Then just ask yum to install the application:


    yum -y install spacewalk-proxy-selinux spacewalk-proxy-installer

This will pull down and install the set of RPMs required for the installer to run. If this is the first time installing an RPM from the Spacewalk repo, yum will prompt you to install the GPG key.

Before you running the installation script, the Spacewalk Proxy requires SSL files from your Spacewalk Server. Create the /root/ssl-build directory:

    mkdir /root/ssl-build

Then copy the public certificate and CA certificate from the desired Spacewalk Server to the new directory:

    scp 'root@spacewalk.example.com:/root/ssl-build/{RHN-ORG-PRIVATE-SSL-KEY,RHN-ORG-TRUSTED-SSL-CERT,rhn-ca-openssl.cnf}' /root/ssl-build


Then you need to configure the proxy. Run:


    configure-proxy.sh

and follow the prompts. This will download and configure the necessary packages. Once installed use /usr/sbin/rhn-proxy to stop and start the service.

You should now be able to navigate to the hostname of your proxy in a web browser and see the Spacewalk Proxy page. If you see an error here there's likely something wrong with your install.

  * If all seems well you can now try [Registering Clients](RegisteringClients)
  * If you're having problems check [Debugging Proxy](DebuggingProxy)
## Automated Installation



The configure-proxy.sh install script supports an answer file to allow you to preanswer the questions. For the full list of variables see "man configure-proxy.sh".


    configure-proxy.sh --answer-file=proxyanswers.txt

proxyanswers.txt:


    VERSION="2.7"
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

As this example doesn't answer all the questions (e.g. SSL_PASSWORD) you'd still be prompted for that value. If you know that you've answered all questions then you can run with --non-interactive.