---
title: Hook State React
slug: hook-state-react
date_published: 2020-04-17T04:51:11.000Z
date_updated: 2020-04-17T05:05:56.000Z
tags: ReactJS, useState
excerpt: How I understand the hook useState and maybe it will be helpful to you.
---

Think of hooks as React letting you tap into its internal features. Each hook is a function and the hook which allows you to manage state is called useState. All of the hooks start with the word "use" and they are used in functional components. 

This is how you can add a useState hook in your functional component

    import React, { useState } from 'react';
    
    const Example = () => {
    	
        const [timesButtonClicked, setTimesButtonClicked] = useState(0);
        
        return (
        	<div>
            	Button has been clicked {timesButtonClicked} times.<br />
            	<button onClick={() => setTimesButtonClicked(timesButtonClicked + 1)}>Click Me</button>
            </div>
        );
        
    };
    
    export default Example;

In the line below an array is being destructured and it is called [destructuring array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) because useState returns an array with two values. 

    const [timesButtonClicked, setTimesButtonClicked] = useState(0);

The first being a variable which holds your state and the second which sets the state. Also note that the state is assigned to a default value of 0.

Because this is a functional component you can access the state variable directly in your return method without this.state because this is not a class based component. You can even call the setTimesButtonClicked method to update the variable timesButtonClicked. Because the state was initialized with a number, we increment the variable timesButtonClicked by 1 and change that variable when called like shown below

    <button onClick={() => setTimesButtonClicked(timesButtonClicked + 1)>Click Me</button>

Notice how in the above line setTimesButtonClicked is not called directly in the onClick function as seen below. If you do what is done below then the function setTimesButtonClicked will be called when the component is rendered and that is not what we want. This is why we use the [ES6 arrow function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) by calling an anonymous function which calls the setTimesButtonClicked method.

    {/* Do not do this */}
    <button onClick={setTimesButtonClicked(timesButtonClicked + 1)>Click Me</button>

If you have a JSON object in your state then the Example component changes as seen below

    import React, { useState } from 'react';
    
    const Example = () => {
    	
        const [timesButtonClicked, setTimesButtonClicked] = useState({counter: 0});
        
        return (
        	<div>
            	Button has been clicked {timesButtonClicked.counter} times.<br/>
            	<button onClick={() => setTimesButtonClicked({counter: timesButtonClicked.counter + 1})}>Click Me</button>
            </div>
        );
        
    };
    
    export default Example;

Few advantages of the hook useState

1. You need not write a class based component
2. Each state can be managed in it's own variable instead of a big JSON object.
3. Updating the state is as easy as calling the method which is returned by the useState hook.
4. Freedom from prop hell because now each functional component can be responsible for managing it's own state. 

The code below shows how to use multiple state variables in a single component

    import React, { useState } from 'react';
    
    const Example = () => {
    	
        const [timesButtonClicked, setTimesButtonClicked] = useState({counter: 0});
        const [timesCheckboxChecked, settimesCheckboxChecked] = useState(0);
        
        return (
        	<div>
                <p>
                    Button has been clicked {timesButtonClicked.counter} times.<br/>
            	    <button onClick={() => setTimesButtonClicked({counter: timesButtonClicked.counter + 1})}>Click Me</button>
                </p>
                <p>
                    Checkbox has been checkd {timesCheckboxChecked} times.<br/>
                    <input type='checkbox' onClick={() => settimesCheckboxChecked(timesCheckboxChecked + 1)} />
                </p>
            </div>
        );
        
    };
    
    export default Example;

The code seen in this example can be downloaded from this [repository](https://github.com/iJKTen/test-use-state) on GitHub.

I hope this mutated your understanding of the hook useState.
