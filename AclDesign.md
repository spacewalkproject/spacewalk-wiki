= How ACLs work =
== General ==
The acls in RHN are used to restrict access or hide elements that a user doesn't have permission to use or see.
The acls don't have to be based on the user, it could be based on other context (like hiding web page elements when a system is locked).
Though they bear the same name as acls in linux, they do not involve permission lists for the most part.

== How they work ==
=== Terminology ===
ACL::
 stands for access control list, but in this context, it refers to an expression in a mini-language that uses acl functions.[[BR]]
ACL function::
 a boolean function present in an acl handler that takes context and parameters and return a boolean.[[BR]]
Handler::
 The handler is a collection of acl functions used to separate out similar functionality (ex: monitoring acl handler)[[BR]]
Mixins::
 A list of handlers presented when using an acl. Required when not using functions from the default handler.[[BR]]
ACL Evaluator::
 A function that parses and evaluates acls. Uses acl functions provided by handlers to evaluate.[[BR]]
Parameter::
 Acl functions take parameters. They are treated as an array of strings.[[BR]]
Context::
 The context is sent to the acl function, but is not present in the acl. Usually, this consists of a map containing user and the request parameters.

=== Basic ===
There is a short list of acl handlers each containing acl functions. All acl functions use context and parameters to return a
single boolean, true for 'has access' and false for 'doesn't have access'. A {{{false}}} return for an acl usually means
that an element will be hidden or a permission error will be displayed to the user.  The currently logged in user is always
included in the context. The acl determines what the parameters contain. Most acl functions use the user and possibly something
from either the context or parameters to run a small database query.

An acl is written using a mini-language. At this time, mixins may be provided to the acl evaluator. Presence of mixins will make
available the acls in specified acl handlers. The default handler (Access) will always be used. The evaluator will take the acl given,
parse the acl with the mini-language, find and invoke acl functions, and return with a boolean.

=== Mini Language ===
acls are written using a mini-language understood by acl evaluators. The mini-language is solely a boolean expression language with
functions. A single acl may contain multiple acl functions using multiple acl handlers. The mini-language contains the following
operators:
 * not
 * and (represented as ';')
 * or

Example acl: is(satellite) or not is(sso_auth)[[BR]]
'is' is the name of an acl function in the default acl handler. That function will be called twice during the evaluation of this acl.[[BR]]
'satellite' and 'sso_auth' are parameters sent during the invocation of the acl function.[[BR]]
'or' and 'not' are boolean expression symbols

{{{
Grammar: The characters <>[]*| are used solely for expressing the grammar.[[BR]]
ACL:= <Expression>[;<Expression>]*[[BR]]
Expression:= <epsilon> | <Statement> [or <Statement>]*[[BR]]
Statement:= [not] <Function>[[BR]]
Function:= <Name>(<Params>)[[BR]]
Name:= <alphanumeric>[[BR]]
Params:= <Param>[,<Param>]*[[BR]]
Param:= <string>
}}}

For the most part, spaces are irrelevant. '''NOTE:''' OR binds more strongly than AND unlike most languages.
Also, there is no way to parenthesize expressions.[[BR]]
Mega-Example acl = not foo(bar,baz);foo(temp) or not is(satellite) or bar(foo) ; cake(cheese  ,  crumb, icing);

=== Code Location and Function ===
Acls are used in both Java and Perl. The java processing code was written directly from the perl equivalent. Therefore, it should
function exactly the same.

