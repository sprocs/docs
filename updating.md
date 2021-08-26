# Updating

Amplify app environments are pointed to a branch on your GitHub fork or clone of
a sprocs repo (see [Setup](setup.md) for more info). When new commits occur on
that branch, your Amplify app environment is automatically updated. Because of
this, updating your sprocs app version is as simple as updating your fork to
reflect upstream sprocs changes.

## Updating Standard Forked Repo

Assuming you forked a sprocs repo with the "Deploy to Amplify Console" button
and have an Amplify app that is tracking the `main` branch on your forked repo
on GitHub, you can update by simply navigating to your forked repo on GitHub and
selecting [Fetch upstream](https://docs.github.com/en/github/collaborating-with-pull-requests/working-with-forks/syncing-a-fork)

If your Amplify app is tracking a different branch or is setup in a custom way,
just fetch the upstream changes by tracking an upstream release branch (ex:
`release-0.1.0`) and creating a pull request to merge those changes into the
branch your Amplify app is setup to track. See "Keeping a fork up to date" link
for some helpful tips.

## Updating Custom Release Branch

If you setup a [Custom Release Branch](setup.md#custom-release-branch) you can
manually update your release branch by:

1. Checkout your `release` branch
2. Fetch upstream changes `git fetch upstream`
3. List release tags `git tags`
4. You can diff changes with `git diff release 0.1.2`
5. Reset your release tag to reflect a specific release `git reset --hard 0.1.2` (you can alternatively create a pull request to merge in changes from a release branch or any other way to merge in desired changes)
6. Force push changes `git push origin release -f` if using `reset` or push if
   you merged or rebased changes.
7. If you reset and force pushed, login to AWS Amplify Console, navigate to your app, select your `release` branch environment under `Frontend environments`, click the `Redeploy this version` button if the build from your most recent push was not picked up (as is the case with the reset method)


## Links

Some helpful links on updating forked repos

* [Syncing a fork](https://docs.github.com/en/github/collaborating-with-pull-requests/working-with-forks/syncing-a-fork)
* [Keeping a fork up to date](https://gist.github.com/CristinaSolana/1885435#gistcomment-2857738) (comment with good instructions for CLI or browser)
* [Configuring a remote for a fork](https://docs.github.com/en/github/collaborating-with-pull-requests/working-with-forks/configuring-a-remote-for-a-fork)
