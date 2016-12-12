Instructions for registering client systems you wish to manage with Spacewalk

= Before Starting =
  1. Create a base channel within Spacewalk (Channels > Manage Software Channels > Create New Channel)
  2. Create an activation key with the new base channel.

{{{
#!html
<div style="background-color:#8e9f00; color:white; background-image:url('/spacewalk/attachment/wiki/WikiSnippetsAndTemplates/note.png?format=raw'); background-repeat:no-repeat; padding-left:7em;background-position:1em;" >
   <h2>Note</h2>
   <div style="padding-bottom:0.5em;" >
       <p>rhnreg_ks is used for registration of clients to Spacewalk. If you need to re-register a client to your Spacewalk server or change registration from one environment or server to another Spacewalk server then use the --force flag with rhnreg_ks, otherwise there is no need to use --force with registration.</p>
   </div>
</div>
}}}
[[BR]]
{{{
#!html
<div style="background-color:#8e9f00; color:white; background-image:url('/spacewalk/attachment/wiki/WikiSnippetsAndTemplates/note.png?format=raw'); background-repeat:no-repeat; padding-left:7em;background-position:1em;" >
   <h2>Note</h2>
   <div style="padding-bottom:0.5em;" >
       <p>When creating a key, do not use the Generate function but create a human-readable version. eg: fedora-server-channel. This makes the your installation more understandable and logical.</p>
   </div>
</div>
}}}

= Fedora 11 (v0.7 / x86_64) =

  1. Install the Spacewalk yum repository by creating a file in /etc/yum.repos.d with the following contents (note the capital F in Fedora):
    {{{
[spacewalk-client]
name=Spacewalk client
baseurl=http://spacewalk.redhat.com/yum/0.7-client/Fedora/11/x86_64/
gpgkey=http://www.redhat.com/spacewalk/yum/RPM-GPG-KEY-spacewalk
enabled=1
gpgcheck=1
}}}
 

{{{
#!html
<div style="background-color:#d08e13; color:white; background-image:url('/spacewalk/attachment/wiki/WikiSnippetsAndTemplates/important.png?format=raw'); background-repeat:no-repeat; padding-left:7em;background-position:1em;" >
   <h2>Important</h2>
   <div style="padding-bottom:0.5em;" >
       <p>Adjust the baseurl line to suit your needs: (you can copy/paste the url to see what you need).</p>
   </div>
</div>
}}}

  1. Install client packages:
    {{{
yum install rhn-client-tools rhn-check rhn-setup rhnsd m2crypto yum-rhn-plugin
    }}}
  1. Register your Fedora system to Spacewalk using the activation key you created earlier.
{{{
rhnreg_ks --serverUrl=http://YourSpacewalk.example.org/XMLRPC --activationkey=<key-with-fedora-custom-channel> 
}}}

= Fedora 12 (v0.7 / x86_64) =

  1. Install the Spacewalk yum repository by creating a file in /etc/yum.repos.d with the following contents (note the capital F in Fedora):
    {{{
[spacewalk-client]
name=Spacewalk Client
baseurl=http://spacewalk.redhat.com/yum/0.7-client/Fedora/12/x86_64/
gpgkey=http://www.redhat.com/spacewalk/yum/RPM-GPG-KEY-spacewalk
enabled=1
gpgcheck=1
}}}
 
{{{
#!html
<div style="background-color:#d08e13; color:white; background-image:url('/spacewalk/attachment/wiki/WikiSnippetsAndTemplates/important.png?format=raw'); background-repeat:no-repeat; padding-left:7em;background-position:1em;" >
   <h2>Important</h2>
   <div style="padding-bottom:0.5em;" >
       <p>Adjust the baseurl line to suit your needs: (you can copy/paste the url to see what you need).</p>
   </div>
</div>
}}}
  1. Install client packages:
    {{{
yum install rhn-client-tools rhn-check rhn-setup rhnsd m2crypto yum-rhn-plugin
    }}}
  1. Register your Fedora system to Spacewalk using the activation key you created earlier.
{{{
rhnreg_ks --serverUrl=http://YourSpacewalk.example.org/XMLRPC --activationkey=<key-with-fedora-custom-channel> 
}}}

