= Adding Pages  =
== How do I add a page to the Java based site? ==

Adding a page to the rhn-java website is a fairly straight-forward and simple
process, however, it isn't as easy a plopping a file into a directory. There
is an *architecture* involved, and while it may seem like a lot of overhead
just to create a dumb web page, adhering to this architecture/framework will
help us scale the site and maintain separation of concerns.


  '''** Do not commit any of the following code or face ridicule.'''[[BR]]
  '''** $RHN-JAVA is the svn checkout of the rhn-java site.'''

Let's walk through an example.

== HELLO-RHN ==
Let's create the jsp. First, create a {{{test}}} directory.

{{{
  mkdir -p $RHN-JAVA/code/webapp/WEB-INF/pages/test
  cd $RHN-JAVA/code/webapp/WEB-INF/pages/test
}}}

Inside of the new test directory, create a file called {{{hello.jsp}}} with the following contents:
{{{
#!text/html
<html:xhtml>
  <body>
    <h1>Hiddy Ho!</h1>
     <h2>A hello world test page by: your name here</h2>
     <p>Hello my name is mud.</p>
   </body>
</html>
}}}

Ok. This is a pretty simple example of a standard html file. To use {{{jstl}}} or {{{rhn}}}
tags, you must first import the taglib. If you don't know how to do this, I
recommend you read [REF 1,2] or look at some of the existing pages for examples.

Now we need to open up the {{{struts-config.xml}}}

{{{
  $RHN-JAVA/code/webapp/WEB-INF/struts-config.xml
}}}

and add an action forward to our page. There are several architecture decisions
that should be discussed here.

 * Why do we put pages in WEB-INF?

   This is to protect your jsp from being accessed by some yahoo typing in {{{yourserver.com/test/hello.jsp}}}. {{{WEB-INF}}} is protected by
   Tomcat and so we can't serve pages, in the traditional sense, out of it. This may seem  like a moot point considering our
   simple hello-rhn page, but consider a form which may need a {{{SetupAction}}} or some other processing to occur
   before the page is rendered.

 * Why do we map every page?

   Besides the points outlined above, mapping pages promotes re-use and easy management. For example, say we needed a
   hello-rhn page under the systems tab, users tab, and on the index page. We could now include the page portal-style on the
   index, and map systems/Hey.do and users/hello/Howdy.do both to the same hello.jsp.

So, on with the show.

Add the following to the struts-config.xml file:

{{{
<action path="/hello/HelloRhn"
        forward="/WEB-INF/pages/test/hello.jsp" />
}}}

There are a few things to notice about the path attribute of the action tag
above. First, note that it is completely random. We could have used
{{{/yankee/doodle/Dandy}}} and as long as the url accessed was
{{{/yankee/doodle/Dandy.do}}} it would have mapped to our new jsp.

This brings me
to the second note you should make about the mapping; The path doesn't include
the ".do" extension. This is because there are multiple ways to tell Tomcat
that a given url should be processed by Struts. Common standards include
{{{/do/<action-name>}}} and {{{<action-name>.do}}}. The important thing here is to
realize that the {{{.do}}} extension could be changed at any time and isn't
important to the action-servlet.

