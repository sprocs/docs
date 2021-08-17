# Deployment

Most sprocs apps are deployed using [AWS Amplify Console](https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html) which allows you to simply specify a git repository and Amplify will provision all necessary AWS resources to run the app and host the frontend within your own AWS account.  Amplify Console provides a UI within AWS Console to list and manage resources, environments, environment variables, Cognito users/groups, enable/view logs, etc.

Updates can be automated by setting up continuous deployment to auto deploy upon new commits to the git repository branch specified. Alternatively, for more control, you can specify a specific brand/tag and update the target branch manually when you want to update.

Amplify can spin up separate environments for each branch to stage updates if
desired.

