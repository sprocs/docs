# Upgrading

* If deployed to target the `main` branch, sprocs apps are updated to new versions via continuous deployment automatically on commits to the `main` branch.
* If deployed to target a specific release branch (ie. `1.0.1`), sprocs apps are updated to new versions by manually changing the target release branch to a new release version in the AWS Amplify Console app settings.
* If deployed via forking a sprocs app, sprocs apps are updated when you fetch
upstream changes and merge with your own. See [Syncing a fork](https://docs.github.com/en/github/collaborating-with-pull-requests/working-with-forks/syncing-a-fork)
