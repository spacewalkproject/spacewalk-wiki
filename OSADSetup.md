# osad Installation Instructions



 * [Server Setup (osa-dispatcher and jabberd)](OSADSetup#ServerSetup)
 * [Client Setup (osad)](OSADSetup#ClientSetup)

## Server Setup
  * Ensure port 5222/tcp is open for inbound connections:
    * See [Configuring the firewall](HowToInstall#configuring-the-firewall) section of the [Spacewalk Installation Instructions](HowToInstall#configuring-the-firewall) page
  * Ensure osa-dispatcher and jabberd are installed and running:
    * osa-dispatcher should have been installed when Spacewalk was installed
     ```shell
     # systemctl status osa-dispatcher.service jabberd.service
     ● osa-dispatcher.service - OSA Dispatcher daemon
        Loaded: loaded (/usr/lib/systemd/system/osa-dispatcher.service; enabled; vendor preset: disabled)
        Active: active (running) since Wed 2018-01-10 15:46:19 EST; 1min 12s ago
       Process: 12371 ExecStart=/usr/sbin/osa-dispatcher --pid-file /var/run/osa-dispatcher.pid (code=exited, status=0/SUCCESS)
       Process: 12370 ExecStartPre=/bin/rm -f /var/run/osa-dispatcher.pid (code=exited, status=0/SUCCESS)
      Main PID: 12375 (osa-dispatcher)
         Tasks: 1 (limit: 4915)
        CGroup: /system.slice/osa-dispatcher.service
                └─12375 /usr/bin/python -s /usr/sbin/osa-dispatcher --pid-file /var/run/osa-dispatcher.pid

     ...

     ● jabberd.service - Jabber Server
        Loaded: loaded (/usr/lib/systemd/system/jabberd.service; enabled; vendor preset: disabled)
        Active: active (exited) since Wed 2018-01-10 15:46:18 EST; 31min ago
       Process: 12322 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
      Main PID: 12322 (code=exited, status=0/SUCCESS)
         Tasks: 0 (limit: 4915)
        CGroup: /system.slice/jabberd.service
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
