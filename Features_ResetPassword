= Reset-Email Workflow: Design Thoughts =

== Problem Statement ==

Currently, Spacewalk can send plaintext emails to users with their passwords. This happens in two cases:

 * Initial login creation
 * The result of using the 'Forgot my Password' page

This is a security exposure. Best-practice for the current web is to send an email with a one-time-use link, that authenticates and leads the user to setting their password themselves.

The orignating BZ is [https://bugzilla.redhat.com/show_bug.cgi?id=608355 Bug 608355] - Password not to be sent in clear text via email after creating new user on Red Hat Satellite.
It has been cloned for Spacewalk as [https://bugzilla.redhat.com/show_bug.cgi?id=1263799 Bug 1263799].

Hee's a very thorough description of the problem, and of solution(s) to it: [http://www.troyhunt.com/2012/05/everything-you-ever-wanted-to-know.html Everything you ever wanted to know about building a secure password reset feature] This is going to be our de-facto design doc.

The approach that Spacewalk is taking in response to [https://bugzilla.redhat.com/show_bug.cgi?id=608355 Bug 608355] is as follows:

 * !Creation/Reset requests send email to the affected email address
 * Email contains a one-time time-limited random token
 * Token, time-of-creation, and still-valid flag stored in a table, along with the user-id of the affected user
   * This table exists so that we can have a permanent record of every password-reset-request, as an audit-record
   * Any existing entries for the affected user are marked as invalid before we create/send the new one - There Can Be Only One!
 * Tokens are valid if and only if (is-valid == true) && (now - time-created < reset-token-validity-period)
   * expiration-period is controlled by satellite-configuration (default to 48 hours)
 * On receiving the reset-URL, look up the requested token in the new table
   * if is-valid, send user directly to "change your password" page
   * otherwise, inform them that the link is invalid, and send them to 'forget my password' page to request a new link
 * Once password has been reset:
   * user has a logged-in session
   * confirmation email sent to email-address on file

== Technical Details ==


=== Table ===

|| column   || type                 || default || nullable? ||
|| user_id  || FK to WEB_CONTACT.ID || N/A     || NO        ||
|| token    || VARCHAR(64)          || N/A     || NO        ||
|| created  || TIMESTAMP            || now     || NO        ||
|| is_valid || BOOLEAN              || T       || NO        ||

Index on user_id and on token

=== Code Changes ===

See the following commits in spacewalk.github master:

{{{
7e63b252b61de573e00264e15b7ca8f750cc8950
e18542b50a95cf4c4b085a2f158645c46287e6e9
d1ede7633f2f52c07f91fb929755ee2aea86780d
52478842a3bdb09e91628b603cb685d74aa26cc8
4e962d33eb058c7d3143c809d01e1ad8b5548993
d8b8c43b00fddca3d1c3c35ba556ac4232af0537
1a64f52b7fd2003d1c7b1e1979f14308cd67f0a7
5d4fb4e2eb4f4d624b35a9d45e1482e1c345d586
daf10a96914ddb2a102083680e6cdb1d2cae0bc3
6b5c5ec2c777502b6dc40df6e677ec3bca099519
3bd414db18f4645433a8c1fde22cbe7b7f6cedd2
b39c8957c65a7389b841f22b65592b5d583fa0cd
1a0db1551caddbcf093f21b02b8cc49970e6e597
}}}

which affect the following files:

{{{
java/code/src/com/redhat/rhn/common/db/ResetPasswordFactory.java
java/code/src/com/redhat/rhn/common/db/datasource/xml/ResetPassword_queries.xml
java/code/src/com/redhat/rhn/common/db/datasource/xml/file-list.xml
java/code/src/com/redhat/rhn/common/db/test/ResetPasswordFactoryTest.java
java/code/src/com/redhat/rhn/domain/common/ResetPassword.java
java/code/src/com/redhat/rhn/frontend/action/help/ForgotCredentialsAction.java
java/code/src/com/redhat/rhn/frontend/action/test/ResetLinkActionTest.java
java/code/src/com/redhat/rhn/frontend/action/test/ResetPasswordSubmitActionTest.java
java/code/src/com/redhat/rhn/frontend/action/user/ResetLinkAction.java
java/code/src/com/redhat/rhn/frontend/action/user/ResetPasswordAction.java
java/code/src/com/redhat/rhn/frontend/action/user/ResetPasswordSubmitAction.java
java/code/src/com/redhat/rhn/frontend/action/user/UserEditActionHelper.java
java/code/src/com/redhat/rhn/frontend/events/NewUserEvent.java
java/code/src/com/redhat/rhn/frontend/events/test/NewUserEventTest.java
java/code/src/com/redhat/rhn/frontend/security/PxtAuthenticationService.java
java/code/src/com/redhat/rhn/frontend/strings/java/StringResource_en_US.xml
java/code/src/com/redhat/rhn/frontend/strings/jsp/StringResource_en_US.xml
java/code/src/com/redhat/rhn/frontend/strings/template/StringResource_en_US.xml
java/code/src/com/redhat/rhn/manager/user/CreateUserCommand.java
java/code/webapp/WEB-INF/pages/common/errors/invalidreset.jsp
java/code/webapp/WEB-INF/pages/common/fragments/user/user_attribute_sizes.jspf
java/code/webapp/WEB-INF/pages/common/passwordreset.jsp
java/code/webapp/WEB-INF/struts-config.xml
java/spacewalk-java.spec
rel-eng/packages/spacewalk-java
rel-eng/packages/spacewalk-schema
schema/spacewalk/common/tables/rhnResetPassword.sql
schema/spacewalk/common/tables/tables.deps
schema/spacewalk/oracle/triggers/rhnResetPassword.sql
schema/spacewalk/postgres/triggers/rhnResetPassword.sql
schema/spacewalk/spacewalk-schema.spec
schema/spacewalk/upgrade/spacewalk-schema-2.3-to-spacewalk-schema-2.4/013-rhnResetPassword-trigger.sql.oracle
schema/spacewalk/upgrade/spacewalk-schema-2.3-to-spacewalk-schema-2.4/013-rhnResetPassword-trigger.sql.postgresql
schema/spacewalk/upgrade/spacewalk-schema-2.3-to-spacewalk-schema-2.4/013-rhnResetPassword.sql
}}}

== Email examples ==

=== New User Added ===

{{{
Subject: Your Spacewalk Account is ready
Body:
Hello,

A Red Hat login has been created for you by <creator-first> <creator-last>. Your
Red Hat login, in combination with an active Red Hat subscription,
provides you with access to manage systems on Spacewalk.

  Red Hat login: <login>
         e-mail: <receiving-email>

Please proceed to https://myspacewalk.example.com/rhn/ResetLink.do?token=c1a6d48d5fef9b116c3955152b237fb30e3df859 to set your password and enable your account.

Thank you for using Spacewalk.
--the Spacewalk Team
}}}

=== Password Reset Requested ===
 
{{{
Subject: Spacewalk Password Reset
Body:
[ This is an automated email sent to <receiving-email> at your request. ]

A request to reset the Spacewalk password for login <login> has been made.

To continue the reset process, please proceed to:

https://myspacewalk.example.com/rhn/ResetLink.do?token=a46c3e3f7bd6497c9c1680e62fe463b7cfa2840a

If you don't want your password reset, you can ignore this email.

If you experience any further difficulties with the reset process,
please contact your Spacewalk administrator for further assistance.

Thank you for using Spacewalk.
}}}

=== Password Reset Confirmation ===

{{{
Subject:
Body:
[ This is an automated email sent to <receiving-email>. ]

The Spacewalk password for login <login> has been reset in response to a reset-request
made at beast-spacewalk-dev.usersys.redhat.com

If you did not initiate this password reset, please IMMEDIATELY contact the Spacewalk
administrator at myspacewalk.example.com for further assistance.

Thank you for using Spacewalk.
}}}

== Stretch Goals ==

Even the one-time-use link can be abused, if we assume (as we should) that email is an insecure delivery method. 

One possible way to provide actually-secure-communications, would be for the reset-form(s) to allow an optional "recipient's public GPG key" field. If provided, the email-body could be encrypted prior to sending.

Many (most?) users would never use this - it's too inconvenient. Those that would want it, would want it A LOT (think goverment/banking/healthcare).

See if we can fit this option into the feature at some point.