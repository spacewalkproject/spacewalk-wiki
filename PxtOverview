{{{
#!div class="important" style="border: 2pt solid; text-align: center" 
'''''DEPRECATED, NO LONGER USED''''' 
}}}

= Introduction =
This page is being included for historical purposes, so that folks can
better understand PXT. It was written on 12 Mar 2001 by Chip Turner.
It explains the thought process behind writing PXT.

= PXT Overview an ASP/Mason Alternative =
by Chip Turner
12 Mar 2001

Here's something I've been working on as an alternative technology for
component based web page rendering.  We could use it for RHN if we
choose.  This should summarize usage from a high level perspective.

== Pseudo-XML Templates for Apache ==

Here is a rough description of a system I've used in the past that may
be useful in RHN.

The basic idea is to embed XML in HTML in such a way that the XML/HTML
mixture is served to a client as pure HTML.  This translation occurs
at the time of a page request.

A typical file would look largely like a normal HTML document, except
for the addition of a handful of tags.  These tags can be standalone
(such as {{{<IMG />}}}) or can be block tags (suck as {{{<DIV> ... </DIV>}}}).
Below is a simple example of a form which uses code to insert default
values but HTML (presumptively written by an HTML designer):

{{{
#!text/html
<pxt-include App="RHN::User::EditTags" />

<pxt-errors>
<pxt-rhn:user:loginform action="/network/main.html">

<input type=text name="username" value=[[pxt-username]]>
<input type=password name="password" value=[[pxt-password]]>
[[pxt-hidden]]

</pxt-rhn:user:editform>
}}}

The above would be rendered in such a way as to become a normal form
for the browser, but with the appropriate values in place that a user
would expect (for instance, {{{<pxt-errors>}}} would be where any errors
would be placed, such as "Invalid username or password"; likewise,
{{{[[pxt-username]]}}} would be where a user's login would be placed if it
was known, perhaps from a failed login).  The {{{<pxt-rhn:user:loginform>}}}
would render everything between itself and its matching close tag as
if it were a normal {{{<FORM>}}} tag.

The general convention is that a tag {{{<pxt-foo>}}} corresponds to a
specific subroutine in code somewhere, and a label {{{[[pxt-bar]]}}} is a
substitution handled by an enclosing tag (in this case,
{{{[[pxt-username]]}}} is expanded by the {{{pxt-rhn:user:loginform}}} tag).

The first line of the example, {{{<pxt-include>}}}, indicated which sets of
tags will be used in this html document.  More can be used by
indicating other {{{pxt-include}}} tags.

Another example; suppose an html document needed to display a list of
packages.

{{{
#!text/html
<pxt-include "RHN::Packages" />

<TABLE BORDER=1>
<TR><TH>Package Name</TH><TH>Size</TH><TH>Download!</TH></TR>

<pxt-rhn:packages:view_all pagesize=20>
<TR><TD>[[name]]</TD><TD>[[size]]</TD><TD>[[download_link]]</TD></TR>
</pxt-rhn:packages:view_all>

</TABLE>
}}}

In this case, it is easy to change the way a list of packages is
displayed (even so far as changing which fields are displayed) without
involving any code change, assuming the tag handler for
{{{pxt-rhn:packages:view_all}}} is written to allow such (and in my
experience with this system, it typically is _easier_ to write tag
handlers to handle that kind of thing than it is to write tag handlers
that are specific and not as flexible).

Now, from the perl side.  The module referenced in {{{pxt-include}}} is
loaded into memory via translation of :: into / and appending a .pm to
the module name (ie, {{{RHN::Packages}}} becomes {{{RHN/Packages.pm}}}, much
like
the standard 'use' operation).

Once a module is loaded, the code then calls {{{$package->register_tags}}}.
In the case above, it calls {{{RHN::Packages->register_tags}}}.  This allows
a module to define a function (named {{{register_tags}}}) that declares what
XML tags it can expand.  This approach allows for ISA inheritance and
requires an upfront declaration of capabilities, instead of relying on
allowing arbitrary tags to be expanded with no registration.  This
wins both speed and clarity when it comes to writing/reading modules.
A similar class method, register_callbacks, does something similar to
the current callback system, except it, too, requires registration
(instead of arbitrary form variables ending in _cb).  Here is a sample
{{{register_tags}}} function:

{{{
#!perl
sub register_tags {
  my $class = shift;
  my $pxt = shift;     # the interface to the core that allows you to
                       # register tags, callbacks, etc

  # let any parent class defined in our ISA register its tags first
  $_->register_tags foreach @ISA;

  # now we register our own
  $pxt->register_tag('rhn:user:loginform' => \&login_form);
  $pxt->register_tag('rhn:user:display_alerts => \&display_alerts);
}
}}}
Fairly simple.  The ISA bit is optional, but it results in a very
object oriented flavor in overriding parent tags and extending base
functionality.  The {{{$pxt->register_tag}}} method takes two parameters --
tag name and a reference to a subroutine that will be called when the
tag is encountered.  

Here is a very simple tag handler:

{{{
#!perl
sub first_tag {
  my $pxt = shift;   # the same pxt object; it is the way we talk to 
                     # Apache and PXT

  my $ret = "This is replacing the tag.<br>";

  # do we have a formvar named test?  if so, we add more to our
  # returned results
  $ret .= "I even have a formvar for test: " . $pxt->param("test") . "<br>"
    if $pxt->param("test");

  $pxt->session->set("first_tag_data" => "something to remember in the
session");

  return $ret;
}
}}}

Pretty much everything is as expected; the same {{{$pxt}}} object (an obj of
class {{{PXT::Request}}}) is how we retrieve formvars as well as how we
access the user's session data (typical key/value access; any scalar
may be associated with the key, which is the first parameter).

One departure of the above from {{{Apache::ASP}}} is the use of full fledged
methods for accessing form variables and session data.  This costs
almost nothing in terms of performance (indeed, session data can be
updated in a "smart" way as opposed to if we had to send an entire
hash back and forth each time) but increases readability and unifies
an interface.  One problem that is all over the current code base is
the passing of large hashes with large numbers of keys back and forth
to functions and methods; this destroys readability and
maintainability, especially as those hashes are often (key, value)
pairs that are column names and column values for a record in a table
somewhere.  This would be the beginning of decoupling the idea of
tossing large hashes back and forth instead of making functions
properly filter inputs and outputs.

Right now, some of the above actually exists, including SQL based
sessions.  Enough for a proof of concept, anyway, should we wish to
persue it further.

Upsides:
 * Clear separation of code and data
 * Fast (can be handled much faster than systems which allow arbitrary
   perl code)
 * Easy for designers to integrate into new/existing HTML
 * Prevents "quick fixes" via sticking perl into html that ASP and 
   Mason would allow; in other words, it somewhat forces separation
   of code and display, and forces display functionality to be
   properly divided among various .pm modules, separate both from
   specific HTML presentation and DBI implementation
 * A handler can serve XML-RPC by simply creating the appropriate
   output in XML instead of HTML (should we need to provide that 
   interface)

Downsides:
 * Would require further development ourselves
 * My view is myopic since I designed the system, so I am not
   impartial in evaluating its merits
 * Would be different than our existing {{{Apache::ASP}}} infrastructure
   (though this will pretty much have to change anyway, if only in
   style)