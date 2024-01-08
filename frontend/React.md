# React

## Table of Contents

- [React](#react)
  - [Table of Contents](#table-of-contents)
  - [Cohesion and Coupling](#cohesion-and-coupling)
  - [Useful VS Code extensions](#useful-vs-code-extensions)
  - [Useful browser extensions](#useful-browser-extensions)
  - [Populer UI Libraries](#populer-ui-libraries)
  - [Icons for UI](#icons-for-ui)
    - [Installation](#installation)
    - [Usage](#usage)
    - [Icon Properties](#icon-properties)
  - [Update logic Libraries](#update-logic-libraries)
  - [React Form Libraries](#react-form-libraries)
  - [Validation Libraries](#validation-libraries)
    - [Joi vs Zod](#joi-vs-zod)
  - [HTTP request library](#http-request-library)
  - [CSS in JS styling libraries](#css-in-js-styling-libraries)
  - [TS types for JS library variables](#ts-types-for-js-library-variables)
  - [Preferred backend stacks](#preferred-backend-stacks)
  - [Setup Local Environment](#setup-local-environment)
    - [1. create-react-app](#1-create-react-app)
    - [2. Vite](#2-vite)
  - [JSX](#jsx)
  - [TSX](#tsx)
  - [Useful JS Functions](#useful-js-functions)
    - [1. Map Function:](#1-map-function)
    - [2. Filter Function:](#2-filter-function)
    - [3. Reduce Function:](#3-reduce-function)
    - [4. Find Function:](#4-find-function)
    - [5. FindIndex Function:](#5-findindex-function)
  - [Using JS Short Circuit and Truthy Falsy Properties in React Codes](#using-js-short-circuit-and-truthy-falsy-properties-in-react-codes)
    - [JS Short Circuiting:](#js-short-circuiting)
    - [Truthy/Falsy Properties:](#truthyfalsy-properties)
    - [React example](#react-example)
  - [What is React?](#what-is-react)
  - [Opinions of React](#opinions-of-react)
  - [React Elements](#react-elements)
  - [React Components](#react-components)
    - [Fragment](#fragment)
    - [Props](#props)
      - [**Passing function via a prop**](#passing-function-via-a-prop)
      - [**Passing children via prop**](#passing-children-via-prop)
  - [State](#state)
    - [Declarative State Programming](#declarative-state-programming)
    - [Imperative State Programming](#imperative-state-programming)
  - [React State](#react-state)
    - [**Understanding the state hook:**](#understanding-the-state-hook)
    - [**Choosing the state structure:**](#choosing-the-state-structure)
    - [**Keeping the components Pure:**](#keeping-the-components-pure)
    - [**Understanding strictmode**](#understanding-strictmode)
    - [**Updating state objects:**](#updating-state-objects)
    - [**Updating nested state objects:**](#updating-nested-state-objects)
    - [**Updating Array states:**](#updating-array-states)
    - [**Updating Array of object:**](#updating-array-of-object)
    - [**Simplify update Logics using immer library:**](#simplify-update-logics-using-immer-library)
      - [**A quick example for comparison:**](#a-quick-example-for-comparison)
      - [**Installing immer**](#installing-immer)
      - [**Using immer with React**](#using-immer-with-react)
    - [**Sharing state between component:**](#sharing-state-between-component)
  - [Basic Overview of react state](#basic-overview-of-react-state)
  - [Props vs State](#props-vs-state)
  - [Hooks in React](#hooks-in-react)
  - [Event Handling in React](#event-handling-in-react)
    - [Using class components](#using-class-components)
    - [Using functions components](#using-functions-components)
  - [React Form](#react-form)
    - [Controlled Components](#controlled-components)
    - [useState vs useRef](#usestate-vs-useref)
      - [**useState**:](#usestate)
      - [**useRef**:](#useref)
      - [**Combining `useState` and `useRef`**:](#combining-usestate-and-useref)
    - [Managing forms with React Hook Form (library)](#managing-forms-with-react-hook-form-library)
  - [React Form Validation](#react-form-validation)
    - [Installing Zod](#installing-zod)
    - [To integrate react-hook-form with zod, we need @hookform/resolvers](#to-integrate-react-hook-form-with-zod-we-need-hookformresolvers)
    - [Usage](#usage-1)
  - [Connecting to a backend](#connecting-to-a-backend)
    - [Reusable API Client \& User Service](#reusable-api-client--user-service)
      - [**api-client.ts**](#api-clientts)
      - [**user-service.ts**](#user-servicets)
      - [**App.tsx**](#apptsx)
      - [Generic Http service](#generic-http-service)
        - [http-service.ts](#http-servicets)
        - [user-service.ts](#user-servicets-1)
        - [App.tsx](#apptsx-1)
  - [Important Links](#important-links)
- [Summary](#summary)
  - [Getting started with React](#getting-started-with-react)
  - [Building components](#building-components)
    - [Creating a component](#creating-a-component)
    - [Rendering a List](#rendering-a-list)
    - [Conditional Rendering](#conditional-rendering)
    - [Handling events](#handling-events)
    - [Defining state](#defining-state)
    - [Props](#props-1)
    - [Passing Children](#passing-children)
  - [Styling components](#styling-components)
    - [Vanilla CSS](#vanilla-css)
    - [CSS modules](#css-modules)
    - [CSS in JS](#css-in-js)
    - [Inline Styling](#inline-styling)
  - [Managing Component State](#managing-component-state)
    - [Updating Objects](#updating-objects)
    - [Updating nested objects](#updating-nested-objects)
    - [Updating arrays](#updating-arrays)
    - [Updating array of objects](#updating-array-of-objects)
    - [Updating with Immer](#updating-with-immer)

## Cohesion and Coupling

- Cohesion and coupling are two key concepts in software engineering that are used to measure the quality of a software system’s design.
- Cohesion refers to the degree to which elements within a module work together to fulfill a single, well-defined purpose.
- High cohesion means that elements are closely related and focused on a single purpose, while low cohesion means that elements are loosely related and serve multiple purposes.
- Coupling refers to the degree of interdependence between software modules.
- High coupling means that modules are closely connected and changes in one module may affect other modules.
- Low coupling means that modules are independent and changes in one module have little impact on other modules.
- Both coupling and cohesion are important factors in determining the maintainability, scalability, and reliability of a software system. High cohesion and low coupling can make a system easier to maintain and improve.

## Useful VS Code extensions

1. [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
2. [Simple React Snippets](https://marketplace.visualstudio.com/items?itemName=burkeholland.simple-react-snippets)
3. [ES7+ React/Redux/React-Native snippets](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)

After installing above extensions, at preference > settings of VS Code, make default formatter as "Prettier" and enable format on save.

## Useful browser extensions

1. [React Dev Tools](https://microsoftedge.microsoft.com/addons/detail/react-developer-tools/gpphkfbcpidddadnkolkpfckpihlkkil)
2. [Pesticide](https://chrome.google.com/webstore/detail/bakpbgckdnepkmkeaiomhmfcnejndkbi)

## Populer UI Libraries

1. [Bootstrap](https://getbootstrap.com/)
2. [Material UI](https://mui.com/material-ui/)
3. [Tailwind CSS](https://tailwindcss.com/)

## Icons for UI

1. [react-icons](https://react-icons.github.io/react-icons/)

### Installation

```bash
npm install react-icons --save
```

### Usage

```tsx
import { FaBeer } from "react-icons/fa";

function Question() {
  return (
    <h3>
      Let's go for a <FaBeer />?
    </h3>
  );
}
```

### Icon Properties

| Key         | Default               | Notes                              |
| ----------- | --------------------- | ---------------------------------- |
| `color`     | `undefined` (inherit) |                                    |
| `size`      | `1em`                 |                                    |
| `className` | `undefined`           |                                    |
| `style`     | `undefined`           | Can overwrite size and color       |
| `attr`      | `undefined`           | Overwritten by other attributes    |
| `title`     | `undefined`           | Icon description for accessibility |

## Update logic Libraries

1. [immer](https://github.com/immerjs/immer#readme)

## React Form Libraries

1. [React hook form](https://www.react-hook-form.com/)

## Validation Libraries

1. [Joi](https://github.com/hapijs/joi#readme)
2. [Yup](https://github.com/jquense/yup)
3. [Zod](https://github.com/colinhacks/zod)

### Joi vs Zod

**Joi** and **Zod** are both popular JavaScript validation libraries, each with their own strengths and use cases.

**Joi** is a mature and feature-rich validation library, often used in Node.js applications. It provides a wide range of validation rules and customization options, making it suitable for complex validation scenarios. However, it can add some overhead to smaller projects and may require more effort for type safety in TypeScript projects.

**Pros of Joi**:

- Battle-tested and widely used in the Node.js ecosystem.
- Extensive set of validation rules and options.
- Robust error handling and customization capabilities.

**Cons of Joi**:

- Bulkier and can add some overhead to smaller projects.
- May require more effort for type safety in TypeScript projects.

On the other hand, **Zod** is a powerful validation library that shines in its ability to leverage TypeScript's type system. If you're using TypeScript, Zod's type inference will ensure that your validation code is type-safe, providing extra confidence in your data handling. However, there might be a learning curve for developers unfamiliar with TypeScript.

**Pros of Zod**:

- Strong TypeScript support for type-safe validation.
- Powerful validation capabilities and rich validation API.
- Excellent error messages and error handling.

**Cons of Zod**:

- Learning curve for developers unfamiliar with TypeScript.
- Might add overhead for projects not using TypeScript.

In conclusion, the choice between Joi and Zod depends on your project's requirements, familiarity with TypeScript, and the level of complexity your data validation demands.

## HTTP request library

1. [Axios](https://axios-http.com/)

## CSS in JS styling libraries

1. [Styled components](https://styled-components.com/)
2. [Emotion](https://github.com/emotion-js/emotion/tree/main#readme)
3. [Polished](https://polished.js.org/)

## TS types for JS library variables

1. @types

## Preferred backend stacks

1. Node + Express
2. C#/ASP.NET
3. Firebase

## Setup Local Environment

1. Install Node LTS.
2. Install VS Code and extensions like "sublime babel" and "vscode icons."
3. Create a React app & run it in dev server mode using the command:

There are two ways to create react app

### 1. create-react-app

React's official tool.

```bash
npx create-react-app my-app
cd my-app
npm start
```

With the above commands we get a development server, Webpack and Babel with zero configurations needed from our side.

However, one can edit the configurations using below command

```bash
npm run eject
```

When ejected, all hidden dependencies (maintained by create-react-app) will be exposed in package.json and can be edited as per our preference.

### 2. Vite

Vite (French word for "quick", pronounced [`/vit/`](https://cdn.jsdelivr.net/gh/vitejs/vite@main/docs/public/vite.mp3), like "veet") is a new breed of frontend build tooling that significantly improves the frontend development experience. It consists of two major parts:

- A dev server that serves your source files over [native ES modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules), with [rich built-in features](https://vitejs.dev/guide/features.html) and astonishingly fast [Hot Module Replacement (HMR)](https://vitejs.dev/guide/features.html#hot-module-replacement).

- A [build command](https://vitejs.dev/guide/build.html) that bundles your code with [Rollup](https://rollupjs.org), pre-configured to output highly optimized static assets for production.

In addition, Vite is highly extensible via its [Plugin API](https://vitejs.dev/guide/api-plugin.html) and [JavaScript API](https://vitejs.dev/guide/api-javascript.html) with full typing support.

[Read the Docs to Learn More](https://vitejs.dev).

```bash
npm create vite@latest
# Then choose your project name and technology etc.,

# Change working directory to your root
cd my-first-react-vite-app

# Initialize git
git init

# Install dependencies
npm install

# Start the dev server
npm run dev
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
     <div>{isLoggedIn ? <p>Welcome, User!</p> : <p>Please log in</p>}</div>
   );
   ```

JSX makes it more convenient to work with React by providing a syntax that closely resembles the final output. While it might look like HTML, it's important to remember that JSX is not HTML; it's a syntactic sugar for `React.createElement` calls, producing React elements that are then rendered to the DOM.

Goto https://babeljs.io/repl for converting JSX to vanilla JS which any browser can acceot for more understanding of JSX.

## TSX

1. **Definition:**

   - TSX stands for TypeScript JSX. It is a syntax extension for TypeScript that allows developers to write JSX (JavaScript XML) syntax with the benefits of static typing provided by TypeScript.

2. **JSX in React:**

   - JSX is a syntax extension for JavaScript often used with React. It allows developers to write HTML-like code within JavaScript files. JSX makes it easier to describe what the UI should look like.

3. **TypeScript Integration:**

   - TypeScript is a superset of JavaScript that adds static typing to the language. TSX is an extension of JSX specifically designed to work seamlessly with TypeScript. It allows developers to write React components with strong typing.

4. **Benefits of TypeScript with JSX (TSX):**

   - **Static Typing:** TypeScript adds a layer of static typing to JavaScript, reducing runtime errors and improving code maintainability.
   - **Code Autocompletion:** Developers get better tooling support, including autocompletion and inline documentation, in TypeScript-enabled IDEs.
   - **Early Error Detection:** TypeScript can catch certain types of errors during development, providing feedback to developers before runtime.

5. **Example TSX Code in React:**

   ```tsx
   import React, { useState } from "react";

   // A simple React component using TSX
   const MyComponent: React.FC = () => {
     const [count, setCount] = useState<number>(0);

     return (
       <div>
         <p>Count: {count}</p>
         <button onClick={() => setCount(count + 1)}>Increment</button>
       </div>
     );
   };

   export default MyComponent;
   ```

6. **Type Annotations in TSX:**

   - TypeScript allows developers to annotate types explicitly. In the example above, `useState<number>` specifies that the `count` state should be of type `number`.

7. **Integration with React Ecosystem:**
   - Many libraries and tools in the React ecosystem support TypeScript, making it easier for developers to adopt TypeScript in their React projects.

In summary, TSX in React refers to using TypeScript with JSX syntax. It enhances React development by providing static typing, improved tooling, and early error detection, making it a popular choice for developers building robust and scalable React applications.

## Useful JS Functions

1. **Map:** Create a new array by doing something in each item of an array.
2. **Filter:** Create a new array by keeping the items that return true.
3. **Reduce:** Accumulate a value by doing something to each item in an array.
4. **Find:** Find the first item that matches from an array.
5. **FindIndex:** Find the index of the first item that matches.

### 1. Map Function:

```javascript
// Example: Create a new array by doubling each item in the original array
const originalArray = [1, 2, 3, 4];
const doubledArray = originalArray.map((item) => item * 2);
// doubledArray: [2, 4, 6, 8]
```

### 2. Filter Function:

```javascript
// Example: Create a new array by keeping only the even numbers from the original array
const originalArray = [1, 2, 3, 4, 5, 6];
const evenNumbers = originalArray.filter((item) => item % 2 === 0);
// evenNumbers: [2, 4, 6]
```

### 3. Reduce Function:

```javascript
// Example: Accumulate the sum of all items in the original array
const originalArray = [1, 2, 3, 4];
const sum = originalArray.reduce((accumulator, item) => accumulator + item, 0);
// sum: 10
```

### 4. Find Function:

```javascript
// Example: Find the first even number in the original array
const originalArray = [1, 3, 5, 2, 7];
const firstEvenNumber = originalArray.find((item) => item % 2 === 0);
// firstEvenNumber: 2
```

### 5. FindIndex Function:

```javascript
// Example: Find the index of the first item greater than 3 in the original array
const originalArray = [1, 2, 4, 5];
const index = originalArray.findIndex((item) => item > 3);
// index: 2
```

These examples showcase the basic usage of each function. You can customize the functions based on your specific use cases and data structures.

## Using JS Short Circuit and Truthy Falsy Properties in React Codes

### JS Short Circuiting:

1. **Using `&&` Operator:**

```javascript
// Example: Only execute the second function if the first function returns true
const executeFirstFunction = true;
const executeSecondFunction = false;

executeFirstFunction && console.log("First function executed");
executeSecondFunction && console.log("Second function executed");
// Output: First function executed
```

2. **Using `||` Operator:**

```javascript
// Example: Use default value if the provided value is falsy
const providedValue = null;
const defaultValue = "Default Value";

const result = providedValue || defaultValue;
console.log(result);
// Output: Default Value
```

### Truthy/Falsy Properties:

1. **Truthy Example:**

```javascript
// Example: Check if a variable has a truthy value
const truthyValue = "Hello, World!";

if (truthyValue) {
  console.log("Truthy value is present");
} else {
  console.log("Falsy value");
}
// Output: Truthy value is present
```

2. **Falsy Example:**

In JavaScript, the following values are considered falsy:

- **Falsy Primitive Values:**

  - `false`: The boolean value `false`.
  - `0`: The number zero.
  - `''` or `""`: Empty string.
  - `null`: A special keyword representing an empty or non-existent value.
  - `undefined`: A variable that has not been assigned a value.
  - `NaN`: Not a Number, resulting from an undefined or unrepresentable mathematical operation.

- **Falsy Objects:**
  - `document.all`: An object representing all HTML elements in the document.
  - `[]` or `new Array()`: An empty array.
  - `{}` or `new Object()`: An empty object.

These values are considered falsy when used in a boolean context, such as in conditional statements (`if`, `while`, etc.) or with logical operators (`&&`, `||`). All other values not mentioned in the falsy list are considered truthy.

```javascript
// Example: Check if a variable has a falsy value
const falsyValue = 0;

if (!falsyValue) {
  console.log("Falsy value is present");
} else {
  console.log("Truthy value");
}
// Output: Falsy value is present
```

These examples illustrate the concept of short circuiting and checking truthy/falsy values in JavaScript. Feel free to modify them based on your specific scenarios.

### React example

Exploit the conditional (ternary) operator, truthy-falsy, and logical short-circuiting for writing concise code.

Example:

```javascript
function Form(props) {
  return (
    <form className="form">
      <input type="text" placeholder="Username" />
      {!props.isRegistered && (
        <input type="password" placeholder="Confirm Password" />
      )}
      <button type="submit">{props.isRegistered ? "Login" : "Register"}</button>
    </form>
  );
}
```

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

## Opinions of React

- UI
- Routing
- HTTP
- Managing app state
- Internationalization
- Form Validation
- Animations

Out of all the above concerns, react has opinion on only the UI part.
Also, react is agnostic to whether the app is web app (ReactDOM) or mobile (React Native).

## React Elements

In React, elements are the smallest building blocks of a React application. They are plain JavaScript objects that represent what you want to see on the screen. A React element is an immutable description of what to render.

Here is a basic example of a React element:

```tsx
const element = <h1>Hello, React!</h1>;
```

In this example, `<h1>Hello, React!</h1>` is a JSX expression that represents a React element. Behind the scenes, JSX gets transpiled into a `React.createElement` function call:

```tsx
const element = React.createElement("h1", null, "Hello, React!");
```

The `React.createElement` function takes three arguments:

1. The type of the element (in this case, `'h1'` for a heading).
2. The properties or attributes of the element (in this case, `null` since there are no attributes).
3. The content or children of the element (in this case, the text string `'Hello, React!'`).

React elements are lightweight, and they describe what you want to see on the screen. They are not the actual rendered UI elements; instead, they are virtual representations of the DOM elements. React uses a process called reconciliation to efficiently update the actual DOM based on changes to the virtual DOM.

Once you have a React element, you can render it to the DOM using the `ReactDOM.render()` function. Here's a simple example:

```jsx
import React from "react";
import ReactDOM from "react-dom";

const element = <h1>Hello, React!</h1>;

ReactDOM.render(element, document.getElementById("root"));
```

```tsx
/* eslint-disable linebreak-style */
import React from "react";
import ReactDOM from "react-dom/client";
import { App } from "./App";

ReactDOM.createRoot(document.getElementById("root") as HTMLElement).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

In this example, the `ReactDOM.render()` function takes the React element (`<h1>Hello, React!</h1>`) and renders it to a container element with the ID of `'root'` in the HTML document.

React elements are often used in conjunction with components. Components are reusable pieces of code that can contain one or more React elements. They help structure the UI and make it easier to manage and maintain large React applications.

## React Components

Component - A piece of UI, that are independent, isolated and reusable.

React apps are made of components, which are JavaScript functions returning markup. Components must start with a capital letter.
Just like HTML attributes, React components have props. Props can be received as arguments.

In React, components are the fundamental building blocks of a user interface. They are reusable, self-contained pieces of code that define a part of a user interface. React applications are typically built by composing these components together to create the complete UI.

A react app always have a root component and contains other child components. So every react app is essentially a tree of components.

There are two main types of components in React:

1. **Functional Components:** These are simpler and more concise components that are defined as JavaScript functions. They follow Pascal case for naming convention. They take in props (properties) as an argument and return React elements, describing what should be rendered on the screen. Functional components were stateless until the introduction of hooks in React 16.8, which allowed functional components to manage state and have lifecycle methods.

   Example of a functional component:

   ```jsx
   import React from "react";

   const MyFunctionalComponent = (props) => {
     return <div>Hello, {props.name}!</div>;
   };

   export default MyFunctionalComponent;
   ```

2. **Class Components:** These are ES6 classes that extend from `React.Component`. Class components have more features than functional components, such as the ability to manage local state and access lifecycle methods. Before the introduction of hooks, class components were the primary way to manage state in React.

   Example of a class component:

   ```jsx
   import React, { Component } from "react";

   class MyClassComponent extends Component {
     constructor(props) {
       super(props);
       this.state = {
         count: 0,
       };
     }

     render() {
       return (
         <div>
           Count: {this.state.count}
           <button
             onClick={() => this.setState({ count: this.state.count + 1 })}
           >
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

### Fragment

To return multiple html elements we can enclose them with a parent div element. However, it will create unnecessary creation of an extra div in DOM.
So, we can import and use Fragment element to wrap them.

```tsx
import { Fragment } from "react";
function MultiElem() {
  return (
    <Fragment>
      <h1>Heading</h1>
      <span>Some Content</span>
    </Fragment>
  );
}
```

There is a shortcut for fragments. We can use empty tags to represent fragments even without importing them.

```tsx
function MultiElem() {
  return (
    <>
      <h1>Heading</h1>
      <span>Some Content</span>
    </>
  );
}
```

### Props

- In React, "props" is a shorthand for "properties," and it refers to the mechanism by which data is passed from a parent component to its child components.
- Props are used to transmit information from one component to another in a unidirectional flow.
- They are immutable and are used to configure and customize child components based on the requirements of the parent component.
- Props can contain various types of data, such as strings, numbers, functions, or even other React components.
- Components can access and use the data passed through props to render dynamic content or perform specific actions.

#### **Passing function via a prop**

- ListGroup.tsx

```tsx
/* eslint-disable linebreak-style */
interface Props {
  items: string[];
  heading: string;
  headingColor?: "red" | "blue" | "Green";
  // (item:string) => void   - A generic function that can take in a string arg and return void
  onSelectItem: (item: string) => void;
}

function ListGroup({
  items,
  heading,
  headingColor = "red",
  onSelectItem,
}: Props) {
  return (
    <>
      <h1 style={{ color: headingColor }}>{heading}</h1>

      {items.map((item, index) => (
        <li
          onClick={() => {
            onSelectItem(item);
          }}
          key={index}
        >
          {item}
        </li>
      ))}
    </>
  );
}

export default ListGroup;
```

- App.tsx

```tsx
import ListGroup from "./Components/ListGroup";

function App() {
  const items = ["Chennai", "Bangalore", "Mumbai"];
  const handleSelectItem = (item: string) => {
    console.log(item);
  };
  return (
    <div>
      <ListGroup
        items={items}
        heading="Cities"
        onSelectItem={handleSelectItem}
      />
    </div>
  );
}

export default App;
```

#### **Passing children via prop**

In React, you can pass children to a component by using the special children prop. The children prop allows you to include arbitrary JSX/TSX elements or components between the opening and closing tags of a component. The content placed between the tags is then passed to the component as the children prop.

- Alert.tsx

```tsx
import React, { ReactNode } from "react";

interface Props {
  // message : string  ---> We use it when we want to pass the message as an attribute
  // example: <Alert message = "Hello World" />

  // children : string ---> We use it to pass data inside Alert tag as a child. But cannot have HTML markups
  // example: <Alert> Hello World !</Alert>

  // children: ReactNode ----> We use it to pass data inside Alert tag as a child and allows html markups in JSX/TSX syntax.
  // example: <Alert> Hello <i>World</i> </Alert>
  children: ReactNode;
}

const Alert = ({ children }: Props) => {
  return <div>{children}</div>;
};

export default Alert;
```

- App.tsx

```tsx
import Alert from "./components/Alert";

function App() {
  return (
    <div>
      <Alert>
        Hello <span>World </span>
      </Alert>
    </div>
  );
}

export default App;
```

## State

State makes UI interactive. UI = f(state). There are declarative and imperative approaches.

### Declarative State Programming

UI depends on the value of a state variable. Hooks help hook into state variables and read/modify them.

### Imperative State Programming

More primitive, used in early JS learning for frontend.

## React State

In React, `state` is a fundamental concept that allows components to keep track of and manage their internal data. The `state` object is used to store and update information that can change over time, causing the component to re-render when the state is updated.

### **Understanding the state hook:**

- React updates states asynchronously
- State is stored outside of components (in memory)
- Use state hooks at the top level of your component

### **Choosing the state structure:**

- Avoid redundant state variables (eg: If you have firstname and lastname as states, don't go for a fullname state variable)
- Group related variables inside an object
- Avoid deeply nested structures

### **Keeping the components Pure:**

**Pure Function:**

Given the same input, always returns the same result.

**Pure Component:**

Every time we give the same props, it must always return the same JSX.

`To keep our components pure, we should keep our changes out of the render phase. We should not change any objects that existed before rendering. However, it is totally fine to update objects that are part of the rendering.`

```tsx
// Impure Component

let count = 0;
const Message = () => {
  count++;
  return <div>Message {count}</div>;
};

export default Message;
```

Output:

```
Message 2
Message 4
Message 6
```

```tsx
// Pure Component

const Message = () => {
  let count = 0;
  count++;
  return <div>Message {count}</div>;
};

export default Message;
```

Output:

```
Message 1
Message 1
Message 1
```

### **Understanding strictmode**

- main.tsx

```
import React from 'react'
import ReactDOM from 'react-dom/client'
import { App } from './App'

ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
```

- The `<React.Strictmode>` components is a built in react component that doesn't have a visual representation.
- It is only there to catch potential problems like impure components.
- When this strict mode is enabled, react renders each component twice and takes only the results of second render.
- Hence, we got Message 2, Message 4, Message 6
- The first render is used for detecting and reporting potential issues within our code and only second render is used to update the user interface.

### **Updating state objects:**

- Just like props the objects and arrays must be treated as immutable
- Example,

```tsx
import { useState } from "react";

function App() {
  const [drink, setDrink] = useState({
    title: "cola",
    price: 5,
  });

  return (
    <div>
      <button
        onClick={() => {
          // Only an object with all the properties can be passed to set state objects
          // No partial updates available
          // To prevent typing entire object again and again, one can use spread operator and type only those properties they wish to update
          setDrink({ ...drink, price: 6 });
        }}
      >
        Click Me
      </button>
    </div>
  );
}
export default App;
```

### **Updating nested state objects:**

- The spread operator `...` in JS is shallow.
- It means that if we spread a nested object, the nested properties are referenced from the same memory space for both the old object and the new object (created by spreading)
- We don't want that to happen in our react states
- To avoid that we need to explicitly spread the nested objects within objects.
- Example

```tsx
import { useState } from "react";

function Employee() {
  const [user, setUser] = useState({
    name: "John",
    address: {
      street: "Baker Street",
      city: "London",
      zipCode: "94111",
    },
  });

  // The more deeper our object structure is, the more complex the update logic becomes
  const handleClick = () => {
    setUser({ ...user, address: { ...user.address, street: "Church Street" } });
  };

  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}
export default App;
```

### **Updating Array states:**

- Just like props the objects and arrays must be treated as immutable
- Instead of using regular array's function like push, pop and splice, we must use special methods to handle insertion, updation and deletion.
- Example

```tsx
import { useState } from "react";

function App() {
  const [tags, setTags] = useState(["happy", "cheerful"]);

  const handleClick = () => {
    // Add a tag called 'exciting'
    setTags([...tags, "exciting"]);

    // Remove cheerful tag
    setTags(tags.filter((tag) => tag !== "cheerful"));

    // Update happy tag to happiness
    setTags(tags.map((tag) => (tag === "happy" ? "happiness" : tag)));
  };
  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}

export default App;
```

### **Updating Array of object:**

```tsx
import { useState } from "react";

function App() {
  const [bugs, setBugs] = useState([
    { id: 1, name: "UI Glitch", fixed: "false" },
    { id: 2, name: "Biometric login failed", fixed: "false" },
  ]);

  const handleClick = () => {
    // Fix the bug 1
    setBugs(
      bugs.map((bug) => (bug.id === 1 ? { ...bug, fixed: "true" } : bug))
    );
  };

  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}

export default App;
```

### **Simplify update Logics using immer library:**

Immer can be used in any context in which immutable data structures need to be used. For example in combination with React state, React or Redux reducers, or configuration management. Immutable data structures allow for (efficient) change detection: if the reference to an object didn't change, the object itself did not change. In addition, it makes cloning relatively cheap: Unchanged parts of a data tree don't need to be copied and are shared in memory with older versions of the same state.

Generally speaking, these benefits can be achieved by making sure you never change any property of an object, array or map, but by always creating an altered copy instead. In practice this can result in code that is quite cumbersome to write, and it is easy to accidentally violate those constraints. Immer will help you to follow the immutable data paradigm by addressing these pain points:

1. Immer will detect accidental mutations and throw an error.
2. Immer will remove the need for the typical boilerplate code that is needed when creating deep updates to immutable objects: Without Immer, object copies need to be made by hand at every level. Typically by using a lot of `...` spread operations. When using Immer, changes are made to a draft object, that records the changes and takes care of creating the necessary copies, without ever affecting the original object.
3. When using Immer, you don't need to learn dedicated APIs or data structures to benefit from the paradigm. With Immer you'll use plain JavaScript data structures, and use the well-known mutable JavaScript APIs, but safely.

#### **A quick example for comparison:**

```tsx
const baseState = [
  {
    title: "Learn TypeScript",
    done: true,
  },
  {
    title: "Try Immer",
    done: false,
  },
];
```

Imagine we have the above base state, and we'll need to update the second todo, and add a third one. However, we don't want to mutate the original baseState, and we want to avoid deep cloning as well (to preserve the first todo).

**Without Immer:**

Without Immer, we'll have to carefully shallow copy every level of the state structure that is affected by our change:

```tsx
const nextState = baseState.slice(); // shallow clone the array
nextState[1] = {
  // replace element 1...
  ...nextState[1], // with a shallow clone of element 1
  done: true, // ...combined with the desired update
};
// since nextState was freshly cloned, using push is safe here,
// but doing the same thing at any arbitrary time in the future would
// violate the immutability principles and introduce a bug!
nextState.push({ title: "Tweet about it" });
```

**With Immer**

With Immer, this process is more straightforward. We can leverage the produce function, which takes as first argument the state we want to start from, and as second argument we pass a function, called the recipe, that is passed a draft to which we can apply straightforward mutations. Those mutations are recorded and used to produce the next state once the recipe is done. produce will take care of all the necessary copying, and protect against future accidental modifications as well by freezing the data.

```tsx
import { produce } from "immer";

const nextState = produce(baseState, (draft) => {
  draft[1].done = true;
  draft.push({ title: "Tweet about it" });
});
```

#### **Installing immer**

```bash
npm i immer
```

#### **Using immer with React**

**useState + Immer**

```tsx
import React, { useCallback, useState } from "react";
import {produce} from "immer";

const TodoList = () => {
  const [todos, setTodos] = useState([
    {
      id: "React",
      title: "Learn React",
      done: true
    },
    {
      id: "Immer",
      title: "Try Immer",
      done: false
    }
  ]);

  const handleToggle = useCallback((id) => {
    setTodos(
      produce((draft) => {
        const todo = draft.find((todo) => todo.id === id);
        todo.done = !todo.done;
      })
    );
  }, []);

  const handleAdd = useCallback(() => {
    setTodos(
      produce((draft) => {
        draft.push({
          id: "todo_" + Math.random(),
          title: "A new todo",
          done: false
        });
      })
    );
  }, []);

  return (<div>{*/ See CodeSandbox */}</div>)
}
```

**useImmer**

Since all state updaters follow the same pattern where the update function is wrapped in produce, it is also possible to simplify the above by leveraging the use-immer package that will wrap updater functions in produce automatically:

```tsx
import React, { useCallback } from "react";
import { useImmer } from "use-immer";

const TodoList = () => {
  const [todos, setTodos] = useImmer([
    {
      id: "React",
      title: "Learn React",
      done: true
    },
    {
      id: "Immer",
      title: "Try Immer",
      done: false
    }
  ]);

  const handleToggle = useCallback((id) => {
    setTodos((draft) => {
      const todo = draft.find((todo) => todo.id === id);
      todo.done = !todo.done;
    });
  }, []);

  const handleAdd = useCallback(() => {
    setTodos((draft) => {
      draft.push({
        id: "todo_" + Math.random(),
        title: "A new todo",
        done: false
      });
    });
  }, []);

  // etc
```

**useReducer + Immer**
Similarly to useState, useReducer combines neatly with Immer as well

```tsx
import React, { useCallback, useReducer } from "react";
import { produce } from "immer";

const TodoList = () => {
  const [todos, dispatch] = useReducer(
    produce((draft, action) => {
      switch (action.type) {
        case "toggle":
          const todo = draft.find((todo) => todo.id === action.id);
          todo.done = !todo.done;
          break;
        case "add":
          draft.push({
            id: action.id,
            title: "A new todo",
            done: false,
          });
          break;
        default:
          break;
      }
    }),
    [
      /* initial todos */
    ]
  );

  const handleToggle = useCallback((id) => {
    dispatch({
      type: "toggle",
      id,
    });
  }, []);

  const handleAdd = useCallback(() => {
    dispatch({
      type: "add",
      id: "todo_" + Math.random(),
    });
  }, []);

  // etc
};
```

**useImmerReducer**

...which again, can be slightly shorted by useImmerReducer from the use-immer package:

```tsx
import React, { useCallback } from "react";
import { useImmerReducer } from "use-immer";

const TodoList = () => {
  const [todos, dispatch] = useImmerReducer(
    (draft, action) => {
      switch (action.type) {
        case "toggle":
          const todo = draft.find((todo) => todo.id === action.id);
          todo.done = !todo.done;
          break;
        case "add":
          draft.push({
            id: action.id,
            title: "A new todo",
            done: false
          });
          break;
        default:
          break;
      }
    },
    [ /* initial todos */ ]
  );

  //etc

```

### **Sharing state between component:**

- when a state needs to be shared between two components A and B, then the state must be handled by the closest parent of both A and B.
- The parent component can then send the states as props for the children components.
- Only the parent component which has the state declared, must handle state changes. The children components must not change them as the props are immutable.
- The children however an notify the parent with information as when to change the said state using functions in props.
- Example
- Parent component: App
- Children: Cart and NavBar

---

- App.tsx

```tsx
import { useState } from "react";
import NavBar from "./components/navBar";
import Cart from "./components/Cart";

function App() {
  const [cartItems, setCartItems] = useState(["Dumbell", "Trimmer"]);

  const handleClear = () => {
    setCartitems([]);
  };

  return (
    <>
      <div>
        <NavBar cartItemsCount={cartItems.length} />
        <Cart cartItems={cartItems} onClear={handleClear} />
      </div>
    </>
  );
}
```

- Cart.tsx

```tsx
import React from "react";

interface Props {
  cartItems: string[];
  onClear: () => void;
}

const Cart = ({ cartItems, onClear }: Props) => {
  return (
    <>
      <div>Cart</div>
      <ul>
        {cartItems.map((item, index) => (
          <li key={index}>{item}</li>
        ))}
      </ul>
      <button onClick={onClear}>Clear</button>;
    </>
  );
};

export default Cart;
```

- NavBar.tsx

```tsx
import React from "react";

interface Props {
  cartItemsCount: number;
}

const NavBar = ({ cartItemsCount }: Props) => {
  return (
    <>
      <div>NavBar: {cartItemsCount}</div>;
    </>
  );
};

export default NavBar;
```

## Basic Overview of react state

Here's a basic overview of how state works in React:

1. **Initializing State:**
   You can initialize the state in a class component by setting the `state` property in the component's constructor. Here's an example:

   ```jsx
   import React, { Component } from "react";

   class MyComponent extends Component {
     constructor(props) {
       super(props);
       this.state = {
         count: 0,
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
   import React, { Component } from "react";

   class MyComponent extends Component {
     constructor(props) {
       super(props);
       this.state = {
         count: 0,
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

   ```tsx
   import React, { useState } from "react";

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

## Props vs State

| Props                       | State                       |
| --------------------------- | --------------------------- |
| Input passed to a component | Data managed by a component |
| Similar to function args    | Similar to local variables  |
| Immutable                   | Mutable                     |
| Cause a re-render           | Cause a re-render           |

## Hooks in React

Functions starting with 'use' are Hooks.
React Hooks are functions that allow functional components to use state and lifecycle features that were previously only available in class components. Introduced in React 16.8, Hooks provide a more direct way to interact with React's features in functional components, making it easier to reuse stateful logic and manage side effects.

Here are some of the most commonly used React Hooks:

1. **useState:**

   - Allows functional components to have local state.
   - Returns an array with two elements: the current state value and a function to update it.

   ```jsx
   import React, { useState } from "react";

   function Example() {
     const [count, setCount] = useState(0);

     return (
       <div>
         <p>You clicked {count} times</p>
         <button onClick={() => setCount(count + 1)}>Click me</button>
       </div>
     );
   }
   ```

2. **useEffect:**

   - Enables performing side effects in functional components.
   - Takes a function that contains the code for the side effect.
   - Can be used for tasks like storing data in local storage, data fetching, subscriptions, manual DOM manipulations, etc.

   ```jsx
   import React, { useState, useEffect } from "react";

   function Example() {
     const [count, setCount] = useState(0);

     useEffect(() => {
       document.title = `You clicked ${count} times`;
     }, [count]); // Run the effect only when count changes

     return (
       <div>
         <p>You clicked {count} times</p>
         <button onClick={() => setCount(count + 1)}>Click me</button>
       </div>
     );
   }
   ```

3. **useContext:**

   - Allows functional components to consume values from the React context.
   - Takes a context object (created by `React.createContext`) and returns the current context value.

   ```jsx
   import React, { useContext } from "react";
   import MyContext from "./MyContext";

   function MyComponent() {
     const contextValue = useContext(MyContext);

     return <p>{contextValue}</p>;
   }
   ```

4. **useReducer:**

   - Provides an alternative to `useState` when dealing with more complex state logic.
   - Takes a reducer function and an initial state, returning the current state and a dispatch function.

   ```jsx
   import React, { useReducer } from "react";

   const initialState = { count: 0 };

   function reducer(state, action) {
     switch (action.type) {
       case "increment":
         return { count: state.count + 1 };
       case "decrement":
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
         <button onClick={() => dispatch({ type: "increment" })}>
           Increment
         </button>
         <button onClick={() => dispatch({ type: "decrement" })}>
           Decrement
         </button>
       </div>
     );
   }
   ```

These are just a few examples of the many hooks available in React. Hooks provide a more concise and readable way to work with state and side effects in functional components, promoting better code organization and reusability.

## Event Handling in React

In React, event handling is similar to handling events in HTML but with some differences due to the nature of React components. React provides a consistent way to handle events across different browsers and abstracts away some of the complexities.

### Using class components

`Class Components are not used much anymore. Look much into function components`
Here's a basic overview of handling events in React:

1. **Event Naming:**

   - React events are named using camelCase, similar to how they are in JavaScript. For example, `onClick` instead of `onclick`.
   - Some common events include `onClick`, `onChange`, `onSubmit`, etc.

2. **Event Handling in TSX:**

   - You can attach event handlers directly to TSX elements. The handler should be a function or a reference to a method.
   - For example, handling a button click event:

   ```jsx
   import React from "react";

   class MyComponent extends React.Component {
     handleClick() {
       console.log("Button clicked!");
     }

     render() {
       return <button onClick={this.handleClick}>Click me</button>;
     }
   }
   ```

3. **Passing Parameters to Event Handlers:**

   - If you need to pass additional parameters to the event handler, you can use an arrow function or bind the function.

   ```jsx
   import React from "react";

   class MyComponent extends React.Component {
     handleClickWithParameter(id) {
       console.log(`Button clicked with ID: ${id}`);
     }

     render() {
       const itemId = 123;
       return (
         <button onClick={() => this.handleClickWithParameter(itemId)}>
           Click me
         </button>
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
   import React from "react";

   class MyForm extends React.Component {
     handleSubmit(event) {
       event.preventDefault();
       console.log("Form submitted!");
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
   import React from "react";

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

### Using functions components

```tsx
import { MouseEvent } from "react";

// Event Handler
const handleClick = (event: MouseEvent) => {
  console.log(event);
};

const MyButton = () => {
  return <button onClick="handleClick">Click Me</button>;
};
```

## React Form

In React, forms are a crucial part of building interactive user interfaces. Managing form data and handling user input efficiently is essential for creating dynamic and responsive web applications. React provides a way to handle forms through controlled components, which means that form elements are controlled by the state of the React components.

- Example of a react form

```tsx
import { FormEvent, useRef } from "react";

const Form = () => {
  // Create refs for input elements using the useRef hook
  const nameRef = useRef<HTMLInputElement>(null);
  const ageRef = useRef<HTMLInputElement>(null);

  // Create an object to store form data
  const person = { name: "", age: 0 };

  // Define a function to handle form submission
  const handleSubmit = (event: FormEvent) => {
    // Prevent the default form submission behavior
    event.preventDefault();

    // If the nameRef has a current value, update the person object with the name
    if (nameRef.current) person.name = nameRef.current.value;

    // If the ageRef has a current value, parse it to an integer and update the person object with the age
    if (ageRef.current) person.age = parseInt(ageRef.current.value);

    // Log the person object to the console
    console.log(person);
  };

  // Render a form with input fields and a submit button
  return (
    <form onSubmit={handleSubmit}>
      <div className="mb-3">
        <label htmlFor="name" className="form-label">
          Name
        </label>
        {/* Connect the nameRef to the input field */}
        <input ref={nameRef} id="name" type="text" className="form-control" />
        <label htmlFor="age" className="form-label">
          Age
        </label>
        {/* Connect the ageRef to the input field */}
        <input ref={ageRef} id="age" type="number" className="form-control" />
        {/* Submit button triggers the handleSubmit function */}
        <button type="submit" className="btn btn-primary">
          Submit
        </button>
      </div>
    </form>
  );
};

export default Form;
```

### Controlled Components

In React, a controlled component refers to a form element, like an input or textarea, whose value is controlled by the React component's state. This means that the component's state is the single source of truth for the value of the form element, and any changes to that value are managed through React state and React's lifecycle methods.

Here's a breakdown of how controlled components work in React:

1. **State Management:**

   - The value of the form element is stored in the component's state.
   - The state is initialized with an initial value, and it's updated whenever the user interacts with the form element.

2. **Event Handling:**

   - React provides event handlers (e.g., `onChange` for input elements) to capture user input.
   - When the user interacts with the form element, an event is triggered, and the event handler updates the component's state with the new value.

3. **Rendering:**
   - The value of the form element is then set explicitly using the value attribute, which is controlled by the component's state.
   - Since the value is controlled by React state, the component re-renders with the updated value whenever the state changes.

Here's an example of a controlled input component in React:

```tsx
import { FormEvent, useState } from "react";
import { produce } from "immer";

const Form = () => {
  // State to hold form data (controlled components)
  const [person, setPerson] = useState({
    name: "",
    age: "",
  });

  // Handle form submission
  const handleSubmit = (event: FormEvent) => {
    event.preventDefault();
    // You can perform further actions with the form data here
  };

  return (
    <form onSubmit={handleSubmit}>
      <div className="mb-3">
        {/* Input for Name */}
        <label htmlFor="name" className="form-label">
          Name
        </label>
        <input
          value={person.name} //Controlled by state
          onChange={(event) => {
            // Update the 'name' property of the 'person' state using immer
            setPerson(
              produce((draft) => {
                draft.name = event.target.value;
              })
            );
          }}
          id="name"
          type="text"
          className="form-control"
        />

        {/* Input for Age */}
        <label htmlFor="age" className="form-label">
          Age
        </label>
        <input
          value={person.age} // Controlled by state
          onChange={(event) => {
            // Update the 'age' property of the 'person' state using immer
            setPerson(
              produce((draft) => {
                draft.age = event.target.value;
              })
            );
          }}
          id="age"
          type="number"
          className="form-control"
        />

        {/* Submit Button */}
        <button type="submit" className="btn btn-primary">
          Submit
        </button>
      </div>
    </form>
  );
};

export default Form;
```

### useState vs useRef

`useState` and `useRef` are both React hooks, but they serve different purposes when it comes to forms.

#### **useState**:

1. **State Management:**

   - `useState` is used to declare state variables in functional components.
   - When you update the state using the setter function, the component re-renders.

2. **Reactivity:**

   - Changes to state trigger a re-render of the component, causing any part of the UI using that state to reflect the updated values.

3. **Usage in Forms:**

   - `useState` is commonly used to manage form input values.
   - Example:

     ```jsx
     import React, { useState } from "react";

     function MyForm() {
       const [inputValue, setInputValue] = useState("");

       const handleChange = (event) => {
         setInputValue(event.target.value);
       };

       return <input type="text" value={inputValue} onChange={handleChange} />;
     }
     ```

#### **useRef**:

1. **Immutability:**

   - `useRef` is useful for accessing and interacting with the DOM directly without causing re-renders.
   - Changes to `useRef` do not trigger re-renders.

2. **Preservation between Renders:**

   - The value of a `useRef` persists across renders and component lifecycles.

3. **Usage in Forms:**

   - `useRef` is often used to reference form elements directly or to store mutable values without triggering re-renders.
   - Example:

     ```jsx
     import React, { useRef } from "react";

     function MyForm() {
       const inputRef = useRef();

       const handleClick = () => {
         // Access and manipulate the input element directly using inputRef.current
         inputRef.current.focus();
       };

       return (
         <div>
           <input ref={inputRef} type="text" />
           <button onClick={handleClick}>Focus Input</button>
         </div>
       );
     }
     ```

#### **Combining `useState` and `useRef`**:

You can also use both `useState` and `useRef` together in certain scenarios. For example, you might use `useState` for managing the form input value and `useRef` to store a reference to the input element for direct manipulation.

```jsx
import React, { useState, useRef } from "react";

function MyForm() {
  const [inputValue, setInputValue] = useState("");
  const inputRef = useRef();

  const handleChange = (event) => {
    setInputValue(event.target.value);
  };

  const handleClick = () => {
    // Access and manipulate the input element directly using inputRef.current
    inputRef.current.focus();
  };

  return (
    <div>
      <input
        type="text"
        value={inputValue}
        onChange={handleChange}
        ref={inputRef}
      />
      <button onClick={handleClick}>Focus Input</button>
    </div>
  );
}
```

In this example, `useRef` is used to store a reference to the input element, allowing you to interact with it directly without causing a re-render. Meanwhile, `useState` manages the input value and triggers re-renders when the value changes.

### Managing forms with React Hook Form (library)

React Hook Form is a performant, flexible, and extensible form library for React. It provides an intuitive, feature-complete API that offers a seamless experience to developers when building forms. Here are some of its key features:

- **HTML Standard**: It leverages existing HTML markup and validates your forms with a constraint-based validation API.
- **Performance**: It minimizes the number of re-renders and validation computations, leading to faster mounting.
- **Lightweight**: React Hook Form is a tiny library without any dependencies.
- **Adoptable**: Since form state is inherently local, it can be easily adopted without other dependencies.
- **User Experience (UX)**: It strives to provide the best user experience and brings consistent validation strategies.

React Hook Form reduces the amount of code you need to write while removing unnecessary re-renders. It allows you to isolate component re-renders, which leads to better performance on your page or app. You also have the ability to subscribe to individual input and form state updates without re-rendering the entire form.

**Installation**:

```bash
npm i react-hook-form
```

**Usage:**

Here's a basic usage example:

```jsx
import { useForm, SubmitHandler } from "react-hook-form"

type Inputs = {
  example: string,
  exampleRequired: string,
}

export default function App() {
  const { register, handleSubmit, watch, formState: { errors } } = useForm<Inputs>()
  const onSubmit: SubmitHandler<Inputs> = (data) => console.log(data)

  console.log(watch("example")) // watch input value by passing the name of it

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input defaultValue="test" {...register("example")} />
      <input {...register("exampleRequired", { required: true })} />
      {errors.exampleRequired && <span>This field is required</span>}
      <input type="submit" />
    </form>
  )
}
```

In this example, `useForm` is a custom React Hook that initializes the form. The `register` function is used to register an input or select ref and apply form validation. The `handleSubmit` function is used to handle form submission.

Another example,

```tsx
import { useForm } from "react-hook-form";

interface FormData {
  name: string;
  age: number;
}

const Form = () => {
  const { register, handleSubmit, formState } = useForm<FormData>();
  console.log(formState.isValid);

  return (
    <form
      className="w-75"
      onSubmit={handleSubmit((data) => {
        console.log(data);
      })}
    >
      <div className="mb-3">
        {/* Input for Name */}
        <label htmlFor="name" className="form-label my-2">
          Name
        </label>
        <input
          {...register("name", { minLength: 3, required: true })}
          id="name"
          type="text"
          className="form-control my-2"
          autoComplete="off"
        />
        {formState.errors.name?.type === "required" && (
          <div className="alert alert-warning">This field is required</div>
        )}
        {formState.errors.name?.type === "minLength" && (
          <div className="alert alert-warning">
            This field must have a minLength of 3
          </div>
        )}
        {/* Input for Age */}
        <label htmlFor="age" className="form-label my-2">
          Age
        </label>
        <input
          {...register("age", { required: true, min: 18 })}
          id="age"
          type="number"
          className="form-control my-2"
        />
        {formState.errors.age?.type === "min" && (
          <div className="alert alert-warning">18+ only</div>
        )}

        {/* Submit Button */}
        <button type="submit" className={"btn btn-primary  my-2"}>
          Submit
        </button>
      </div>
    </form>
  );
};

export default Form;
```

## React Form Validation

- Although `react-hook-form` provides validation, it is better to have a schema based validation ratehr than validating everything line by line in the TSX part
- Joi and Zod are two main validation libraries
- Joi can be used for JS based projects
- Zod is for TS based projects
- Since we use react with typescript, we can do validation using Zod

### Installing Zod

```bash
npm i zod
```

### To integrate react-hook-form with zod, we need @hookform/resolvers

```bash
npm i @hookform/resolvers
# Actually this library provides integration with various schema based validation libraries like Zod, Joi, Yup et.,
```

### Usage

```tsx
// Import the 'useForm' hook from the 'react-hook-form' library
import { useForm } from "react-hook-form";

// Import the CSS module for styling
import styles from "./ZodForm.module.css";

// Import 'z' for creating Zod schemas
import { z } from "zod";

// Import 'zodResolver' from '@hookform/resolvers/zod' to use Zod with React Hook Form
import { zodResolver } from "@hookform/resolvers/zod";

// Define a Zod schema for form validation
const schema = z.object({
  // Define a 'name' field with validation rules
  name: z.string().min(1, { message: "Name field is required" }).min(3),

  // Define an 'age' field with validation rules
  age: z
    .number({ invalid_type_error: "Age field is required." })
    .min(18, { message: "18+ only" }),
});

// Define a TypeScript type 'FormData' using the inferred type from the Zod schema
type FormData = z.infer<typeof schema>;

// Functional component 'ZodForm'
const ZodForm = () => {
  // Destructure the values returned from the 'useForm' hook
  const {
    register, // Input registration function
    handleSubmit, // Function to handle form submission
    formState: { errors }, // Form state, including validation errors
    reset, // Function to reset the form
  } = useForm<FormData>({ resolver: zodResolver(schema) }); // Initialize the form with Zod schema validation

  // Render the form
  return (
    <form
      // Apply CSS classes to the form container
      className={[styles.zodReactForm, "w-75"].join(" ")}
      // Handle form submission
      onSubmit={handleSubmit((data) => {
        // Log form data to the console
        console.log(data);
        // Reset the form after submission
        reset();
      })}
    >
      <div className="heading">
        <h2>Form using 'Zod' validation</h2>
      </div>

      {/* Input for 'name' field */}
      <label htmlFor="name" className="form-label">
        Name
      </label>
      <input
        // Register 'name' input with React Hook Form
        {...register("name")}
        // Set input attributes
        id="name"
        type="text"
        className="form-control"
        autoComplete="off"
      />
      {/* Display validation error message if there is an error for 'name' */}
      {errors.name && (
        <div className="alert alert-warning">{errors.name.message}</div>
      )}

      {/* Input for 'age' field */}
      <label htmlFor="age" className="form-label">
        Age
      </label>
      <input
        // Register 'age' input with React Hook Form, also convert input value to a number
        {...register("age", { valueAsNumber: true })}
        // Set input attributes
        id="age"
        type="number"
        className="form-control"
      />
      {/* Display validation error message if there is an error for 'age' */}
      {errors.age && (
        <div className="alert alert-warning">{errors.age.message}</div>
      )}

      {/* Submit button */}
      <button type="submit" className="btn btn-secondary">
        Submit
      </button>
    </form>
  );
};

// Export the 'ZodForm' component
export default ZodForm;
```

## Connecting to a backend

- For this example, we use a fake backend provided by https://jsonplaceholder.typicode.com/
- This implementation is using `axios` for sending http requests
- Not modular enough for being production ready, in further examples we can build a reusable API client.

```tsx
import axios, { AxiosError, CanceledError } from "axios";
import { useState, useEffect } from "react";

interface UserData {
  id: number;
  name: string;
}

const App = () => {
  const [users, setUsers] = useState<UserData[]>([]);
  const [httpErrors, setHttpErrors] = useState("");
  const [isLoading, setIsLoading] = useState(false);

  useEffect(() => {
    // Create an AbortController to cancel the fetch operation when the component is unmounted
    const controller = new AbortController();

    async function fetchUsers(): Promise<void> {
      try {
        setIsLoading(true);
        // Simulate a delay for loading spinner (1 second)
        const delay = (milliseconds: number) =>
          new Promise((resolve) => setTimeout(resolve, milliseconds));

        await delay(1000);
        // Make an HTTP GET request to fetch user data
        const resp = await axios.get<UserData[]>(
          "https://jsonplaceholder.typicode.com/users",
          { signal: controller.signal }
        );

        // Update the state with fetched user data
        setUsers(resp.data);
        setHttpErrors("");
      } catch (error) {
        // Handle errors, including canceled requests
        if (error instanceof CanceledError) return;
        const err = error as AxiosError;
        setHttpErrors(err.message);
      } finally {
        // Set loading state to false after the request is completed
        setIsLoading(false);
      }
    }
    // Invoke the fetchUsers function
    fetchUsers();

    // Cleanup: Abort the fetch operation when the component is unmounted
    return () => controller.abort();
  }, []); // Empty dependency array ensures the effect runs only once

  // Function to handle deletion of a user
  const handleDelete = async (user: UserData) => {
    // Optimistic Update
    // Update the UI, then call the server
    const originalUsers = [...users];
    setUsers(users.filter((x) => x.id !== user.id));
    try {
      // Make an HTTP DELETE request to delete the user
      await axios.delete(
        "https://jsonplaceholder.typicode.com/users/" + user.id
      );
    } catch (error) {
      // Handle errors and revert to the original state on failure
      const err = error as AxiosError;
      setHttpErrors(err.message);
      setUsers(originalUsers);
    }

    // Pessimistic Update
    // Call the server, then Update the UI
    // try {
    //   await axios.delete(
    //     "https://jsonplaceholder.typicode.com/users/" + user.id
    //   );
    //   setUsers(users.filter((x) => x.id !== user.id));
    // } catch (error) {
    //   const err = error as AxiosError;
    //   setHttpErrors(err.message);
    // }
  };

  // Function to handle addition of a new user
  const handleAdd = async () => {
    // usually we get the data from a form submit
    // here we can omit it and hardcode the new user data
    const newUser: UserData = {
      id: 0,
      name: "Naren",
    };

    // Pessimistic Update
    // Because only server can provide ID
    try {
      // Make an HTTP POST request to add a new user
      const resp = await axios.post<UserData>(
        "https://jsonplaceholder.typicode.com/users",
        newUser
      );

      // Update the state with the new user data
      setUsers([...users, resp.data]);
    } catch (error) {
      // Handle errors on failure
      const err = error as AxiosError;
      setHttpErrors(err.message);
    }
  };

  // Function to handle updating an existing user
  const handleUpdate = async (modifiedUser: UserData) => {
    // usually we get the data from a form submit
    // here we can omit it and hardcode the user data for updation
    try {
      // put can be used if we do complete object replacement
      // patch is for partial updates
      // depending on the backend we work with, this may vary

      // Make an HTTP PUT request to update an existing user
      const resp = await axios.put<UserData>(
        "https://jsonplaceholder.typicode.com/users/" + modifiedUser.id,
        modifiedUser
      );
      // Update the state with the modified user data
      setUsers(users.map((x) => (x.id === resp.data.id ? resp.data : x)));
    } catch (error) {
      // Handle errors on failure
      const err = error as AxiosError;
      setHttpErrors(err.message);
    }
  };

  // JSX for rendering UI based on loading state
  const returnData = !isLoading ? (
    <div className="mx-5">
      {httpErrors && <div className="alert alert-danger">{httpErrors}</div>}

      <button onClick={handleAdd} className="btn btn-primary my-2">
        Add
      </button>
      <ul className="list-group">
        {users.map((user) => (
          <li
            className="list-group-item d-flex justify-content-between"
            key={user.id}
          >
            <div>{user.id}</div>
            <div>{user.name}</div>
            <div>
              <button
                onClick={() => {
                  // Simulate Form Modification of user data
                  const modifiedUser = { ...user, name: "mod " + user.name };
                  handleUpdate(modifiedUser);
                }}
                className="btn btn-outline-secondary mx-2"
              >
                Update
              </button>
              <button
                onClick={() => {
                  handleDelete(user);
                }}
                className="btn btn-outline-danger mx-2"
              >
                Delete
              </button>
            </div>
          </li>
        ))}
      </ul>
    </div>
  ) : (
    <>
      <div className="w-100 d-flex flex-column align-items-center my-5">
        <div className="spinner-border my-5"></div>

        <div className="text-info">Loading...</div>
      </div>
    </>
  );

  return returnData;
};

export default App;
```

### Reusable API Client & User Service

##### **api-client.ts**

```ts
// src\services\api-client.ts
import axios from "axios";

export default axios.create({
  baseURL: "https://jsonplaceholder.typicode.com",
  //   headers:{
  //     'api-key':'...'}
});
```

---

##### **user-service.ts**

```ts
// src\services\user-service.ts
import apiClient from "./api-client";

export interface UserData {
  id: number;
  name: string;
}

class UserService {
  async getAllUsers() {
    const controller = new AbortController();
    const resp = await apiClient.get<UserData[]>("/users", {
      signal: controller.signal,
    });
    return {
      resp,
      cancel: () => {
        controller.abort();
      },
    };
  }
  async getUserByID(userID: number) {
    const resp = await apiClient.get<UserData>("/users/" + userID);
    return resp;
  }
  async deleteUserByID(userID: number) {
    await apiClient.delete("/users/" + userID);
  }
  async createUser(newUser: UserData) {
    const resp = await apiClient.post<UserData>("/users", newUser);
    return resp;
  }

  async updateUserByID(userID: number, modifiedUser: UserData) {
    const resp = await apiClient.put<UserData>(
      "/users/" + userID,
      modifiedUser
    );
    return resp;
  }
}

export default new UserService();
```

---

##### **App.tsx**

```tsx
// src\App.tsx

import { useState, useEffect } from "react";
import { AxiosError, CanceledError } from "./services/api-client";
import userService, { UserData } from "./services/user-service";

const App = () => {
  const [users, setUsers] = useState<UserData[]>([]);
  const [httpErrors, setHttpErrors] = useState("");
  const [isLoading, setIsLoading] = useState(false);

  useEffect(() => {
    let controllerAbort: () => void;

    async function fetchUsers(): Promise<void> {
      try {
        setIsLoading(true);
        const delay = (milliseconds: number) =>
          new Promise((resolve) => setTimeout(resolve, milliseconds));

        await delay(1000);
        const { resp, cancel } = await userService.getAllUsers();
        controllerAbort = cancel;

        setUsers(resp.data);
        setHttpErrors("");
      } catch (error) {
        if (error instanceof CanceledError) return;
        const err = error as AxiosError;
        setHttpErrors(err.message);
      } finally {
        setIsLoading(false);
      }
    }

    fetchUsers();

    return () => controllerAbort && controllerAbort();
  }, []);

  const handleDelete = async (user: UserData) => {
    const originalUsers = [...users];
    setUsers(users.filter((x) => x.id !== user.id));
    try {
      await userService.deleteUserByID(user.id);
    } catch (error) {
      const err = error as AxiosError;
      setHttpErrors(err.message);
      setUsers(originalUsers);
    }
  };

  const handleAdd = async () => {
    const newUser: UserData = {
      id: 0,
      name: "Naren",
    };

    try {
      const resp = await userService.createUser(newUser);
      setUsers([...users, resp.data]);
    } catch (error) {
      const err = error as AxiosError;
      setHttpErrors(err.message);
    }
  };

  const handleUpdate = async (modifiedUser: UserData) => {
    try {
      const resp = await userService.updateUserByID(
        modifiedUser.id,
        modifiedUser
      );
      setUsers(users.map((x) => (x.id === resp.data.id ? resp.data : x)));
    } catch (error) {
      const err = error as AxiosError;
      setHttpErrors(err.message);
    }
  };

  const returnData = !isLoading ? (
    <div className="mx-5">
      {httpErrors && <div className="alert alert-danger">{httpErrors}</div>}

      <button onClick={handleAdd} className="btn btn-primary my-2">
        Add
      </button>
      <ul className="list-group">
        {users.map((user) => (
          <li
            className="list-group-item d-flex justify-content-between"
            key={user.id}
          >
            <div>{user.id}</div>
            <div>{user.name}</div>
            <div>
              <button
                onClick={() => {
                  const modifiedUser = { ...user, name: "mod " + user.name };
                  handleUpdate(modifiedUser);
                }}
                className="btn btn-outline-secondary mx-2"
              >
                Update
              </button>
              <button
                onClick={() => {
                  handleDelete(user);
                }}
                className="btn btn-outline-danger mx-2"
              >
                Delete
              </button>
            </div>
          </li>
        ))}
      </ul>
    </div>
  ) : (
    <>
      <div className="w-100 d-flex flex-column align-items-center my-5">
        <div className="spinner-border my-5"></div>

        <div className="text-info">Loading...</div>
      </div>
    </>
  );

  return returnData;
};

export default App;
```

#### Generic Http service

- Can be used for different endpoint like users, posts, etc
- Write only once and it will work for multiple cases
- Must be initiated with endpoint and requests must use data types explicitly based on the endpoint

##### http-service.ts

```ts
// src\services\http-service.ts
import apiClient from "./api-client";

interface Entity {
  id: number;
}

class HttpService {
  endpoint: string;

  constructor(endpoint: string) {
    this.endpoint =
      endpoint[endpoint.length - 1] === "/" ? endpoint : endpoint + "/";
  }

  async getAll<T>() {
    const controller = new AbortController();
    const resp = await apiClient.get<T[]>(this.endpoint, {
      signal: controller.signal,
    });
    return {
      resp,
      cancel: () => {
        controller.abort();
      },
    };
  }
  async getByID<T extends Entity>(entity: T) {
    const resp = await apiClient.get<T>(this.endpoint + entity.id);
    return resp;
  }
  async deleteByID<T extends Entity>(entity: T) {
    await apiClient.delete(this.endpoint + entity.id);
  }
  async create<T extends Entity>(entity: T) {
    const resp = await apiClient.post<T>(this.endpoint, entity);
    return resp;
  }

  async updateByID<T extends Entity>(entity: T) {
    const resp = await apiClient.put<T>(this.endpoint + entity.id, entity);
    return resp;
  }
}

const create = (endpoint: string) => new HttpService(endpoint);
export default create;
```

---

##### user-service.ts

```ts
// src\services\user-service.ts

import create from "./http-service";

export interface UserData {
  id: number;
  name: string;
}

export default create("/users");
```

##### App.tsx

```tsx
import { useState, useEffect } from "react";
import { AxiosError, CanceledError } from "./services/api-client";
import userService, { UserData } from "./services/user-service";

const App = () => {
  const [users, setUsers] = useState<UserData[]>([]);
  const [httpErrors, setHttpErrors] = useState("");
  const [isLoading, setIsLoading] = useState(false);

  useEffect(() => {
    let controllerAbort: () => void;

    async function fetchUsers(): Promise<void> {
      try {
        setIsLoading(true);
        const delay = (milliseconds: number) =>
          new Promise((resolve) => setTimeout(resolve, milliseconds));

        await delay(1000);
        const { resp, cancel } = await userService.getAll<UserData>();
        controllerAbort = cancel;

        setUsers(resp.data);
        setHttpErrors("");
      } catch (error) {
        if (error instanceof CanceledError) return;
        const err = error as AxiosError;
        setHttpErrors(err.message);
      } finally {
        setIsLoading(false);
      }
    }

    fetchUsers();

    return () => controllerAbort && controllerAbort();
  }, []);

  const handleDelete = async (user: UserData) => {
    const originalUsers = [...users];
    setUsers(users.filter((x) => x.id !== user.id));
    try {
      await userService.deleteByID(user);
    } catch (error) {
      const err = error as AxiosError;
      setHttpErrors(err.message);
      setUsers(originalUsers);
    }
  };

  const handleAdd = async () => {
    const newUser: UserData = {
      id: 0,
      name: "Naren",
    };

    try {
      const resp = await userService.create<UserData>(newUser);
      setUsers([...users, resp.data]);
    } catch (error) {
      const err = error as AxiosError;
      setHttpErrors(err.message);
    }
  };

  const handleUpdate = async (modifiedUser: UserData) => {
    try {
      const resp = await userService.updateByID<UserData>(modifiedUser);
      setUsers(users.map((x) => (x.id === resp.data.id ? resp.data : x)));
    } catch (error) {
      const err = error as AxiosError;
      setHttpErrors(err.message);
    }
  };

  const returnData = !isLoading ? (
    <div className="mx-5">
      {httpErrors && <div className="alert alert-danger">{httpErrors}</div>}

      <button onClick={handleAdd} className="btn btn-primary my-2">
        Add
      </button>
      <ul className="list-group">
        {users.map((user) => (
          <li
            className="list-group-item d-flex justify-content-between"
            key={user.id}
          >
            <div>{user.id}</div>
            <div>{user.name}</div>
            <div>
              <button
                onClick={() => {
                  const modifiedUser = { ...user, name: "mod " + user.name };
                  handleUpdate(modifiedUser);
                }}
                className="btn btn-outline-secondary mx-2"
              >
                Update
              </button>
              <button
                onClick={() => {
                  handleDelete(user);
                }}
                className="btn btn-outline-danger mx-2"
              >
                Delete
              </button>
            </div>
          </li>
        ))}
      </ul>
    </div>
  ) : (
    <>
      <div className="w-100 d-flex flex-column align-items-center my-5">
        <div className="spinner-border my-5"></div>

        <div className="text-info">Loading...</div>
      </div>
    </>
  );

  return returnData;
};

export default App;
```

## Important Links

1. [My Code Sample](https://codesandbox.io/s/introduction-to-jsx-forked-j5wyjn?file=/src/index.js)
2. [JSX Syntax](https://github.com/airbnb/javascript/blob/master/react/README.md)
3. [Practice: Keeper App](https://codesandbox.io/s/keeper-app-part-1-starting-forked-7ht3v2)
4. [HTML, CSS Notes](https://codepen.io/NarendranAI/pen/VwXENJO)

# Summary

## Getting started with React

- React is a JavaScript library for building dynamic and interactive user interfaces.
- In React applications, we don’t query and update the DOM. Instead, we describe our
  application using small, reusable components. React will take care of efficiently creating
  and updating DOM elements.
- React components can be created using a function or a class. Function-based
  components are the preferred approach as they’re more concise and easier to work
  with.
- JSX stands for JavaScript XML. It is a syntax that allows us to write components that
  combine HTML and JavaScript in a readable and expressive way, making it easier to
  create complex user interfaces.
- When our application starts, React takes a tree of components and builds a JavaScript
  data structure called the virtual DOM. This virtual DOM is different from the actual
  DOM in the browser. It’s a lightweight, in-memory representation of our component
  tree
- When the state or the data of a component changes, React updates the
  corresponding node in the virtual DOM to reflect the new state. Then, it compares
  the current version of virtual DOM with the previous version to identify the nodes
  that should be updated. It’ll then update those nodes in the actual DOM.
- In browser-based apps, updating the DOM is done by a companion library called
  ReactDOM. In mobile apps, React Native uses native components to render the
  user interface.
- Since React is just a library and not a framework like Angular or Vue, we often
  need other tools for concerns such as routing, state management,
  internationalization, form validation, etc

## Building components

In React apps, a component can only return a single element. To return multiple
elements, we wrap them in a fragment, which is represented by empty angle brackets.

- To render a list in JSX, we use the ‘array.map()’ method. When mapping items, each
  item must have a unique key, which can be a string or a number.
- To conditionally render content, we can use an ‘if’ statement or a ternary operator.
- We use the state hook to define state (data that can change over time) in a component. A
  hook is a function that allows us to tap into built-in features in React.
- Components can optionally have props (short for properties) to accept input.
- We can pass data and functions to a component using props. Functions are used to
  notify the parent (consumer) of a component about certain events that occur in the
  component, such as an item being clicked or selected.
- We should treat props as immutable (read-only) and not modify them.
- When the state or props of a component change, React will re-render the component
  and update the DOM accordingly.

### Creating a component

```tsx
const Message = () => {
  return <h1>Hello World</h1>;
};

export default Message;
```

### Rendering a List

```tsx
const Component = () => {
  const items = ["a", "b", "c"];
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{item}</li>
      ))}
    </ul>
  );
};
```

### Conditional Rendering

```tsx
{
  items.length === 0 ? "a" : "b";
}
{
  items.length === 0 && "a";
}
```

### Handling events

```tsx
<button
  onClick={() => {
    console.log("clicked");
  }}
></button>
```

### Defining state

```tsx
const [name, setName] = useState("");
```

### Props

```tsx
interface Props {
  name: string;
}

const Component = ({ name }: Props) => {
  return <p>{name}</p>;
};
```

### Passing Children

```tsx
interface Props {
  children: ReactNode;
}
const Component = ({ children }: Props) => {
  return <div>{children}</div>;
};
```

## Styling components

- We have several options for styling React components, including vanilla CSS, CSS modules, CSS-in-JS, and inline styles.
- With vanilla CSS, we write our component styles in a separate CSS file and import it into the component file. However, we may encounter conflicts if the same CSS classes are defined in multiple files.
- CSS modules resolve this issue by generating unique class names during the build process.
- With CSS-in-JS, we define all the styles for a component alongside its code. Like CSS modules, this provides scoping for CSS classes and eliminates conflicts. It also makes it easier for us to change or delete a component without affecting other components.
- The separation of concerns principle suggests that we divide a program into distinct sections or modules where each section handles a specific functionality. It helps us build modular and maintainable applications.
- With this principle, the complexity and implementation details of a module are hidden behind a well-defined interface.
- Separation of concerns is not just about organizing code into files, but rather dividing areas of functionality. Therefore, CSS-in-JS does not violate the separation of concerns principle as all the complexity for a component remains hidden behind its interface.
- Although inline styles are easy to apply, they can make our code difficult to maintain over time and should only be used as a last resort.
- We can add icons to our applications using the react-icons library.
- There are several UI libraries available that can assist us in quickly building beautiful and modern applications. Some popular options include Bootstrap, Material UI, TailwindCSS, DaisyUI, ChakraUI, and more.

### Vanilla CSS

```tsx
import "./ListGroup.css";

function ListGroup() {
  return <ul className="list-group"></ul>;
}
```

### CSS modules

```tsx
import styles from "./ListGroup.module.css";

function ListGroup() {
  return <ul className={[styles.listGroup, styles.container].join(" ")}></ul>;
}
```

### CSS in JS

- Libraries available for CSS-In-JS styling are

  - Styled components
  - Emotion
  - Polished

- Example using styled-components library

```bash
npm i styled-components
# @types is a library that contains type defintion for variables used in popular JS libraries.
# Useful when using JS libraries without type defined in TS
npm i @types/styled-components
```

```tsx
import styled from "styled-components";
import { useState } from "react";

const [selectedIndex, setSelectedIndex] = useState(-1);

interface Props {
  items: string[];
  heading: string;
}

// Props for styled React Component
interface ListItemProps {
  active: boolean;
}

// styled contains all the HTML elements
// This line returns a react component with the styles we defined
const List = styled.ul`
  list-style: none;
  color: white;
  background: black;
  padding: 0;
`;

// Styled React component with props
const ListItem = styled.li<ListItemProps>`
  padding: 3px 0;
  background: ${(props) => (props.active ? "blue" : "none")};
`;

function ListGroup({ items, heading }: Props) {
  return (
    <List>
      {items.map((item, index) => (
        <ListItem active={index === selectedIndex} key={index}>
          {item}
        </ListItem>
      ))}
    </List>
  );
}
```

### Inline Styling

**NOT RECOMMENDED. USE AS A LAST RESORT ONLY**

Inline styling in React refers to the practice of applying styles directly to individual React elements using the `style` attribute. Here are some key points to note about inline styling in React:

1. **JavaScript Object Syntax:**

   - Inline styles in React use a JavaScript object syntax to define CSS styles.
   - Each style property is represented as a camelCased key in the object.

   ```jsx
   const styleObject = {
     color: "red",
     fontSize: "16px",
     backgroundColor: "#f0f0f0",
   };

   <div style={styleObject}>Hello, React!</div>;
   ```

2. **Dynamic Styles:**

   - Since styles are defined using JavaScript objects, you can dynamically generate styles based on variables or conditions.

   ```jsx
   const dynamicColor = "blue";

   const dynamicStyle = {
     color: dynamicColor,
     fontSize: "18px",
   };

   <div style={dynamicStyle}>Dynamic Style</div>;
   ```

3. **String Interpolation:**

   - String interpolation can be used within style values when necessary.
   - However, it's generally more common and recommended to use JavaScript objects.

   ```jsx
   const fontSizeValue = "20px";

   const dynamicStyle = {
     fontSize: fontSizeValue,
   };

   <div style={dynamicStyle}>Dynamic Font Size</div>;
   ```

4. **Style Prop in JSX:**

   - The `style` attribute is a standard prop in JSX that accepts a JavaScript object for styling.

   ```jsx
   const myStyle = {
     fontWeight: "bold",
     textAlign: "center",
   };

   <div style={myStyle}>Styled Component</div>;
   ```

5. **Avoiding XSS Attacks:**

   - Be cautious when dynamically generating styles from user inputs to avoid XSS attacks.
   - Ensure that user inputs are sanitized and properly validated before applying them as styles.

6. **CSS-in-JS Libraries:**

   - For more advanced styling needs, you may consider using CSS-in-JS libraries like Styled Components or Emotion.
   - These libraries provide a more sophisticated way to handle styles in React applications.

   ```jsx
   import styled from "styled-components";

   const StyledDiv = styled.div`
     color: red;
     font-size: 16px;
   `;

   <StyledDiv>Hello, Styled Components!</StyledDiv>;
   ```

Inline styling is a convenient and flexible way to apply styles directly to React components, especially for smaller applications or components where the use of external stylesheets or CSS-in-JS libraries might be overkill. However, for larger projects, using external stylesheets or CSS-in-JS libraries is often recommended for better maintainability and organization.

## Managing Component State

- The state hook allows us to add state to function components.
- Hooks can only be called at the top level of components.
- State variables, unlike local variables in a function, stay in memory as long as the component is visible on the screen. This is because state is tied to the component instance, and React will destroy the component and its state when it is removed from the screen.
- React updates state in an asynchronous manner, so updates are not applied immediately. Instead, they’re batched and applied at once after all event handlers have
  finished execution. Once the state is updated, React re-renders our component.
- Group related state variables into an object to keep them organized.
- Avoid deeply nested state objects as they can be hard to update and maintain.
- To keep state as minimal as possible, avoid redundant state variables that can be computed from existing variables.
- A pure function is one that always returns the same result given the same input. Pure functions should not modify objects outside of the function.
- React expects our function components to be pure. A pure component should always return the same JSX given the same input.
- To keep our components pure, we should avoid making changes during the render phase.
- Strict mode helps us catch potential problems such as impure components. Starting from React 18, it is enabled by default. It renders our components twice in development mode to detect any potential side effects.
- When updating objects or arrays, we should treat them as immutable objects. Instead of mutating them, we should create new objects or arrays to update the state.
- Immer is a library that can help us update objects and arrays in a more concise and mutable way.
- To share state between components, we should lift the state up to the closest parent component and pass it down as props to child components.
- The component that holds some state should be the one that updates it. If a child component needs to update some state, it should notify the parent component using a callback function passed down as a prop.

### Updating Objects

```tsx
import { useState } from "react";

function App() {
  const [drink, setDrink] = useState({
    title: "cola",
    price: 5,
  });

  return (
    <div>
      <button
        onClick={() => {
          // Only an object with all the properties can be passed to set state objects
          // No partial updates available
          // To prevent typing entire object again and again, one can use spread operator and type only those properties they wish to update
          setDrink({ ...drink, price: 6 });
        }}
      >
        Click Me
      </button>
    </div>
  );
}
export default App;
```

### Updating nested objects

```tsx
import { useState } from "react";

function Employee() {
  const [user, setUser] = useState({
    name: "John",
    address: {
      street: "Baker Street",
      city: "London",
      zipCode: "94111",
    },
  });

  // The more deeper our object structure is, the more complex the update logic becomes
  const handleClick = () => {
    setUser({ ...user, address: { ...user.address, street: "Church Street" } });
  };

  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}
export default App;
```

### Updating arrays

```tsx
import { useState } from "react";

function App() {
  const [tags, setTags] = useState(["happy", "cheerful"]);

  const handleClick = () => {
    // Add a tag called 'exciting'
    setTags([...tags, "exciting"]);

    // Remove cheerful tag
    setTags(tags.filter((tag) => tag !== "cheerful"));

    // Update happy tag to happiness
    setTags(tags.map((tag) => (tag === "happy" ? "happiness" : tag)));
  };
  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}

export default App;
```

### Updating array of objects

```tsx
import { useState } from "react";

function App() {
  const [bugs, setBugs] = useState([
    { id: 1, name: "UI Glitch", fixed: "false" },
    { id: 2, name: "Biometric login failed", fixed: "false" },
  ]);

  const handleClick = () => {
    // Fix the bug 1
    setBugs(
      bugs.map((bug) => (bug.id === 1 ? { ...bug, fixed: "true" } : bug))
    );
  };

  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}

export default App;
```

### Updating with Immer

```tsx
import { produce } from "immer";

function App() {
  const [bugs, setBugs] = useState([
    { id: 1, name: "UI Glitch", fixed: "false" },
    { id: 2, name: "Biometric login failed", fixed: "false" },
  ]);

  const handleClick = () => {
    // Fix the bug 1
    setBugs(
      produce((draft) => {
        const bug = draft.find((bug) => bug.id === 1);
        if (bug) bug.fixed = true;
      })
    );
  };

  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}
```
