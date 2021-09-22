# Troubleshooting

## AWS Credential Setup

#### `UnhandledPromiseRejectionWarning: ConfigError: Missing region in config`

If you experience this error when running `npx` commands locally (for example when using `wormhole` or `spawn` clients):

* If using `AWS_PROFILE`, set `export AWS_SDK_LOAD_CONFIG=true` in your .bashrc/.zshrc profile or on the command line. This loads your region from `~/.aws/config`
* If that does not fix the issue, set `AWS_REGION` in your bash/zsh profile (`export
AWS_REGION=us-east-1`) or on the command line directly `AWS_REGION=us-east-1
AWS_PROFILE=my-profile npx @sprocs/spawn ...`