Java:
 * {{{com.redhat.rhn.common.security.acl}}} is the main package for most of the acl code, especially Handlers.
 * {{{com.redhat.rhn.common.security.acl.Access}}} the default acl handler
 * {{{com.redhat.rhn.common.security.acl.AclHandler}}} interface that defines the acl handler type.
 * {{{com.redhat.rhn.common.security.acl.Acl}}} main code path. object representation of an acl. The java acl evaluator. Also contains the code for registering acl handlers.
 * {{{com.redhat.rhn.common.security.acl.AclFactory}}} the way you get an Acl object.
 * {{{com.redhat.rhn.manager.acl.AclManager}}} Standard pipeline for evaluating acls from java code.
 * {{{com.redhat.rhn.frontend.nav.AclGuard}}} Used for nav rendering acl checks.
 * {{{com.redhat.rhn.frontend.struts.RhnActionMapping}}} used for allowing acl properties in struts-config action stanzas.
 * {{{com.redhat.rhn.frontend.struts.RhnRequestProcessor}}} does the evaluation for acl properties used in struts-config.
 * {{{com.redhat.rhn.frontend.taglibs.RequireTag}}} processes acl checks performed in jsps

Perl:
 * {{{PXT::ACL}}} main code path. The perl acl evaluator. Also contains the code for registering acl handlers.
 * {{{RHN::Access}}} the default acl handler.
 * {{{RHN::Access::}}} the specific acl handlers.
 * {{{Sniglets::Navi::Tree}}} and {{{Sniglets::Navi::Node}}} takes care of the nav acls.

== How to use acls ==
There are basically four different places where one might use an acl. Each place allows at least the ability to pass acl strings and mixins. The acl string is written using the mini-language described above (allows not, and, or, and functions with parameters). The mixins in java are fully qualified class names using a comma as a delimeter.

1. navigation xmls
 The mixins are described using an {{{acl_mixins}}} attribute for the parent {{{rhn-navi-tree}}} element. The same mixins are used for each acl in the nav tree.
 acls are used as an attribute named {{{acl}}} on the {{{rhn-tab}}} elements.
 A false return for a nav element means that that node can not be active and will not appear on the page. However, our nav tree
 is not evaluated as a tree, so nodes under a hidden node can still be active.
 Almost all acls used in the nav tree should also be used in the similar struts-config action stanzas.
2. struts-config
 acls and mixins are used as a {{{set-property}}} child element of the action element in {{{struts-config.xml}}}. The properties are named {{{'acls'}}} and {{{'mixins'}}}. ex:
  {{{<set-property property="acls" value="show_monitoring(); is(satellite)"/>}}}[[BR]]
  {{{<set-property property="mixins" value="com.redhat.rhn.common.security.acl.MonitoringAclHandler" />}}}

 Note: you cannot use acls in struts config unless you action uses our custom mapping class. Just add this attribute to your action element: 
 {{{className="com.redhat.rhn.frontend.struts.RhnActionMapping"}}}
 A false return for a struts-config acl will send the user to a permission error page.
 If you add an acl to the struts config, remember that the same acl should apply to all links leading to the same page, especially those in the navigation.
3. jsp
 The common use of acls withing a jsp is by using our custom require tag. It has attributes named {{{'acl'}}} and {{{'mixins'}}}. ex: 
    {{{<rhn:require acl="not config_channel_type(server_import); config_channel_has_files()"}}}[[BR]]
    {{{mixins="com.redhat.rhn.common.security.acl.ConfigAclHandler">}}}
 This tag works basically the same way as the JSTL core {{{c:if}}} tag. Anything in the body of the tag will be skipped if the acl returns with a false.
 Some of our other custom tags also accept acls to hide portions of the output. The toolbar and list tag are two examples. When these use multiple
 acls, all the acls will be evaluated with the same mixins.
4. java code
 The {{{com.redhat.rhn.manager.acl.AclManager}}} should be used to evaluate acls in java code. In java, as well as passing the mixins and acl strings,
 you can also control the context map that gets sent to the acl function. A user object will always be present in the map. The hasAcl methods
 both return booleans, so the consequence of a failed acl is up to you.

== How to implement an ACL ==
First off, you need to decide in which handler your acl function will live. Handlers are based on the features of RHN for the most part. For example, there is a {{{MonitoringAclHandler}}} for the monitoring feature and a {{{ConfigAclHandler}}} for the configuration management feature. {{{Access}}} is the default handler, but don't put functions into Access unless they are truly common. All of the available acl handlers are found in the {{{com.redhat.rhn.common.security.acl}}} package.

