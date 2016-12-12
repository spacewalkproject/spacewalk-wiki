## Patch Process



So you'd like to contribute to Spacewalk?   Great!

We are using github to host the code and manage patch submissions through the github pull request process. What you're going to want to do generally is:

1. [Fork](https://help.github.com/articles/fork-a-repo) the official Spacewalk repository in github (found on the [Downloads](DownloadIt) page).
1. Make your changes in your own git repository (read the git instructions in the [git guide](GitGuide)).
1. Submit a [pull request](https://help.github.com/articles/using-pull-requests) to Spacewalk from your fork.

If you are a returning contributor and already have checkout out the old fedorahosted git repository, then in step 2 of the forking guide instead of cloning the repository you can simply update your current one to point to your new github fork:

    git remote set-url origin git@github.com:username/spacewalk.git 
--or--

    git remote set-url origin https://github.com/username/spacewalk.git 

Changes should be made to the to master branch; we'll take care of applying them to other releases as appropriate.