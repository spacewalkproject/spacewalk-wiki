*_DEPRECATED, NO LONGER USED*_
# Remaining PXT pages



The following is a list of the remaining pxt pages located in the `web/html` directory.
#### Errata RSS feed

It would be useful to have a generic RSS feed servlet to offer other types of information as RSS. I'd expect the url to be something like `http://myspacewalkserver.com/rhn/feeds.do?format=rss&type=errata`. This gives us the ability to add other types and formats.

 * /rpc/recent-errata.pxt
#### Email confirmation pages

 * /confirm_email_success.pxt

 * /confirm_email.pxt
#### Custom error files

Not sure if it's possible to redirect errors to the tomcat side, then we could simply remove these error pages as well.

 * /errors/404.pxt
 * /errors/413.pxt
 * /errors/cookies.pxt
 * /errors/permission.pxt
 * /errors/download-expired.pxt
 * /errors/download-mismatch.pxt
 * /errors/500.pxt
 * /errors/cert-expired.pxt
 * /errata_not_found.pxt
#### Common look and feel templates

Templates can not be removed until we finish all of the pxt migration.

 * /templates/c.pxt
 * /templates/header.pxt
 * /templates/profile.pxt
 * /templates/equals_sign.pxt
 * /templates/footer.pxt
#### Misc

 * /cve_not_found.pxt

 * /legal/terms.pxt  - *REMOVE*
 * /file_not_found.pxt  *REMOVE*
 * /konq.pxt  *REMOVE* ?
 * /errata_hidden/rh_template.pxt *REMOVE*
#### Help files



 * /help/about.pxt
 * /help/faq.pxt - *REMOVE*
 * /help/rhlmigrationfaq/index.pxt - *REMOVE*
 * /help/contact.pxt - *REMOVE*
 * /help/browser-info.pxt - seems useless
 * /help/ssl_cert.pxt - needs updating
 * /help/release-notes/unix/index.pxt - *REMOVE*
 * /help/latest-up2date.pxt - *REMOVE*
 * /help/forgot_password.pxt
#### GPG and SSL key editor

These have already been migrated.

 * /network/keys/edit.pxt - *REMOVE*
 * /network/keys/key_list.pxt - *REMOVE*
 * /network/keys/delete.pxt - *REMOVE*
#### monitoring scout

 * /network/monitoring/scout/index.pxt

 * /network/monitoring/scout/details.pxt
#### Software Channel Management

 * /network/software/cannot_download.pxt

 * /network/software/channels/license.pxt (Can be removed??)
 * /network/software/channels/patches.pxt
 * /network/software/channels/subscribers.pxt
 * /network/software/channels/patchsets.pxt
 * /network/software/channels/download_packages.pxt
 * /network/software/channels/subscribe_confirm.pxt   (Can be removed if license.pxf is removed).
 * /network/software/channels/display_license.pxt 
 * /network/software/channels/managers.pxt
 * /network/software/channels/gpg_info.pxt
 * /network/software/channels/manage/edit.pxt
 * /network/software/channels/manage/patchsets/add_confirm.pxt
 * /network/software/channels/manage/patchsets/index.pxt
 * /network/software/channels/manage/patchsets/add.pxt
 * /network/software/channels/manage/subscribers.pxt
 * /network/software/channels/manage/index.pxt
 * /network/software/channels/manage/clone.pxt
 * /network/software/channels/manage/delete_confirm.pxt
 * /network/software/channels/manage/managers.pxt
 * /network/software/channels/manage/packages/compare/sync_confirm.pxt
 * /network/software/channels/manage/packages/compare/index.pxt
 * /network/software/channels/manage/packages/compare/sync_preview.pxt
 * /network/software/channels/manage/packages/compare/sync_prompt.pxt
 * /network/software/channels/manage/errata/add_confirm.pxt
 * /network/software/channels/manage/errata/clone.pxt
 * /network/software/channels/manage/errata/clone_confirm.pxt
 * /network/software/channels/manage/patches/add_confirm.pxt
 * /network/software/channels/manage/patches/remove_confirm.pxt
 * /network/software/channels/manage/patches/patch_list.pxt
 * /network/software/channels/manage/patches/index.pxt
 * /network/software/channels/manage/patches/add.pxt
#### Packages

 * /network/software/packages/system_list_for_patchset.pxt

 * /network/software/packages/file_list.pxt
 * /network/software/packages/patch_packages.pxt
 * /network/software/packages/patches_patching_package.pxt
 * /network/software/packages/unknown_package.pxt
 * /network/software/packages/patchset_patches.pxt
 * /network/software/packages/patch_patchsets.pxt
 * /network/software/packages/package_map.pxt
 * /network/software/packages/install_confirm.pxt
 * /network/software/packages/newer_versions.pxt
 * /network/software/packages/target_system_list.pxt
 * /network/software/packages/change_log.pxt
 * /network/software/packages/target_system_list_for_patchset.pxt
 * /network/software/packages/target_system_list_for_patch.pxt
 * /network/software/packages/system_list.pxt
 * /network/software/packages/dependencies.pxt
 * /network/software/packages/system_list_for_patch.pxt
 * /network/software/all_channels.pxt
 * /network/software/retired_channels.pxt
