---
title: Reacting with emotions
slug: reacting-with-emotions
date_published: 2020-04-26T17:08:31.000Z
date_updated: 2020-04-26T17:10:03.000Z
tags: ReactJS
excerpt: On using Emotion with React.
---

Just when I think, I know how to write CSS, emotions get the better of me. 

### React without Emotion

Writing CSS is "easy". You create a style.css file, include it in your HTML page and the page will be styled. All good when you are working alone. 

When working in a team you quickly realize that CSS has limitations and those are

1. No one agrees on the best way to write class names
2. No one agrees on how to best structure your CSS. 
3. As the website becomes huge your CSS ends up having dead code, meaning CSS that is not used by any HTML element. 
4. Every other team member is afraid to change CSS because it is tough to know how the site will change, so new CSS is added at the end of the file and guess what, with a new class name
5. Your CSS is now 1000 or more lines

Of course some of the issues mentioned above, the community has tried to solve by introducing libraries like bootstrap, tailwindCSS, & SASS to name a few. Of course it solved some of the issues.But in React there is another way to do things which just might be better if you are in a team or working alone.

### React with Emotion

Emotion is a library designed for writing css styles with JavaScript. 

Install emotion

    npm i emotion

Below is an example.

    
    // Add the line below. If you do not, your css wil render as [object object]
    /** @jsx jsx */
    import { jsx } from '@emotion/core'
    
    // Pause to reflect that we do not import React here.
    
    const ExampleCssOne = (props) => {
      return (
        <div
        // We are passing an object to the css prop. 
        // The first curly braces say that we want to write some JavaScript code
        // The second curly braces is the JSON object
        // When using object, pseudo classes will get a colon
        // hover is a pseudo selector. Learn more https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes
        // Since this is JavaScript, you write backgroundColor and not background-color
          css={{
            backgroundColor: 'green',
            color: 'yellow',
            '&:hover': {
              color: 'green',
              backgroundColor: 'yellow'
            }
          }}
        >
          I am a simple div
        </div>
      )
    }
    
    export default ExampleCssOne;
    

In the above code we are adding our CSS as a JavaScript object to the div and here is the isolated example

    css={{
            backgroundColor: 'green',
            color: 'yellow',
            '&:hover': {
              color: 'green',
              backgroundColor: 'yellow'
            }
          }}

We add the hover pseudo class to the button by introducing a new syntax '&:' and the pseudo class name. 

The above example can be modified by adding our CSS as a [tagged template string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) as seen below

    
    // Add the line below. If you do not, your css wil render as [object object]
    /** @jsx jsx */
    import { css, jsx } from '@emotion/core'
    
    // Pause to reflect that we do not import React here.
    
    const ExampleCssTwo = () => {
      return (
        <div
          // Here you import css module from emotion core
          // Now you can write css as a tagged template literal.
          // Find out more about tagged template literals https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals
          // Inside the tagged template literal you can now write css as you would normally
          // except any pseudo class still begin with &: and the pseudo class will not get a semi-colon.
          // Adding a semi-colon to your pseudo class will cause your pseudo class to not work.
          css={css`
              background-color: purple;
              color: white;
              &:hover {
                color: purple;
                background-color: white;
              }
            `}
        >
          I am a second simple div
        </div>
      )
    }
    
    export default ExampleCssTwo;
    

