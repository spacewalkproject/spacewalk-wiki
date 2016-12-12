= Debugging Oracle Call Interface driver issues =

If you see any issues using the new OCI driver, there are a few things you need to do:

Edit /etc/rhn/rhn.conf and change:
{{{
hibernate.connection.driver_proto=jdbc:oracle:oci
}}}
to
{{{
hibernate.connection.driver_proto=jdbc:oracle:thin
}}}


If you are using the logdriver (Development only) then set it to Normal users ignore this:
{{{
hibernate.connection.driver_proto=jdbc:log:oracle.jdbc.driver.OracleDriver:oracle:thin
}}}




Edit `/usr/share/rhn/search/classes/com/redhat/satellite/search/db/config.xml` and change:
{{{
        <property name="JDBC.ConnectionURL" value="${search.connection.driver_proto}:@${db_name}"/>
}}}
to
{{{
        <property name="JDBC.ConnectionURL" value="${search.connection.driver_proto}:@${db_host}:${db_port}:${db_name}"/>
}}}


Finally restart the Spacewalk services.