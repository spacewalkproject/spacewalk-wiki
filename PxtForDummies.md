
# **DEPRECATED, NO LONGER USED**

# PXT For Dummies (plus a little extra)

by Bret McMillan

## Section 1:  Core PXT

### I. What, who, and why?




PXT stands for Pseudo-XML Templates.  Essentially, you embed XML tags
within HTML and have those tags replaced dynamically upon each
request.  This is similar to, say, defining your own custom tag
library in JSP.
 
PXT is the brain-child of Chip Turner, one of the
members of the Red Hat Network web team.  Other members of the team
included Robin Norwood, Greg DeKoenigsberg, and myself.

The RHN web team uses PXT for three main reasons:

 1. Separation of church and state: Unlike JSP, PXT does not allow you
    to embed actual code within the dynamic HTML documents.  This forces
    the webpages to be legible, and therefore more maintainable.  All the
    heavy lifting for dynamic code is relegated to Perl modules.  Trust
    us--this is a !VeryGoodThing(tm.)

 2. It's lightweight.  RHN once had speed issues, largely due to
    Apache::ASP overhead, and some decisions made in the usage of
    Apache::ASP.  It no longer does, partly because of PXT.  A simplified
    XML parser allows for fast parsing, resulting in little overhead spent
    around a request.

 3. Easy (read: near trivial) integration of XMLRPC, SOAP, and general
    XML calls.  This allows a page that serves HTML to also service those
    types of calls.  This means creating a command line interface for
    clients or other forms of automation will be much easier, as the code
    to handle both the HTML display and the XML response can be shared.
    This simplifies interfaces and increases code reuse.
### II.  Sample .pxt file



Here's a simple PXT file (foo.pxt):

    #!text/html
        <pxt-use module="ColorSniglet" />   
        <html>
          <my_tag>
                <body bgcolor={bgcolor}>
              Hello world.<br>
              The background color is {bgcolor}.
              The posted color is 
                    <pxt-formvar>{formvar:posted_color}</pxt-formvar>.
                </body>
          </my_tag>
        </html>
### III.  Legal <pxt-XXX/> Directives

#### 1. `<pxt-use module="Foo"/>` (synonymous with `<pxt-use class="Foo"/>`)




This basically is registering a perl module that, for the rest of the
document, will be either watching for tags to replace (like
<body_tag>), awaiting callbacks, or awaiting XML-RPC/SOAP calls.  For
our example, let's say that the HTML output after foo.pxt processes
is:

    #!text/html
        <html>
          <body bgcolor=red>
            Hello world.
            The background color is red.
            The posted color is blue.
          </body>
        </html>

The perl module (known as a "Sniglet" by the RHN Web Team) registered
the <body_tag> tag in the following subroutine (in ColorSniglet.pm):


    #!perl
        sub register_tags {
          my $class = shift;
          my $pxt = shift;
    
          $pxt->register_tag("my_tag" => \&my_tag);
        }

The \&my_tag is a reference to a function.  The function name does
not have to match the tag, though adopting that convention improves
maintainability.  Here is the source for the body_tag subroutine:


    #!perl
        sub my_tag {
          my $pxt = shift;
          my %params = @_;
          
          # equivalent to $pxt->block, this returns everything between
          # <my_tag> and </my_tag>, in this case:
          #
          # Hello world.<br>
          # The background color is {bgcolor}.
          # The posted color is {formvar:posted_color}.
          #   
          my $ret = $params{__block__};
    
              # subtag substitution is most easily done through a simple s///
              # reg-exp expression.  
          my $ret =~ s/\{bgcolor}\}/red/gism;
    
          return $ret;
        }

