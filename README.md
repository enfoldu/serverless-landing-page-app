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

- `aws-cli`
- Route 53 hosted zone created for your_domain_name.com

## Setup

Rename `env.json.example` to `env.json` and replace your_domain_name. For the "name" value under "domain" use only a root domain:

[Accepted]
- your_domain_name.com
- your_domain_name.org

[Incorrect]
- sub.your_domain_name.com
- www.your_domain_name.org

You must have an email for the domain you use in order to accept the ACM certificate (SSL Cert). Amazon will email you at all of these addresses to prove ownership of the domain:

- administrator@your_domain_name
- hostmaster@your_domain_name
- postmaster@your_domain_name
- webmaster@your_domain_name
- admin@your_domain_name

# Deploy

Warning: Whenever you making changes to CloudFront resource in `serverless.yml` the deployment might take a while e.g 15-20 minutes.

In order to deploy the Landing Page Application you need to setup the infrastructure first.

```bash
sls deploy
```

Note: all resources will have the "stage" value appended to it (e.g dev.your_domain_name.com).

Now you must accept the ACM Certificate to complete the deployment. Login to one of the email address under the #Setup section of this README and click the link provided in the email.

Afterwards the expected result should be similar to:

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
aws s3 sync app/ s3://yourBucketName123
```

The expected result should be similar to

```bash
upload: app/index.html to s3://yourBucketName123/index.html
upload: app/app.js to s3://yourBucketName123/app.js
```

Now you just need to go to your domain. You can use the AWS Console UI or run to view it

```bash
sls info --verbose
```

- Built on <a href="https://github.com/serverless/examples/tree/master/aws-node-single-page-app-via-cloudfront">Single Page Application</a>