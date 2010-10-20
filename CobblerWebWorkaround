There is a manual workaround to be able to install cobbler-web on a Spacewalk server.

This has been tested with Spacewalk 1.1, cobbler-2.0.3.1-3.el5, and cobbler-web-2.0.3.1-3.el5.

Install cobbler-web:
{{{
yum install cobbler-web
}}}

Edit /etc/httpd/conf.d/cobbler_web.conf and delete both the following lines:
{{{
Line 4: <VirtualHost *:80>
Line 26: </VirtualHost>
}}}

Edit /etc/cobbler/settings and go to line 232.  Read the complete warning.  If you still wish to let cobbler authenticate through Spacewalk, change redhat_management_permissive on line 244 to 1.