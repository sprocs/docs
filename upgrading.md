# Upgrading

Amplify app environments are pointed to a branch on your GitHub fork or clone of
a sprocs repo (see [Setup](setup.md) for more info). When new commits occur on
that branch, your Amplify app environment is automatically updated. Because of
this, updating your sprocs app version is as simple as updating your fork to
reflect upstream sprocs changes.

Assuming you forked a sprocs repo with the "Deploy to Amplify Console" button
and have an Amplify app that is tracking the `main` branch on your forked repo
on GitHub, you can update by simply navigating to your forked repo on GitHub and
selecting [Fetch upstream](https://docs.github.com/en/github/collaborating-with-pull-requests/working-with-forks/syncing-a-fork)

If your Amplify app is tracking a different branch or is setup in a custom way,
just fetch the upstream changes by tracking an upstream release branch (ex:
`release-0.1.0`) and creating a pull request to merge those changes into the
branch your Amplify app is setup to track.

Some helpful links on updating forked repos:

* [Syncing a fork](https://docs.github.com/en/github/collaborating-with-pull-requests/working-with-forks/syncing-a-fork)
* [Keeping a form up to date](https://gist.github.com/CristinaSolana/1885435#gistcomment-2857738) (comment with good instructions for CLI or browser)
* [Configuring a remote for a fork](https://docs.github.com/en/github/collaborating-with-pull-requests/working-with-forks/configuring-a-remote-for-a-fork)
