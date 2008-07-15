= Python Code Documentation =

[http://www.redhat.com/spacewalk/documentation/python-doc/]

= Python Backend Architecture =

If you examine the backend source code comprising all of Spacewalk you will see it split up into 2 major areas:

== Python client code: ==

This is the set of client side libraries that run on the managed system. The client code periodically checks in with the server which sends instructions to the client to execute. Examples: rhn_check, rhn_register, yum-rhn-plugin, rhnmd.

== Python Server backend code: ==

This is the server side code that exposes an XMLRPC interface which the clients interact with. It encapsulates a good portion of RHN Satellite's business rules and validation. In order to service up this XMLRPC interface a hand coded python object mapping of RHN's database schema was developed.


{{{
                         SPACEWALK SERVER BACKEND ARCHITECTURE 
                        ======================================
                                                _____________
    _________        ________________          | FILESYSTEM |----------|
   |         |      |    APACHE      |-------->|____________|          |
   | CLIENT  |----->|      +         |         ________          ______v_____
   |         |      |    SERVER      |         |       |         | SATELLITE |     CHANNEL
   |_________|      |    BACKEND     |-------->|  DB   |-------->| EXPORTER  |----> DUMPS
       ^            |      +         |         |_______|         |___________|
       | rhn-check  |    XMLRPC      |             ^  ^
       |            |    HANDLERS    |             |  |            ____________
       |            |________________|             |  |-----------| SAT-SYNC  |---> HOSTED
  __________                                   ____|__________    |___________|
 |  OSAD   |------------- jabber ------------->|  OSA        |
 |_________|                                   |DISPATCHER   |
                                               |_____________|

}}}

== XMLRPC Handlers ==

=== APP & PACKAGE-PUSH: ===

This handler provides capability for rhnpush client to push the package information and associate it with the db and the filesystem.

=== APPLET: ===

This handler provides calls for rhn-applet tool for polling package information.

=== CONFIG & CONFIG-MGMT: ===

These handlers are used by config management client to communicate with the server in managing config files and channels.

=== SAT: ===

This handler is mainly internal to the satellite itself and essentially used by satellite sync.

=== XMLRPC: ===

This handler is mainly responsible for providing capability for registation, scheduled actions, package updates and proxy functionality. Tools using this handler are registration client, up2date, rhn_check and proxy.

=== XP: ===

This is used for package information uploads and other additional capabilities can be added here


== Installing Backend Server Code: ==

Deploying the python backend on your local workstation could be useful if you wanna test/work on the code locally.

 * First, make sure you have httpd installed. 
   
  {{{ $ yum install httpd (or whatever you do) }}}

 * Make sure, rhnlib is installed
  
  {{{ $ yum install rhnlib }}}

  {{{ or build it from source : $ cd <git checkout>/client/rhel/rhnlib   $ sudo make install -f Makefile.rhnlib  }}}
 
 * Go to the directory where you have your backend checkout 
   
  {{{ $ cd <git checkout>/backend }}}

 * Install the code using the 'install' makefile target 
    
  {{{ $ sudo make install -f Makefile.backend }}}

 * Assuming you are running Apache 2.X, you need to edit some of the configuration files to make them work. Here is a bash command that can simplify it for you: 
   
  {{{ for i in /etc/httpd/conf/rhn/*.conf; do sed -i 's/\"/default/' $i; done; }}}

 * Edit some of the rhn configuration variables. 
   
  {{{ In /etc/rhn/rhn.conf: default_db = rhnsat/rhnsat@rhnsat (or whatever db you want)}}}

 * Depending on if the passwords in the db you are using are encrypted are not you may have to change the value for encrypted_passwords: 
  
  {{{ encrypted_passwords = 1 }}}

 * Start apache. 
  
  {{{ $ service httpd start }}}