#### Monitoring Notification Methods

 * /network/users/details/contact_methods/edit.pxt

 * /network/users/details/contact_methods/index.pxt
 * /network/users/details/contact_methods/create.pxt
 * /network/users/details/contact_methods/delete_confirm.pxt
#### Errata management

These don't seem to be even used anymore except by each other????? 

 * /network/errata/manage/edit.pxt
 * /network/errata/manage/select_channels.pxt
 * /network/errata/manage/confirm_mail.pxt
 * /network/errata/manage/errata_channel_intersection.pxt  Except this one which is referred to by a jsp
 * /network/errata/manage/confirm_publish.pxt
 * /network/errata/manage/update_channel_packages.pxt
 * /network/errata/manage/delete_confirm.pxt
 * /network/errata/manage/packages/add_confirm.pxt
 * /network/errata/manage/packages/remove_confirm.pxt
 * /network/errata/manage/packages/package_list.pxt
 * /network/errata/manage/packages/index.pxt
 * /network/errata/manage/packages/add.pxt
 * /network/errata/manage/clone/confirm.pxt
 * /network/errata/manage/clone/index.pxt
#### Kickstart

 * /network/systems/provisioning/sessions/cancel_session.pxt

 * /network/systems/provisioning/sessions/details.pxt
 * /network/systems/provisioning/sessions/history.pxt
 * /network/systems/provisioning/preservation/edit.pxt
 * /network/systems/provisioning/preservation/delete.pxt
 * /network/systems/provisioning/preservation/preservation_list.pxt
#### Custom info

 * /network/systems/custominfo/edit.pxt *Ported to Java*

 * /network/systems/custominfo/index.pxt *Ported to Java*
 * /network/systems/custominfo/delete.pxt *Ported to Java*
#### System Group management

 * /network/systems/groups/admin_list.pxt

 * /network/systems/groups/edit_properties.pxt
 * /network/systems/groups/errata_list.pxt
 * /network/systems/groups/create.pxt *Ported to Java*
 * /network/systems/groups/delete_confirm.pxt
 * /network/systems/groups/details.pxt
 * /network/systems/groups/probe_list.pxt
 * /network/systems/groups/systems_affected_by_errata.pxt
 * /network/systems/groups/apply_errata_confirm.pxt
#### System Detail (SDC) pages

We should re-evaluate our system details pages and SSM. It would seem that the SDC is nothing more than a system set of 1, with some exceptions.

 * /network/systems/details/history/pending.pxt
 * /network/systems/details/history/event.pxt
 * /network/systems/details/history/cancel_events_confirm.pxt
 * /network/systems/details/history/snapshots/remove_confirm.pxt
 * /network/systems/details/history/snapshots/groups.pxt
 * /network/systems/details/history/snapshots/snapshot_tags.pxt
 * /network/systems/details/history/snapshots/packages.pxt
 * /network/systems/details/history/snapshots/system_tags.pxt
 * /network/systems/details/history/snapshots/index.pxt
 * /network/systems/details/history/snapshots/namespaces.pxt
 * /network/systems/details/history/snapshots/unservable_packages.pxt
 * /network/systems/details/history/snapshots/rollback.pxt
 * /network/systems/details/history/snapshots/add_snapshot_tag.pxt
 * /network/systems/details/history/snapshots/config.pxt
 * /network/systems/details/history/snapshots/channels.pxt
 * /network/systems/details/history/snapshots/add_system_tag.pxt
 * /network/systems/details/history/dependency_failures.pxt
 * /network/systems/details/history/history.pxt
 * /network/systems/details/history/verify_results.pxt
 * /network/systems/details/history/package_event_results.pxt
 * /network/systems/details/satellite.pxt
 * /network/systems/details/activation.pxt
 * /network/systems/details/hardware.pxt  *(2010-08-04 Converted to Java)*
 * /network/systems/details/notes/edit.pxt *(2010-08-13 Converted to Java)*
 * /network/systems/details/notes/delete_note_conf.pxt *(2010-08-13 Converted to Java)*
 * /network/systems/details/notes/notes_list.pxt *(2010-08-13 Converted to Java)*
 * /network/systems/details/custominfo/edit.pxt *Ported to Java*
 * /network/systems/details/custominfo/remove_value_conf.pxt *Ported to Java*
 * /network/systems/details/custominfo/new_value.pxt *Ported to Java*
 * /network/systems/details/custominfo/index.pxt *Ported to Java*
 * /network/systems/details/channel_license.pxt
 * /network/systems/details/proxy-clients.pxt
 * /network/systems/details/remote_commands.pxt
 * /network/systems/details/proxy/terms-and-conditions.pxt
 * /network/systems/details/proxy/install_progress.pxt
 * /network/systems/details/proxy/deactivate.pxt
 * /network/systems/details/proxy/index.pxt
 * /network/systems/details/proxy/configure.pxt
 * /network/systems/details/proxy/install.pxt
 * /network/systems/details/connection.pxt
 * /network/systems/details/delete_confirm.pxt *Ported to Java*
 * /network/systems/details/proxy.pxt
 * /network/systems/details/deactivate_satellite_confirm.pxt
 * /network/systems/details/kickstart/missing_packages.pxt
 * /network/systems/details/kickstart/index.pxt
 * /network/systems/details/kickstart/session_status.pxt
 * /network/systems/details/kickstart/options.pxt
 * /network/systems/details/reboot_confirm.pxt
 * /network/systems/details/channels.pxt
