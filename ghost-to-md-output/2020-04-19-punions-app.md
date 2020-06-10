---
title: Punions App
slug: punions-app
date_published: 2020-04-20T02:13:04.000Z
date_updated: 2020-04-28T10:31:27.000Z
tags: Punions
excerpt: An multi player game running on AWS DynamoDB, Lambdas using REST API and WebSockets, and a React app running on S3
---

If you want to play Cards Aganist Humanity with your friends then you can play [online](http://cardsagainsthumanity.online/). This gave me an idea to create a game to play with your friends online, a game of puns, punions. To make it interesting, the database will be a NOSQL database running on Amazon DynamoDB (which is like MongoDB but Amazon doesn't say what it is), the server will be a mixture of REST API and a Web Socket, and the client will be created in React JS running on S3. 

This architecture is completely serverless and it can scale as needed without managing servers. 

### Web Socket

What is a Web Socket you might ask? It is a porotocol like HTTP but low level which always keeps a connection open to the client so the server can send messages to the client. Traditionaly, when it comes to the web, the client / browser sends a message to the server and the server responds. This is the other way around but our Web Socket will only reply when it receives a message but using Web Sockets we can send a message to all the connected players (browser) instead of the player (browser) that sent the message.

### How will this app work?

A friend will start the game by going to a URL. This will call a REST API which will create a unique id which will be saved in DynamoDB and sent back to the client and we will create a unique URL and going to that page will start the game. Let's call the destination page, the board page. Even though we do not have a board. Our JSON in DynamoDB will look like

### Create Game

    //TableName Game
    {"id": "qwerty"}

Visiting the board page, our client React app will ask for your name and when this name is submitted we will create a connection with the WebSocket running as a Lambda and we will send the game id and name. In our lambda, we get a new connection id (thanks to AWS which does the automatically) and we add a new player to the game. If this is the first player then "currentPlaying" will be true and we remember the order in which a player joins. Our JSON in DynamoDB will look like

### Add the first player to the game

    //TableName Game
    {
    	"id": "qwerty",
        "players": [{
        	"id": "asdfg",
            "name": "player-name",
            "currentlyPlaying": true,
            "playingOrder": 1
        }]
    }

The Web Socket will send a message to all the connected players with this response (we are going to have 4 players per game)

    {
    	"player 1": 0,
        "player 2": 0,
    	"player 3": 0,
    	"player 4": 0
    }

The above JSON will be used to display the names of the players and when each player is joined each message will look like

    //Player 1 joined
    {
    	"player 1": 0
    }
    
    //Player 2 joined
    {
    	"player 1": 0,
        "player 2": 0
    }
    
    //Player 3 joined
    {
    	"player 1": 0,
        "player 2": 0,
    	"player 3": 0
    }
    
    //Player 4 joined
    {
    	"player 1": 0,
        "player 2": 0,
    	"player 3": 0,
    	"player 4": 0
    }

In the above message player 1-4 are player names which each player will enter when they land on the board page.

The React app will also download a deck of cards which will happen with the help of a REST endpoint and the JSON response will look like this

    //TableName Cards
    [
    	{
        	"id": "card-id-1",
            "title": "pun here"
        },
    	{
        	"id": "card-id-2",
            "title": "pun here"
        },
    	{
        	"id": "card-id-3",
            "title": "pun here"
        }
    ]

Once four players are joined our table in DynamoDB will look like this

    //TableName Game
    {
    	"id": "qwerty",
        "players": [{
        	"id": "asdfg",
            "name": "player name",
            "currentlyPlaying": true,
            "playingOrder": 1
        },
        {
        	"id": "asdfg2",
            "name": "player name",     
            "currentlyPlaying": false,
            "playingOrder": 2
        },
        {
        	"id": "asdfg3",
            "currentlyPlaying": false,
            "name": "player name",
            "playingOrder": 3
        },
        {
        	"id": "asdfg4",
            "name": "player name",
            "currentlyPlaying": false,
            "playingOrder": 4
        }]
    }

we will begin the game and send the below JSON to the first person who is supposed to start the game via Web Sockets

### Player starts playing

    {
        "currentlyPlaying": true
    }

This will allow the player to select a card and a pun will be displayed only to that person. This persion will be able to write their answer in a textbox and upon clicking a submit button we will send another message to the Web Socket and the JSON will look like this and the React app will also disallow the player from picking up another card

    {
    	"id": "qwerty",
        "playerId": "asdfg",
        "cardId": "card-id-1",
        "answer": "player-1-answer"
    }

Web Socket on the server will receive our message and record the answer in our DynamoDB table and the JSON in our DynamoDB table will look like this

    //TableName Game
    {
    	"id": "qwerty",
        "players": [{
        	"id": "asdfg",
            "name": "player name",
            "currentlyPlaying": true,
            "playingOrder": 1,
            "playedCards": [{
            	"card-id-1": "card-id-1",
                "answer": "player-1-answer"
            }]
        },
        {
        	"id": "asdfg2",
            "name": "player name",
            "currentlyPlaying": false,
            "playingOrder": 2
        },
        {
        	"id": "asdfg3",
            "name": "player name",
            "currentlyPlaying": false,
            "playingOrder": 3
        },
        {
        	"id": "asdfg4",
            "name": "player name",
            "currentlyPlaying": false,
            "playingOrder": 4
        }]
    }

and send a message to all the other players except the player that answered with this message which will be displayed to the users.

    {
    	"cardId": "card-id-1",
        "playerId": "asdfg",
        "answer": "player-1-answer"
    }

Each player will have the ability to like or dislike the answer. Like will be sent as 1 and dislike will be sent as a 0 like this

    {
        "gameId": "qwerty"
    	"playerId": "player-id-2",
    	"score": 1
    }

After the other three players have sent their answers the table in DynamoDB will look like

    //TableName Game
    {
    	"id": "qwerty",
        "players": [{
        	"id": "asdfg",
            "name": "player name",
            "currentlyPlaying": false,
            "playingOrder": 1,
            "startGame": true,
            "score": 2,
            "playedCards": [{
            	"card-id-1": "card-id-1",
                "answer": "player-1-answer"
            }]
        },
        {
        	"id": "asdfg2",
            "name": "player name",
            "currentlyPlaying": true,
            "playingOrder": 2
        },
        {
        	"id": "asdfg3",
            "name": "player name",
            "currentlyPlaying": false,
            "playingOrder": 3
        },
        {
        	"id": "asdfg4",
            "name": "player name",
            "currentlyPlaying": false,
            "playingOrder": 4
        }]
    }

The above JSON tells us that player with id asdfg has a score of 2 meaning two players liked player1's pun. Now we send a message from the server Web Socket to the second player who is supposed to play and it will look like

    {
    	"startPlaying": true
    }

The web socket also sends another message which will display the score for Player 1 and this will be sent to all the players

    {
    	"player 1": "score",
        "player 2": "score",
    	"player 3": "score",
    	"player 4": "score"
    }

The score is updated for all the players and the same steps are repated for each player starting with section "Player starts playing"

Now we have a basic spec and the things we have to do is

1. Create two tables in DynamoDB: Cards and Games
2. Create a REST API which wll create a game, save the game in DynamoDB and send the game id back to the React app and the app will create a unique URL
3. A player loading the board page (by visiting the unique URL) will have to enter their name and the game id and the name will be sent to the server via a Web Socket. So we need a Web Socket on the server which can receive this message. Let's call this message "CreatePlayer"
4. Create a REST API which will return a deck of cards after the player has submitted their name
5. When a player joins, the Web Socket will respond to all the connected players with a message showing the names of the players added and their default scores.
6. After the last (4th) player is joined the web socket will send the first player a message to start playing
7. The player will send a message to the server telling which card was picked including the answer, and the game id. Let's call this message "Card Played". Now the Web Socket will send a message to all the other connected players telling what player 1 played.
8. Each player will now submit their scores and when the last player has submitted their score the server will send a message to the second player to begin playing. Lets' call this message that the player sends to score "ScoreAnswer". The Web Socket will now also send a list of updated scores to all the players. 

I may have missed a few things but that is okay. Let's begin!
