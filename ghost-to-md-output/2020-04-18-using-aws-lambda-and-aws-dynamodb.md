---
title: Using AWS Lambda and AWS DynamoDb
slug: using-aws-lambda-and-aws-dynamodb
date_published: 2020-04-18T19:33:30.000Z
date_updated: 2020-04-18T19:38:29.000Z
tags: AWS, Lambda, DynamoDb
excerpt: AWS Lambda connecting to a NOSQL database.
---

AWS DynamoDB is a fully managed on cloud NOSQL key-value document storage system. We will see how we can connect to a DynamoDB table and use an AWS Lambda function to output data.

Sign up for an [AWS](https://aws.amazon.com) account which includes 12 Months of Free Tier Access.

### Setting up a NOSQL table in DynamoDB
![](/content/images/2020/04/image-35.png)Search for dynamo in your AWS Management Console![](/content/images/2020/04/image-37.png)On the next page, Click on Create table![](/content/images/2020/04/image-39.png)Enter a table name and id as the primary key and click on Create![](/content/images/2020/04/image-40.png)On the next page click on Items to see data in your table![](/content/images/2020/04/image-42.png)Click on Create item from the previous screen and from the popup change Tree to Text and enter the following JSON object![](/content/images/2020/04/image-43.png)I find it easier to write JSON and click on Save to create a row / item / document![](/content/images/2020/04/image-44.png)Create three documents / items / JSON objects as shown above
### Creating a Lambda function which reads data from the above table

Create a new Lambda function and add the following code. If this is your first time creating an AWS Lambda function then checkout this [article](/creating-a-lambda-function-in-aws-using-aws-console/). 

I have named my function ExampleFunctionDynamoDB and I am using NodeJS

    const AWS = require("aws-sdk");
    const documentClient = new AWS.DynamoDB.DocumentClient();
    
    exports.handler = async (event) => {
        const data = await documentClient.scan({TableName: "ExampleFunctionTable"}).promise();
        
        const response = {
            statusCode: 200,
            body: JSON.stringify(data),
        };
        return response;
    };
    

If you added a test to your above function and after running that test you will get an error which says something like Access denied and cannot connect to DynamoDB. 

For our lambda to connect to your DynamoDB table you need to create a rule which can access access your DynamoDb from your lambda function. 

### Creating a rule to access DynamoDB from Lambda
![](/content/images/2020/04/image-45.png)Click on Permissions from the above tab![](/content/images/2020/04/image-46.png)Click on the role below Role name which will open a new window showing you the role atached to this lambda function![](/content/images/2020/04/image-47.png)From the newly opened page click on Attach policies![](/content/images/2020/04/image-48.png)After clicking on Attach policies, enter Dynamodb to filter policies and select AmazonDynamoDBFullAccess and click on Attach policy at the bottom right corner of your page![](/content/images/2020/04/image-49.png)Running your test function again you should see the output above indicating that your Lambda can access DynamoDB
Download the code in this article from this [repository](https://github.com/iJKTen/test-aws-lambdas) on GitHub.
