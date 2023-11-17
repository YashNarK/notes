# React JS

## Table of Contents
- [React JS](#react-js)
  - [Table of Contents](#table-of-contents)
  - [Setup Local Environment](#setup-local-environment)
  - [JSX](#jsx)
  - [What is React?](#what-is-react)
  - [Basics](#basics)
  - [React Elements](#react-elements)
  - [React Components](#react-components)
  - [State](#state)
    - [Declarative State Programming](#declarative-state-programming)
    - [Imperative State Programming](#imperative-state-programming)
    - [React State](#react-state)
  - [Hooks in React](#hooks-in-react)
  - [Event Handling in React](#event-handling-in-react)
  - [Useful JS Functions](#useful-js-functions)
    - [Map Function](#map-function)
  - [Using JS Short Circuit and Truthy Falsy Properties in React Codes](#using-js-short-circuit-and-truthy-falsy-properties-in-react-codes)
  - [Important Links](#important-links)


## Setup Local Environment
1. Install Node LTS.
2. Install VS Code and extensions like "sublime babel" and "vscode icons."
3. Create a React app & run it in dev server mode using the command:
   ```bash
   npx create-react-app my-app
   cd my-app
   npm start
   ```
## JSX
JSX (JavaScript XML) is a syntax extension for JavaScript that looks similar to XML or HTML. It is often used with React to describe what the UI should look like. JSX provides a concise and expressive syntax for defining React elements, making it more readable and easier to write and understand UI components.
JSX (JavaScript Extension) allows embedding HTML within JS files and supports JS within HTML. It is compiled by Babel.


Here are some key points about JSX:

1. **Embedding Expressions:**
   JSX allows you to embed JavaScript expressions within curly braces `{}`. This allows you to include dynamic content or JavaScript logic within your markup.

   ```jsx
   const name = "World";
   const element = <p>Hello, {name}!</p>;
   ```

2. **HTML-Like Syntax:**
   JSX resembles HTML, making it familiar and easy for developers who are accustomed to working with HTML. However, there are some differences, such as using `className` instead of `class` for defining CSS classes.

   ```jsx
   const element = <div className="container">This is a JSX element</div>;
   ```

3. **Creating React Elements:**
   JSX gets transpiled into JavaScript code that creates React elements. The `React.createElement` function is used under the hood.

   ```jsx
   const element = <h1>Hello, React!</h1>;
   // Transpiles to: const element = React.createElement('h1', null, 'Hello, React!');
   ```

4. **Attributes and Props:**
   JSX allows you to define attributes similar to HTML. These attributes are called props (short for properties) in React.

   ```jsx
   const element = <img src="path/to/image.jpg" alt="An example image" />;
   ```

5. **JSX Fragments:**
   JSX fragments (`<></>`) allow you to return multiple elements without introducing an additional parent wrapper element.

   ```jsx
   const element = (
     <>
       <h1>Title</h1>
       <p>Paragraph</p>
     </>
   );
   ```

6. **Expressions in JSX:**
   You can use JavaScript expressions within JSX to compute values or perform conditional rendering.

   ```jsx
   const isLoggedIn = true;
   const element = (
     <div>
       {isLoggedIn ? <p>Welcome, User!</p> : <p>Please log in</p>}
     </div>
   );
   ```

JSX makes it more convenient to work with React by providing a syntax that closely resembles the final output. While it might look like HTML, it's important to remember that JSX is not HTML; it's a syntactic sugar for `React.createElement` calls, producing React elements that are then rendered to the DOM.

## What is React?
React is an open-source JavaScript library for building user interfaces or UI components, primarily for single-page applications where user interactions are dynamic and frequent. It was developed and is maintained by Facebook. React allows developers to create reusable UI components and manage the state of these components efficiently.

Key features of React include:

1. **Component-Based Architecture:** React applications are built using components, which are modular and self-contained units of code. Each component represents a part of the user interface and can be reused throughout the application.

2. **Virtual DOM:** React uses a virtual representation of the DOM (Document Object Model) to improve performance. Instead of updating the entire DOM when changes occur, React updates a virtual DOM first and then efficiently updates only the parts of the actual DOM that have changed. This minimizes the amount of manipulation needed on the real DOM, leading to better performance.

3. **Declarative Syntax:** React uses a declarative approach to define how the UI should look in response to different states. Developers describe the desired outcome, and React takes care of updating the DOM to match that state.

4. **JSX (JavaScript XML):** JSX is a syntax extension for JavaScript that looks similar to XML or HTML. It allows developers to write UI components in a syntax that closely resembles the final output. JSX is later transformed into JavaScript code that React can use.

5. **Unidirectional Data Flow:** React follows a unidirectional data flow, which means that data in an application flows in a single direction, from parent components to child components. This makes it easier to understand and manage the state of an application.

6. **React Hooks:** Introduced in React 16.8, hooks are functions that allow developers to use state and other React features in functional components instead of class components. This makes it easier to reuse stateful logic across components.

React is often used in conjunction with other libraries or frameworks (such as Redux for state management) and is a popular choice for building modern web applications. It has a large and active community, which contributes to its ecosystem of third-party libraries and tools.

## Basics

## React Elements
In React, elements are the smallest building blocks of a React application. They are plain JavaScript objects that represent what you want to see on the screen. A React element is an immutable description of what to render.

Here is a basic example of a React element:

```jsx
const element = <h1>Hello, React!</h1>;
```

In this example, `<h1>Hello, React!</h1>` is a JSX expression that represents a React element. Behind the scenes, JSX gets transpiled into a `React.createElement` function call:

```jsx
const element = React.createElement('h1', null, 'Hello, React!');
```

The `React.createElement` function takes three arguments:

1. The type of the element (in this case, `'h1'` for a heading).
2. The properties or attributes of the element (in this case, `null` since there are no attributes).
3. The content or children of the element (in this case, the text string `'Hello, React!'`).

React elements are lightweight, and they describe what you want to see on the screen. They are not the actual rendered UI elements; instead, they are virtual representations of the DOM elements. React uses a process called reconciliation to efficiently update the actual DOM based on changes to the virtual DOM.

Once you have a React element, you can render it to the DOM using the `ReactDOM.render()` function. Here's a simple example:

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

const element = <h1>Hello, React!</h1>;

ReactDOM.render(element, document.getElementById('root'));
```

In this example, the `ReactDOM.render()` function takes the React element (`<h1>Hello, React!</h1>`) and renders it to a container element with the ID of `'root'` in the HTML document.

React elements are often used in conjunction with components. Components are reusable pieces of code that can contain one or more React elements. They help structure the UI and make it easier to manage and maintain large React applications.

## React Components

React apps are made of components, which are JavaScript functions returning markup. Components must start with a capital letter.
Just like HTML attributes, React components have props. Props can be received as arguments.

In React, components are the fundamental building blocks of a user interface. They are reusable, self-contained pieces of code that define a part of a user interface. React applications are typically built by composing these components together to create the complete UI.

There are two main types of components in React:

1. **Functional Components:** These are simpler and more concise components that are defined as JavaScript functions. They take in props (properties) as an argument and return React elements, describing what should be rendered on the screen. Functional components were stateless until the introduction of hooks in React 16.8, which allowed functional components to manage state and have lifecycle methods.

   Example of a functional component:

   ```jsx
   import React from 'react';

   const MyFunctionalComponent = (props) => {
     return <div>Hello, {props.name}!</div>;
   };

   export default MyFunctionalComponent;
   ```

2. **Class Components:** These are ES6 classes that extend from `React.Component`. Class components have more features than functional components, such as the ability to manage local state and access lifecycle methods. Before the introduction of hooks, class components were the primary way to manage state in React.

   Example of a class component:

   ```jsx
   import React, { Component } from 'react';

   class MyClassComponent extends Component {
     constructor(props) {
       super(props);
       this.state = {
         count: 0
       };
     }

     render() {
       return (
         <div>
           Count: {this.state.count}
           <button onClick={() => this.setState({ count: this.state.count + 1 })}>
             Increment
           </button>
         </div>
       );
     }
   }

   export default MyClassComponent;
   ```

With the introduction of React Hooks, functional components can now also have state and lifecycle features, reducing the need for class components in many cases. Hooks, such as `useState` and `useEffect`, allow developers to manage state and perform side effects in functional components.

Using components allows for a modular and organized approach to building React applications. Components can be composed and reused, making it easier to reason about the structure and behavior of the UI.

## State

State makes UI interactive. UI = f(state). There are declarative and imperative approaches.

### Declarative State Programming

UI depends on the value of a state variable. Hooks help hook into state variables and read/modify them.

### Imperative State Programming

More primitive, used in early JS learning for frontend.

### React State
In React, `state` is a fundamental concept that allows components to keep track of and manage their internal data. The `state` object is used to store and update information that can change over time, causing the component to re-render when the state is updated.

Here's a basic overview of how state works in React:

1. **Initializing State:**
   You can initialize the state in a class component by setting the `state` property in the component's constructor. Here's an example:

   ```jsx
   import React, { Component } from 'react';

   class MyComponent extends Component {
     constructor(props) {
       super(props);
       this.state = {
         count: 0
       };
     }

     render() {
       return (
         <div>
           <p>Count: {this.state.count}</p>
         </div>
       );
     }
   }

   export default MyComponent;
   ```

   In this example, the component has a state property named `count` initialized to `0`.

2. **Updating State:**
   To update the state, you should never modify it directly. Instead, use the `setState` method provided by React. This method takes an object as an argument, representing the updated state. React then merges the provided object with the current state and triggers a re-render.

   ```jsx
   import React, { Component } from 'react';

   class MyComponent extends Component {
     constructor(props) {
       super(props);
       this.state = {
         count: 0
       };
     }

     handleIncrement = () => {
       this.setState({ count: this.state.count + 1 });
     };

     render() {
       return (
         <div>
           <p>Count: {this.state.count}</p>
           <button onClick={this.handleIncrement}>Increment</button>
         </div>
       );
     }
   }

   export default MyComponent;
   ```

   In this example, the `handleIncrement` method uses `setState` to increment the `count` property in the component's state.

3. **Functional Components and Hooks:**
   With the introduction of Hooks in React 16.8, functional components can now also have state using the `useState` hook. Here's a simple example:

   ```jsx
   import React, { useState } from 'react';

   const MyFunctionalComponent = () => {
     const [count, setCount] = useState(0);

     const handleIncrement = () => {
       setCount(count + 1);
     };

     return (
       <div>
         <p>Count: {count}</p>
         <button onClick={handleIncrement}>Increment</button>
       </div>
     );
   };

   export default MyFunctionalComponent;
   ```

   In this functional component, the `useState` hook is used to declare the `count` state variable and the `setCount` function, which is used to update the state.

State management is crucial for building dynamic and interactive user interfaces in React. By using state, components can respond to user interactions, data changes, and other events, providing a dynamic and responsive user experience.

## Hooks in React

Functions starting with 'use' are Hooks. 
React Hooks are functions that allow functional components to use state and lifecycle features that were previously only available in class components. Introduced in React 16.8, Hooks provide a more direct way to interact with React's features in functional components, making it easier to reuse stateful logic and manage side effects.

Here are some of the most commonly used React Hooks:

1. **useState:**
   - Allows functional components to have local state.
   - Returns an array with two elements: the current state value and a function to update it.
   
   ```jsx
   import React, { useState } from 'react';

   function Example() {
     const [count, setCount] = useState(0);

     return (
       <div>
         <p>You clicked {count} times</p>
         <button onClick={() => setCount(count + 1)}>
           Click me
         </button>
       </div>
     );
   }
   ```

2. **useEffect:**
   - Enables performing side effects in functional components.
   - Takes a function that contains the code for the side effect.
   - Can be used for tasks like data fetching, subscriptions, manual DOM manipulations, etc.

   ```jsx
   import React, { useState, useEffect } from 'react';

   function Example() {
     const [count, setCount] = useState(0);

     useEffect(() => {
       document.title = `You clicked ${count} times`;
     }, [count]); // Run the effect only when count changes

     return (
       <div>
         <p>You clicked {count} times</p>
         <button onClick={() => setCount(count + 1)}>
           Click me
         </button>
       </div>
     );
   }
   ```

3. **useContext:**
   - Allows functional components to consume values from the React context.
   - Takes a context object (created by `React.createContext`) and returns the current context value.

   ```jsx
   import React, { useContext } from 'react';
   import MyContext from './MyContext';

   function MyComponent() {
     const contextValue = useContext(MyContext);

     return <p>{contextValue}</p>;
   }
   ```

4. **useReducer:**
   - Provides an alternative to `useState` when dealing with more complex state logic.
   - Takes a reducer function and an initial state, returning the current state and a dispatch function.

   ```jsx
   import React, { useReducer } from 'react';

   const initialState = { count: 0 };

   function reducer(state, action) {
     switch (action.type) {
       case 'increment':
         return { count: state.count + 1 };
       case 'decrement':
         return { count: state.count - 1 };
       default:
         return state;
     }
   }

   function Counter() {
     const [state, dispatch] = useReducer(reducer, initialState);

     return (
       <div>
         Count: {state.count}
         <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
         <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
       </div>
     );
   }
   ```

These are just a few examples of the many hooks available in React. Hooks provide a more concise and readable way to work with state and side effects in functional components, promoting better code organization and reusability.

## Event Handling in React

In React, event handling is similar to handling events in HTML but with some differences due to the nature of React components. React provides a consistent way to handle events across different browsers and abstracts away some of the complexities.

Here's a basic overview of handling events in React:

1. **Event Naming:**
   - React events are named using camelCase, similar to how they are in JavaScript. For example, `onClick` instead of `onclick`.
   - Some common events include `onClick`, `onChange`, `onSubmit`, etc.

2. **Event Handling in JSX:**
   - You can attach event handlers directly to JSX elements. The handler should be a function or a reference to a method.
   - For example, handling a button click event:

   ```jsx
   import React from 'react';

   class MyComponent extends React.Component {
     handleClick() {
       console.log('Button clicked!');
     }

     render() {
       return (
         <button onClick={this.handleClick}>Click me</button>
       );
     }
   }
   ```

3. **Passing Parameters to Event Handlers:**
   - If you need to pass additional parameters to the event handler, you can use an arrow function or bind the function.

   ```jsx
   import React from 'react';

   class MyComponent extends React.Component {
     handleClickWithParameter(id) {
       console.log(`Button clicked with ID: ${id}`);
     }

     render() {
       const itemId = 123;
       return (
         <button onClick={() => this.handleClickWithParameter(itemId)}>Click me</button>
       );
     }
   }
   ```

   - Alternatively, you can bind the function in the constructor:

   ```jsx
   constructor(props) {
     super(props);
     this.handleClickWithParameter = this.handleClickWithParameter.bind(this);
   }
   ```

4. **Preventing Default Behavior:**
   - If you're handling a form submission, you might want to prevent the default behavior to avoid a page reload.

   ```jsx
   import React from 'react';

   class MyForm extends React.Component {
     handleSubmit(event) {
       event.preventDefault();
       console.log('Form submitted!');
     }

     render() {
       return (
         <form onSubmit={this.handleSubmit}>
           {/* Form elements */}
           <button type="submit">Submit</button>
         </form>
       );
     }
   }
   ```

5. **Handling State in Event Handlers:**
   - You often want to update the component state based on user interactions.

   ```jsx
   import React from 'react';

   class Counter extends React.Component {
     constructor(props) {
       super(props);
       this.state = {
         count: 0,
       };
     }

     handleIncrement() {
       this.setState({ count: this.state.count + 1 });
     }

     render() {
       return (
         <div>
           <p>Count: {this.state.count}</p>
           <button onClick={() => this.handleIncrement()}>Increment</button>
         </div>
       );
     }
   }
   ```

These are some basic patterns for event handling in React. Remember to be cautious when using arrow functions in JSX, as they can lead to unnecessary re-renders if used in render methods. Using class methods or binding functions in the constructor can help mitigate this issue.

## Useful JS Functions

1. **Map:** Create a new array by doing something in each item of an array.
2. **Filter:** Create a new array by keeping the items that return true.
3. **Reduce:** Accumulate a value by doing something to each item in an array.
4. **Find:** Find the first item that matches from an array.
5. **FindIndex:** Find the index of the first item that matches.

### Map Function

Each React component must have a `create{ComponentName}()` function, taking in the passed object and returning the component's function to generate a list of data as a list of React components.

```javascript
function createMyComponent(item) {
  return (
    <MyComponent key={item.id} data={item} />
  );
}

const myComponents = myData.map(createMyComponent);
```

## Using JS Short Circuit and Truthy Falsy Properties in React Codes
Exploit the conditional (ternary) operator, truthy-falsy, and logical short-circuiting for writing concise code.

Example:

```javascript
function Form(props) {
  return (
    <form className="form">
      <input type="text" placeholder="Username" />
      { !props.isRegistered && <input type="password" placeholder="Confirm Password" />}
      <button type="submit">{props.isRegistered ? "Login" : "Register"}</button>
    </form>
  );
}

```

## Important Links

1. [My Code Sample](https://codesandbox.io/s/introduction-to-jsx-forked-j5wyjn?file=/src/index.js)
2. [JSX Syntax](https://github.com/airbnb/javascript/blob/master/react/README.md)
3. [Practice: Keeper App](https://codesandbox.io/s/keeper-app-part-1-starting-forked-7ht3v2)
4. [HTML, CSS Notes](https://codepen.io/NarendranAI/pen/VwXENJO)

