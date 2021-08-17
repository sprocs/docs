# Security

sprocs apps create AWS IAM roles/profiles during AWS Amplify deployment (via CloudFormation) with only necessary permissions to resources used (and often created) by the app and app environment. You can review these permissions in the CloudFormation templates located in `amplify/backend/...` in each respective repository.

For added security/visibility, we recommend [creating an AWS subaccount](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts_create.html) via `AWS Organizations` to isolate your sprocs.

Potential security vulnerabilities can be reported directly to us at `security@sprocs.com`.

