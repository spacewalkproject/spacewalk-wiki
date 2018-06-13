## Release Cycles



Each Spacewalk release will be roughly two (6) months in duration.
See [Roadmap](https://hosted.fedoraproject.org/spacewalk/roadmap) for upcoming releases.

## Mechanics

We build Spacewalk rpms using [Fedora Copr Build Service](https://copr.fedorainfracloud.org/).

Builds in `master` branch are completely automated - once you commit your changes, tag the package
and push th changes upstream with 

        tito tag
        git push && git push --tags

an automatic build is issued via webhook from Github repo to Copr Build Service.             

## Branching a release

When we're nearing the end of a release cycle, we need to branch the release.


Make sure your `master` is clean and updated:

       git checkout master
       git pull (make sure things are clean)

Now create a local branch, where x.y is the release number, i.e. 0.2:

       git checkout -b spacewalk-x.y

Then, push your branch to the remote server, where x.y is the release number:

       git push origin spacewalk-x.y:refs/heads/SPACEWALK-x.y

Whenever doing any change and fixes in that branch, make sure they do not get
lost -- commit to master first and then `git cherry-pick -x` to that "release"
branch.
