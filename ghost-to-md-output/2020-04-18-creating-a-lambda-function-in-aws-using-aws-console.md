---
title: Creating a Lambda function in AWS using AWS Management Console
slug: creating-a-lambda-function-in-aws-using-aws-console
date_published: 2020-04-18T18:30:20.000Z
date_updated: 2020-04-18T19:37:27.000Z
tags: AWS, Lambda
excerpt: Creating an AWS Lambda function via AWS Management Console
---

With AWS Lambda you can run code without a server and you can call your lambda function with a URL among many other ways. 

Sign up for an [AWS](https://aws.amazon.com) account which includes 12 Months of Free Tier Access.

Let's create a simple function which returns a static JSON object. 
![](/content/images/2020/04/image.png)From the AWS Management Console search for Lambda![](/content/images/2020/04/image-1.png)Clicking on Create function will create a function in N. Virginia but don't worry you can cal this function from anywhere![](/content/images/2020/04/image-2.png)Select Author from scratch and enter Function name as ExampleFunction and click on Create function![](/content/images/2020/04/image-3.png)Under Designer selecting your function name will display the source code below![](/content/images/2020/04/image-4.png)Write your NodeJS code in the editor![](/content/images/2020/04/image-5.png)Test your function![](/content/images/2020/04/image-6.png)Click on the Test button to test your function![](/content/images/2020/04/image-7.png)Successful test should show you this output
How do I call this function from a URL? We now have to create an API by using AWS API Gateway. The function we created does not accept query string parameters or any post data so this function can be called with a GET HTTP verb, meaning you can call it by visiting a URL in your browser which is what we will do. 
![](/content/images/2020/04/image-8.png)From the AWS Management console search for API and select API Gateway![](/content/images/2020/04/image-9.png)We are creating our API in the same region where our function was created![](/content/images/2020/04/image-10.png)We are going to create a REST API so click on Build![](/content/images/2020/04/image-11.png)Select REST under protocol, NEw API under Create new API and give your API name like ExampleFunctionAPI and click on Create API![](/content/images/2020/04/image-21.png)Instead of calling our lambda function at the root (/) we are going to create a resource /examplefunction so we can scope it better and this way we can call another lambda at /anotherlambda![](/content/images/2020/04/image-22.png)Create a resource as shown above![](/content/images/2020/04/image-23.png)Since our lambda will be called at a GET request ![](/content/images/2020/04/image-24.png)Select GET and save![](/content/images/2020/04/image-25.png)
In the above screenshot check Use Lambda Proxy Integration if you want AWS to pass dynamic resource from your URL. I always check this. Select your Lambda function by typing "ExampleFunction" in Lambda Function and select your function and click on Save
![](/content/images/2020/04/image-26.png)AWS wants to add permission to invoke your function so click on OK![](/content/images/2020/04/image-28.png)This diagram shows how our lambda function is executed when visiting a URL
The last step is to deploy your API so we can test it. We can deploy our URL at a path like /development, /staging, or /production so we can test our functions independently. To deploy your API click on Actions -> Deploy API
![](/content/images/2020/04/image-29.png)![](/content/images/2020/04/image-30.png)Deploy your API at a new stage and call it development
Go back to your lambda function and select API Gateway which will show you the URL where your lambda function can be called
![](/content/images/2020/04/image-31.png)![](/content/images/2020/04/image-32.png)![](/content/images/2020/04/image-33.png)Clicking on the URL you will see the JSON in the browser
Download the code seen here from this [repository](https://github.com/iJKTen/test-aws-lambdas) on GitHub.