`$ret` is going to be the replacement value for everything between the 
`<my_tag></my_tag>` tags, inclusive.  Note that as a point of style,
no HTML exists in the perl module.  Use subtags (things within {}'s)
appropriately so that the separation between presentation and data
remains clear.

If two Perl modules register the same tag, the first one registered
"wins" and determines what handles the tag.  It is either a feature or
a bug that PXT doesn't warn about such clashes; we haven't decided
yet.  In general, do not rely on this feature.  If you have two of the
same tags registered, be sure they both reference the same function.
#### 2. `<pxt-include file="Foo.pxt"/>`  or  `<pxt-include file="Foo.html"/>`



`<pxt-include/>` reads the contents of a file and dumps it into the
current document.  Any `<pxt-XXXX/>` tags within the included document
will be parsed, regardless of its extension.  Note that all files are 
relative to the document root of the current virtual host.  Also note 
that parsing is re-entrant and recursive, meaning tags registered are 
not necessarily registered when the `pxt-include` tag is expanded.
#### 3. `<pxt-formvar>{formvar:foobar}</pxt-formvar>`



In the url `http://www.foo.com/index.pxt?foobar=1` {{{"<pxt-formvar>
{formvar:foobar}</pxt-formvar>"}}} is automatically replaced with "1" by
PXT.
#### 4. `<pxt-comment> blah blah blah </pxt-comment>`



Your basic comment.  Anything begin the start and end tags will be utterly
ignored by PXT, including valid PXT tags.  This is different from an
HTML comment which, while not affecting the rendering of a page, is
still visible in the source of the page.  pxt-comments are filtered
out before serving.
### IV. Tag and Callback Handlers



Typical form interaction on the web usually falls into a simple
pattern: display form, process results, display results.  PXT
accommodates this model through the use of tags and callbacks.

As discussed earlier, tags can create arbitrary HTML output.  This
obviously includes form elements, such as <INPUT TYPE=TEXT> and so
forth.  So the first stage, displaying a form, is typically handled in
this way.  Whether your PXT tag generates all of the necessary form
elements, or whether most are in the HTML itself is up to the
designer.  Typically, though, it is best to let an HTML designer make
the form to their satisfaction, and then create a tag that renders the
form itself and inserts appropriate values.  For instance:


    #!text/html
      <FORM METHOD="POST">
        Username: <INPUT TYPE="TEXT" NAME="USERNAME" VALUE=""><BR>
        Password: <INPUT TYPE="PASSWORD" NAME="PASSWORD" VALUE=""><BR>
      </FORM>

would become:


    #!text/html
      <pxt-use class="Sniglets::Users">
      <rhn:login_form METHOD="POST">
        {login:error_message}
        Username: <INPUT TYPE="TEXT" NAME="USERNAME" VALUE="{formvar:USERNAME}"><BR>
        Password: <INPUT TYPE="PASSWORD" NAME="PASSWORD" VALUE=""><BR>
        {login:login_hidden_formvars}
      </rhn:login_form>

The code to render the above would essentially return the block it is
handed wrapped inside of a true <FORM> tag.  Note the use of
{formvar:USERNAME} to insert the value of a form variable named
USERNAME, should it exist.

Also note the use of {login:error_message} -- this is where you might
display something to the effect of "Invalid username or password" so
that the user has visual feedback as to why they didn't log in.  Such
a situation is where the formvar:USERNAME substitution would make
sense as well, so that the user need not retype their username.
(Note: there is actually core functionality for generally handling
displaying messages to the user like the above using a system of
message queues.  This is necessary when you wish to display a message
outside of a block inside of the tag handler that might generate the
tag).

That's all well and good, but what happens when the user hits the
submit button?  The next phase is called a trap or callback, which
corresponds to the "process results" stage.  This is where you would
do things like: verify username and password; update a database
record; load more database records to display.  In other words, this
is the place you put your code that handles various input form
variables and decides what to do with them.  In the above example, you
would use the USERNAME and PASSWORD form variables to either log a
user in or present them with an error.

A callback is similar to a tag, in that the first parameter it
receives is the ubiquitous $pxt object.  However, it receives no other
parameters, because it isn't declared as an XML tag (in other words,
where a tag handler would receive tag attributes, the callback handler
receives nothing).

Typically, a callback will verify data (though often it isn't capable
of doing so meaningfully without tying unnecessary business rules at
this level).  It will then either shove it in a database, update a
given record, or perhaps register a user as logged in.  It could also
perform some kind of search, or any number of things.  Once it is
done, the callback can either simply return, which then tells PXT to
go ahead and render the page loaded, or it can issue a redirect, so
that the user is sent to a different page (for instance, the page that
shows that you have successfully logged in, or the page that will
display search results).  Use of redirection depends largely on the
ACTION= attribute of the form itself.  Typically, leaving ACTION=
blank and issuing a redirect to wherever a request should go is what
seems to work well, but there is no clear best practice in this case.

PXT itself decides whether or not to execute a callback based on a
simple test.  If there is a form variable named pxt-trap, and if a .pxt
file has registered a callback whose name is the contents of that form
variable, then it executes it.  Otherwise it completely ignores it.
That means IT IS VERY IMPORTANT TO HAVE A FORM'S ACTION HANDLER POINT
TO A .PXT THAT REGISTERS THE CALLBACK YOU WISH TO USE.  Otherwise, it
will silently ignore you.  This is typically why I choose to have
ACTION= point back to the same page.  Also note that having one
Sniglet point to a callback in another is bad practice, in that it
creates possibly tangled dependencies.  It should be avoided when
possible, and documented clearly when it is necessitated.

TODO:  note how to pull out tag attributes as params in the tag
handler

TODO:  explain tag priorities/order.  basically, you can give a tag
handler function a priority (lower #'s execute 1st, negative #'s are
legal).  
  # order of 2 means it happens later than something of order < 2...  if you 
  # don't specify order, it defaults to 0.
  # negative orders are legal.
  $pxt->register_tag('some_tag' => \&some_tag_handler, 2);
### V. PXT::Request object



Every tag, callback handler, XML-RPC handler, and SOAP handler receives
a `PXT::Request` object (in RHN code, it's usually the $pxt thingy).  This
$pxt object is the first parameter passed to the handling function.

The $pxt object is the only way a given tag handler, callback handler,
XML-RPC handler, or any other kind of handler can interact with the web
server or user.  The $pxt object is where you get information about
the user who is logged in, any session data, form variables, cookies,
pnotes, etc.  It is absolutely verboten to directly speak with the
Apache->request module in a Sniglet.  All interaction MUST go through
the $pxt object.

Since the $pxt object is the chief method of interacting with the
server or user, it has a large number of methods.  However, they can
typically be grouped together into functional sets.  For instance,
there are the methods typically used when a PXT registration begins
(register_handler, register_callback, etc).  Others typically are used
to ask Apache about the user's request: what IP address are they from,
what headers did they send, what cookies did they send, what formvars
they provided.  Others alter the current state: redirect, for
instance, pnotes, push_message.  Some are shortcuts:
prefill_form_values, message_tag_handler.  However, all revolve around
the same axiom: if you need to know something about the HTTP request,
the user, or the server, use $pxt.  It is your connection to
everything that involves HTTP and the web server.
### VI.  Redirect and Form Handling



TODO:  Explain this more fully.
### VII. Adding `<pxt-XXX/>` Directives



When the httpd/mod_perl gets a request for a .pxt file, it's sent to
the `PXT::ApacheHandler->handler` function. It sets up all utility data
objects, registers tag handlers and callbacks, does any redirect
voodoo necessary, and also initiates the actual parsing/replacement of
the tags.  Adding any `<pxt-XXX/>` directives will require full
comprehension of the flow of execution from
`PXT::ApacheHandler->handler` down.
## Section 2:  PXT <--> RHN interaction

(or, how to build a database website with PXT...)

### I.  Intro



Creating a website to talk to RHN using PXT involves a number of points of
understanding.  You need to first understand how PXT interact with sniglets,
and then understand how to leverage the RHN::XXXX and RHN::DB::XXXX classes
from within your sniglets.  Then add to the list session management, and 
authentication issues.

TODO:  Clean this piece of introduction up.

Visually:


                               +-------------+
                               |  index.pxt  |
                               +-------------+
                               |   Sniglet   |
                               +-------------+
                               |    RHN::    |
                               +-------------+
                               |  RHN::DB::  |
                               +-------------+
                               |     DBI     |
                               +-------------+

Arranged in that way, it looks like a cake.  No layer of the cake can
touch (or even knows the existence of) any layer it doesn't touch.
Specifically, a Sniglet handler doesn't talk to DBI; an RHN:: object
doesn't talk to DBI; RHN:: doesn't speak HTML.  
### II.  Sniglets and PXT



Sniglets are what the RHN Web team calls the lovely little perl modules that
register tag handlers, callback handlers and the like.  Building a web 
application primarily will involve writing .pxt files containing html and 
PXT tags, as well as writing the sniglets that handle said PXT tags.

For the RedHat Network, we've created a group of classes that contain the
business logic, as well as the necessary knowledge about the database. 
The RHN::XXXX modules are the hooks to build websites upon.  Nearly every
RHN::XXXX module has a corresponding RHN::DB::XXXX module that is very
chummy with our Oracle database.

If you're going to be making a website that ties into RHN, your sniglets will
be making copious use of the RHN::XXXX modules.  Occaisionally, the RHN::XXXX
functions will return an RHN::DB::XXXX object that you can use.  More on this
later.
### III.  RHN::DB <--> RHN interaction



TODO:  Talk about "the man behind the curtain" stuff.  Basically, our reasons
for organizing things this way, as well as the automagic accessor generation,
and creation/update/commit stuff.
### V. Sessions in PXT



Included as a part of PXT is session handling, and is made available in
the following manner:


    #!perl
    $session = $pxt->session();

The $session object itself is defined in RHN::Session, not under PXT::*
The session object in PXT is created at the initialization phase of the
request. Before we delve into more detail about the methods availble for
use in a session:

A session is a place to store permanent data
A session is *not* a place to store transient data

"Why," you ask?  Because RHN::Session utilizes a database, and there are
better places than a db to store transient data. 

Session data for a visitor is stored in key/value fashion, with the 
exception of the visitors user id. To set a new session variable during 
a request:


    #!perl
        # some request
        $session = $pxt->session();
        $session->set('visits',1);

In order to access session information during a request:


    #!perl
        # another seperate request
        $session = $pxt->session();
        $visits = $session->get('visits'); 
        print "You have visited $visits pages for this session";

Notice that you do not need to check for the existence of a session,
because every user is given a session at the initialization phase. Also,
note that a users uid is retrieved directly from the following session
method (i.e. *not* from $session->get): 


    #!perl
        $session = $pxt->session();
        $userid = $session->uid;
### V.  Pnotes in PXT



TODO:  explain how they're useful; contrast to sessions
### VI.  The RHN::User Object in PXT



As a convenience you can access RHN::User via an accessor method in PXT. Note
that unlike sessions, an RHN::User object is not created during initialization
nor at any other time. A user of PXT retrieves the user object as follows:


    #!perl
    
    $user = $pxt->user();           #object returned from RHN::User
    print "no such user exists" if(!$user);

In addition, you can clear the user from pxt, if you so wish:


    #!perl
    $pxt->clear_user();
### VII. XML-RPC and PXT



- Otherwise known as - "The kitchen sink, -- how much stuff can we cram in?"

You may be wondering "Why are you talking about XML-RPC? This is a templating
solution."  And in many ways you would be right, except, that PXT changes the
'paradigm' [[sorry]] on how many XML-RPC servers work.  In the CURRENT MOD_PERL
WORLD, the httpd.conf controls what directories are "XML-RPC listeners".
These listeners correspond to an XML-RPC handler (which is nothing more than a
wrapper around Frontier::RPC2) and a server file.  There can only be one
server file per directory.  Listeners can branch out and obtain routines from
other files, but there must be a single point of entry, making a somewhat
rigid framework to work with...  (Theoretically you could create a filetype as
a handler for Frontier to handle directly, but it would be rather
non-intuitive, and not exactly the "norm"..)

PXT works a little differently.  Instead of registering directories or special
filetypes, PXT uses the exact filenames that you would be talking to via a web
browser.  

Let me say that again a little differently..

`http://localhost/foo/product_list.pxt` and via a web browser could, say
bring
up a list of products on a nice html page... (maybe some buttons to some links
to start messing with the data)...

`http://localhost/foo/product_list.pxt` called by an XML-RPC client would
have
the following methods available to them.


    product.list - returns an arrayRef of objects
    product.modify - takes an arugment, returns the returned objects affected ..

The PXT would inherit a few functions from PXT itself... All describing
methods about the functions that have been registered...


    methods.list - returns a list of documentation on the above two functions 

 1. PXT - XML-RPC Example -- "message of the day"


    #!text/html
    <!-- Begin xmltest.pxt -->
    
    <pxt-use class="Sniglets::XMLTest.pm"/>
    <display_motd>
    <p> Now here this! -> {motd} !!!
    </display_motd>
    
    <!-- END xmltest.pxt -->


    #!perl
    # === Begin Sniglets::XMLTest.pm ===
    
    use strict;
    
    package Sniglets::XMLTest;
    
    sub register_xmlrpc {
      my $class = shift;
      my $pxt = shift;
    
      $pxt->register_xmlrpc("display_motd", \&display_motd_xml);
    }
    
    sub register_tags {
      my $class = shift;
      my $pxt = shift;
    
      $pxt->register_tag("display_motd", \&display_motd);
    }
    
    sub get_motd {
      unless ( -f "/etc/motd") {
        die "motd not found!";
      }
    
      open(MOTD, "/etc/motd");
      my $motd;
      while ( <MOTD> ) {
        $motd .= $_;
      }
      close MOTD;
      return $motd;
    }
    
    sub display_motd {
      my $pxt = shift;
      my $motd;
    
      eval {
        $motd = get_motd();
      };
    
      if ( $@ ) {
        $motd = "No motd on file...";
      }
    
      my $block =  $pxt->block();
      
      $block =~ s/\{motd\}/$motd/gism;
      return $block;
    }
    
    sub display_motd_xml {
      my $pxt = shift;
      my $motd;
      eval {
        $motd = get_motd();
      };
      
      if ( $@ ) { 
        die['-1', "$@"];
      }
      return $motd;
    }
    
    1;
    
    # === END Sniglets::XMLTest.pm ===

 2. Example Explained.

This routine bases itself around getting the message of the day that resides
in /etc .. If the file does not exist, the http request simply fills in some
dummy data to let the user know whats up... Via XML-RPC though, it is returned
via a fault... Note how the call dies by an arrayRef, the first value
corresponding to the fault code, the other the fault string.

Both of these functions share the same data accessor, yet, display the data
slightly differently (one by altering the block and returning it, the other 
by just returning it to RPC2 for encapsulation)..
