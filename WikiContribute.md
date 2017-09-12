# Spacewalk wiki

## How to contribute to Spacewalk wiki

1. Make sure you have GitHub account.
2. Fork [spacewalkproject/spacewalk-wiki](https://github.com/spacewalkproject/spacewalk-wiki) into your account.
3. Make changes in your forked `spacewalk-wiki` repository.
4. Open new PR for [spacewalkproject/spacewalk-wiki](https://github.com/spacewalkproject/spacewalk-wiki).

## How to test your changes before opening PR

1. Fork [spacewalkproject/spacewalk](https://github.com/spacewalkproject/spacewalk) into your account.
2. Go to `https://github.com/<youraccount>/spacewalk/wiki` and click **Create the first page** and initialize wiki repository with some content.
3. Clone your forked spacewalk-wiki repository to your workstation.

        $ git clone git@github.com:<youraccount>/spacewalk-wiki.git

4. Go to the `spacewalk-wiki` repository and add a remote called `wiki` pointing to the `spacewalk.wiki` repository.

        $ cd spacewalk-wiki
        $ git remote add wiki git@github.com:<youraccount>/spacewalk.wiki.git

5. Make some new changes in `test` branch.

        $ git checkout -b test
        ...
        $ git commit

6. Publish `test` branch changes to `spacewalk.wiki` repository.

        $ git push -f wiki test:master

7. Check your changes on web - `https://github.com/<youraccount>/spacewalk/wiki`.
8. Now you can even edit wiki pages using GitHub web UI and then pull changes back to your working copy.

        $ git pull wiki master

9. When you are happy with changes in your working copy, publish these changes to forked `spacewalk-wiki` repository and open PR from `test` branch.

        $ git push -f origin test:test