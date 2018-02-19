### Java pages (any pages that end in ".do" )




Since we use struts in our project, the so called master index of java pages is struts-config.xml, located in /eng/java/code/webapp/WEB-INF/struts-config.xml .

Lets take a page /rhn/users/ActiveList.do, for example, and drill down all the way.  Within struts-config.xml we find:

        <action path="/users/ActiveList"
            scope="request"
            input="/WEB-INF/pages/admin/users/activelist.jsp"
            type="com.redhat.rhn.frontend.action.user.EnabledListSetupAction"
            className="com.redhat.rhn.frontend.struts.RhnActionMapping">
          <set-property property="acls" 
                        value="org_entitlement(sw_mgr_enterprise); user_role(org_admin)"/>
          <forward name="default" path="/WEB-INF/pages/admin/users/activelist.jsp" />
        </action>
Notice that the action name does not include the '.do'.  Here we see that the main Action class is "com.redhat.rhn.frontend.action.user.EnabledListSetupAction" and the jsp it is forwarded to is "/WEB-INF/pages/admin/users/activelist.jsp".  All actions (or one of their parent classes) will have an execute() method.  Within EnabledListSetupAction.java's execute() method you can see a call to the Manager layer "UserManager.activeInOrg2(user);".

Follow the bread crumbs and you will see that the activeInOrg2() method uses our internal ORM called Datasource (Hibernate is used as well in some places), it consists of xml files with queries, which either populate a map or a dto.  The activeInOrg2() method consists of the following: 

    #!java
            SelectMode m = ModeFactory.getMode("User_queries", "active_in_org");
            Map params = new HashMap();
            params.put("org_id", user.getOrg().getId());
            DataResult dr = m.execute(params);
            dr.elaborate(Collections.EMPTY_MAP);
            return dr;

To see the query it is using, simply look in "/eng/java/code/src/com/redhat/rhn/common/db/datasource/xml/User_queries.xml" for a query marked "active_in_org".  

Looking at this query:

     <mode name="active_in_org" class="com.redhat.rhn.frontend.dto.UserOverview">
      <query params="org_id">
        SELECT wce.id AS ID,
           wce.login,
           wce.login_uc
        FROM rhnWebContactEnabled wce
        WHERE wce.org_id = :org_id
        ORDER BY wce.login_uc
      </query>
      <elaborator name="users_in_org_overview" />
     </mode>

You can see that it is using a UserOverview dto to store all the information.  This is the driving query used to display the list (but is not all of the information needed).  The elaborator "users_in_org_overview" is used to populate the !serOverview objects with the rest of the needed information on just the displayed items.  (For example, on a list 5 items long of 100 total items, the driving query would return all 100 items, but the elaborator would only be used on the 5 that will be displayed.)  This elaborator is simply another query within the "User_queries.xml" file.
