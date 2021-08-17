# FAQ

* [AWS Pricing](#aws-pricing)
* [AWS Budget Setup](#aws-budget-setup)

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
