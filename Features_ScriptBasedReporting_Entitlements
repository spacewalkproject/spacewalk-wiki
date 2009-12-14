= Subscription and entitlement report =

2009-08-05: spacewalk-backend 0.6.29-1 contains the subscription and entitlement report.

Report fields:

{{{
# spacewalk-report --info --list-fields-info entitlements
Entitlement and channel list and usage

    For each organization, list of system entitlements and
    channels to which the organization is entitled, together
    with current usage.

Fields in the report:

    organization_id: Organization identifier
    organization: Organization name
    entitlement_type: 'system' or 'channel'
    entitlement: System entitlement name or channel name
    used: How many entitlements are currently used
    total: The total number of entitlements
}}}

The output:

{{{
# spacewalk-report entitlements
organization_id,organization,entitlement_type,entitlement,used,total
1,Miroslav Suchy,system,RHN Management Entitled Servers,75,200
1,Miroslav Suchy,system,RHN Monitoring Entitled Servers,1,200
1,Miroslav Suchy,system,RHN Provisioning Entitled Servers,1,200
1,Miroslav Suchy,system,Virtualization Host Entitled Servers,0,50
1,Miroslav Suchy,system,Virtualization Host Platform Entitled Servers,9,30
1,Miroslav Suchy,channel,"JBoss Enterprise Application Platform (v 4, zip format)",0,700
1,Miroslav Suchy,channel,JBoss Enterprise Middleware,0,700
1,Miroslav Suchy,channel,Jan Pazdziora (1) Channel Family,2,
1,Miroslav Suchy,channel,RHEL Cluster-Storage,4,300
1,Miroslav Suchy,channel,RHEL Cluster-Storage EUS,0,400
[...]
}}}

----

Back to [wiki:Features/ScriptBasedReporting#Progress].