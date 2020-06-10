---
title: Setting up AWS CLI
slug: setting-up-aws-cli
date_published: 2020-04-19T21:39:32.000Z
date_updated: 2020-04-19T21:39:32.000Z
tags: AWS
excerpt: Setting up AWS CLI to work with AWS resources
---

In the past two articles we created our lambdas using the AWS Management Console which is a browser based interface. It would be better if can write code locally and push it to AWS and this can be done via the AWS CLI. 

Download the AWS CLI from [here](https://aws.amazon.com/cli/)

Once you have it installed for your operating system you can check if it is working or not by running 

    aws --version

Now we have to authenticate your machine so it can access AWS resources. Follow instructions from Amazon [here](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html#cli-quick-configuration)

Once you have completed the steps above run this command to check if everything is working correctly

    aws s3 ls

You are not ready to create a lambda project locally and push it.
