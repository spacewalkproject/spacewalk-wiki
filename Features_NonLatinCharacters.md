http://www.icann.org/en/announcements/announcement-30oct09-en.htm

From article:
"The coming introduction of non-Latin characters represents the biggest technical change to the Internet since it was created four decades ago," said ICANN chairman Peter Dengate Thrush. "Right now Internet address endings are limited to Latin characters – A to Z. But the Fast Track Process is the first step in bringing the 100,000 characters of the languages of the world online for domain names."

Do we know what that means for Spacewalk? Will handle it. Can somebody test it?

According to my information Fedora 12 should be able to handle such domains. At least bind will support it. Older Fedoras and RHELs will probably not support it. 

 * server Hostname uses this, thus what you type in URL
  * will tomcat, apache etc in F12 support them?
  * will our java, perl and python stacks be nice to the request or filter out the chars and end up with a broken url
 * kickstarts use urls
 * Proxy server hostname - do things work
 * client tools communicating to Satellite - rhnpush, config etc
 * rhel/fedora client tools - yum-rhn-plugin, rhn_check

Links:
 * IANA Guidelines for the Implementation of Internationalized Domain Names
   http://www.icann.org/en/general/idn-guidelines-20jun03.htm
 * IDN on Wikipedia
   http://en.wikipedia.org/wiki/Internationalized_domain_name
 * RFC
   http://tools.ietf.org/html/rfc3490

Example:

IDN name: Bücher.ch
Pune, i.e real hostname: xn--bcher-kva.ch

IDN name: 
موقع.وزارة-الاتصالات.مصر
Pune: xn--4gbrim.xn----ymcbaaajlc6dj7bxne2c.xn--wgbh1c

You can use 
http://idna-converter.com
for testing purposes.
## Tracking bugzilla



https://bugzilla.redhat.com/show_bug.cgi?id=683200
## Code changes

### Perl




We can use module Net::LibIDN, which is available as package perl-Net-LibIDN in Fedora, RHEL6 and EPEL5.

We need to use this code before every connect.


     $ cat example
     use Net::LibIDN;
     print Net::LibIDN::idn_to_ascii("müller.org"), "\n";
     print Net::LibIDN::idn_to_ascii("ascii.org"), "\n";
     $ perl p
     xn--m14ller-xwa2143e.org
     ascii.org
I.e. for classic url, this operation is idempotent, for IDN url, it give you punny encoded url.

For presentetaion layer (PXT), we will need to use something like:

     use Encode;
     print decode('utf-8', Net::LibIDN::idn_to_unicode("xn--m14ller-xwa2143e.org", 'utf-8')), "\n";
     print Net::LibIDN::idn_to_unicode("ascii.org"), "\n";
Again for classic url, this is idempotent, for IDN we will get Unicode (!) representation of URL.
### Java



There exist [library java.net.IDN](http://download.oracle.com/javase/6/docs/api/java/net/IDN.html) directly in java, which contains IDN bindings. Usage is similar as in Perl example above.
### Python



We can use module [encodings.idna](http://docs.python.org/release/2.3.2/lib/module-encodings.idna.html).


I verified for Python, that you really need to do the transformation. 
If you run:

     import socket
     # -*- coding: utf-8 -*-
     server_name='bücher.ch'
     sock.connect((server_name, 80))
You will get:

     socket.gaierror: [Errno -2] Name or service not known
While running

    import encodings.idna
    # -*- coding: utf-8 -*-
    server_name=u'bücher.ch'
    server_name=encodings.idna.ToASCII(server_name)
    sock.connect((server_name, 80))
Will run without problem.
Beware that you must pass instance of unicode object to ToASCII(), otherwise you will get Traceback.
## Requirements



Client tools need to make sure that all host names are in Pune encoding before sending to rhnParent.
Client tools should accept non latin domains as entry (eg. serverURL) and encode it to Pune, before connect() or before it is send to rhnParent.

We will store host names in DB in Pune encoding.

We will have to go through WebUI and anytime we present hostname (SDC, device info, monitoring...) we will decode Pune string to UTF-8.

We will modify JavaDoc for API calls and whenever hostname is used, we will note that we accept/return hostname in Pune encoding.


