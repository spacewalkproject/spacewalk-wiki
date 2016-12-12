= Oracle XE Web access prep =
The Oracle XE web interface is accessible on localhost (127.0.0.1) by default. If you want to access it from outside the box, there
are a number of ways to address this.

== The Oracle way  ==
The 'correct' oracle way would be to execute the following with sysdba privileges:
{{{
SQL> EXEC DBMS_XDB.SETLISTENERLOCALACCESS(FALSE);
}}}

If you messed up the APEX port in configure, you can use the following command to 'fix' it:
{{{
SQL> EXEC DBMS_XDB.SETHTTPPORT(9000); 
}}}

== Change listener configuration ==
{{{
    (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=0.0.0.0)(PORT=9000))(Presentation=HTTP)(Session=RAW))
}}}

Do not forget to restart the listener.
Example of a modified {{{/usr/lib/oracle/xe/app/oracle/product/10.2.0/server/network/admin/listener.ora}}}:

{{{
# listener.ora Network Configuration File:

SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (SID_NAME = PLSExtProc)
      (ORACLE_HOME = /usr/lib/oracle/xe/app/oracle/product/10.2.0/server)
      (PROGRAM = extproc)
    )
  )

LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC_FOR_XE))
      (ADDRESS = (PROTOCOL = TCP)(HOST = spacewalk-server.example.com)(PORT = 1521))
    )
    (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=spacewalk-server.example.com)(PORT=9000))(Presentation=HTTP)(Session=RAW))
  )

DEFAULT_SERVICE_LISTENER = (XE)
}}}

== ssh forwarding trick ==
You can also forward http://127.0.0.1:9000/apex via ssh tunnel:
  {{{ssh -L 9000:localhost:9000 -l root -N <oracle machine name>}}}. This should allow you to browse to {{{localhost}}}.


