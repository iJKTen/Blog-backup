---
title: On importing modules
slug: on-importing-modules
date_published: 2020-04-18T16:26:21.000Z
date_updated: 2020-04-20T02:33:43.000Z
tags: ReactJS
excerpt: How I understand importing default modules vs named modules
---

There is a difference when importing React from the react module and importing React and useState from the React module. Why?

    //Importing only React
    import React from 'react';
    
    //Importing React and a hook
    import React, { useState } from 'react';

Why is useState in curly braces? It is because how the useState function was exported from the react module. The way useEffect was exported is called a named export and it looks like this

    export function useState...

When you write your own custom functional component in React you would write it like this

    import React, { useState } from 'react';
    
    const Example = () => {
    
      const [aVariable, setAVariable] = useState(0);
    
      return(
        <div>Value of the variable is {aVariable}</div>
      )
    }
    
    export default Example;
    
    //you would import the module in App.js like this
    import Example from './Example';

If you export your Example functional component like below

    import React, { useState } from 'react';
    
    export const Example = () => {
    
      const [aVariable, setAVariable] = useState(0);
    
      return(
        <div>Value of the variable is {aVariable}</div>
      )
    }

Then in your App.js you importing Example functional component will throw an error

    import Example from './Example';

You would have to import your named module like seen below

    import { Example } from './Example';

In a React module you would have many functions and you now have the option of exporting one default function and you would export the other functions as named functions and it those named functions that are surrounded in curly braces. 

A simple example which mutated my understanding of exporting and importing modules. 

The code seen in this example can be downloaded from this [repository](https://github.com/iJKTen/test-use-state) on GitHub.
