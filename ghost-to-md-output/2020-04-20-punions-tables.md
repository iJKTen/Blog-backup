---
title: Punions Tables
slug: punions-tables
date_published: 2020-04-20T16:41:00.000Z
date_updated: 2020-04-30T11:26:01.000Z
tags: Punions
excerpt: Puions NOSQL document storage
---

The first thing I need to do is create two DynamoDB NOSQL tables called Cards and Games. 

> Update: I will be deploying everything on AWS using [serverless](https://www.serverless.com) so please use this post as an example on how to use the AWS Management Console when it comes to creating DyanmoDB tables. 

From your AWS Management Studio search for DynamoDB and select to open
![](/content/images/2020/04/image-60.png)![](/content/images/2020/04/image-61.png)Click on Create table![](/content/images/2020/04/image-62.png)Enter table name as Cards, primary key as id of type Numner and click on Create
It will take up to 10 seconds for your table to be created and once created click on the Items tab
![](/content/images/2020/04/image-63.png)
Create 10 JSON documents which will show up like below and remember this is a NOSQL Document storage so you can add your own fields after defining your id field
![](/content/images/2020/04/image-64.png)
Similary create a table to hold our games and I am going to call it Games
![](/content/images/2020/04/image-65.png)We won't create any data or attributes for Games because that will be created by our Lambda functions
Now we are ready to create our Lambda functions behind a REST API and Web Socket.
