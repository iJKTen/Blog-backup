---
title: On Showing Off
slug: on-showing-off
date_published: 2020-05-17T22:29:45.000Z
date_updated: 2020-05-17T23:16:45.000Z
tags: GitHub
excerpt: Show off your GitHub repository with a GitHub card on your own site.
---

All the social networks have some kind of ability to show off your profile on other sites but not GitHub. The best you can do with GitHub is add a link which takes you to the repository. I decided it could be better. 

I introduce GitHub card and this is what it looks like
![](/content/images/2020/05/example-1.png)
It displays the name of your repository at the top, with your profile picture, and your GitHub username at the bottom. It shows the number of stars your repo has including summary and the language. Clicking on the repo name, and the number of stars will open a new link to the repository. 

### How do I use it

    // Add this CSS line in your head tag
    <link href="https://github.com/iJKTen/github-card/blob/master/src/app.css" rel="stylesheet" />
    
    // Add this line in your HTML
    <div data-repo="ijkten/github-card"></div>
    
    // Add this line at the bottom of your HTML page above the close of thebody tag
    <script src="https://github.com/iJKTen/github-card/blob/master/dist/githubcard.min.js"></script>

On the same page you can highlight multiple repos

    // Add this line in your HTML
    <div data-repo="ijkten/github-card"></div>
    
    // Somewhere else on the same HTML page
    <div data-repo="iJKTen/punions-server"></div>

The custom attribute data-repo should have the value of your GitHub username followed by a slash (/) and the name of the repo. 

I plan to host this project on a CDN soon. 

You can find the source code of the repo [here](https://github.com/iJKTen/github-card).
