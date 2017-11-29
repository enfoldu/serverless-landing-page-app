<!--
title: AWS Landing Page Application example in NodeJS
description: This example demonstrates how to setup a Landing Page Application.
layout: Doc
-->
# Landing Page Application

This script builds a complete landing page with everything need, out of the box. All the content is served via HTTPS only and any HTTP requests are redirected to HTTPS.

S3 in combination with CloudFront, Route 53, and ACM (AWS Certificate Manager). S3 is used to store our static HTML file while CloudFront is responsible for making it available via Amazon's Content Delivery Network. ACM provides the free SSL certificates for your domain. We then use Route 53 for DNS.

Any traffic to www is redirected to the root domain. So www.your_domain_name.com will redirect to your_domain_name.com.

## Prerequisite

- `aws-cli`
- Route 53 hosted zone created for your_domain_name.com
- One of these emails for your domain.
    - administrator@your_domain_name
    - hostmaster@your_domain_name
    - postmaster@your_domain_name
    - webmaster@your_domain_name
    - admin@your_domain_name

## Setup

`cd` into your project directory and run `npm install --save serverless-s3-sync`

Rename `env.json.example` to `env.json` and replace all variables as needed.

# Deploy

Warning: Whenever you making changes to CloudFront resource in `serverless.yml` the deployment might take a while e.g 15-20 minutes.

In order to deploy the Landing Page Application you need to setup the infrastructure first.

```bash
sls deploy
```

Now you must accept the ACM Certificate to complete the deployment. Login to one of the email address and click the link provided.

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
stage: prod
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
sls s3sync
```

Note: All files within the folder provided under `appDirectory` in `env.json` will be synced to your s3.

The expected result should be similar to

```bash
S3 Sync: Syncing directories and S3 prefixes...
....
S3 Sync: Synced.
```

Now go to your_domain_name.com in your web browser. All done!