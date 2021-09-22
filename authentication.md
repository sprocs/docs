# Authentication

## spawn-cli

The easiest and initial way to login to a sprocs app is with the [spawn
CLI](https://github.com/sprocs/spawn-cli).

The spawn CLI is node script that generates an AWS STS temporary credential,
assuming the role of either an `admin` or `user` depending on your AWS user/profile
access level.

The temporary credentials are passed to the frontend sprocs web UI to grant a
temporary (default 8 hour) session to manage/use your app.

## SAML

To setup multi-user access or easy access to sprocs apps via existing corporate
SSO, you can setup most sprocs apps as a SAML service provider.

SAML has been tested with [AWS Single Sign-On](https://aws.amazon.com/single-sign-on/), Okta, GSuite, and Ping Identity.

To accept SAML logins, setup the following variables in [AWS System Manager](https://aws.amazon.com/systems-manager/) Parameter Store.

To setup one of the following parameters in the Parameter Store:
1. Navigate to `Systems Manager` in the AWS Console
2. Navigate to `Parameter Store` in the sidebar
3. Select `Create parameter` button
4. As `Name` specify the sprocs app name and target Amplify backend environment
   in the format: `/sprocs/sprocsappname/myenv/ENV_VAR`, for example: `/sprocs/raincloud/main/SAML_CERT`
5. Select String or SecureString (use default current account KMS if requested)
6. Input value as described below (no quotations)
7. Save

#### SAML_CERT

* **Required**
* Encoded certificate, ex: `MIIDqDCCApCgAwIBAgIGAXva...==`
* Exclude BEGIN/END certificate markers

#### SAML_ENTRYPOINT

* **Required**
* Entrypoint provided by idP
* Example: `https://dev-123.okta.com/app/dev-123/xyz/sso/saml`

#### SAML_ISSUER

* **Required**
* Issuer provided by idP
* Example: `http://www.okta.com/xyz`

#### SAML_AUDIENCE

* **Required**
* Your sprocs app main API Gateway URL
* Example: `https://xyz123.execute-api.us-east-2.amazonaws.com`
* You must supply this value to your iDP during application setup and also
specify this variable in your Parameter Store.
* You can find you API Gateway URL by navigating to `API Gateway -> APIs` and
finding the API that matches your Amplify app environment and sprocs app name,
ie: `raincloudApi-dev`

#### SAML_ADMIN_GROUP

* *Optional*
* Which group in the `groups` assertion should grant access to the sprocs app's respective admin role
* Example: `my-org-admins`

#### SAML_USER_GROUP

* *Optional*
* Which group in the `groups` assertion should grant access to the sprocs app's respective user role
* Example: `developers`

#### SAML_DEFAULT_ACCESS

* *Optional*
* If no group is matching or none are setup, what is default access level for a
successfully authenticated SAML user. Valid values are `USER` or `ADMIN` or
no value.
* Example: `USER`

#### SAML_ADMIN_BOOLEAN_ATTRIBUTE

* *Optional*
* Specify a SAML assertion attribute that should be parsed as a boolean to
determine admin role access. If you have an attribute called `isAdmin` with
boolean value `true` or `false` you could use this for instance.
* Example: `isAdmin`

#### SAML_USER_BOOLEAN_ATTRIBUTE

* *Optional*
* Specify a SAML assertion attribute that should be parsed as a boolean to
determine user role access. If you have an attribute called `isRaincloudUser` with
boolean value `true` or `false` you could use this for instance.
* Example: `isRaincloudUser`

## AWS Single Sign-On Setup

[AWS SSO](https://aws.amazon.com/single-sign-on/) provides an AWS integrated single sign-on
experience. You can setup to be powered by Active Directory or other idP
including managing the users/groups manually within AWS SSO.

To setup a sprocs app within AWS SSO:

1. Sign-in to AWS Console with root account credentials to setup AWS SSO (an AWS
   requirement)
2. Navigate to the `AWS Single Sign-On` service
3. Setup AWS SSO and users/groups if you have not (follow relevant AWS
   instructions to do so)
4. Setup a new sprocs app by selecting `Applications -> Add a new application ->
   Add a custom SAML 2.0 application`
5. Name you application appropriately, ex: `sprocs raincloud`
6. Copy the `AWS SSO sign-in URL` as the `SAML_ENTRYPOINT` Parameter listed
   above
7. Copy the `AWS SSO issuer URL` as the `SAML_ISSUER` Parameter listed
   above
8. Download the certificate and encode into the `SAML_CERT` Parameter listed
   above. You can alternatively select `Download` on the metadata file and copy the encoded certificate string in the `<ds:X509Certificate>` XML node.
9. In the `Relay state` field, you can input the root URL of your sprocs app
   frontend, example: `https://appid123.amplifyapp.com` (or custom domain if
   setup). This will be the location where a successful authentication will
   redirect with temporary credentials.
10. Under `Application metadata` select `If you don't have a metadata file, you can manually type your metadata values.`
11. Determine your sprocs app API Gateway endpoint. You can find you API Gateway URL by navigating to `API Gateway -> APIs` and finding the API that matches your Amplify app environment and sprocs app name. Your endpoint will look like `https://api123.execute-api.us-east-2.amazonaws.com`.
12. For `Application ACS URL` input your sprocs app API Gateway endpoint with the path of `/public/auth/saml_callback`, example: `https://api123.execute-api.us-east-2.amazonaws.com/public/auth/saml_callback`.
13. For `Application SAML audience` use your API Gateway endpoint, example: `https://api123.execute-api.us-east-2.amazonaws.com`
14. Save
15. Select `Attribute mappings` tab
16. For `Subject`, map to `${user:email}` with format `emailAddress`
17. Add attribute `email`, map to `${user:email}` with format `basic`
18. Add attribute `givenName`, map to `${user:givenName}` with format `basic`
19. Add attribute `familyName`, map to `${user:familyName}` with format `basic`
20. Add attribute `groups`, map to `${user:groups}` with format `unspecified`
21. Save changes

## Alternative SAML idPs

Required SAML assertion attributes:
* Set subject/nameid to `email` or a different unique ID
* `email`

Optional assertion attributes
* `givenName`
* `familyName`
* `groups`

### Okta

Sample configuration:
```
SAML_CERT="MIIDqDCCApCgAwIBAgIGAXva..G5m4QqZ/ZlOdctwSy114BN2xTYw4DTTJF/x8WQg=="
SAML_ENTRYPOINT="https://dev-123.okta.com/app/dev-123/exk1rtnesh5ID9kV123/sso/saml"
SAML_ISSUER="http://www.okta.com/exk1rtnesh5ID9kV123"
SAML_AUDIENCE="https://api123.execute-api.us-east-2.amazonaws.com"
SAML_ADMIN_GROUP="sprocs-admins"
SAML_USER_GROUP="sprocs-users"
```

[Create SAML app integrations using AIW](https://help.okta.com/en/prod/Content/Topics/Apps/Apps_App_Integration_Wizard_SAML.htm)

### GSuite

You can [setup GSuite as an AWS SSO idP](https://aws.amazon.com/blogs/security/how-to-use-g-suite-as-external-identity-provider-aws-sso/#:~:text=If%20your%20organization%20is%20using,accounts%20governed%20by%20AWS%20Organizations.) or you can add a sprocs app as an [application in GSuite](https://support.google.com/a/answer/6087519?hl=en)

```
SAML_CERT="MIIDqDCCApCgAwIBAgIGAXva..G5m4QqZ/ZlOdctwSy114BN2xTYw4DTTJF/x8WQg=="
SAML_ENTRYPOINT="https://accounts.google.com/o/saml2/idp?idpid=C01hv123"
SAML_ISSUER="https://accounts.google.com/o/saml2/idp?idpid=C01hv123"
SAML_AUDIENCE="https://api123.execute-api.us-east-2.amazonaws.com"
```

GSuite does not pass groups in SAML assertions so either use a protected attribute to
grant access or you can set `SAML_DEFAULT_ACCESS` = `USER` and grant access to
all gsuite users and only assign the application to appropriate users in gsuite.
