---
title: "React Hook useEffect has a missing dependency: Either include include it or remove the dependency array"
slug: react-hook-useeffect-has-a-missing-dependency
date_published: 2020-04-16T22:36:33.000Z
date_updated: 2020-04-17T00:20:30.000Z
tags: ReactJS, useEffect, Warnings
excerpt: React is letting you know that the hook useEffect is dependent on a variable but it cannot find that variable. So let's react nicely and do our part
---

Your project is getting this error and letting you know that useEffect method is using a variable whose value can change. Which means useEffect is dependent on that variable and since that variable can change which will cause another render and that is why React needs to know about this variable. React is smart enough to cause another render only when it needs to and that is why React needs to know if this variable is going to change or not. 

Why can't React figure this out on it's own when it's smart enough to show the warning? Ah computers... Robots are not going to take over after all

Hook useEffect needs to know which variables are dependent and you do that by passing the variable in an array. Why an array? Because useEffect could be using more than one variable.

How do you pass these dependents to useEffect method?

    let someVariable = "some value;
    
    useEffect(() => {
    	//someVariable used inside here
    }, [someVariable]);

useEffect method may not be using a variable but a function which is dispatched from redux. The solution remains the same with one tiny change on how it is implemented. 

    const onSomethingChanged = props.onSomethingChanged;
    useEffect(() => {
    	onSomethingCahnged('may be it takes a parameter');
    }, [onSomethingChanged]);