You have to remember one basic distinction how you write your CSS properties. If it is in an object then you use the camelCase as seen in the ExampleCssOne, if not you use the [kebab-case](https://en.wikipedia.org/wiki/Letter_case#Special_case_styles) as seen in ExampleCssTwo. CSS properties in ExampleCssTwo are the actual CSS property names. 

What about props. How do I pass props to my components which has the emotion css module. Like seen below

    
    // Add the line below. If you do not, your css wil render as [object object]
    /** @jsx jsx */
    import { jsx } from '@emotion/core'
    
    // Pause to reflect that we do not import React here.
    
    const SomeDiv = (props) => {
      return (
        <div css={{
          backgroundColor: 'pink',
          color: 'white'
        }} {...props}>
          Some div here
        </div>
      )
    }
    
    const ExampleThree = (props) => {
      return (
        <div>
          <SomeDiv css={{
            color: 'grey',
            fontFamily: 'Helvetica'
          }} />
          Some div in example Example Three
        </div>
      )
    }
    
    export default ExampleThree;
    

In the above example you can also override a CSS property by declaring it again. Notice the SomeDiv component, props has the [spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) applied to it and using the spread operator we can override the CSS property and also apply or pass the props from ExampleThree to SomeDiv. 

You can also dynamically set a CSS property by passing it as a prop as seen below. sizeOfFont is passed as a prop with a font size which is applied to the CSS. This approach is helpful when you have multiple components sharing the CSS but setting one value per component.

    
    // Add the line below. If you do not, your css wil render as [object object]
    /** @jsx jsx */
    import { jsx } from '@emotion/core'
    
    // Pause to reflect that we do not import React here.
    
    const ExampleFour = (props) => {
      return (
        <div
        // We are passing an object to the css prop. 
        // The first curly braces say that we want to write some JavaScript code
        // The second curly braces is the JSON object
        // When using object, pseudo classes will get a colon
        // hover is a pseudo selector. Learn more https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes
        // Since this is JavaScript, you write backgroundColor and not background-color
        // If you have a JavaScript then you can use props like below.
          css={{
            backgroundColor: 'green',
            color: props.primary ? 'white' : 'yellow',
            fontSize: props.sizeOfFont
          }}
        >
          I am a simple div example four
        </div>
      )
    }
    
    export default ExampleFour;
    
    // You would use ExampleFour component like this and pass custom attributes to it
    
    <ExampleFour primary sizeOfFont="24px" />
    

If you were using tagged template string then you would use the props this way

    
    // Add the line below. If you do not, your css wil render as [object object]
    /** @jsx jsx */
    import { css, jsx } from '@emotion/core'
    
    // Pause to reflect that we do not import React here.
    
    const ExampleFive = (props) => {
      return (
        <div
          // Here you import css module from emotion core
          // Now you can write css as a tagged template literal.
          // Find out more about tagged template literals https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals
          // Inside the tagged template literal you can now write css as you would normally
          // except any pseudo class still begin with &: and the pseudo class will not get a semi-colon.
          // Adding a semi-colon to your pseudo class will cause your pseudo class to not work.
          css={css`
              background-color: purple;
              color: white;
              &:hover {
                color: purple;
                background-color: white;
              }
            `}
            {...props}
        >
          I am a five simple div
        </div>
      )
    }
    
    export default ExampleFive;
    
    // This is how you would use ExampleFive
    <ExampleFive css={{backgroundColor: 'pink'}} />
    

When using a tagged template string you can pass props to the CSS like seen below

    import styled from '@emotion/styled'
    import React from 'react';
    
    const ExampleSix = (props) => {
      const ButtonStyled = styled.button`
        color: turquoise;
        background-color: ${props.bgColor};
        font-size: ${props.iAmACondition ? '24px' : '12px'};
        padding: ${props => props.primary ? '20px' : '0px'};
      `
      return (
        <ButtonStyled primary>I am a button</ButtonStyled>
      )
    }
    
    export default ExampleSix;
    
    // This is how you would use ExampleSix
    <ExampleSix bgColor="yellow" iAmACondition/>

So far we have been adding CSS directly to our elements. Emotion gives us the ability to create styled components, meaning if you want to render a component, then you get to apply CSS to that entire component. You do that this way

Install @emotion/styled

    npm i @emotion/styled

    import styled from '@emotion/styled'
    
    
    export const ButtonStyled = styled.button({
      color: 'turquoise'
    }, props => ({
      backgroundColor: props.bgColor,
      fontSize: props.iAmACondition ? '24px' : '12px',
      padding: props.primary ? '20px' : '0px'
    }))
    

In the above example we are exporting a named module with the syntax export const ButtonStyled and this is how you would use your styled component

    import React from 'react';
    import {ButtonStyled as ExampleSevenStyles} from './ExampleSevenStyled';
    
    const ExampleSeven = (props) => {
      return (
        <ExampleSevenStyles primary {...props}>I am a button</ExampleSevenStyles>
      )
    }
    
    export default ExampleSeven;
    
    // This is how you would use ExampleSeven
    <ExampleSeven bgColor="red"/>

In the above example we have a styled component called ButtonStyled which is created from the @emotion/styled modules using styled.button. Using the styled module you can create any HTML element. Also notice how we import our ButtonStyled component as a named module but we change it's name when importing using ButtonStyled as ExampleSevenStyles.

It took some timeto understand @emotion/styled and to get the examples working and the best way to learn is to use it in a project like I am doing in my personal site which you can find [here](https://github.com/iJKTen/ijk.me)

The code in this article can be downloaded from this [repo](https://github.com/iJKTen/emotionapp) on GitHub.
