---
title: Basic WebPack
slug: basic-webpack
date_published: 2020-04-20T19:59:44.000Z
date_updated: 2020-04-20T20:10:35.000Z
tags: Webpack
excerpt: Merging JS and CSS files for older applications
---

You may be working on an old application which has references to multiple JS and CSS files and you would like to use webpack to merge them into one single bundle. 

What you need to do is create an index.js file and import all the other JavaScript files into that JS file like this

    import './location-to-file/without-extension/index';
    import './location-to-file/without-extension/second-file';

Install the following modules

    npm install webpack-merge-and-include-globally
    npm install --save-dev webpack
    npm install --save-dev webpack-cli

Create a new webpack.config.js file at the root

    const path = require('path');
    const MergeIntoSingle = require('webpack-merge-and-include-globally');
    
    module.exports = {
      entry: './src/one.js', //one.js has all the imports from your other JS files
      output: {
        filename: 'main.js', //output file name
        path: path.resolve(__dirname, 'dist'), //output file will be created in the dist folder at the root
      },
      plugins: [
        new MergeIntoSingle({
          files: [{
            src: ['src/style.css', 'src/style2.css'], //list of css files to merge
            dest: 'bundle.css' //will be created in the dist folder
          }]
        })
      ]
    };

Run npx webpack and you should see one JS and one CSS file in the dist folder. Reference those files in your HTML page and you are good to go.

The code seen in this example can be downloaded from this [repository](https://github.com/iJKTen/basic-webpack) on GitHub.
