Registering Clients
===================

## Instructions for registering client systems you wish to manage with Spacewalk nightly.

> **Instructions for registering Spacewalk 2.10 are available at [[RegisteringClients]]**

### Before Starting
1. Create a base channel within Spacewalk (Channels > Manage Software Channels > Create New Channel)
2. Create an activation key (Systems > Activation Keys > Create Key) with the new base channel. When creating a registration key do not use the generate function, create a human-readable version. eg: fedora-server-channel. This makes your installation more understandable and provides greater logical consistency to the whole system. On the other hand, if you want to prevent people from getting access to your channels, letting Spacewalk to generate random activation key name is the best way to go.

> **Note:**
> `rhnreg_ks` is used for registration of clients to Spacewalk. If you need to re-register a client to your Spacewalk server or change registration from one environment or server to another Spacewalk server then use the "--force" flag with `rhnreg_ks`, otherwise there is no need to use "--force".

### Fedora, Red Hat Enterprise Linux/Scientific Linux/CentOS 8

1. Install the Spacewalk client yum repository
      ```
      dnf copr enable @spacewalkproject/nightly-client
      ```

2. Install client packages
      ```
      dnf -y install rhn-client-tools rhn-check rhn-setup rhnsd dnf-plugin-spacewalk
      ```

3. Install Spacewalk's CA certificate on the server to enable SSL communication (change rpm version in this command if needed)     
      ```
      rpm -Uvh http://YourSpacewalk.example.com/pub/rhn-org-trusted-ssl-cert-1.0-1.noarch.rpm
      ```

4. Register your Fedora system to Spacewalk using the activation key you created earlier  
      ```
      rhnreg_ks --serverUrl=https://YourSpacewalk.example.org/XMLRPC --sslCACert=/usr/share/rhn/RHN-ORG-TRUSTED-SSL-CERT --activationkey=<key-with-fedora-custom-channel>
      ```

### Red Hat Enterprise Linux, Scientific Linux and CentOS 6 and 7

> **Warning:**
> If you are installing these packages on a Red Hat Enterprise Linux installation it will override some of the original base packages and you may well be invalidating your support agreement with Red Hat!

1. The latest client tools bring the upstream development to your client boxes. That means that the packages may have dependencies that are not found in core Red Hat Enterprise Linux. These dependencies can be found in EPEL. Install the Spacewalk yum repository and matching EPEL repository.

     * RHEL / SL / CentOS 6
       ```
       yum install -y yum-plugin-tmprepo
       yum install -y spacewalk-client-repo --tmprepo=https://copr-be.cloud.fedoraproject.org/results/%40spacewalkproject/spacewalk-2.10-client/epel-6-x86_64/repodata/repomd.xml --nogpg
       rpm -Uvh http://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
       ```

     * RHEL / SL / CentOS 7
       > RHEL: *yum-plugin-copr* is available in optional repository

       ```
       yum -y install yum-plugin-copr
       yum -y copr enable @spacewalkproject/nightly-client

       rpm -Uvh http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
       ```

2. Install client packages
   ```
   yum -y install rhn-client-tools rhn-check rhn-setup rhnsd m2crypto yum-rhn-plugin
   ```

3. Install Spacewalk's CA certificate on the server to enable SSL communication (change rpm version in this command if needed)
   ```
   rpm -Uvh http://YourSpacewalk.example.com/pub/rhn-org-trusted-ssl-cert-1.0-1.noarch.rpm
   ```

4. Register your system to Spacewalk using the activation key you created earlier
   ```
   rhnreg_ks --serverUrl=https://YourSpacewalk.example.org/XMLRPC --sslCACert=/usr/share/rhn/RHN-ORG-TRUSTED-SSL-CERT --activationkey=<key-with-rhel-custom-channel>
   ```

## SUSE and Debian
For SUSE and Debian are not available nightly builds. You could try using packages from latest stable build. See [[RegisteringClients]] for more info.
