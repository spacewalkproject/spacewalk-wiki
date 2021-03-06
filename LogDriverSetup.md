If you want to see the SQL output that Tomcat or Taskomatic generates for any given page or unit of work you can utilize a special logging JDBC driver that will dump all your SQL to the catalina.out log as well as the params used in each call.
## Steps to Enable



1.  Verify logdriver RPM installed.  install if not:


    # rpm -q logdriver 
    logdriver-0.1-2
    ## Otherwise install
    # yum install logdriver

2.  Edit /etc/rhn/rhn.conf:

Replace: 


    hibernate.connection.url=jdbc:oracle:thin:@localhost:1521:xe

With:


    hibernate.connection.url=jdbc:log:oracle.jdbc.driver.OracleDriver:oracle:thin:@localhost:1521:xe

And add:


    hibernate.connection.driver_class=net.rkbloom.logdriver.LogDriver


3. edit: /var/lib/tomcat5/webapps/rhn/WEB-INF/classes/log4j.properties  

Note:  If you are using a development environment

       cp  /var/lib/tomcat5/webapps/rhn/WEB-INF/classes/log4j.properties ~/.log4j.properties
       edit ~/.log4j.properties 

Uncomment or add:


    log4j.logger.net.rkbloom.logdriver.LogPreparedStatement=DEBUG
    log4j.logger.net.rkbloom.logdriver.LogStatement=DEBUG
    log4j.logger.net.rkbloom.logdriver.LogCallableStatement=DEBUG

4.  restart tomcat:


    /sbin/service tomcat5 restart

5.  watch log file:


    tail -f /var/log/tomcat5/catalina.out


6.  You should start to see output:


    2008-12-01 14:36:53,432 [TP-Processor3] DEBUG net.rkbloom.logdriver.LogPreparedStatement - executing PreparedStatement: 'select pxt_id_seq.nextval from dual' with bind parameters: {}
    2008-12-01 14:36:53,516 [TP-Processor3] DEBUG net.rkbloom.logdriver.LogPreparedStatement - executing PreparedStatement: 'select roleimpl0_.id as id172_, roleimpl0_.name as name172_, roleimpl0_.label as label172_, roleimpl0_.created as created172_, roleimpl0_.modified as modified172_ from RHNUSERGROUPTYPE roleimpl0_ where roleimpl0_.label=?' with bind parameters: {1=org_admin}
    2008-12-01 14:36:53,528 [TP-Processor3] DEBUG net.rkbloom.logdriver.LogPreparedStatement - executing PreparedStatement: 'select roleimpl0_.id as id172_, roleimpl0_.name as name172_, roleimpl0_.label as label172_, roleimpl0_.created as created172_, roleimpl0_.modified as modified172_ from RHNUSERGROUPTYPE roleimpl0_ where roleimpl0_.label=?' with bind parameters: {1=satellite_admin}
    2008-12-01 14:36:53,530 [TP-Processor3] DEBUG net.rkbloom.logdriver.LogPreparedStatement - executing PreparedStatement: 'select roleimpl0_.id as id172_, roleimpl0_.name as name172_, roleimpl0_.label as label172_, roleimpl0_.created as created172_, roleimpl0_.modified as modified172_ from RHNUSERGROUPTYPE roleimpl0_ where roleimpl0_.label=?' with bind parameters: {1=org_applicant}
    2008-12-01 14:36:53,531 [TP-Processor3] DEBUG net.rkbloom.logdriver.LogPreparedStatement - executing PreparedStatement: 'select roleimpl0_.id as id172_, roleimpl0_.name as name172_, roleimpl0_.label as label172_, roleimpl0_.created as created172_, roleimpl0_.modified as modified172_ from RHNUSERGROUPTYPE roleimpl0_ where roleimpl0_.label=?' with bind parameters: {1=channel_admin}

The {1=channel_admin} is the params in order of ?s in the query.
