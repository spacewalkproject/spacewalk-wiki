# Basic FISMA Report: user report



User reports were created based on the following requirements:

 1. List all users
 2. Username, Full Name, Position, User type?, email, roles (pipe/semi-colon deliminated?), creation date, last-login date, active state
 3. Optionally list usernames associated to channels and systems? Would that even be sensible?

2009-11-18: spacewalk-backend 0.7.14-1 contains "users" and "users-systems" reports.

Report fields:


    # spacewalk-report --info --list-fields-info users
    Users in the system
    
        List of all users for all organizations, with their details
        and roles.
    
    Fields in the report:
    
        organization_id: Organization identifier
        organization: Organization name
        user_id: Internal user id
        username: User name / login
        last_name: Last name
        first_name: First name(s)
        position: Position of the user
        email: Email address
        role: Roles assigned to the user
        creation_time: Date and time when the user was created
        last_login_time: When the user last accessed the system
        active: String enabled or disabled

Note that there are two additional columns identifying the organization, and the user type is not present because the User Type field is basically list of roles in older Satellites, and empty in 5.1+.


    # spacewalk-report --info --list-fields-info users-systems
    Systems administered by individual users
    
        List of systems that users can administer.
    
    Fields in the report:
    
        organization_id: Organization identifier
        user_id: Internal user id
        username: User name / login
        server_id: System identifier
        group: Group through the user has access to the system
        admin_access: 1 if the user has access by being org administrator

The output:


    # spacewalk-report users | grep '^25,'
    25,foobar,275,doe,Doe,John,,doe@example.com,Organization Administrator,2008-10-31 10:24:49,2008-10-31 10:25:47,enabled
    25,foobar,276,barfoo,a,a,,a-a@example.com,Activation Key Administrator;System Group Administrator;Configuration Administrator,2008-10-31 10:26:38,,disabled
    25,foobar,312,helen,Vondra,Helen,singer,helen@example.com,,2009-10-31 11:53:23,2009-11-18 12:42:44,enabled
    # spacewalk-report users-systems
    25,275,doe,1000012508,,1
    25,275,doe,1000016367,,1
    25,275,doe,1000016407,,1
    25,276,barfoo,1000016407,public,
    25,312,helen,1000016407,public,
    25,312,helen,1000016367,helens,

----

Back to [[Features_ScriptBasedReporting]].