Restart Tomcat (to pick up the struts-config changes) and go to
{{{
  http://<your box>/rhn/hello/HelloRhn.do 
}}}
and you should see your new page! Don't forget '''NOT''' to check this in!

== WHAT ARE FRAGMENTS? ==
JSP Fragments are a simple naming convention from [REF 1], and are meant to
specify that a jsp is not to be used on its own. For example, a .jspf file may
have a jstl or rhn tag in it and require that the page using the fragment
declare the taglib for it.

Fragments are used throughout our jsps wherever we have a piece of
code that should be reused across several jsp pages. Fragments are located
under
{{{
 $RHN-JAVA/code/webapp/WEB-INF/pages/common/fragments 
}}}
and have a .jspf
file extension rather than the usual .jsp.

== HOW DO I ADD/PROCESS A FORM? ==
Instead of creating our own, I'm going to use the user edit functionality of
the current rhn-java code to demonstrate the needed components. A form page,
by definition, will take input from the user and do something with it. Because
of this, each form/situation will be unique and therefore will require you to
either look at existing examples on the site or consult a reference book [REF
3].

For the most part, rhn-java uses {{{DynaActionForms}}} instead of {{{ActionForms}}}. This
allows us to define the form in our struts-config.xml and have a dto class
generated for us, that will handle moving data between the form and the action
class. Find the following in {{{struts-config.xml}}}: 

{{{
    <form-bean name="userDetailsForm" 
               type="org.apache.struts.action.DynaActionForm">
        <form-property name="uid" type="java.lang.Long"/>
        <form-property name="firstNames" type="java.lang.String"/>
        <form-property name="lastName" type="java.lang.String"/>
        <form-property name="title" type="java.lang.String"/>
        <form-property name="prefix" type="java.lang.String"/>
        <form-property name="password" type="java.lang.String"/>
        <form-property name="passwordConfirm" type="java.lang.String"/>
        <form-property name="possibleRoles" 
                       type="org.apache.struts.util.LabelValueBean[]"/>
        <form-property name="selectedRoles" type="java.lang.String[]"/>
    </form-bean>
}}}

This defines a form bean. Each form-property corresponds to an input field on
the form. Struts handles the type conversions for you. Next we will define
paths to help us setup/process the form. Find the following in
{{{struts-config.xml}}}:

{{{
  <action path="/network/account/UserDetails"
      name="userDetailsForm"
      scope="request"
      input="/WEB-INF/pages/user/edit/yourdetails.jsp"
      type="com.redhat.rhn.frontend.action.UserEditSetupAction">
    <forward name="success" path="/WEB-INF/pages/user/edit/yourdetails.jsp"/>
  </action>

  <action path="/network/account/UserDetailsSubmit"
      name="userDetailsForm"
      scope="request"
      input="/WEB-INF/pages/user/edit/yourdetails.jsp"
      type="com.redhat.rhn.frontend.action.UserEditAction">
    <forward name="success" path="/network/account/UserDetails.do" 
             redirect="true" />
    <forward name="failure" path="/network/account/UserDetails.do" />
  </action>
}}}

It is a best-practice/rhn-convention to name your paths and actions like this.
The first time a user would see a form, the action is named regularly, but uses 
a {{{*SetupAction}}} for its type. This means that when a user types
{{{/network/account/UserDetails.do}}} into their browser, this path is called and
the {{{UserEditSetupAction}}} is called. After the {{{SetupAction}}} has finished
successfully, the yourdetails.jsp page is displayed.

Check out the yourdetails.jsp page. Note that the form's action is {{{UserDetailsSubmit}}}. This means that when the form is submitted, we will run the {{{UserDetailsSubmit}}} action defined above and the {{{UserEditAction}}} will get called. If successful, we forward to {{{UserDetails.do}}}. Notice the {{{redirect="true"}}} attribute. This means that we are doing a full redirect to the next page. If you are confused about the differences between a forward and a
redirect, see [REF 1, 2].

Ok. So now is probably a good time to conceptually step through the process of what is going on behind the scenes so that you can get a good feel for what these mappings are doing.

 1. A user clicks on an "edit details" link from someplace. The url {{{/network/account/UserDetails.do}}} is requested to Tomcat and forwarded to the Struts action servlet. At this point, Struts gets a {{{userDetailsForm DynaActionForm}}} and forwards that, along with the request, to {{{UserEditSetupAction}}}.
 1. {{{UserEditSetupAction}}} gets the user from the request and pre-populates our form by using the setters defined by our {{{userDetailsForm}}} bean. Note that near the end of the class, we create a new variable and put it into the request. 
 1. The {{{yourdetails.jsp}}} form is displayed. Note that we use a fragment to display some of the elements in the form. This is because the same jsp code is used elsewhere. Also, note that we are getting the  {{{availablePrefixes}}} variable out of the request that was set by our {{{SetupAction}}}.
 1. When the form is submitted the url {{{/network/account/UserDetailsSubmit.do}}} is called. This calls the {{{UserEditAction}}} class with the form. In this class we perform the editing of a the users info and return a "success" note that if we find any errors, we add them to {{{ActionErrors}}} and return a "failure". The {{{ActionErrors}}} is a way to send error messages from our action, back to the page.

That's it. Fairly simple. To familiarize yourself with all of the different
types of things you can do, please investigate the existing source code
yourself. 

In particular, the {{{LookupDispatchAction}}} class is very powerful and can be used to do CRUD operations on an object all from a single form!


=== REFERENCES ===
Books:
 1. "!JavaServer Pages (3rd ed.)" - Bergsten
 1. "Core Servlets and !JavaServer Pages (2nd ed.)" - Marty Hall
 1. "Struts In Action" - Husted
