# osad Installation Instructions



 * [Server Setup (osa-dispatcher and jabberd)](OSADSetup#ServerSetup)
 * [Client Setup (osad)](OSADSetup#ClientSetup)

## Server Setup
  * Install osa-dispatcher:
    ```shell
    yum install osa-dispatcher
    chkconfig osa-dispatcher on
    service osa-dispatcher start
    ```

## Client Setup
 * Install osad
    ```shell
    yum install osad
    ```
 * Change *osa_ssl_cert* in /etc/sysconfig/rhn/osad.conf to:

    `osa_ssl_cert = /usr/share/rhn/RHN-ORG-TRUSTED-SSL-CERT`

 * If you haven't already, download the trusted cert to your client
    ```shell
    cd /usr/share/rhn/
    wget http://yourspacewalk.example.org/pub/RHN-ORG-TRUSTED-SSL-CERT
    ```
 * Start osad
  ```shell
    service osad start
  ```
