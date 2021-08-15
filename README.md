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

Copyright (c) 2021 Kaytos, LLC dba sprocs
