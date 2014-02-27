= Downloads =
{{{
#!html
<!-- do you see this -->
}}}
== Binary ==

Releases: The binaries for Fedora and RHEL and derivates can be downloaded from the [http://yum.spacewalkproject.org/ yum repositories]. Please [HowToInstall#SettingupSpacewalkrepo follow installation information] on how to best setup yum to have dependencies resolved properly.

Nightly Builds: Composes of packages built for the upcoming release are generated a couple of times per day and available as [HowToInstall#Nightlybuilds nightly builds].

== Source ==

=== Getting source code with git ===

Spacewalk uses [http://git.or.cz git] for source control.

To checkout the latest version from git:

{{{
git clone git@github.com:spacewalkproject/spacewalk.git/
}}}

If you have a firewall that is blocking the git clone operation, you can also clone over http, although it is slower:

{{{
git clone https://github.com/spacewalkproject/spacewalk.git/
}}}

You can view the source via [https://github.com/spacewalkproject/spacewalk github]

=== Commiter Git Information ===

In general, we want folks to send [PatchProcess github pull requests].  If you have commit access to Spacewalk, however, you can use either of the above URLs to push your changes. The https URL forces you to type your username / password each time, whereas if you use the git URL you can upload your ssh keys to github and not have to authenticate each time.

=== Source RPMs ===

Source packages are available from [http://yum.spacewalkproject.org/]version/OS/release/source directories.

== RPM Package Signing ==

We use a number of GNU Privacy Guard (GPG) keys to sign our software packages. The necessary public keys are included in relevant products and are used automatically to verify software updates. You can also check the packages manually using the keys on this page.

To verify a RPM package for Spacewalk, run the command
{{{
rpm --checksig -v <filename>.rpm
}}}

The output of this command will show you if the package is signed, and which key was used to sign it.

Release Package Signing
{{{
Key ID: b3892132
Name: RPM-GPG-KEY-spacewalk-2010
Fingerprint: 7AB4 0878 5D10 5907 0572  F465 ED63 5379 B389 2132
}}}

The public key is here: [http://yum.spacewalkproject.org/RPM-GPG-KEY-spacewalk-2010]