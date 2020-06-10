---
title: Structure React Project
slug: structure-react-project
date_published: 2020-04-21T01:17:52.000Z
date_updated: 2020-04-21T01:18:54.000Z
tags: ReactJS, Punions
excerpt: Use the same React project structure in all your applications and this is my approach and I hope it helps you
---

I have been thinking about how to best structure my React projects. I have come down to three folders under my src folder and those are 

### Scenes

A "scenes" folder has a sub folder for each screen in my React application. Each subfolder will have an index.js file which will be the scene and it may have a "components" folder if this component can only be used by this scene.

### Components

A "components" folder will contain all the common components which can be used  by any scene. With functional components I don't feel like I need a containers folder specially when creating a new project where I do not have any existing class based components.

### API

Most of the tutorials show us how fetch data from an API inside the container but this can be refactored in a module making it easy for any container to use it. This will not have any UI code. If I am using redux then that can be placed here too.

### What about CSS?

Next to each component, meaning at the same level as the component whether this is a scene or a component in my components folder, I plan to create a css file so the CSS and the component where the CSS is used in one single location.

May be for a bigger project it would be nice to use something like [@emotion](https://emotion.sh/docs/introduction) but I am happy with this simple approach.

I think this approach goes well because React does not like deep nested folders. 

How will this look like in my Punions app?

    /src
    	/api
        	/game
            	api.js
            /players
            	api.js
        /components
        	/card
            	index.js
                styles.css
        /scenes
        	/Home
            	index.js
                style.css
            /Board
            	index.js
                style.css

This project structure may change as I develop my app but this a good starting point.

I like the name of this article. Structure. You need to have a good strong base and you react from that base and you project what you feel. Little play on words...
