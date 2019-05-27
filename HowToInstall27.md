# Spacewalk Installation Instructions

These are installation instructions for new installations of Spacewalk 2.7. If you are upgrading from older versions, see [[HowToUpgrade]]. If you want to use the nightly builds please see instructions on [[HowToInstallNightly]]. Spacewalk 2.6 installation instructions are available at [[HowToInstall26]].

*NOTE:* Nightly repo contains developers' snapshot and it is not suitable for production environment. Especially beware that you might not be able to upgrade from the nightly installation to the next release, especially with respect to the database schema.

----
## Prerequisites

 * Outbound open ports 80, 443
 * Inbound open ports 80, 443, 5222 (only if you want to push actions to client machines) and 5269 (only for push actions to a Spacewalk Proxy), 69 udp if you want to use tftp
 * Storage for database: 250 KiB per client system +  500 KiB per channel + 230 KiB per package in channel (i.e. 1.1GiB for channel with 5000 packages)
 * Storage for packages (default /var/satellite): Depends on what you're storing; Red Hat recommend 6GB per channel for their channels
 * 2GB RAM minimum, 4GB recommended
 * Make sure your underlying OS is fully up-to-date.
 * If you use LDAP as a central identity service and wish to pull user and group information from it, see [[SpacewalkWithLDAP]]
 * In the following steps we assume you have a default, vanilla installation of your operating system, without any customized setup of yum repositories, user management, security, etc.

### Special Note For Those Who Will Manage Debian/Ubuntu Clients
There is a change that will need to be made to Debian/Ubuntu client systems.  For more details see [[DebianUbuntuSupportIn27]]

## Setting up Spacewalk repo

RPM downloads of the project are available through yum repositories at

  * https://copr-be.cloud.fedoraproject.org/archive/spacewalk/ - Binary RPMs

To use this repository easily, install spacewalk-repo package with commands below:


### Red Hat Enterprise Linux 6, Scientific Linux 6, CentOS 6

for x86_64:


    rpm -Uvh https://copr-be.cloud.fedoraproject.org/archive/spacewalk/2.7/RHEL/6/x86_64/spacewalk-repo-2.7-2.el6.noarch.rpm

for i386:

    rpm -Uvh https://copr-be.cloud.fedoraproject.org/archive/spacewalk/2.7/RHEL/6/i386/spacewalk-repo-2.7-2.el6.noarch.rpm


### Red Hat Enterprise Linux 7, Scientific Linux 7, CentOS 7

for x86_64:

    rpm -Uvh https://copr-be.cloud.fedoraproject.org/archive/spacewalk/2.7/RHEL/7/x86_64/spacewalk-repo-2.7-2.el7.noarch.rpm

### Fedora 24

for x86_64:

    rpm -Uvh https://copr-be.cloud.fedoraproject.org/archive/spacewalk/2.7/Fedora/24/x86_64/spacewalk-repo-2.7-2.fc24.noarch.rpm

for i386:

    rpm -Uvh https://copr-be.cloud.fedoraproject.org/archive/spacewalk/2.7/Fedora/24/i386/spacewalk-repo-2.7-2.fc24.noarch.rpm

### Fedora 25

for x86_64:

    rpm -Uvh https://copr-be.cloud.fedoraproject.org/archive/spacewalk/2.7/Fedora/25/x86_64/spacewalk-repo-2.7-2.fc25.noarch.rpm

for i386:

    rpm -Uvh https://copr-be.cloud.fedoraproject.org/archive/spacewalk/2.7/Fedora/25/i386/spacewalk-repo-2.7-2.fc25.noarch.rpm

### Fedora 26

for x86_64:

    rpm -Uvh https://copr-be.cloud.fedoraproject.org/archive/spacewalk//2.7/Fedora/26/x86_64/spacewalk-repo-2.7-2.fc26.noarch.rpm

for i386:

    rpm -Uvh https://copr-be.cloud.fedoraproject.org/archive/spacewalk//2.7/Fedora/26/i386/spacewalk-repo-2.7-2.fc26.noarch.rpm


## Additional repos & packages

### EPEL (Red Hat Enterprise Linux, CentOS, Scientific Linux)

