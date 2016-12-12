# Code Analysis

A page that captures what code is what, and what it does.


All package names are assumed to be prefixed with `com.redhat.rhn.`

 * ~~[client](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/client)~~
   * ~~[InvalidUserNameException](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/client/InvalidUserNameException.java) - used by UnitTestHandler; move to `frontend.xmlrpc.test` package.~~
 * [common](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common)
   * [client](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/client) - looks ok
   * [conf](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/conf) - configuration code looks ok
   * ~~[daemon](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/daemon) - move to `taskomatic.core` package. NOTE: `taskomatic.core` would be a new package.~~
   * [db](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/db)
     * [datasource](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/db/datasource) - pretty stable, seems to do most of what we want already.
       * [xml](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/db/datasource/xml) - datasource query files
   * ~~[download](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/download) - move to [common.util](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/util) package~~
   * [errors](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/errors) - looks ok
   * [filediff](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/filediff) - looks ok
   * [finder](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/finder) - ok; might be worth packaging separately.
   * [hibernate](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/hibernate) - ok
     * ~~usertypes - *REMOVE*~~
   * [localization](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/localization) - ok; i18n framework
   * [messaging](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/messaging) - ok
   * [security](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/security) - ok
     * [acl](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/security/acl) - ok, best javadoc in the codebase :)
       * [action](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/security/acl/action) - probably should move the files in here up one level.
   * [translation](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/translation) - move to [common.util](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/util) package
   * [util](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/util) - ok
     * [manifestfactory](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/util/manifestfactory) - ok
   * [validator](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/validator) - ui validation utils; do we need a `ui` package? move to `util` package?
 * [domain](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/domain)
 * [frontend](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend) - one of the largest packages in the codebase
   * [action](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/action) - has all of the struts actions in support of the jsp view. This package will required *major* refactoring. There are a lot of old actions that use a Setup/Submit type of format. We also have a number of ways of doing ListTags and confirmation pages.
   * [configuration.tags](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/configuration/tags) - this should be move to the [taglibs](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/taglibs) package with the other taglibs.
   * [context](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/context) - contains a support class for the Locale and other infrastructure.
   * [dto](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/dto) - [Data Transfer Objects](http://java.sun.com/blueprints/patterns/TransferObject.html) used for the UI.
   * [events](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/events) - event and action classes used by the [Messaging](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common/messaging) infrastructure.
   * [filter](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/filter) - not sure what this is for.
   * [graphing](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/graphing) - generates graphing for monitoring; do we move this to the common package.
   * [html](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/html) - HTML generating utility classes.
   * [listview](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/listview) - support classes from the *original* listview and used by the *old* [listtag](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/taglibs/ListDisplayTag.java).
   * [nav](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/nav) - ok. Contains the [Navi](NavigationSystem) code
   * [security](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/security) - remove or refactor this code, it is a remnant of the hosted days that is no longer needed.
   * [servlets](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/servlets) - contains the servlet infrastructure classes and servlet filters. The one set of classes that needs to get gutted are the `PxtSessionDelegate*` classes. The rest look ok.
   * [strings](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/strings) - ok. used to load the `StringResource` files.
   * [struts](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/struts) - this needs some major refactoring especially with anything relating to the ListTag code. Also remove `StrutsDelegate*` code.
      * [wizard](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/struts/wizard) - ok. used for creating wizard uis like the one for kickstart creation.
   * [taglibs](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/taglibs) - tag libraries. Need to remove the *old* [ListTag.java](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/taglibs/ListTag.java)
   * [xmlrpc](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/frontend/xmlrpc) - XML-RPC api handlers. Biggest problem is consistency between api calls.
 * [manager](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/manager)
 * ~~[task](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/task) - contains the taskomatic tasks. Need to move `task` package into the [taskomatic](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/taskomatic)/task package;~~
 * ~~[taskomatic](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/taskomatic) move the current classes to a `core` subpackage; add a `task` subpackage as well.~~
 * [testing](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/testing) - do we move these into  [common](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/common)
 * [webapp](https://hosted.fedoraproject.org/spacewalk/browser/java/code/src/com/redhat/rhn/webapp) - contains a single ServletListener.