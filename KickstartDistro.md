= As of Spacewalk 0.4 you should refer to this page [HowToKickstartCobbler] =

== Steps to kickstarting Fedora/CentOS (Pre Spacewalk 0.4) ==
  


== Accessing the kickstart tree directly ==

If you look at a kickstart generated from the satellite, it uses --url !http://HOSTNAME/ty/FkpH3KcN.  This is used by spacewalk to track a particular kickstart session.  These tiny urls are time based and expire after several hours.  If you want to access the kickstart tree directly, simply use this url:

!http://<spacewalk-hostname>/kickstart/dist/<ks-tree-label>

so for example, if my spacewalk server is "spacewalk.example.com" and my kickstart tree label (that I specified in the instructions above) is "fedora-9-i386", I would use:  !http://spacewalk.example.com/kickstart/dist/fedora-9-i386  to access the full kickstart tree.

Please note:  the kickstart trees are not browsable from the web UI.  If you just put that url in your web browser you'll get a 404 since it isn't browsable.  However, if you add a file that is in the tree, you should be able to download it.  Example:  !http://spacewalk.example.com/kickstart/dist/fedora-9-i386/GPL would allow you to download the GPL file that is in the kickstart tree. 


