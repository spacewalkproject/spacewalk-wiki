[wiki:PostgresTasks Tasks Page]

'''Note''': The below list of procedures/functions includes the ones under src/schema/spacewalk/rhnsat/procs.

== Procedures and Functions to Migrate/Replace ==

||'''Procedure/Function'''||'''Assigned'''||'''Notes'''||
||adler32|| Farrukh || Done || 
||channel_name_join|| || Discarded, the procedure was not exposed to the app, was being used by a view which has been modified to work without the procedure|| 
||create_first_org|| Gurjeet || Done || 
||create_new_org|| Farrukh || Done || 
||create_new_user|| Farrukh || Done || 
||create_pxt_session|| Gurjeet || Done || 
||delete_channel|| Farrukh || Done || 
||delete_errata|| Farrukh || Done || 
||delete_server_bulk|| Gurjeet || Done || 
||delete_server|| Farrukh || Done || 
||does_user_have_role|| Farrukh || Done || 
||entitle_order|| Farrukh || This procedure has 2 OUT parameter, but luckily it's not being anywhere in the app so skipping this one|| 
||id_join|| || Discarded, the procedure was not exposed to the app, was being used by a view which has been modified to work without the procedure|| 
||is_user_applicant|| Farrukh || Done || 
||label_join|| || Discarded, the procedure was not exposed to the app, was being used by a view which has been modified to work without the procedure|| 
||lookup_arch_type|| Gurjeet || Done || 
||lookup_cf_state|| Gurjeet || Done || 
||lookup_channel_arch|| Farrukh || Done || 
||lookup_client_capability|| Farrukh || Done || 
||lookup_config_filename|| Farrukh || Done || 
||lookup_config_info|| Farrukh || Done || 
||lookup_cve|| Farrukh|| Done || 
||lookup_erratafile_type|| Farrukh || Done || 
||lookup_evr|| Farrukh || Done || 
||lookup_feature_type|| Gurjeet || Done || 
||lookup_first_matching_cf|| Farrukh || Done || 
||lookup_package_arch|| Farrukh || Done || 
||lookup_package_capability|| Farrukh || Done || 
||lookup_package_delta|| Farrukh || Done || 
||lookup_package_key_type|| Gurjeet || Done || 
||lookup_package_name|| Farrukh || Done || 
||lookup_package_nevra|| Farrukh || Done || 
||lookup_package_provider|| Gurjeet || Done || 
||lookup_server_arch|| Farrukh || Done || 
||lookup_sg_type|| Farrukh || Done || 
||lookup_snapshot_invalid_reason|| Farrukh || Done || 
||lookup_source_name|| Farrukh || Done || 
||lookup_tag_name|| Farrukh || Done || 
||lookup_tag|| Farrukh || Done || 
||lookup_transaction_package|| Farrukh || Done || 
||lookup_virt_sub_level|| Farrukh || Done || 
||name_join|| || Discarded, the procedure was not exposed to the app, was being used by a view which has been modified to work without the procedure || 
||new_user_postop|| Farrukh || Done || 
||pxt_session_cleanup|| Farrukh || Done || 
||queue_errata|| Gurjeet || Done || 
||queue_server|| Farrukh || Done || 
||rhn_install_org_satellites|| Farrukh || Done || 
||rhn_install_satellite|| Farrukh || Done || 
||rhn_prepare_install|| Farrukh || Done || 
||rhn_synch_probe_state|| Farrukh || Done || 
||sequence_currval|| Gurjeet || Done || 
||sequence_nextval|| Gurjeet || Done || 
||set_ks_session_history_message|| Farrukh|| Done || 
||truncateCacheQueue|| Gurjeet || Done ||


[[BR]]
[[BR]]

'''Workaround for OUT parameters'''
[[BR]]
For tackling OUT parameters we can go with one of the below possibilities:

1. Use functions for postgres and use RETURN keyword instead of OUT to obtain the desired results. This would require changes in application and every procedure call (with OUT parameters) would have two code paths; one for Oracle and One for Postgres in a fashion similar to below:

    if Oracle then
            Oracle procedure call statement
    else if postgres
           Postgres function call statement
   end if;

And more changes might also be required to handle/take care of the function return value.


2. As mentioned above there is only one OUT parameter in all the procedures and they can easily be converted to functions with return values to get the results of OUT parameter. We can change those procedures for both Oracle and Postgres to behave the same way i.e. using RETURN values. This would provide similar calling code from application for both Oracle and Postgres. This would/might also require changes in the application but according to my understanding the changes would be minimal as compared to CASE1 and the application won't require separate codes for calling Oracle and Postgers functions.

We have opted for second approach.