Spacewalk requires a Java Virtual Machine with version 1.6.0 or greater.  [EPEL - Extra Packages for Enterprise Linux](http://fedoraproject.org/wiki/EPEL) contains a version of the openjdk that works with Spacewalk.  Other dependencies can get installed from EPEL as well.
To get packages from EPEL just install this RPM:

#### EPEL 6 (use for Red Hat Enterprise Linux 6, Scientific Linux 6, CentOS 6)
    rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm

#### EPEL 7 (use for Red Hat Enterprise Linux 7, Scientific Linux 7, CentOS 7)
    rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

### Java Packages (Red Hat Enterprise Linux, CentOS, Scientific Linux)

Additional java dependencies are needed.
Please configure the following yum repository before beginning your Spacewalk installation:

#### Java packages 7 (use for Red Hat Enterprise Linux 7, Scientific Linux 7, CentOS 7)

    (cd /etc/yum.repos.d && curl -O https://copr.fedorainfracloud.org/coprs/g/spacewalkproject/java-packages/repo/epel-7/group_spacewalkproject-java-packages-epel-7.repo)

#### Java packages 6 (use for Red Hat Enterprise Linux 6, Scientific Linux 6, CentOS 6)
NOTE: You must set up both repos below... the epel-7 repo is not a mistake.

    (cd /etc/yum.repos.d && curl -O https://copr.fedorainfracloud.org/coprs/g/spacewalkproject/java-packages/repo/epel-7/group_spacewalkproject-java-packages-epel-7.repo)
    (cd /etc/yum.repos.d && curl -O https://copr.fedorainfracloud.org/coprs/g/spacewalkproject/epel6-addons/repo/epel-6/group_spacewalkproject-epel6-addons-epel-6.repo)

### Red Hat Optional Server (Red Hat Enterprise Linux)

When using Red Hat Enterprise Linux 6 or 7, make sure you are subscribed to the appropriate Red Hat Optional Server channel:

  * *Red Hat Optional Server 6* , OR
  * *Red Hat Optional Server 7*


## Database server

Spacewalk uses database server to store its primary data. It supports either PostgreSQL (version 8.4 and higher) or Oracle RDBMS (version 10g or higher).

### PostgreSQL server, set up by Spacewalk (embedded)

You can let Spacewalk setup the PostgreSQL server on your machine without any manual intervention. Run:

    yum -y install spacewalk-setup-postgresql

and skip to the section [*Installing Spacewalk*](#installing-spacewalk).

### PostgreSQL server, set up manually

If you prefer to setup the PostgreSQL manually, you have a choice to install it on the same machine as Spacewalk or different machine. Use [[PostgreSQLServerSetup]] as a guide to get the server installed and setup. Namely, you need a database and a user, the user should be a superuser and the database should have the plpgsql and pltclu languages created.

When using external PostgreSQL database, make sure the postgresql-contrib (or postgresql84-contrib on RHEL 5) package is installed on the database server.

### Oracle Pre-Requisites

In order to get Spacewalk to run with Oracle database backend, you need:

 * Instant Client 11.2.0.4.0 installed on the Spacewalk machine
 * Oracle database server, either on the Spacewalk machine or on another host; versions 10g and 11g are known to work

Installation of the free but limited Oracle 11g XE database server can be found on the [Oracle 11g Express Edition Setup](OracleXeSetup) page. Hints for making Spacewalk work with external Oracle database server are on the [Full Oracle Setup](FullOracleSetup) page.


## Installing Spacewalk

Just ask yum to install the necessary packages. This will pull down and install the set of RPMs required to get Spacewalk to run.

If you prefer the [[PostgreSQL]] backend, including the embedded version mentioned above:

    yum -y install spacewalk-postgresql 

If you tend to use the Oracle backend:

    yum -y install spacewalk-oracle 


## Configuring the firewall

Spacewalk needs various inbound ports to be accessible. Use `system-config-firewall` or edit `/etc/sysconfig/iptables`, adding the ports needed -- 80 and 443.

On a system with `firewalld` use `firewall-cmd --add-service=http ; firewall-cmd --add-service=https ; firewall-cmd --runtime-to-perm`. 

Add port 5222 if you want to push actions to client machines and 5269 for push actions to a Spacewalk Proxy, 69 udp if you want to use tftp.


## Configuring Spacewalk

Your Spacewalk server should have a resolvable fully-qualified domain name (FQDN) such as 'hostname.domain.com'.  If the installer complains that the hostname is not the FQDN, do not use the --skip-fqdn-test flag to skip !  

The setup requires that the database account has a password.

*Note:* Please don't use * '#' * (number sign/pound/hash) and * '@' * in your database password otherwise installation will fail.

Once the Spacewalk RPM is installed you need to configure the application.

If you wish to use the default PostgreSql database, and have installed `spacewalk-setup-postgresql`, just run

    spacewalk-setup

If you are planning to use your own database setup (either locally or on a separate machine), then run

    spacewalk-setup --external-oracle

if you are using an Oracle database, *OR*

    spacewalk-setup --external-postgresql

if you are using postgresql.

An example session is as follows:

    # spacewalk-setup
    * Setting up SELinux..
    ** Database: Setting up database connection for PostgreSQL backend.
    ** Database: Installing the database:
    ** Database: This is a long process that is logged in:
    ** Database:   /var/log/rhn/install_db.log
    *** Progress: #
    ** Database: Installation complete.
    ** Database: Populating database.
    *** Progress: #####################################
    * Configuring tomcat.
    * Setting up users and groups.
    ** GPG: Initializing GPG and importing key.
    ** GPG: Creating /root/.gnupg directory
    You must enter an email address.
    Admin Email Address? root@localhost
    * Performing initial configuration.
    * Configuring apache SSL virtual host.
    Should setup configure apache's default ssl server for you (saves original ssl.conf) [Y]? 
    ** /etc/httpd/conf.d/ssl.conf has been backed up to ssl.conf-swsave
    * Configuring jabberd.
    * Creating SSL certificates.
    CA certificate password? 
    Re-enter CA certificate password? 
    Cname alias of the machine (comma seperated)?
    Organization? Fedora
    Organization Unit [spacewalk.server.com]? Spacewalk Unit
    Email Address [root@localhost]? 
    City? Brno
    State? CZ
    Country code (Examples: "US", "JP", "IN", or type "?" to see a list)? CZ
    ** SSL: Generating CA certificate.
    ** SSL: Deploying CA certificate.
    ** SSL: Generating server certificate.
    ** SSL: Storing SSL certificates.
    * Deploying configuration files.
    * Update configuration in database.
    * Setting up Cobbler..
    Cobbler requires tftp and xinetd services be turned on for PXE provisioning functionality. Enable these services [Y]? 
    * Restarting services.
    Installation complete.
    Visit https://spacewalk.server.com to create the Spacewalk administrator account.

Should the spacewalk-setup fail, check the error it gives you and also investigate the logs in /var/log/rhn, as well as the logs from your database server, Apache server and tomcat.

### Configuring Spacewalk with an Answer File

You can also configure Spacewalk by using an answer file, by running `spacewalk-setup` like

    spacewalk-setup --answer-file=<FILENAME>

An example answer file for the Oracle database backend:

    admin-email = root@localhost
    ssl-set-cnames = spacewalk2
    ssl-set-org = Spacewalk Org
    ssl-set-org-unit = spacewalk
    ssl-set-city = My City
    ssl-set-state = My State
    ssl-set-country = US
    ssl-password = spacewalk
    ssl-set-email = root@localhost
    ssl-config-sslvhost = Y
    db-backend=oracle
    db-name=spaceschema
    db-user=spaceuser
    db-password=spacepw
    db-host=localhost
    db-port=1521
    enable-tftp=Y

If you do not supply a value or leave out a key you will be prompted to supply that answer.

For PostgreSQL, you need to create something like this:

    admin-email = root@localhost
    ssl-set-cnames = spacewalk2
    ssl-set-org = Spacewalk Org
    ssl-set-org-unit = spacewalk
    ssl-set-city = My City
    ssl-set-state = My State
    ssl-set-country = US
    ssl-password = spacewalk
    ssl-set-email = root@localhost
    ssl-config-sslvhost = Y
    db-backend=postgresql
    db-name=spaceschema
    db-user=spaceuser
    db-password=spacepw
    db-host=localhost
    db-port=5432
    enable-tftp=Y

After `spacewalk-setup` is complete your application is ready to go!

Once you've made sure you can login to the Spacewalk WebUI, you can then proceed to the next step: [Uploading Content](UploadFedoraContent).


## Managing Spacewalk

Spacewalk consists of several services. Each of them has its own init.d script to stop/start/restart. If you want manage all spacewalk services at once use 

    /usr/sbin/spacewalk-service [stop|start|restart].


## Spacewalk and Let's Encrypt certificate

If you want to replace default self-signed certificate with "trusted" one here is a detailed HowTo written by Avi Miller: [Using Let's Encrypt SSL Certificates with Spacewalk](https://omg.dje.li/2017/04/using-lets-encrypt-ssl-certificates-with-spacewalk/).
