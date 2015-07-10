{{{
#!div class="important" style="border: 2pt solid; text-align: center" 
'''''DEPRECATED, NO LONGER USED''''' 
}}}

Seeing some trouble with repo generation for a particular channel?

{{{
1.  rm -rf /var/cache/rhn/repodata/CHANNEL_LABEL/*
2.  sqlplus spacewalk/spacewalk@xe
3.  sql> delete from rhnRepoRegenQueue
4.  sql> commit;
5.  sql> quit
6.  Then run 'yum clean all && yum list' on a client that is connected to that channel
7.  Monitor /var/cache/rhn/repodata/CHANNEL_LABEL/ for files being created. 
}}}

Still not working?   Make sure taskomatic is running:

/etc/init.d/taskomatic restart


and look for error messages in /var/log/rhn/rhn_taskomatic_daemon.log


Are you seeing the following in the log file:

{{{
| com.redhat.rhn.taskomatic.core.TaskomaticDaemon - Tomcat is not up yet, sleeping 4   │ mccun934
| seconds
}}}

Most likely something is interfering with the apache configs:
{{{
# grep VirtualHost  /etc/httpd/conf.d/*
}}}

Look for any <VirtualHost *:80> or <VirtualHost 192.168.0.1:80>  entries   (Where 192.168.0.1 is your ip address)
