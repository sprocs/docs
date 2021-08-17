## Overview

* [About](#about)
* [License & Copyright](#license--copyright)

## Docs

* [FAQ](faq.md)
* [Deployment](deployment.md)
* [Upgrading](upgrading.md)
* [Security](security.md)

## About

[sprocs](https://sprocs.com) develops easy-to-deploy and easy-to-maintain serverless apps for AWS.

* **Open source**
* **Easy to deploy**: click-through deployment with managed hosting via [AWS Amplify Console](https://aws.amazon.com/amplify/hosting/)
* **Easy to upgrade** via git: automated "deploy on commit" or controlled via branches
or forked repositories
* **Easy to run** via native AWS serverless services: no kubernetes, no linux
upgrades, no SSH keys...only AWS managed services
* **Control your own data and endpoints** in your own AWS account/region
* **Integrate your data** with native AWS services (access DynamoDB streams, trigger S3 events, handle SNS notifications,
subscribe to EventBridge events, etc)
* **Less third-party vendors** means less data processors and less uncontrolled access to you and your customers sensitive data
* **Improve security and privacy** by gaining visibility into your data policies and
access controls
* **Less SaaS** lock-in and pricing power over your data
* **Use your own domain and email** for increased brand recognition, deliverability, and
ownership
* **Multiple environments**: easily spin up multiple environments per git branch or use case
* **Free and commercial** apps

With so much SaaS acting as a thin reseller layer over AWS cloud services, we believe the
future of many generic SaaS offerings is managed open-source code, running on managed infrastructure, within customer cloud
accounts.

## License & Copyright

Most server side code is licensed under [Server Side Public License (SSPL) 1.0](https://www.mongodb.com/licensing/server-side-public-license) or a commercial license depending on the app.

Most client side code is licensed under [Apache 2.0](https://opensource.org/licenses/Apache-2.0).

See the respective sprocs app repository LICENSE for details.
=======
* [About](#about)
* [Deployment](#deployment)
* [AWS Pricing](#aws-pricing)
* [AWS Budget Setup](#aws-budget-setup)
* [Security](#security)
* [License & Copyright](#license--copyright)

## About

[sprocs](https://sprocs.com) develops both free and commercial serverless apps for AWS.

## Deployment

Most sprocs apps are deployed using [AWS Amplify Console](https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html) which allows you to simply specify a git repository and Amplify will provision all necessary AWS resources to run the app and host the frontend within your own AWS account.  Amplify Console provides a UI within AWS Console to list and manage resources, environments, environment variables, Cognito users/groups, enable/view logs, etc.

Updates can be automated by setting up continuous deployment to auto deploy upon new commits to the git repository branch specified. Alternatively, for more control, you can specify a specific brand/tag and update the target branch manually when you want to update.

Amplify can spin up separate environments for each branch to stage updates if
desired.

## AWS Pricing

sprocs app usage will generate an AWS bill that is your responsibility (likely small for casual use but do your own diligence).

sprocs apps setup the following AWS tags on resources it creates `sprocs_app = APP_NAME` and `sprocs_env = AMPLIFY_ENV_HERE` for billing reporting purposes.

Setup `AWS Budget` notifications to monitor for unexpected serverless costs.

By using only serverless AWS resources, you only pay for what you use and not for idle time. Comparable SaaS offerings are almost always magnitudes more costly.

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

## Security

sprocs apps create AWS IAM roles/profiles during AWS Amplify deployment (via CloudFormation) with only necessary permissions to resources used (and often created) by the app and app environment. You can review these permissions in the CloudFormation templates located in `amplify/backend/...` in each respective repository.

For added security/visibility, we recommend [creating an AWS subaccount](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts_create.html) via `AWS Organizations` to isolate your sprocs.

Potential security vulnerabilities can be reported directly to us at `security@sprocs.com`.

## License & Copyright

Most server side code is licensed under [Server Side Public License (SSPL) 1.0](https://www.mongodb.com/licensing/server-side-public-license) or a sprocs commercial license depending on the app.

Most client side code is licensed under [Apache 2.0](https://opensource.org/licenses/Apache-2.0).

Please see the respective sprocs app repository LICENSE for details.

Copyright (c) 2021 Kaytos, LLC dba sprocs