=== Writing Your own ACL Handler ===
First, put your acl handler class in the {{{com.redhat.rhn.common.security.acl}}} package. Second, for it to be recognized as an acl handler, you must implement {{{AclHandler}}}. Third, consider extending {{{BaseHandler}}} so you have the {{{getAsLong}}} method (as well as anything else we put there in the future). And you're done. That's it. Make sure there is always a public null constructor. Since mixins provide the fully qualified class name, there is no config entry or other such list of all the acl handlers.

=== Writing an ACL function ===
The most important thing to remember for writing the ACL function is the method signature. Your method must be a public method with a boolean return. You will have two parameters, an Object (context) and a String array (params). The context object is almost always casted to a map. The name of your acl starts with acl and the rest is a camel case representation of the acl's function name. For example, the acl used as 'user_role' has a method name 'aclUserRole'. Note that this means you may have private or public helper methods in an aclHandler that can not be used as acl functions because they don't start with 'acl'.

Now you may do pretty much anything with your method, although throwing exceptions during an acl is usually considered rude (many acls are called after the response has already been committed and even written to). You can expect the context object to have the currently logged in user and you may make whatever rules around the parameters as you want. Note that acl functions should be very lightweight. The user_role acl function, for example, can easily be invoked 20+ times per request.

== Issues and Proposed Changes ==
'''Superfluous Code:''' In the java version of acls, there seems to be alot of extra infrastructure[[BR]]
{{{AclManager}}} barely has a reason to exist. {{{AclFactory}}} doesn't seem to have any reason to exist.
We have both a {{{BaseHandler}}} abstract class and an {{{AclHandler}}} interface. It seems that these could simply be one and perform both functions.
There are more than a couple custom jsp tags that contain acl support. These custom tags could use some common code to do the very similar functions that they perform. In some cases, it seems that the tag is too large for its own health. Take toolbar tag for example. It has acl attributes for add, delete, and misc pieces. Also, those add, delete, and misc pieces have several elements only pertaining to themselves. Toolbar tag could be broken up so that there could be another custom tag that renders the different buttons that must be a child of a toolbar tag. Then, those elements wouldn't have to have any acl support because a require tag could be placed around them separately.

'''Regex instead of Grammar:''' there are several java libraries that do grammar support, but instead we have used regular expressions[[BR]]
Since we are defining a mini-language that recognizes boolean expressions, we could easily leverage tools that do the hard part for us instead of using several regular expressions to match pieces. This would also make a missing piece of the language (parentheses) possible. Regular expressions cannot recognize parenthesis grouping, whereas a true grammar can. There are also several flaws with how we recognize our mini-language. One should be able to put a semi-colon as part of a parameter for an acl. A grammar could easily distinguish it as part of the parameter based on context, but our current regex '''always''' treats semi-colons as statement separators. With a defined grammar, we could also throw a parse exception if the acl isn't valid according to the grammar. Right now, we only recognize a couple possible parsing errors because our language definition isn't well described in code.

'''ANDs should bind stronger than ORs:''' Our mini-language doesn't match with the normal boolean expressions of other languages.[[BR]]
The choice of using a semi-colon to represent an AND and 'or' to represent OR most likely caused this strange blunder. IMO, our mini-language should
have 'and' as well as '&' for an AND and 'or' as well as '|' for an OR. Parentheses should bind stronger than everything followed by NOT followed by AND followed by OR. This is how most other boolean expression languages work.

'''Multiple instances used:''' We continually create new instances of our ACL objects.
Whenever an acl is used, we create a new instance of Acl followed by new instances of each of the acl handlers used including Access. Then we call the few functions we call and throw away all of those objects. Considering that there could easily be thirty acls used in a single page rendering, this seems to be much more object processing than necessary. Just as well, the separate instances of all of the handlers are '''exactly''' the same. A single instance of each of them could be used for the entirety of the java stack and all users without any problem. This could be the long lost reason for {{{AclFactory}}}'s existence.
