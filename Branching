= Branching =
Our basic branching philosophy is: 
 1. the main development and bugfixes go into '''master'''
 2. when stabilizing code for a release, cut a branch from '''master'''
 3. branch as late as possible
 4. when committing code for a release, the code goes in the release branch(es) (if any) and '''master'''.
 5. experimental stuff and big changes might have their own branches

See [https://github.com/spacewalkproject/spacewalk/branches branches] for the most up to date branches.

The way we work right now, you will check everything into '''master'''. If there is a branch for the target release(s), you should check the code into the branch(es) as well. If you have code that needs to go in a release between the latest branch and '''master''', then you need to have someone create a branch. 

So:

The current version is x.y. It was built from the RELEASE-x.y branch.

{{{
            ---RELEASE-x.y-----------------------------------
           /   (Changes for release x.y and beyond go here)
          /
         /
-master---/---(All changes go here)-------------------
             Releases x.y+1 (or x+1.0) will be built from this code
             eventually.
}}}

An example:

Development for the 0.6 release is proceeding. Code is being checked into '''master''', and there are no active branches. The {{{Version:}}} in the spec files in '''master''' contain {{{0.6.x 1%{?dist}.}}} Eventually 0.6 becomes stable enough that we are ready to build RPMS, or a developer is ready to work on release 0.7. 

So, we create the branch. There may be point releases, so we name the branch appropriately.
{{{
git checkout master
git pull (make sure things are clean)
git checkout -b spacewalk-0.6
git push origin spacewalk-0.6:refs/heads/SPACEWALK-0.6
git checkout --track -b my-localbranch origin/SPACEWALK-0.6
}}}

SPACEWALK-0.6 branch will then be used to build any 0.6 point releases (including .0).

So far so good. One problem: The {{{Version:}}} in the spec files in the branch and in '''master''' are identical! If we build RPMs from both the branch and '''master''', we will end up with version conflicts, and be very sad. Now we can go back to '''master''' and fix the {{{Version:}}} in the spec files there. 

All code committed to release branches must also be included in master. (git cherry-pick and git merge very useful for this). For help with the git commands see GitGuide