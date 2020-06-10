---
title: Hook Effect React
slug: hook-effect-react
date_published: 2020-04-18T15:55:12.000Z
date_updated: 2020-04-18T15:58:23.000Z
tags: ReactJS
excerpt: How I understand the hook useEffect and maybe it will be helpful to you.
---

Think of hooks as React letting you tap into its internal features. Each hook is a function and the hook which allows you to manage state is called useState. All of the hooks start with the word "use" and they are used in functional components.

The hook useEffect is only used in a functional component and it combies three functions found in a class based component: componentDidMount, componentDidUpdate, and componentWillUnmount. So anything you would do in the beforehand mentioned methods you will do them in one method, useEffect. useEffect takes in a function which is run after the render. Let's see a simple use case of the hook useEffect.

    import React, { useState, useEffect } from 'react';
    
    const Example = () => {
    
    	const [timesButtonClicked, setTimesButtonClicked] = useState(0);
        
    	useEffect(() => {
        	console.log(`Button was clicked ${timesButtonClicked} times`);
        });
        
    	return(
        	<div>
            	<button onClick={() => {setTimesButtonClicked(timesButtonClicked + 1)}}>Click Me</button>
            </div>
        )
    };
    
    export default Example;
    

Start your app and open the development console for the web page and you will see a message. Clicking on the button will display another message. This is similar to componentDidMount and componentDidUpdate methods found in a class based component. You can already see one of the many advantages of using a functional component, we replaced two class based functions: componentDidMount and componentDidUpdate.

In our example useEffect is called after the state was updated in button click and if this was a class based component then we would have written the code in componentDidUpdate. You would have to write the same code in componentDidMount if you wanted to display the message in the console. With a functional component and a hook, code is much simpler.

What about componentWillUnmount, how do we clean up using the hook useEffect. Let's take a look by updating our Example component

    import React, { useState, useEffect } from 'react';
    
    const Example = () => {
    
    	const [timesButtonClicked, setTimesButtonClicked] = useState(0);
        
    	useEffect(() => {
        	console.log(`Button was clicked ${timesButtonClicked} times`);
            
            return function cleanup() {
            	console.log('perform cleanup in this function');
            }
        });
        
    	return(
        	<div>
            	<button onClick={() => {setTimesButtonClicked(timesButtonClicked + 1)}}>Click Me</button>
            </div>
        )
    };
    
    export default Example;
    
    

Every hook useEffect can return a function that cleans up after itself. This allows us to keep all of our code in one place. 

By taking a second look at the cleanup function it is obvious that we can write a function inside the hook useEffect and later call that function inside the hook useEffect. This allows us to structure our code. You can also refactor your code in a different module, import that module and use it in the hook useEffect. Lets see how to do this with our Examle function

    import React, { useState, useEffect } from 'react';
    
    const Example = () => {
    
    	const [timesButtonClicked, setTimesButtonClicked] = useState(0);
        
    	useEffect(() => {
        	const logToConsole = () => {
            	console.log(`Button was clicked ${timesButtonClicked} times`);
            };
        	
            logToConsole();
            
            return function cleanup() {
            	console.log('perform cleanup in this function');
            }
        });
        
    	return(
        	<div>
            	<button onClick={() => {setTimesButtonClicked(timesButtonClicked + 1)}}>Click Me</button>
            </div>
        )
    };
    
    export default Example;
    
    
    

But there is more, just as we can use multiple useState hooks in a functional component, we can use multile useEffect hooks. Let us expand our Example functional component

    import React, { useState, useEffect } from 'react';
    
    const Example = () => {
    
    	const [timesButtonClicked, setTimesButtonClicked] = useState(0);
        const [timesCheckboxClicked, setTimesCheckboxClicked] = useState(0);
        
        useEffect(() => {
        	console.log(`Checkbox was checked ${timesCheckboxClicked} times`);
        });
        
    	useEffect(() => {
        	const logToConsole = () => {
            	console.log(`Button was clicked ${timesButtonClicked} times`);
            };
        	
            logToConsole();
            
            return function cleanup() {
            	console.log('perform cleanup in this function');
            }
        });
        
    	return(
        	<div>
            	<button onClick={() => {setTimesButtonClicked(timesButtonClicked + 1)}}>Click Me</button><br/>
                <input type="checkbox" onClick={() => setTimesCheckboxClicked(timesCheckboxClicked + 1)} /> Check me
            </div>
        )
    };
    
    export default Example;
    

Clicking the checkbox you will notice that you also see the message for how many times the button was clicked and this is because React will run every effect in the order it was specified. Also the cleanup method is called after every render unlike class based component componentWillUnmount.

But sometimes we do not want our useEffect to run after every render because this may causes performance issues. We only want to display the checkbox mesage if the timeCheckboxClicked is changed and display the message when timesButtonClicked was changed. Let's see how we can do this

    import React, { useState, useEffect } from 'react';
    
    const Example = () => {
    
      const [timesButtonClicked, setTimesButtonClicked] = useState(0);
      const [timesCheckboxClicked, setTimesCheckboxClicked] = useState(0);
    
      useEffect(() => {
        console.log(`Checkbox was checked ${timesCheckboxClicked} times`);
      }, [timesCheckboxClicked]);
    
      useEffect(() => {
        const logToConsole = () => {
          console.log(`Button was clicked ${timesButtonClicked} times`);
        };
    
        logToConsole();
    
        return function cleanup() {
          console.log('perform cleanup in this function');
        }
      }, [timesButtonClicked]);
    
      return (
        <div>
          <button onClick={() => { setTimesButtonClicked(timesButtonClicked + 1) }}>Click Me</button><br />
          <input type="checkbox" onClick={() => setTimesCheckboxClicked(timesCheckboxClicked + 1)} /> Check me
        </div>
      )
    };
    
    export default Example;
    

In the above example we pass a timesCheckboxClicked and timesButtonClicked as a second parameter which is an array and this tells useEffect to run that effect when that value changes. React will then compare the previous value of timesButtonClicked and the new value and only run if old value is different than the new value. This also works for the cleanup function. 

The useEffect hook runs asynchronously but sometimes you want to run something synchronously and that is when you use the hook useLayoutEffect. 

Let's review some of the advantages of the hook useEffect

1. It allows us to write code which we would write in two different class based components componentDidMount and componentDidUpdate
2. The code for componentDidUnmount is also in the same hook useEffect
3. Using multiple useEffect hooks allows us to keep all the related code together.

The code seen in this example can be downloaded from this [repository](https://github.com/iJKTen/test-use-effect) on GitHub.
