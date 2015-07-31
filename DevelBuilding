== devel building and its purpose ==

As sometimes there's a pull request that might destabilize Spacewalk, we've created extra build targets on koji.spacewalkproject.org, basically a nightly for nightly. With extra build targets come also extra new repositories available at http://yum.spacewalkproject.org/devel/ and http://yum.spacewalkproject.org/devel-client/ so it's possible to have installable Spacewalk based on other branch than master.

This page assumes that you have aready configured koji and tito including your koji certificate. Configuration can be found there: https://fedorahosted.org/spacewalk/wiki/ReleaseProcess

=== workflow ===

 * Create pull request branch (let it be called pr-branch) and push some commits into it.
 * Create new branch on top of pr-branch (called pr-branch-devel) and cherry-pick following commit fe14986915fb3d046563f54ffe99770584eac961 into pr-branch-devel

At this point everything should be set up for package tagging and building. Changes which are necessary for package building and package tags will be kept in separate branch so they won't have to be purged during process of accepting PR.

 * For each new package which has not yet been built from pr-branch-devel yet edit it's .spec file in following way:
{{{
diff --git a/reporting/spacewalk-reports.spec b/reporting/spacewalk-reports.spec
index 513b618..a88dfac 100644
--- a/reporting/spacewalk-reports.spec
+++ b/reporting/spacewalk-reports.spec
@@ -2,7 +2,7 @@ Name: spacewalk-reports
 Summary: Script based reporting
 Group: Applications/Internet
 License: GPLv2
-Version: 2.4.1
+Version: 2.4.100.dev
 Release: 1%{?dist}
 URL: https://fedorahosted.org/spacewalk
 Source0: https://fedorahosted.org/releases/s/p/spacewalk/%{name}-%{version}.tar.gz
}}}
 * Increase package version by a lot - devel tags do inherit from nightly which is likely to be more active as for package building and for one package built into devel tags there can be multiple nightly packages. We want devel packages to take precedence in dependency resolution so we need to increase package version. Also add .dev suffix into Version string - we need this to keep tags unique and to mark package as devel.
 * After package version has been increased and .dev suffix has been added you can tag and build packages like in nightly. Make sure your tags stay in pr-branch-devel.
 * Tag package: 
{{{
tito tag
}}}
 * and continue with building:
{{{
tito release --all
}}}

 * If you want to update packages simply rebase pr-branch-devel on top of pr-branch, tito tag and tito release --all.


=== failing builds ===

You may encounter failing build (most likely on Fedoras) due to 404 in root.log, this is caused by updated package landing in Fedora repositories which are necessary for build process. As Fedora repositories keep only latest package in repo (unlike rhel which keeps all package versions). You will have to force regeneration of build repository by running
{{{
koji -c $PATH_TO_KOJI_CONFIG regen-repo spacewalk-devel-$OS_VERSION-build
}}}
where PATH_TO_KOJI_CONFIG is path to your koji configuration file and $OS_VERSION is one of the following: rhel6, rhel7, fedora21, fedora22. After regeneration of repository resubmit the build either via koji web interface or on commandline by koji resubmit command.


If build fails due to checkstyle, pylint or other buildtime related stuff you can find this information in build.log, these are usually not koji related but code related and can be fixed without koji maintainers assistance.

If you're unsure what happened please ping tkasparek on #spacewalk-devel.