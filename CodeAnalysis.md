= Code Analysis =
A page that captures what code is what, and what it does.

All package names are assumed to be prefixed with {{{com.redhat.rhn.}}}

 * ~~[https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/client client]~~
   * ~~[https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/client/InvalidUserNameException.java InvalidUserNameException] - used by !UnitTestHandler; move to {{{frontend.xmlrpc.test}}} package.~~
 * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common common]
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/client client] - looks ok
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/conf conf] - configuration code looks ok
   * ~~[https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/daemon daemon] - move to {{{taskomatic.core}}} package. NOTE: {{{taskomatic.core}}} would be a new package.~~
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/db db]
     * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/db/datasource datasource] - pretty stable, seems to do most of what we want already.
       * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/db/datasource/xml xml] - datasource query files
   * ~~[https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/download download] - move to [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/util common.util] package~~
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/errors errors] - looks ok
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/filediff filediff] - looks ok
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/finder finder] - ok; might be worth packaging separately.
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/hibernate hibernate] - ok
     * ~~usertypes - '''REMOVE'''~~
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/localization localization] - ok; i18n framework
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/messaging messaging] - ok
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/security security] - ok
     * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/security/acl acl] - ok, best javadoc in the codebase :)
       * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/security/acl/action action] - probably should move the files in here up one level.
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/translation translation] - move to [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/util common.util] package
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/util util] - ok
     * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/util/manifestfactory manifestfactory] - ok
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/validator validator] - ui validation utils; do we need a {{{ui}}} package? move to {{{util}}} package?
 * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/domain domain]
 * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend frontend] - one of the largest packages in the codebase
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/action action] - has all of the struts actions in support of the jsp view. This package will required '''major''' refactoring. There are a lot of old actions that use a Setup/Submit type of format. We also have a number of ways of doing !ListTags and confirmation pages.
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/configuration/tags configuration.tags] - this should be move to the [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/taglibs taglibs] package with the other taglibs.
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/context context] - contains a support class for the Locale and other infrastructure.
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/dto dto] - [http://java.sun.com/blueprints/patterns/TransferObject.html Data Transfer Objects] used for the UI.
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/events events] - event and action classes used by the [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/messaging Messaging] infrastructure.
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/filter filter] - not sure what this is for.
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/graphing graphing] - generates graphing for monitoring; do we move this to the common package.
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/html html] - HTML generating utility classes.
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/listview listview] - support classes from the '''original''' listview and used by the '''old''' [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/taglibs/ListDisplayTag.java listtag].
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/nav nav] - ok. Contains the [NavigationSystem Navi] code
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/security security] - remove or refactor this code, it is a remnant of the hosted days that is no longer needed.
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/servlets servlets] - contains the servlet infrastructure classes and servlet filters. The one set of classes that needs to get gutted are the {{{PxtSessionDelegate*}}} classes. The rest look ok.
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/strings strings] - ok. used to load the {{{StringResource}}} files.
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/struts struts] - this needs some major refactoring especially with anything relating to the !ListTag code. Also remove {{{StrutsDelegate*}}} code.
      * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/struts/wizard wizard] - ok. used for creating wizard uis like the one for kickstart creation.
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/taglibs taglibs] - tag libraries. Need to remove the '''old''' [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/taglibs/ListTag.java ListTag.java]
   * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/xmlrpc xmlrpc] - XML-RPC api handlers. Biggest problem is consistency between api calls.
 * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/manager manager]
 * ~~[https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/task task] - contains the taskomatic tasks. Need to move {{{task}}} package into the [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/taskomatic taskomatic]/task package;~~
 * ~~[https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/taskomatic taskomatic] move the current classes to a {{{core}}} subpackage; add a {{{task}}} subpackage as well.~~
 * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/testing testing] - do we move these into  [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common common]
 * [https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/webapp webapp] - contains a single !ServletListener.