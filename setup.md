# Setup

## Deployment

Most sprocs apps are deployed using [AWS Amplify Console](https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html) which allows you to simply specify a git repository and Amplify will provision all necessary AWS resources to run the app and host the frontend within your own AWS account.  Amplify Console provides a UI within AWS Console to list and manage resources, environments, environment variables, Cognito users/groups, enable/view logs, etc.

Once deployed, Amplify provides an admin UI to view your backend (APIs,
functions, authentication, DynamoDB tables, etc.) as well as your frontend build
status. From this UI, you can also setup a custom domain, setup logging, set environment variables to toggle specific
features, and more.

Updates can be automated by setting up continuous deployment to auto deploy upon new commits to the git repository branch specified. Alternatively, for more control, you can specify a specific brand/tag and update the target branch manually when you want to update. See [Deployment](deployment.md)

Amplify can also spin up separate environments for each branch to stage updates if desired.

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
