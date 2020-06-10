---
title: My approach to projects
slug: my-approach-to-projects
date_published: 2020-04-20T02:27:35.000Z
date_updated: 2020-04-21T02:32:07.000Z
tags: Punions
excerpt: My approach on working on projects and how I get into the mindset
---

Punions app has a lot of things going on with different technologies like AWS DyamoDB which is a NOSQL documet storage like MongoDB but on cloud and managed by Amazon. Then we have lambda which are functions running on cloud behind a URL. Then you also have Web Sockets which can send messages to clients. then you have the React client which will run as a static site on S3. If the app is running on S3 so we need to handle CORS as well. 

It happens to me and it may happen to you. You feel overwhelmed by the amount of things you have to do and different technnologies that you may not know a lot about. 

I always remember that few things will be easy and few things will be tough. I remember that my self confidence increases with each task I finish, no matter how small the task may seem. 

Focus on one small thing at a time. Don't be too hard on yourself.

I also try and remember my favorite Steve Jobs quote

> Everything around you that you call life was made up by people that were no smarter than you. And you can change it, you can influence it… Once you learn that, you'll never be the same again

Always remember that you can always learn something new and there will be someone who can always teach you something new.

> In life or programming, I get satisfaction by taking action, even if that action is a small step or a big commit.

This is my process. 

### Focus on the easy things first

Punions app has a NOSQL document storage so I will get started with that. I will create two tables called: Cards and Games. Cards will contain at least 10 cards and the Games table will be empty because that will be created by the lambda function running behind a RESP API and it is called with a POST method.

### Create a Game

Next thing is to write a lambda function to create a Game. You need to know how to generated a random string and insert that in the Games table to create a new JSON document. Send the game id back to the client and the React app will be create a unique URL. 

### Multi page React app

Now I know I have multiple pages in my React app so I need to make use of react-router. The home page will load the Game component which will create a unique URL and the board component will have everything to play the game and this will be at a different URL.

### Add a player

Now I have to create a Web Socket on server to listen for a message called "AddPlayer" and the body of the message will contain gameid and AWS will give you a connection id which will be used as the player id. Take those two values, and find the game in the Games table via game id and add the player where the player id is the connection id we get from AWS. The web socket will send a messge back to the client with a list of players and default score. Remember this happens every time a new player is added so your React app will have to update the score board everytime. 

### Return a list of Cards

Now I need a lambda function behind a REST API which can be called via a GET method and it will return a list of cards from the Cards table. This is simple.

> Second secret. Remember to take a break. One cannot keep on working without taking breaks. Feel comfortable knowing when you are in [Flow](https://www.amazon.com/Flow-Psychology-Experience-Perennial-Classics/dp/0061339202) ([inner game of tennis](https://www.amazon.com/dp/0679778314)) and when you are out of it.

More to come...
