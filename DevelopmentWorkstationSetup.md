= Intro = 

The below instructions are how to set up the Spacewalk Development Environment. There are two versions of this setup. I have been unable to get the full workstation working properly on operating systems running tomcat 7. It refuses to compile the jsps at run time for reasons I don't understand. As such, newer operating systems like Fedora 19 and 20 should use the "Poor Man's Dev Workstation" below. Older operating systems like RHEL 5 and 6 can use the "Dev Workstation" instructions. There are positives and negatives to both.

With the regular "Dev Workstation" it is possible to see the difference in jsps you have just edited by simply refreshing the page. It is also possible to configure eclipse to auto-build class files when you save and have tomcat auto-reload when it detects the class files have changed. This leads to a very smooth and fast development environment when it's all working correctly. However, it tends to be very fragile and rely on specific versions of jar files that are named exactly the correct thing at exactly the correct location on the filesystem.

In the "Poor Man's Dev Workstation" you have to run an ant command every time you want your changes to take effect, which can take several minutes. However, it tends to be much less fragile and actually works with tomcat 7, which is good.

In either case python and perl code can be changed by running 'sudo make install' and restarting httpd. There are some common prerequisites followed by the setup instructions for both setups.

= Prerequisites =

=== Jpackage-Generic Repo ===

Add the following to /etc/yum.repos.d/jpackage-generic.repo

{{{
[jpackage-generic]
name=JPackage generic
#baseurl=http://mirrors.dotsrc.org/pub/jpackage/5.0/generic/free/
mirrorlist=http://www.jpackage.org/mirrorlist.php?dist=generic&type=free&release=5.0
enabled=1
gpgcheck=0
}}}

=== Spacewalk Runtime Environment ===

In order to setup the  Spacewalk development environment, you first need to have a running version of Spacewalk installed following the instructions in [HowToInstall Spacewalk Installation Instructions].

