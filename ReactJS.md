# React JS
## Table of contents
- [Local Environment Setup](#setup-local-environment)
- [Basics](#basics)
- [React Components](#react-components)
- [React States](#state-in-react)
- [React Hooks](#hooks-in-react)
- [React Events](#event-handling-in-react)
- [Useful JS functions](#useful-js-functions)
- [Using JS properties in React](#using-js-short-circuit-and-truthy-falsy-properties-in-react-codes)
- [Important Links](#important-links)


## Setup local environment
1) Install Node LTS
2) Install VS Code and extensions like "sublime babel" and "vscode icons"
3) Create react app & run it in dev server mode using command
```
npx create-react-app my-app
cd my-app
npm start
```

## Basics

 My code example in codesandbox
 https://codesandbox.io/s/introduction-to-jsx-forked-j5wyjn?file=/src/index.js

 A javascript library to build user interfaces.
 React combines html, css and JS into a single modular component.
 
 The modules needed for react app is react, react-dom, react-scripts

 We can render the elements in dom using 
 react-dom.render("WHAT TO SHOW","WHERE TO SHOW","CALLBACK FUNCTION TO DO THINGS AFTER RENDERING")
 A render can take only one html element.

 JSX: JavaScript Extension
 When we write html in plain js files using react libraries, those js files are jsx files.
 Inside react module, it has babel (a js compiler) which compiles jsx to plain js.
 JSX not only allows html inside JS but also JS inside HTML which is already inside JS

 We can achieve this by using {} inside html tags.
 However, only JS variables and expressions are allowed like "1+2" or "user.name" 
 but not statements "if (user.name==='naren')"

 All the html attributes & CCS properties must have camel case notation.

 A component can be created using functions. And function names in react uses pascal case notation.
 And those functions can be called by creating a html tag with the function name

 For maintaining proper syntax in your jsx file, refer Airbnb React/JSX Style Guide 
 https://github.com/airbnb/javascript/blob/master/react/README.md

Just like how we have attributes for html tags, we have props for react tags.
These props can be received as arguments.
className will not work on any react tags, so it must be used only on html tags.

## React Components
React apps are made out of components. A component is a piece of the UI (user interface) that has its own logic and appearance. A component can be as small as a button, or as large as an entire page.

React components are JavaScript functions that return markup:
``` js
function MyButton() {
  return (
    <button>I'm a button</button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  );
}
```
Notice that <MyButton /> starts with a capital letter. That’s how you know it’s a React component. React component names must always start with a capital letter, while HTML tags must be lowercase.

## State in React
Often, you’ll want your component to “remember” some information and display it. For example, maybe you want to count the number of times a button is clicked. To do this, add state to your component.

UI is a function of state. State makes the UI interactive.
UI = f(state)
We have 2 approaches
1) Declarative
2) Imperative

### Declarative state programming
An UI will depend on the value of a state variable. We are declaring how our UI should look under different conditions or depending upon state.
Even if we use state variables and change the value of state to change UI, we wont get the desired effect without re rendering the UI after a state change.
For this purpose, we use hooks, which are functions that helps us to hook into react state variables and read/modify it. 

### Imperative state programming
This is more primitive and we use it during the early stage of learning JS for frontend. Like having a button which on click calls a function, inside the function we get element by ID and do the necessary changes.

## Hooks in React
Functions starting with 'use' are called Hooks. useState is a built-in Hook provided by React. You can find other built-in Hooks in the [API reference](https://react.dev/reference/react). You can also write your own Hooks by combining the existing ones.

Hooks are more restrictive than other functions. You can only call Hooks at the top of your components (or other Hooks). If you want to use useState in a condition or a loop, extract a new component and put it there.

Hooks help us re render the part of html that depends on a state variable whenever there is a change in said state.
A react hook will only work inside a function.

Example,
useState Hook
``` js
import React from "react";
function App(){
    // we are using ES6 concept called destructuring (for arrays and objects)
    // count is the state variable and setCount is a function which will define the logic for changing state
    // value passed inside useState while decalaration is initial value. (here 0)
    // the value passed here inside setCount will be set as current value.
    const [count, setCount] = useState(0);

    function increase(){
        setCount(count+1);
    }
    function decrease(){
        setCount(count-1);
    }
    return (
        <div>
        <h1>{count}</h1>
        <button onClick={increase}> + </button>
        <button onClick={decrease}> - </button>
        </div>
    )

}

```

## Event handling in React

Events are handled just like we do in vanilla JavaScript, except we use state variables instead of JS variables.
You can respond to events by declaring event handler functions inside your components:
``` js
import React, { useState } from "react";

function App() {
  const [headingText, setHeadingText] = useState("Hello");
  const [isMousedOver, setMouseOver] = useState(false);

  function handleClick() {
    setHeadingText("Submitted");
  }

  function handleMouseOver() {
    setMouseOver(true);
  }

  function handleMouseOut() {
    setMouseOver(false);
  }

  return (
    <div className="container">
      <h1>{headingText}</h1>
      <input type="text" placeholder="What's your name?" />
      <button
        style={{ backgroundColor: isMousedOver ? "black" : "white" }}
        onClick={handleClick}
        onMouseOver={handleMouseOver}
        onMouseOut={handleMouseOut}
      >
        Submit
      </button>
    </div>
  );
}

export default App;

```
Notice how onClick={handleClick} has no parentheses at the end! Do not call the event handler function: you only need to pass it down. React will call your event handler when the user clicks the button.

## Useful JS functions
1) Map - Create a new array by doing something in each item of an array.
2) Filter - Create a new array by keeping the items that return true.
3) Reduce - Accumulate a value by doing something to each item in an array.
4) Find - find the first item that matches from an array.
5) FindIndex - find the index of the first item that matches.

### Map function
Each React component must have a create{ComponentName}() function which takes in the passed object and returns the component's .
 function so that a list of data can be generated as a list of react components. 
When we use map function (loop feature) to render a component multiple times with different inputs (from a datasource),
we must definitely have a unique ID as "key" property. (Each child in a list should have a unique "key" prop.)

However, we must not try to access key in the rendering part. It will render as undefined. To access it for some reason, pass it in a different prop other than "key"

## Using JS short circuit and Truthy Falsy properties in React Codes
We can exploit the conditional (ternary) operator, truthy-falsy and logical short circuiting for writing short codes

Example:
``` js

function Form(props) {
  return (
    <form className="form">
      <input type="text" placeholder="Username" />
      <input type="password" placeholder="Password" />
      { !props.isRegistered && <input type="password" placeholder="Confirm Password" />} //Input element rendered only if props.isRegistered is false
      <button type="submit">{props.isRegistered? "Login":"Register"}</button> // If props.isRegistered is true, then button says login else register
    </form>
  );
}
```


## Important Links
1) My code sample: https://codesandbox.io/s/introduction-to-jsx-forked-j5wyjn?file=/src/index.js
2) JSX syntax: https://github.com/airbnb/javascript/blob/master/react/README.md
3) Practice: Keeper App: https://codesandbox.io/s/keeper-app-part-1-starting-forked-7ht3v2 
4) HTML, CSS notes: https://codepen.io/NarendranAI/pen/VwXENJO
