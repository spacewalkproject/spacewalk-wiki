# Intro
 
The below instructions are how to set up the Spacewalk Development Environment. In this guide Fedora 27 will be used as a development workstartion, whereas IntelliJ Idea will be our IDE. Switching to other IDE is generally not a problem. 

We'll start by setting up git repository followed by building project structure including required dependencies. These depedencies might be installed locally or using different install root, if someone doesn't want to flood system with single-purpose rpms.

After completing steps in this guide, you'll be able to comfortably manage, modify and compile Java for Spacewalk. Furthermore, you'll be able to debug, quick build and hotswap code running in Tomcat remotely/locally, depending where your Spacewalk instance will be installed. 

Let's get started!


# Workstation setup

## Dependencies and Build

### Prerequisites

Following steps will require to have [IntelliJ Idea](https://www.jetbrains.com/idea/) installed and [Spacewalk git repository](https://github.com/spacewalkproject/spacewalk) cloned. To setup the sources follow the instructions in [Install Git](GitGuide) and [clone the Spacewalk repository](GitGuide).

### Preparing workspace directory

Let's define where we'd like to keep our IDE project. Create a directory and link java source files in it. **~/spacewalk** is root of our git repository and **~/workspace** will be root of IDE projects.

* `mkdir -p ~/workspace/spacewalk/`
* `ln -s ~/spacewalk/java/ ~/workspace/spacewalk/java`

### Get dependencies

In order to setup development workstation, you need to have some dependencies prepared. You can either install them locally or keep them in different install root. 

1. Create a folder where install-root will be created

     * `mkdir -p ~/workspace/spacewalk/root/`

2. Install all required dependencies to that folder from spacewalk nightly repo. Get them from **spacewalk-java.spec**
     * `dnf copr enable @spacewalkproject/nightly`
     * `cd ~/workspace/spacewalk/java`
     * `dnf builddep spacewalk-java.spec --installroot=/home/jdostal/workspace/spacewalk/root/ --releasever=27`
    
3. Set *JAVA_LIBDIR* variable in `~/.java/java.conf` to point to java libary in your install-root

     * `mkdir -p ~/.java/`
     *  Don't forget to adjust below username
     * `echo "JAVA_LIBDIR=/home/user/workspace/spacewalk/root/usr/share/java/" >> ~/.java/java.conf`

4. Link required jars and try initial compilation
   
     * `cd ~/workspace/spacewalk/java`
     * `ant init-install compile`

## IDEA Setup
### Setup IDEA to work with Java

Let's see how to configure IDEA to work with our source files. 

1. On welcome screen click "Import Project"
2. Navigate to **~/workspace/spacewalk/java**
3. Select *"Create project from existing sources"*
4. Set *"Project name"* to whatever you want, just check that the *"Project location"* is correct
5. Now you are asked to select which roots we'd like to manage. Unmark all except **~/workspace/spacewalk/java/code/src**
6. On next page, just click next
7. Next
8. Now set JDK home path to java you'd like to use. (i.e. java-1.8.0-openjdk)
9. Let it search for frameworks and check *"Web"* and *"Struts"* have been found. Keep them marked
10. Finish

### Setup IDEA to work with Taskomatic

Taskomatic code is part of spacewalk-java package, so it's the same except [Setup IDEA to debug and hot-swap Spacewalk code](setup-idea-to-debug-and-hot-swap-spacewalk-code)

### Setup IDEA to debug and hot-swap Spacewalk code

**Important:** Please make sure you have completed steps [Setup Tomcat for Java Debugging](#setup-tomcat-for-java-debugging) or [Setup Taskomatic for Java Debugging](#setup-taskomatic-for-java-debugging), depending what code you'd like to debug.

1. Navigate to **Run->Edit Configurations**
2. On the left, click green plus
3. Select *Remote*
4. Leave everything as it is, just change host and port to either Tomcat or Taskomatic (8000/8001, depending on your setup)
5. Try to attach debugger by clicking bug icon in the top right corner. 

With attached debugger, you can rebuild your project and changes will be propagated immediately to Spacewalk. To stop at certain line of code, place a breakpoint on that line and make Spacewalk go along this code - for example visit coresponding web interface page.

**Note:** Versions of java source must be the same on both server and workstation. If not debugging may not work or may show incorrect state. 

### Setup IDEA to deploy JSP

**Important:** Please complete steps from [Setup Tomcat to recompile JSP](#setup-tomcat-to-recompile-jsp), otherwise Tomcat won't trigger recompilation.

In Idea:

1. Navigate to **Tools->Deployment->Configuration**
2. Click green plus, set desired deployment name and select **SFTP** for type
3. Fill in SFTP host, Port, Root path and credentials. It may look as follows
![JSP Deployment screenshot](https://github.com/spacewalkproject/spacewalk-wiki/blob/master/images/jspdeploy.png)
4. Switch to tab Mapping, where we'll map source files to server JSPs. Please follow below image:
![JSP Deployment screenshot](https://github.com/spacewalkproject/spacewalk-wiki/blob/master/images/jspdeploy2.png)
5. Done. Now navigate to **Tools->Deployment** and check what options you've achieved. :-)

## Tomcat setup
### Setup Tomcat for Java Debugging

Do you hurry? [Enable debugging using a script](#enable-debugging-using-a-script)

In order to enable debugging code running in Tomcat, you need to add options to **/etc/sysconfig/tomcat\***

1. Add `CATALINA_OPTS="-Xdebug -Xrunjdwp:transport=dt_socket,address=8000,server=y,suspend=n` at the end of file
2. Note the *Port* value from above command, you'll need it
3. Restart Tomcat

### Setup Taskomatic for Java Debugging

Do you hurry? [Enable debugging using a script](#enable-debugging-using-a-script)

Taskomatic is running in a wrapper, which will require some additional configuration to setup debugging. Navigate to **/usr/share/rhn/config-defaults/rhn_taskomatic_daemon.conf**

1. Find lines starting with `wrapper.java.additional.`. Note the highest number that follows. (Usually 2)
2. Add following lines at the end of file and follow the numbering mentioned above (here, we'll pick 3, 4)
```
wrapper.java.additional.3=-Xdebug 
wrapper.java.additional.4=-Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n

wrapper.java.detect_debug_jvm=TRUE
```

3. Note the *Port* value from above command, you'll need it
4. Restart Taskomatic



### Enable debugging using a script

If you want to set it up as fast as possible, just check script in `spacewalk/scripts/` called `setup-debugging.sh`.

From help:
```
Simple script for setting up remote debugging for Tomcat (port 8000) and Taskomatic (port 8001)
Usage:
-u -- Username
-s -- Server
-h -- shows this help
```

Example run:

`./setup-debugging -u root -s http://localhost`

### Setup Tomcat to recompile JSP

JSPs are compiled different way than Java source so it will require to do some additional configuration. As JSPs are compiled, it's not enough to transfer files to correct location on server - this will require editing Tomcat configuration. This guide will provide information on how to deploy, compare or sync JSPs between Spacewalk server and development workstation. First, let's start with setting up Tomcat on Spacewalk.

1. Connect to Spacewalk server
2. Edit **/etc/tomcat\*/web.xml**
3. Set development to *true* in servlet section *jsp*. Set it as it looks as following
```xml
   <servlet>
        <servlet-name>jsp</servlet-name>
        <servlet-class>org.apache.jasper.servlet.JspServlet</servlet-class>
        <init-param>
            <param-name>fork</param-name>
            <param-value>false</param-value>
        </init-param>
        <init-param>
            <param-name>xpoweredBy</param-name>
            <param-value>false</param-value>
        </init-param>
        <init-param>
            <param-name>development</param-name>
            <param-value>true</param-value>
        </init-param>
        <load-on-startup>3</load-on-startup>
    </servlet>

```

4. If you want to edit **WEB-INF/pages/common/login.jsp** for example, comment out following lines in **/var/lib/tomcat\*/webapps/rhn/WEB-INF/web.xml**

```xml
 <servlet-mapping>
        <servlet-name>org.apache.jsp.WEB_002dINF.pages.common.login_jsp</servlet-name>
        <url-pattern>/WEB-INF/pages/common/login.jsp</url-pattern>
 </servlet-mapping>
```
and

```xml
 <servlet>
        <servlet-name>org.apache.jsp.WEB_002dINF.pages.common.login_jsp</servlet-name>
        <servlet-class>org.apache.jsp.WEB_002dINF.pages.common.login_jsp</servlet-class>
 </servlet>
```

5. Do this for desired JSP files, you can even comment out whole section 
6. Restart Tomcat



## Uninstallation

Simply delete your workspace folder (including possible install root for dependencies). No additional changes are required.


## Search Server Setup

If you are going to be updating the search server java code, just follow steps from [Setup IDE to work with Java](#setup-ide-to-work-with-java) and point it to **spacewalk/search-server/spacewalk-search**

# Issues

If you had any issues regarding this guide, please send an e-mail to spacewalk-list@redhat.com. Suggestions are welcomed as well. :-)

