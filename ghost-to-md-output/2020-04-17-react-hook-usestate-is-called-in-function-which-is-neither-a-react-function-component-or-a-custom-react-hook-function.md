---
title: React Hook useState is called in function which is neither a React function component or a custom React Hook function
slug: react-hook-usestate-is-called-in-function-which-is-neither-a-react-function-component-or-a-custom-react-hook-function
date_published: 2020-04-18T01:48:14.000Z
date_updated: 2020-04-20T02:34:18.000Z
tags: ReactJS, Warnings
excerpt: Easy to fix error which caught me by surprise 
---

When you experience this error chances are that you already had a working functional component and decided to hook into React features, by making use of one of the many hooks like useState or useEffect. I know, it happened to me. 

My functional component looks something like this

    import React, { useState } from 'react';
    
    const example = () => {
    	
        const [something, setSomething] = useState(0);
        
    	return (
        	<div>Hi from Example</div>
        )
    }
    
    export default example;

Remove the hook from the above code and the error goes away. So why do I get an error after using a hook? The name of the functional component is perfectly fine according to JavaScript.

The error comes from rules of Hooks in the ESLint plugin and there is a check to see if a component or function name is valid. Take a look at the [file](https://github.com/facebook/react/blob/c11015ff4f610ac2924d1fc6d569a17657a404fd/packages/eslint-plugin-react-hooks/src/RulesOfHooks.js) and find a rule that says "*Checks if the node is a React component name. React component names must* always start with a non-lowercase letter.*" on line 44. 

Changing the functional component name to Example fixes the error as seen in the code below

    import React, { useState } from 'react';
    
    const Example = () => {
    	
        const [something, setSomething] = useState(0);
        
    	return (
        	<div>Hi from Example</div>
        )
    }
    
    export default Example;

Always start your function names with a non-lowercase letter. So MyComponent or _MyComponent. 

The error message is not helpful and should probably say, hey, start your function names with a non-lowercase letter.
