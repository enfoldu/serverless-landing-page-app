<!--
title: AWS Landing Page Application example in NodeJS
description: This example demonstrates how to setup a Landing Page Application.
layout: Doc
-->
# Landing Page Application

This script builds a complete landing page with everything need, out of the box. All the content is served via HTTPS only. HTTP requests are redirected to HTTPS.

S3 in combination with CloudFront, Route 53, and ACM (AWS Certificate Manager). S3 is used to store our static HTML file while CloudFront is responsible for making it available via Amazon's Content Delivery Network. ACM provides the free SSL certificates for your domain. We then use Route 53 to add the proper domains.

Any traffic to www is redirected to the root domain. So www.your_domain_name.com will redirect to your_domain_name.com.

## Prerequisite

The `serverless-single-page-app-plugin` in this example requires the Serverless Framework version 1.2.0 or higher and the AWS Command Line Interface. Learn more [here](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) on how to install the AWS Command Line Interface.

## Setup

Replace the name under domain in `serverless.yaml` which you can find inside the `custom` section. There is a placeholder text `your_domain_name.com`. Use only root a domain:

[Accepted]
- your_domain_name.com
- your_domain_name.org

[Incorrect]
- sub.your_domain_name.com
- www.your_domain_name.org

Since this plugin uses a custom Serverless plugin you need to setup the `node_modules` by running:

```bash
npm install
```

The `serverless-single-page-app-plugin` plugin in this example is there to simplify the experience using this example. It's not necessary to understand the plugin to deploy your Landing Page Application.

You must have an email for the domain you use in order to accept the ACM certificate (SSL Cert). Amazon will you email you at all of these addresses to prove ownership of the domain:

- administrator@your_domain_name
- hostmaster@your_domain_name
- postmaster@your_domain_name
- webmaster@your_domain_name
- admin@your_domain_name

# Deploy

Warning: Whenever you making changes to CloudFront resource in `serverless.yml` the deployment might take a while e.g 15-20 minutes.

In order to deploy the Landing Page Application you need to setup the infrastructure first.

For Development:

```bash
serverless deploy
```
Note: all resources will have "dev" appended to it.


For Production:

```bash
serverless deploy --stage prod
```

Note: all resources will have "prod" appended to it.

Now you must accept the ACM Certificate to complete the deployment. Login to one of the email address under the #Setup section of this READEME and click the link provided in the email.

The expected result should be similar to:

```bash
Serverless: Packaging service...
Serverless: Uploading CloudFormation file to S3...
Serverless: Uploading artifacts...
Serverless: Validating template...
Serverless: Updating Stack...
Serverless: Checking Stack update progress...
...........................
Serverless: Stack update finished…

Service Information
service: serverless-simple-http-endpoint
stage: dev
region: us-east-1
api keys:
  None
endpoints:
  None
functions:
  None
```

After this step your S3 bucket and CloudFront distribution is setup. Now you need to upload your static file e.g. `index.html` and `app.js` to S3. You can do this by running

```bash
serverless syncToS3
```

The expected result should be similar to

```bash
Serverless: upload: app/index.html to s3://yourBucketName123/index.html
Serverless: upload: app/app.js to s3://yourBucketName123/app.js
Serverless: Successfully synced to the S3 bucket
```

Hint: The plugin is simply running the AWS CLI command: `aws S3 sync app/ s3://yourBucketName123/`

Now you just need to figure out the deployed URL. You can use the AWS Console UI or run

```bash
sls domainInfo
```

The expected result should be similar to

```bash
Serverless: Web App Domain: dyj5gf0t6nqke.cloudfront.net
```

Visit the printed domain domain and navigate on the web site. It should automatically redirect you to HTTPS and visiting <yourURL>/about will not result in an error with the status code 404, but rather serves the `index.html` and renders the about page.

- Built on <a href="https://github.com/serverless/examples/tree/master/aws-node-single-page-app-via-cloudfront">Single Page Application</a>