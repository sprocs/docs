# Setup

## Deployment Overview

Most sprocs apps are deployed using [AWS Amplify Console](https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html) which allows you to simply specify a git repository and Amplify will provision all necessary AWS resources to run the app and host the frontend within your own AWS account.  Amplify Console provides a UI within AWS Console to list and manage resources, environments, environment variables, Cognito users/groups, enable/view logs, setup a custom domain, etc. similar to a PaaS like Heroku.

Under the hood, Amplify authenticates with read-only access to your forked sprocs repository on GitHub which will serve as your personal release branch. When you
make changes to your `main` branch (by default, see [Deployment Options](#deployment-options)), or other branch (if you setup your Amplify
app manually, see "Manual Setup"), Amplify will automatically generate and apply new
CloudFormation templates for relevant AWS services.

### Deployment Options

* [**"Deploy to Amplify Console" Button**](#deploy-to-amplify-console-button) (easiest)
   * easy UI-only path to spin up a new environment
   * update your Amplify app with the latest release by simply clicking `Fetch upstream` button in the GitHub UI of your forked repo
   * less flexibility to target a specific branch (will be pointed at `main`)
   * Amplify automatically forks the sprocs repo to your own GitHub account;
   forks on GitHub are public
* [**Deploy within Amplify Console Manually**](#deploy-within-amplify-console-manually)
   * setup in AWS Amplify Console within AWS Console
   * can specify a specific branch to target
   * Amplify automatically forks the sprocs repo to your own GitHub account;
   forks on GitHub are public
* [**Private Fork/Clone Deployment**](#private-forkclone-deployment)
   * use CLI git to clone and setup git remotes back to upstream sprocs repo
   * allows more flexibility for branch naming and upstream branch tracking
   (track multiple release branches)
   * repo can be private

#### "Deploy to Amplify Console" Button

1. On the original sprocs repo, click the "Deploy to Amplify Console" button
2. Login to AWS Console
3. Select `Connect to GitHub`
4. After authorization, select the `amplifyconsole-backend-role` or create a new
   role
5. Click `Save and deploy`

#### Deploy within Amplify Console Manually

1. Fork or clone a sprocs repo to your GitHub account
2. Navigate to `AWS Amplify -> Connect app (button)`
3. Select `GitHub`
4. Authorize read-only with `GitHub`
5. Select your forked/cloned sprocs repo and desired branch (likely `main` or
   `release` if you [setup your own custom release branch](#custom-release-branch))
6. Select or add an environment name (ie: `production` or `dev`)
7. Acknowledge AWS will perform continuous deployment when you fetch new
   upstream commits on your forked/cloned repo on the branch that is being
   deployed.
8. Click `Save and deploy`

#### Private Fork/Clone Deployment

If it is necessary to have a private fork, you can clone the sprocs
repo and add an upstream remote back to the original sprocs repo to fetch
updates. See [Configuring a remote for a fork](https://docs.github.com/en/github/collaborating-with-pull-requests/working-with-forks/configuring-a-remote-for-a-fork).

After you have your cloned repo setup on your GitHub account, you can follow the
instructions to [Deploy within Amplify Console Manually](#deploy-within-amplify-console-manually)

## Custom Release Branch

If you'd like to target specific release versions of upstream sprocs repos, we
recommend managing your own `release` branch and merge release tagged branches
into your `release` branch.

An example workflow:

1. Fork or clone a sprocs repo
2. Track upstream repo if not already setup (`git remote -v` will list remotes,
   verify `upstream` remote is setup to track original sprocs repo)
   * Setup upstream tracking (example: `git remote add upstream git@github.com:sprocs/wormhole.git`)
   * Fetch upstream to update release branches/tags: `git fetch upstream`
3. Create your own `release` branch:
   * `git checkout -b release`
   * List available release tags: `git tag`
   * Reset your new `release` branch to the release tag you wish to deploy, example: `git reset --hard 0.1.2`
   * Push (or force push if necessary) your new branch, example: `git push -u origin release`
   * The `release` branch on your cloned/forked repo now reflects the `0.1.2` release
4. Follow [Deploy within Amplify Console Manually](#deploy-within-amplify-console-manually) and setup Amplify to watch the `release` branch the you just setup

## Multiple Branches

You can setup multiple environments/frontend builds for separate branches by
selecting `Connect branch` in Amplify Console and linking a branch to an Amplify
environment.

This can be helpful to test a new release or other changes before
merging to `main`.

### Staging Changes/New Environments

To stage changes and spin up a new environment to test:

1. Create a remote branch
2. Select `Connect branch`
3. Select your remote branch
4. Select `Create new environment` (or an existing environment if you are sure no backend changes will be made)
5. Select `Save and deploy`
6. An entirely separate environment will be spun up using your branch, you can
   use it to test and then create an associated PR to track using your release
   branch as the base to merge into if the changes look good

## Troubleshooting

* Some regions are not yet supported by Amplify CLI [List](https://github.com/aws-amplify/amplify-cli/blob/master/packages/amplify-provider-awscloudformation/src/aws-regions.js). Supported regions:
   ```
   const regionMappings = {
      'us-east-1': 'US East (N. Virginia)',
      'us-east-2': 'US East (Ohio)',
      'us-west-2': 'US West (Oregon)',
      'eu-west-1': 'EU (Ireland)',
      'eu-west-2': 'EU (London)',
      'eu-central-1': 'EU (Frankfurt)',
      'ap-northeast-1': 'Asia Pacific (Tokyo)',
      'ap-northeast-2': 'Asia Pacific (Seoul)',
      'ap-southeast-1': 'Asia Pacific (Singapore)',
      'ap-southeast-2': 'Asia Pacific (Sydney)',
      'ap-south-1': 'Asia Pacific (Mumbai)',
      'ca-central-1': 'Canada (Central)',
   };
   ```
* [Contact us](team@sprocs.com) if you require assistance and we'll try our best
to help.

## AWS Budget Setup

To setup a simple cost budget monitor for your serverless components:

1. Navigate to `AWS Budgets` and select `Create budget`
2. Select `Cost budget` then `Next`
3. Set the desired budget amount to receive alerts and name your budget
4. If you are using a separate subaccount for your sprocs, you can just monitor
   the entire subaccount costs (as you cannot use billing tags on a subaccount).
   To monitor sprocs usage using billing tags, make sure you are on the "payer
   AWS account" (the root account) and make sure your sprocs tags are `active`
   by navigating to `Billing -> Cost allocation tags` and [activating](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/activating-tags.html)
   `sprocs_app` and `sprocs_env` tags if they are not already. This may take up
   to 24 hours for them to show in budget filtering options as per the [docs](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/activating-tags.html).
5. If you wish to budget just your sprocs usage with billing tags previously
   setup, add a Filter when creating your Budget and select `Tag` for dimension
   and filter by `sprocs_app` and/or `sprocs_env` as appropriate.
