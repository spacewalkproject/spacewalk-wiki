Issue: Spacewalk 2.4 (and previous versions) has expired on 2018-07-13

Solution: Upgrade Spacewalk to 2.5 (which does not use entitlement certificates anymore)

OR

download following gpg public key which was used for signing the entitlement certificate:
`wget https://raw.githubusercontent.com/spacewalkproject/spacewalk/SPACEWALK-2.4/branding/setup/spacewalk-cert-key.gpg`

import the key: `gpg --keyring /etc/webapp-keyring.gpg --no-default-keyring --import spacewalk-cert-key.gpg`

NOTE: keyring path might differ, the default is `/etc/webapp-keyring.gpg` if you have changed it, look for `web.gpg_keyring` in `/usr/share/rhn/config-defaults/rhn_web.conf` and `/etc/rhn/rhn.conf`

and then download the updated entitlement certificate: `wget https://raw.githubusercontent.com/spacewalkproject/spacewalk/SPACEWALK-2.4/branding/setup/spacewalk-public.cert`

and import it: `rhn-satellite-activate --disconnected --rhn-cert=spacewalk-public.cert`