#### System Set Manager (SSM)



 * /network/systems/ssm/patchsets/install_conf.pxt
 * /network/systems/ssm/patchsets/index.pxt
 * /network/systems/ssm/patchsets/install_channel.pxt
 * /network/systems/ssm/patchsets/install.pxt
 * /network/systems/ssm/provisioning/remote_command_conf.pxt
 * /network/systems/ssm/provisioning/rollback_by_tag_conf.pxt
 * /network/systems/ssm/provisioning/tag_systems.pxt
 * /network/systems/ssm/provisioning/rollback.pxt
 * /network/systems/ssm/provisioning/remote_command.pxt
 * /network/systems/ssm/misc/unlock_systems_conf.pxt
 * /network/systems/ssm/misc/delete_systems_conf.pxt
 * /network/systems/ssm/misc/change_sys_pref_conf.pxt
 * /network/systems/ssm/misc/set_value.pxt
 * /network/systems/ssm/misc/lock_systems_conf.pxt
 * /network/systems/ssm/misc/reboot_systems.pxt
 * /network/systems/ssm/misc/index.pxt *Ported to Java*
 * /network/systems/ssm/misc/choose_value_to_set.pxt
 * /network/systems/ssm/misc/choose_value_to_remove.pxt
 * /network/systems/ssm/misc/hw_prof_update_conf.pxt *Ported to Java*
 * /network/systems/ssm/misc/reboot_confirm.pxt
 * /network/systems/ssm/misc/remove_value.pxt



 * /network/systems/ssm/misc/pkg_prof_update_conf.pxt *Ported to Java*
 * /network/systems/ssm/index.pxt  *Ported to Java*
 * /network/systems/ssm/groups/alter_membership_conf.pxt
 * /network/systems/ssm/groups/index.pxt
 * /network/systems/ssm/groups/create.pxt
 * /network/systems/ssm/channels/license.pxt
 * /network/systems/ssm/channels/base_channel_alteration.pxt
 * /network/systems/ssm/channels/index.pxt
 * /network/systems/ssm/channels/alter_subscriptions_conf.pxt
 * /network/systems/ssm/packages/verify_system_list.pxt
 * /network/systems/ssm/packages/verify_conf.pxt
 * /network/systems/ssm/packages/index.pxt
 * /network/systems/ssm/packages/installed_systems.pxt
 * /network/systems/ssm/packages/schedule_remote_command.pxt
 * /network/systems/ssm/packages/verify.pxt
 * /network/systems/ssm/packages/remove_system_list.pxt
 * /network/systems/ssm/packages/upload_answer_file.pxt
 * /network/systems/ssm/errata/systems_affected.pxt
 * /network/systems/ssm/errata/apply_errata_conf.pxt
 * /network/systems/ssm/errata/index.pxt
 * /network/systems/ssm/system_list.pxt   *Ported to Java*
 * /network/systems/ssm/patches/remove_conf.pxt
 * /network/systems/ssm/patches/remove.pxt
 * /network/systems/ssm/patches/install_conf.pxt
 * /network/systems/ssm/patches/index.pxt
 * /network/systems/ssm/patches/install_channel.pxt
 * /network/systems/ssm/patches/remove_system_list.pxt
 * /network/systems/ssm/patches/install.pxt
 * /network/systems/ssm/work_with_group.pxt
#### Activation keys

 * /network/account/activation_keys/packages.pxt *(Looks like this one has already been done)*

 * /network/account/activation_keys/child_channels.pxt  *Ported to Java*