=== Git  ===
To setup the sources follow the instructions in [GitGuide#InstallGit Install Git] and [GitGuide#ClonetheSpacewalkgitrepository clone the Spacewalk repository].

= Web Application =

== Poor Man's Dev Workstation ==
Use on any OS that runs tomcat 7, or if you just prefer a simpler and less fragile but slower development environment. FIXME: This is not working yet. Figure out what needs to be changed to make it work.

=== Installation ===
 * {{{sudo /usr/sbin/rhn-satellite stop}}}
 * {{{sudo yum install ant-contrib yum-plugin-versionlock}}}
 * {{{sudo yum install jmock --disablerepo=* --enablerepo=jpackage-generic}}}
   * {{{sudo yum versionlock jmock}}}
 * {{{cd $SPACEWALK_GIT/java}}}
 * {{{sudo ant install-web}}} 

=== Developing ===
To develop changes in the java stack, make your changes and then from $SPACEWALK_GIT/java:
 * {{{sudo ant install-tomcat7}}} (or as appropriate for your version of tomcat)
 * {{{sudo service tomcat restart}}}

== Full Dev Workstation ==

=== Installation ===
 * {{{sudo /usr/sbin/rhn-satellite stop}}}
 * {{{sudo yum install ant-nodeps ant-contrib junit ant-junit java-1.7.0-openjdk-devel postgresql-jdbc}}}
 * {{{sudo yum install jmock --disablerepo=* --enablerepo=jpackage-generic}}}
   * {{{sudo yum install -y yum-plugin-versionlock; sudo yum versionlock jmock}}}
 * {{{sudo mkdir /usr/lib/java-1.7.0 /usr/share/java-1.7.0}}}
 * {{{cd $SPACEWALK_GIT/java}}}
 * {{{ant init-install compile}}}
 * If you run into an error like {{{Unsupported major.minor version 52.0}}}:
   * {{{sudo yum downgrade jasper-libs}}}
   * Try again. It should work, if not repeat downgrade until it does.
   * {{{sudo yum versionlock jasper-libs}}}
 * {{{ant all, or ant clean all }}}
 * {{{ant create-webapp-dir}}}
 * {{{sudo ant install-devel}}} 
   * Note: if this fails the first time do not run again! Instead look at the install-java target in java/buildconf/build-webapp.xml and manually do whatever remains to be done.
 * {{{sudo /usr/sbin/rhn-satellite start}}}

=== Set Permissions ===

Make sure that permissions are properly set.  If your Git repo is in your homedir, then apache and tomcat must be able to get read and execute access to not only the Git repo, but your homedir as well.  The following commands should work when run from homedir:
 * {{{[you@yourmachine ~]$ sudo setfacl  -R  -m  u:apache:rx .}}}
 * {{{[you@yourmachine ~]$ sudo setfacl  -R  -m  u:tomcat:rx .}}}

=== Uninstallation ===

In case you decide to undo your development environment changes and go back to running your Spacewalk.

'''NOTE:''' this operation right now is a bit dodgy

 * {{{sudo ant uninstall-devel}}}

Your development environment should now be ready to go. Having problems? Check out the [DevelopmentWorkstationSetup#Troubleshooting troubleshooting] section below.


= Search Server Setup =
If you are going to be updating the search server java code, you would have to do the following:
 * {{{sudo /usr/sbin/rhn-satellite stop}}}
 * {{{cd $SPACEWALK_GIT/search-server}}}
 * Change what ever you need to .. Commit it in a local branch in git.
 * {{{../rel-eng/bin/tito build --test --rpm && /tmp/spacewalk-build/noarch/spacewalk-search-*.git.*.noarch.rpm }}}
 * {{{it's a good idea to wipe out the search indexes, "sudo /sbin/service rhn-search cleanindex"}}}
 * {{{sudo /usr/sbin/rhn-satellite start}}}


= Deploying development schema =

This will completely drop all data from the database and recreate all tables, stored procs, etc.  Any systems, channels, or packages will no longer exist within the DB after this operation.  It is not reversible.  When it is finished, you should have a database deployed with a schema that is inline with your git checkout.

{{{ 
 /etc/init.d/rhn-satellite stop
 /etc/init.d/oracle-xe restart
 cd $SPACEWALK_GIT/schema/spacewalk 
 make satellite-clean satellite-release TBS=USERS SQLUSER=spacewalk/spacewalk@xe     
 rhn-satellite-activate --disconnected  (you may have to rename old validate-sat-cert.pl and soft link $SPACEWALK_GIT/spacewalk/admin/validate-sat-cert.pl to /usr/bin/
 /etc/init.d/rhn-satellite start
}}} 

{{{
  If pointing to an old Satellite's DB, use TBS=DATA_TBS  or you will be sorry!
}}}

{{{
  To determine if it was successful, look for
  SQL_FILE
  ---------------
  rhnsat/quit.sql
}}}


= Taskomatic =

To be able to edit Taskomatic code you simply need to create a symlink:
{{{
  cd /usr/share/rhn/lib/
  mv rhn.jar rhn.jar.bak
  ln -s ~/spacewalk/java/build/run-lib/rhn.jar rhn.jar
  service taskomatic restart
}}}

Now you can simply edit the code as desired.  To have taskomatic pick up those changes simply run 'ant' from the java directory and restart taskomatic.



= Troubleshooting =
 * "ant create-webapp-dir" fails with "Problem: failed to create task or type for":
   ant-contrib.jar needs to have a For.class. Newer versions do not have it. Grab the version of ant-contrib.jar from our ivy repo and use it instead.
   If your the real jar (not a symlink) is at /usr/share/java/ant-contrib-1.0.jar:
   * {{{wget http://sherr.fedorapeople.org/ivy/ant-contrib-1.0.jar -O /usr/share/java/ant-contrib-1.0.jar}}}

 * catalina.out has the following error:
{{{
SEVERE: Error starting static Resources
java.lang.IllegalArgumentException: Document base /usr/share/tomcat5/webapps/rhn does not exist or is not a readable directory
        at org.apache.naming.resources.FileDirContext.setDocBase(FileDirContext.java:141)
        at org.apache.catalina.core.StandardContext.resourcesStart(StandardContext.java:3855)
        at org.apache.catalina.core.StandardContext.start(StandardContext.java:4024)
...
}}}
   This typically means the {{{tomcat}}} user can not read {{{/usr/share/tomcat5/webapps/rhn}}}, give {{{tomcat}}} user access to directory.  (See https://fedorahosted.org/spacewalk/wiki/DevelopmentWorkstationSetup#SetPermissions)

 * If catalina.out just says the following, then look in localhost.<date>.txt:
{{{
SEVERE: Error listenerStart
SEVERE: Context [/rhn] startup failed due to previous errors
}}}
   * If localhost.<date>.txt says the following:
{{{
java.lang.ClassNotFoundException: com.mchange.v2.log.MLog
}}}
     Then you need to downgrade c3p0.
{{{
sudo rpm -e --nodeps c3p0
sudo yum install -y c3p0 --disablerepo=* --enablerepo=jpackage-generic
sudo yum versionlock c3p0
}}}
   * If localhost.<date>.txt says this instead:
{{{
java.lang.NoSuchMethodError: org.objectweb.asm.ClassWriter.<init>(I)V
}}}
     Then you need to downgrade cglib
{{{
sudo rpm -e --nodeps cglib
sudo yum install cglib --disablerepo=* --enablerepo=jpackage-generic
sudo yum versionlock cglib
}}}

 * catalina.out says the following:
{{{
ERROR org.apache.struts.action.ActionServlet - Unable to initialize Struts ActionServlet due to an unexpected exception or error thrown, so marking the servlet as unavailable.  Most likely, this is due to an incorrect or missing library dependency.
java.lang.NoClassDefFoundError: org/apache/commons/chain/config/ConfigParser
}}}
   Then you need to add commons-chain to your libs:
{{{
ln -s /usr/share/java/commons-chain.jar rhnwebapp/WEB-INF/lib/commons-chain.jar
}}}

 * browse webapp and it looks plain like a webapp from 1995, usually caused by the css files not being loaded. 
{{{
[Wed Jul 16 14:56:20 2008] [error] [client 192.168.1.106] Symbolic link not allowed or link target not accessible: /var/www/html, referer: https://bugatti.usersys.redhat.com/rhn/systems/Overview.do
}}}
  The most likely culprit is actually a permissions problem for the apache user, give {{{apache}}} user access to the directory.
  Also check that required static content served up by httpd is in the right place.  Compare /var/www/html.old with /var/www/html and look for files and directories that are in /var/www/html.old but not in /var/www/html - move these files and directories into /var/www/html.  Be sure to check the javascript and css directories within /var/www/html/ for missing files as well.

 * using '''tomcat6''' and getting following traceback:
{{{
org.apache.jasper.JasperException: Unable to read TLD "META-INF/c.tld" from JAR file "file:~/spacewalk/java/lib/taglibs-standard-1.1.0.jar": org.apache.jasper.JasperException: Failed to load or instantiate TagLibraryValidator class: org.apache.taglibs.standard.tlv.JstlCoreTLV
}}}
   Just remove jspapi-5.0.18.jar:
{{{
mv /usr/share/tomcat6/webapps/rhn/WEB-INF/lib/jspapi-5.0.18.jar /tmp/
}}}
   and restart tomcat.

 * tomcat starts too quickly (and browsing to the webUI gives a 404)

From IRC:
{{{
[20:41] <jsherrill> i think your rhn.xml disappeared
[20:41] <jsherrill> if you're using tomcat5 copy ~/spacewalk/java/conf/rhn.xml to /etc/tomcat5/Catalina/localhost/
[20:42] <jsherrill> for tomcat6 copy ~/spacewalk/java/conf/rhn6.xml to /etc/tomcat6/Catalina/localhost/
}}}


* Java crashes on startup (a hard JVM crash, not a normal traceback):
{{{
- remove ~/spacewalk/java/lib/ojdbc14-1.10.jar
- create simlink ~/spacewalk/java/lib/ojdbc14.jar that points to /usr/share/java/ojdbc14.jar

- unlink /var/lib/tomcat6/webapps/rhn/WEB-INF/lib/ojdbc14-1.10.jar
- create simlink /var/lib/tomcat6/webapps/rhn/WEB-INF/lib/ojdbc14.jar that points to /usr/share/java/ojdbc14.jar

- unlink ~/spacewalk/java/build/run-lib/external/ojdbc14-1.10.jar
}}}

* Tomcat is unable to access antlr.jar:
{{{
SEVERE: ContainerBase.addChild: start:
LifecycleException:  start: :  java.io.IOException: Failed to access resource /WEB-INF/lib/antlr.jar
        at org.apache.catalina.loader.WebappLoader.start(WebappLoader.java:679
}}}
   This is a bizarre problem (that I believe is a bug in tomcat) where tomcat has randomly decided to ignore the bit in rhn.xml where we explicitly told it to follow symlinks. It can be worked around by telling it the exact same thing in a different place and restarting the service:
{{{
cp /etc/tomcat*/Catalina/localhost/rhn.xml $git_checkout/java/rhnwebapp/META-INF/context.xml
}}}
When no rhn.xml can be found on your system, /etc/tomcat*/Catalina/localhost/rhn.xml or $git_checkout/java/rhnwebapp/META-INF/context.xml you can make one your self, the content of rhn.xml is
{{{
<Context path="/rhn" reloadable="false" allowLinking="true"> </Context>
}}}
Make sure you stop and start your spacewalk server


* Behind a proxy?

Add the following to your .bash_profile or an equivalent:
{{{
export ANT_OPTS="-Dhttp.proxyHost=your.proxy.com -Dhttp.proxyPort=3128"
}}}

* Ant fails with 'Error: Could not find or load main class org.apache.tools.ant.launch.Launcher'

Make the java 7 directories so that ant can lookup libraries properly.
{{{
sudo mkdir /usr/lib/java-1.7.0 /usr/share/java-1.7.0
}}}

* Accessing the webui returns HTTP/404 and shows the following exception:

{{{
[TP-Processor3] ERROR com.redhat.rhn.frontend.servlets.SessionFilter - Error during transaction. Rolling back
org.apache.jasper.JasperException: java.lang.ClassCastException: org.apache.catalina.util.DefaultAnnotationProcessor cannot be cast to org.apache.AnnotationProcessor
        at org.apache.jasper.servlet.JspServletWrapper.getServlet(JspServletWrapper.java:156)
        at org.apache.jasper.servlet.JspServletWrapper.service(JspServletWrapper.java:329)
        at org.apache.jasper.servlet.JspServlet.serviceJspFile(JspServlet.java:313)
        at org.apache.jasper.servlet.JspServlet.service(JspServlet.java:260)
        at javax.servlet.http.HttpServlet.service(HttpServlet.java:717)
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:290)
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
        at org.apache.catalina.core.ApplicationDispatcher.invoke(ApplicationDispatcher.java:646)
        at org.apache.catalina.core.ApplicationDispatcher.processRequest(ApplicationDispatcher.java:436)
        at org.apache.catalina.core.ApplicationDispatcher.doForward(ApplicationDispatcher.java:374)
        at org.apache.catalina.core.ApplicationDispatcher.forward(ApplicationDispatcher.java:302)
        at org.apache.struts.action.RequestProcessor.doForward(RequestProcessor.java:1078)
        at org.apache.struts.action.RequestProcessor.processForwardConfig(RequestProcessor.java:396)
        at org.apache.struts.action.RequestProcessor.process(RequestProcessor.java:232)
        at com.redhat.rhn.frontend.struts.RhnRequestProcessor.process(RhnRequestProcessor.java:102)
...
}}}

To resolve this issue, make sure your ''/etc/tomcat[567]/Catalina/localhost/rhn.xml'' contains the following:
{{{
<Context path="/rhn" reloadable="false" allowLinking="true">
    <Loader delegate="true"/>
</Context>
}}}

* Httpd fails to start with error in /var/log/httpd/error_log: {{{Can't locate Digest/SHA.pm in @INC}}}
  * {{{sudo yum install perl-Digest-SHA}}}

== Still having issues? ==

Make sure there are no broken symlinks in /var/lib/tomcat5/common/lib:
{{{
  ls -l /var/lib/tomcat5/common/lib/
}}}

Also make sure your symlink is setup correctly for the webapp

{{{
ll /var/lib/tomcat5/webapps/rhn
lrwxrwxrwx 1 root root 26 Feb 18  2009 /var/lib/tomcat5/webapps/rhn -> /home/USER/spacewalk/java/rhnwebapp/
}}}

On Fedora 14 (and maybe previous Fedora versions) if the git repo is in a users home directory, the followin maybe neccessary if Apache reports "DocumentRoot must be a directory".

{{{
sudo setsebool -P httpd_enable_homedirs 1
sudo setsebool -P httpd_read_user_content 1
}}}

= Building spacewalk-java RPM =

* See here for info on rebuilding the spacewalk-java RPM: BuildingJavaRpm

= Ant problems? =

{{{
[coec@sw-pg java]$ ant
Buildfile: build.xml
     [echo] Importing buildconf/build-props-${tomcat}.xml

BUILD FAILED
/home/coec/git/spacewalk/java/build.xml:8: Cannot find buildconf/build-props-${tomcat}.xml imported from /home/coec/git/spacewalk/java/build.xml

Total time: 0 seconds
[coec@sw-pg java]$
}}}

Try 
{{{
ant -Dtomcat=tomcat6
Buildfile: build.xml
     [echo] Importing buildconf/build-props-tomcat6.xml

boot-deps:
    [mkdir] Created dir: /home/coec/git/spacewalk/java/build/boot-lib
     [echo] Symlinking ivy

init-ivy:
...
}}}


= Debugging code running in tomcat with eclipse = 

If you're running java code on your dev workstation and want to trace through it, you can easily do that with eclipse.

1.  enable debugging within tomcat5:
   edit /etc/tomcat5/tomcat5.conf  and look for:
{{{
  # Turn on debugging
  #JAVA_OPTS="$JAVA_OPTS -Xdebug -Xrunjdwp:transport=dt_socket,address=8000,server=y,suspend=n"
}}}

  simply uncomment the 2nd line.

  For tomcat6 according to [http://stackoverflow.com/questions/1096642/tomcat-failed-to-shutdown stackoverflow] it's recommended to use:

{{{
   CATALINA_OPTS="-Xdebug -Xrunjdwp:transport=dt_socket,address=8000,server=y,suspend=n"
}}}
  Put the line either to /etc/tomcat6/tomcat6.conf or to /etc/sysconfig/tomcat6 .

2.  restart tomcat 
 service tomcat[56] restart

3.  add a debugging profile in eclipse:
  a.  run -> Debug configurations   (or click the little arrow beside the bug in the toolbar and click 'debug configurations')
  b.  In the left hand list, find and double click on "Remote Java Application".  This will create a new 'profile'
  c.  Give it a name, change "Host" to whatever host you want to connect to (Maybe localhost), and make sure "port" is set to 8000
  d.  Hit apply and then close

4.  Initiate the debugger:
     Click on the little arrow beside the 'bug' in the toolbar and select the name that you gave the debug profile in the previous step.  If you open the debug perspective (Window -> Open Perspective -> Debug)  you should see a debug tab showing you connected to tomcat.

5.  Set break points.  
   Simply add breakpoints by double clicking on the left grey strip running from top to bottom beside the code.

6.  Run some code.  Simple browse the webUI or do whatever for it to run over that section of the code, and the debugger will activate.

= Debugging code running in taskomatic with eclipse =

1. edit your /usr/share/rhn/config-defaults/rhn_taskomatic_daemon.conf and add following lines:

{{{
wrapper.java.additional.3=-Xdebug
wrapper.java.additional.4=-Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n

wrapper.java.detect_debug_jvm=TRUE
}}}

2. restart taskomatic and follow the tomcat debugging procedure with one change: connect port is 8001