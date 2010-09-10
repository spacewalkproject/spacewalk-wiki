This is about adding any random HTML piece to the Satellite front (logon) page 

Edit the file /var/lib/tomcat5/webapps/rhn/WEB-INF/pages/common/login.jsp and put in any changes you like. However, because the jsp's are compiled, your changes are not seen until you also do the following:

Comment out the following 2 lines in this file /var/lib/tomcat5/webapps/rhn/WEB-INF/web.xml (for a devel satellite, eng/java/code/webapp/WEB-INF/web.xml)
{{{
    <servlet-mapping>
        <servlet-name>org.apache.jsp.WEB_002dINF.pages.common.login_jsp</servlet-name>
        <url-pattern>/WEB-INF/pages/common/login.jsp</url-pattern>
    </servlet-mapping>
}}}
and
{{{
    <servlet>
        <servlet-name>org.apache.jsp.WEB_002dINF.pages.common.login_jsp</servlet-name>
        <servlet-class>org.apache.jsp.WEB_002dINF.pages.common.login_jsp</servlet-class>
    </servlet>
}}}



The following shouldn't be needed since development mode is turned on by default, but if the above doesn't work, try this:

Then apply this patch to /etc/tomcat5/web.xml
{{{
  --- web.xml     2007-06-12 16:55:38.000000000 -0400
  +++ web.xml.work        2007-06-12 16:55:25.000000000 -0400
  @@ -194,6 +194,10 @@
               <param-name>xpoweredBy</param-name>
               <param-value>false</param-value>
           </init-param>
  +        <init-param>
  +            <param-name>development</param-name>
  +            <param-value>true</param-value>
  +        </init-param>
           <load-on-startup>3</load-on-startup>
       </servlet>
}}}