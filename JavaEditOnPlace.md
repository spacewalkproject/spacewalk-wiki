## Java edit on place
 


Let's say you want to edit a single .java file on existing running machine. Let's say the file you want to edit is KickstartCommand.java (picked up by random):
* Create your temporary working dir

     `cd /tmp` <br>
     `mkdir -p com/redhat/rhn/domain/kickstart/`
* Transfer desired java file to your server

     `scp yourmachine:/yourdir/java/src/com/redhat/rhn/domain/kickstart/KickstartCommand.java  com/redhat/rhn/domain/kickstart/`
* Compile using required libs

     `javac -extdirs /var/lib/tomcat5/webapps/rhn/WEB-INF/lib/ com/redhat/rhn/domain/kickstart/KickstartCommand.java`
* Add .class to rhn.jar

    ` jar uf /usr/share/rhn/lib/rhn.jar com/redhat/rhn/domain/kickstart/KickstartCommand.class`
     `/etc/init.d/tomcat5 restart`

