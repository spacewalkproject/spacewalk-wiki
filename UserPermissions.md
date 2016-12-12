{{{
#!div class="important" style="border: 2pt solid; text-align: center"
'''''DEPRECATED, NO LONGER USED'''''
}}}


Today UI and API access is limited though the use of a half-dozen or so pre-defined broad roles, Channel Administrator, Org Administrator, Configuration Administrator, etc. These roles have proven insufficient for many customers and more fine grained control is a frequently requested feature.

Currently this page is just being used for an implementation discussion, but should eventually evolve to be the developer facing documentation on how to understand and use fine grained permissions within the project.

= Proposed Implementation =

(note this just some initial thoughts (dgoodwin), will be doing some research into other projects and perhaps even frameworks although I suspect we'll have to do this in house given the code base we're working with)

 * Define a new table for specific fine grained rights.
  * Columns:
   * id - Unique.
   * label - Unique and hierarchical. Something perhaps like system.canEditChannels, system.canEditProperties.
   * resource_key - Resource bundle description key.
 * Allow these specific rights to be associated with users via two means:
  * Directly associated with user account.
  * Associated with a "Role" that is in turn, associated to the user account.
  * User has a right if present in either place.
 * Roles would be collections of fine grained rights and could be managed in the UI.
  * Several would be provided out of the box after a satellite install, but *could* be customized if required. These roles would likely closely mirror (or perhaps even directly leverage) the existing satellite roles today.
  * Admins can create their own roles manually or by copying an existing role as a template.
 * Rights are "can do", not "can't do".
 * Implement means to check for a right(s) before loading pages, rendering links, rendering submit buttons, or executing API calls.
  * This could likely be done easily by leveraging the existing ACL framework we have today. Instead of using the ACL user_role() we could implement has_right() and gradually shy away from the old role checks.

== Outstanding Questions ==

 1. How do we define multi-org permissions and access rights?
 1. How do we grant specific permissions on specific objects? (i.e. user has rights to do certain things only in a particular org)

== Disadvantages ==

 * Could grow to be a very large number of these fine grained rights. Developers would have to continually be diligent in ensuring we only create new rights when it doesn't make sense to re-use an existing one.
 * Management of custom roles could be a bit daunting if there are many rights, although most customers should only have to make a few customizations to the out of the box roles, if any.
 * Care would have to be exercised to keep the rights consistently named and used.
 * Developers must ensure the rights are actually checked in the code.
 * Leveraging existing roles today could cause problems for the way we indicate in the UI that having the Satellite admin role implies all the other roles. As described above, moving to a more generic rights/roles system would mean we can't have this implicit relationship in the UI. The admin *could* change the satellite admin role to *not* give out all these other roles for instance.
 
= Existing Solutions =

Projects we may be able to integrate, or at least use concepts from:

 * [http://static.springframework.org/spring-security/site/ Spring Security]

