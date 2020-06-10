---
title: On removing lint
slug: on-removing-lint
date_published: 2020-05-14T01:14:32.000Z
date_updated: 2020-05-14T01:24:36.000Z
tags: ESLint
excerpt: Using ESLint to clean up code
---

It's no secret that two developers will not only write different code but they may not follow the language guidelines. Yes, every language has a style guide on how the code should be written. 

A lot of teams publish the style guide and all the developers are meant to follow it, which rarely happens. Wouldn't it be nice if our editor would complain as the code is being written when it finds one of the rules are not being met. [ESLint](https://eslint.org/) to the rescue. 

### ESlint

ESLint allows you to do the following but more

1. Easily fix syntax errors
2. Warn about variables not being used
3. Warn about variables not being changed so its type can be changed to const
4. Warn about globals creeping into your code
5. All the developers on your team will end up following the same code. 

### How to use

Install eslint via a npm package

    npm i -D eslint

Now you need a file to configure your eslint and to do that you need to create a configuration file called .eslintrc. But you probably want to use an existing configuration from someone who has spent a lot of time in writing good defaults. Let's use Google's for our example

    npm i -D eslint-config-google

Use it like this in your .eslintrc file

    {
      "extends": ["eslint:recommended", "google"],
      "env": {
        "jest": true,
        "node": true,
        "es6": true
      },
      "rules": {
        "comma-dangle": ["error", "never"],
        "prefer-template": "error",
        "complexity": ["error", 50],
        "max-len": ["error", {"code": 120}],
        "consistent-return": 2,
        "indent": [1, 2],
        "no-else-return": 1,
        "semi": [1, "always"],
        "space-unary-ops": 2
      }
    }
    

If this is a node application which uses the jest testing framework and you want to use es6 features then your env looks like how it show up above. 

ESLint comes with default rules that you can quickly turn on with extends key as seen above. In our example, we are asking ESLint that we want to use the defualt rules including the rules provided by Google. In our example we have turned on some other rules and you see a list of all the rules [here](https://eslint.org/docs/rules/)

Now as you write code ESLint will warn you and throw an error keeping you on your [toes](https://www.youtube.com/watch?v=z4ifSSg1HAo). (got toes and I can smile)

The code in this article can be downloaded from this [repo](https://github.com/iJKTen/blog-post-eslint) on GitHub.