= CentOS 5 or Red Hat Enterprise Linux 5 =
{{{
#!html
<div style="background-color:#d08e13; color:white; background-image:url('/spacewalk/attachment/wiki/WikiSnippetsAndTemplates/important.png?format=raw'); background-repeat:no-repeat; padding-left:7em;background-position:1em;" >
   <h2>Important</h2>
   <div style="padding-bottom:0.5em;" >
       <p>
If your spacewalk server is not running on official RHEL you will need to install the rhel-instnum package on CentOS, Scientific, etc if it is hosting your spacewalk server. You can download this package <a href="http://stahnma.fedorapeople.org/spacewalk-tools/5Server/i386/rhel-instnum-1.0.8-1.el5.noarch.rpm">here</a>.
</p>
   </div>
</div>
}}}

{{{
#!html
<div style="background-color:#9e292b; color:white; background-image:url('/spacewalk/attachment/wiki/WikiSnippetsAndTemplates/warning.png?format=raw'); background-repeat:no-repeat; padding-left:7em;background-position:1em;" >
   <h2>Warning</h2>
   <div style="padding-bottom:0.5em;" >
      <p>if you are installing these packages on Red Hat Enterprise Linux it will override some original base packages and you may be invalidating your support agreement!</p>
   </div>
</div>
}}}

  1. Install required packages before the red hat network packages: 
    {{{
yum -q -y install usermode-gtk pyOpenSSL 1>/dev/null 2>&1
    }}}
  
  2. Install the Spacewalk yum repository by creating a file in /etc/yum.repos.d with the following contents (note the capital F in Fedora):
    {{{
[spacewalk-client]
name=Spacewalk Client
baseurl=http://spacewalk.redhat.com/yum/0.7-client/RHEL/5/x86_64/
gpgkey=http://www.redhat.com/spacewalk/yum/RPM-GPG-KEY-spacewalk
enabled=1
gpgcheck=1
}}}
 
{{{
#!html
<div style="background-color:#d08e13; color:white; background-image:url('/spacewalk/attachment/wiki/WikiSnippetsAndTemplates/important.png?format=raw'); background-repeat:no-repeat; padding-left:7em;background-position:1em;" >
   <h2>Important</h2>
   <div style="padding-bottom:0.5em;" >
       <p>Adjust the baseurl line to suit your needs: (you can copy/paste the url to see what you need).</p>
   </div>
</div>
}}}

  3. Install client packages:
    {{{
yum install rhn-client-tools rhn-check rhn-setup rhnsd m2crypto yum-rhn-plugin
    }}}

  4. Register your CentOS or Red Hat Enterprise Linux system to Spacewalk using the activation key you created earlier.
{{{
rhnreg_ks --serverUrl=http://YourSpacewalk.example.org/XMLRPC --activationkey=<key-with-rhel-custom-channel> 
}}}

= CentOS 4 =

Registering a CentOS 4 server to Spacewalk is exactly the same as it would be for CentOS 5, but rhnreg_ks, rhn_check and other related scripts are located in the package '''up2date''', and not in '''rhn-setup'''.

1. Enable '''spacewalk-tools''' repo for Yum and install '''up2date''' package:

{{{
rpm -ivh http://stahnma.fedorapeople.org/spacewalk-tools/spacewalk-client-tools-0.0-1.noarch.rpm
yum install up2date
}}}

2. Register your CentOS system to Spacewalk using the activation key you created earlier:

{{{
rhnreg_ks --serverUrl=http://YourSpacewalk.example.org/XMLRPC --activationkey=<key-with-centos-custom-channel>
}}}

= Solaris 8, 9 or 10 =

See [wiki:Solaris Solaris Support].