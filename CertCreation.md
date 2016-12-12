= Steps to creating Certs =

== Overview ==

The Spacewalk Server provides all services to customers through the setting of entitlements. The entitlements available through Spacewalk are determined by the Entitlement Certificate which contains the precise set of entitlements attributed to your organization. The entitlement certificiate is encryped with a GPG key. For Red Hat supported channels, the certificiate is encryped with Red Hat's security GPG key. It is possible to create your own GPG key and certificate for private non Red Hat channels. This process involves creating a GPG key and signing a certificate for use with Spacewalk. The GPG signed certificate is used to activate the Spacewalk using rhn-satellite-activate. A certificate template and signing script is included for your use. 

== Generate GPG keys ==

 1. First create a GPG key,
   
    {{{
gpg --gen-key
    }}}

 2. Then see if your key appears in the output of the following commands. For example, we generated a key for Scooby below.

    {{{
[scooby@scooby ~]$ gpg --list-keys
/home/scooby/.gnupg/pubring.gpg
-------------------------------
pub   1024D/74E73973 2008-06-11
uid                  Scooby Doo <scooby@redhat.com>
sub   2048g/AFD3819B 2008-06-11

[scooby@scooby ~]$ gpg --list-secret-keys
/home/scooby/.gnupg/secring.gpg
-------------------------------
sec   1024D/74E73973 2008-06-11
uid                  Scooby Doo <scooby@redhat.com>
ssb   2048g/AFD3819B 2008-06-11

   }}}

 3. Finally, export the public and secret keys.

   {{{
gpg --export -a 74E73973 > mycertkey.gpg
gpg --export-secret-keys -a 74E73973 > mysecretkey.gpg
   }}}

== Install GPG keys into webapp ==

Set the {{{web.gpg_keyring}}} in {{{/usr/share/rhn/config-defaults/rhn_web.conf}}} to your newly exported keyring.

Alternatively, you can add your keys to the {{{/etc/webapp-keyring.gpg}}} keyring.  Here are some commands to help you accomplish this:

   {{{
gpg --keyring /etc/webapp-keyring.gpg --no-default-keyring --import mycertkey.gpg mysecretkey.gpg
   }}}

To see the keys in this keyring, use the following command:

   {{{
gpg --keyring /etc/webapp-keyring.gpg --no-default-keyring --list-keys
   }}}

NOTE: The {{{--no-default-keyring}}} option excludes the user's keyrings (usually {{{~/.gnupg/*.gpg}}}) from the operation, otherwise they would be included as the {{{--keyring}}} option merely adds the specified keyring to the list of default ones.

== Signing the Certificate ==

 1. Download the [https://github.com/spacewalkproject/spacewalk/blob/master/scripts/gen-oss-sat-cert.pl] script to your Spacewalk.
 2. Modify the {{{spacewalk-public.cert}}} or create a new certifcate to sign
 3. Review the usage statement from {{{scripts_gen-oss-sat-cert.pl}}}:
{{{
Usage: ./scripts_gen-oss-sat-cert.pl --orgid <org_id> --owner <owner_name> --signer <signer> --no-pass-phrase --output <dest> --expires <when> --slots <num> [ --provisioning-slots <num> ] [ --channel-family label=n ] [ --satellite-version X.Y ] [ --resign ]
}}}
 4. Make sure you import your public and secret keys as the user you are running the script as. 
{{{
gpg --import mycertkey.gpg
gpg --import mysecretkey.gpg
}}}
 5. Resign/sign the cert, for example:
{{{
perl scripts_gen-oss-sat-cert.pl --signer 74E73973 --resign template-eval.cert
}}}

== Activating the Spacewalk locally (optional) ==
If you came to this page from the HowToInstall page, activation will occur during {{{spacewalk-setup}}}, so you
can safely ignore this section. Otherwise, continue reading.

You should use the options below to accomplish the following tasks in this order:

   1. Validate the Entitlement Certificate's sanity (or usefulness).
   2. Activate the Spacewalk locally by inserting the Entitlement Certificate into the local database. 

Here are some examples depicting use of the tool and these options.

To validate an Entitlement Certificate's sanity only:
{{{
rhn-satellite-activate --sanity-only --rhn-cert=/path/to/demo.cert
}}}
To validate an Entitlement Certificate and populate the local database:
{{{
rhn-satellite-activate --disconnected --rhn-cert=/path/to/demo.cert
}}}
Once you run this final command, the Spacewalk is running and able to serve packages locally.


