
# Table of Contents
- [Table of Contents](#table-of-contents)
- [General Programming Concepts \& Libraries](#general-programming-concepts--libraries)
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
  - [Cache, Retry AJAX, Infinite Scrolling/Pagination library](#cache-retry-ajax-infinite-scrollingpagination-library)
    - [Installation](#installation-1)
    - [Additional dev tools](#additional-dev-tools)
  - [Infinite scrolling library - (to use with useInifiniteQuery of react-query)](#infinite-scrolling-library---to-use-with-useinifinitequery-of-react-query)
    - [Installation](#installation-2)
  - [Client State Management library](#client-state-management-library)
    - [Installation](#installation-3)
    - [Additional Dev Tools](#additional-dev-tools-1)
  - [Routing Library](#routing-library)
  - [Preferred backend stacks](#preferred-backend-stacks)
  - [Deploying app in GitHub Pages](#deploying-app-in-github-pages)
  - [Angular vs React](#angular-vs-react)
  - [JSX](#jsx)
  - [TSX](#tsx)
- [Setup Local Environment](#setup-local-environment)
  - [1. create-react-app](#1-create-react-app)
  - [2. Vite](#2-vite)
- [JavaScript Concepts](#javascript-concepts)
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
- [React Basics](#react-basics)
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
- [State in React](#state-in-react)
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
  - [1. **useState**:](#1-usestate)
  - [2. **useEffect**:](#2-useeffect)
  - [3. **useContext**:](#3-usecontext)
  - [4. **useReducer**:](#4-usereducer)
  - [5. **useRef**:](#5-useref)
  - [6. **useCallback**:](#6-usecallback)
  - [7. **useMemo**:](#7-usememo)
  - [8. **useLayoutEffect**:](#8-uselayouteffect)
- [Event Handling in React](#event-handling-in-react)
  - [Using class components](#using-class-components)
  - [Using functions components](#using-functions-components)
- [React Event Pooling](#react-event-pooling)
- [Life Cycle of a React Component](#life-cycle-of-a-react-component)
  - [Life cycle implementation in Functional Component](#life-cycle-implementation-in-functional-component)
- [Uni directional data flow](#uni-directional-data-flow)
  - [How Unidirectional Data Flow Works in React](#how-unidirectional-data-flow-works-in-react)
  - [Benefits of Unidirectional Data Flow](#benefits-of-unidirectional-data-flow)
  - [**What happens when we use callback function as props?**](#what-happens-when-we-use-callback-function-as-props)
    - [How Callbacks Maintain Unidirectional Data Flow](#how-callbacks-maintain-unidirectional-data-flow)
    - [Example:](#example)
    - [Explanation:](#explanation)
    - [Data Flow:](#data-flow)
    - [Key Points:](#key-points)
    - [Summary](#summary)
  - [**What happens when someone uses Redux?**](#what-happens-when-someone-uses-redux)
    - [Data Flow in Redux:](#data-flow-in-redux)
    - [Key Points:](#key-points-1)
  - [Conclusion](#conclusion)
- [Memoizing](#memoizing)
- [React Form](#react-form)
  - [1. **Controlled Components**](#1-controlled-components)
    - [Example of a Simple Controlled Form:](#example-of-a-simple-controlled-form)
  - [2. **Uncontrolled Components**](#2-uncontrolled-components)
    - [Example of an Uncontrolled Form:](#example-of-an-uncontrolled-form)
  - [3. **Form Libraries**](#3-form-libraries)
    - [Example with Formik:](#example-with-formik)
    - [Example with React Hook Form:](#example-with-react-hook-form)
  - [Summary](#summary-1)
  - [Controlled Components](#controlled-components)
  - [useState vs useRef](#usestate-vs-useref)
    - [**useState**:](#usestate)
    - [**useRef**:](#useref)
      - [**Combining `useState` and `useRef`**:](#combining-usestate-and-useref)
  - [Managing forms with React Hook Form (library)](#managing-forms-with-react-hook-form-library)
- [React Form Validation](#react-form-validation)
  - [1. **Manual Validation**](#1-manual-validation)
    - [Example of Manual Validation:](#example-of-manual-validation)
  - [2. **Using Formik**](#2-using-formik)
    - [Example with Formik:](#example-with-formik-1)
  - [3. **Using React Hook Form**](#3-using-react-hook-form)
    - [Example with React Hook Form:](#example-with-react-hook-form-1)
  - [Summary](#summary-2)
  - [Installing Zod](#installing-zod)
  - [To integrate react-hook-form with zod, we need @hookform/resolvers](#to-integrate-react-hook-form-with-zod-we-need-hookformresolvers)
  - [Usage](#usage-1)
- [Connecting to a backend](#connecting-to-a-backend)
  - [Reusable API Client \& User Service](#reusable-api-client--user-service)
    - [**api-client.ts**](#api-clientts)
    - [**user-service.ts**](#user-servicets)
    - [**App.tsx**](#apptsx)
  - [Generic Http service](#generic-http-service)
    - [**http-service.ts**](#http-servicets)
    - [**user-service.ts**](#user-servicets-1)
    - [**App.tsx**](#apptsx-1)
  - [Custom data fetching (useUsers) hook for better state management](#custom-data-fetching-useusers-hook-for-better-state-management)
    - [**useUsers.ts**](#useusersts)
    - [**App.tsx**](#apptsx-2)
- [TanStack Query](#tanstack-query)
  - [What is **`Redux?`**](#what-is-redux)
  - [Redux vs TanStack Query](#redux-vs-tanstack-query)
  - [Redux - should we use it ?](#redux---should-we-use-it-)
  - [Caching](#caching)
  - [Problems with useEffect and direct Axios queries](#problems-with-useeffect-and-direct-axios-queries)
  - [Installation](#installation-4)
  - [Core Concepts of React (TanStack) Query:](#core-concepts-of-react-tanstack-query)
  - [QueryClient and QueryClientProvider](#queryclient-and-queryclientprovider)
  - [useQuery Hook](#usequery-hook)
  - [useQuery and Pagination](#usequery-and-pagination)
  - [useInfiniteQuery and Infinite loading queries](#useinfinitequery-and-infinite-loading-queries)
  - [useMutation and Invalidate Cache, optimistic and pessimistic approaches](#usemutation-and-invalidate-cache-optimistic-and-pessimistic-approaches)
  - [Query Basics](#query-basics)
    - [**FetchStatus**](#fetchstatus)
    - [**Why two different states?**](#why-two-different-states)
  - [Mutation basics](#mutation-basics)
    - [**Resetting Mutation State**](#resetting-mutation-state)
    - [**Mutation Side Effects**](#mutation-side-effects)
    - [**onMutate Side Effect**](#onmutate-side-effect)
    - [**Consecutive mutations**](#consecutive-mutations)
    - [**Promises**](#promises)
    - [**Retry**](#retry)
    - [**Persist mutations**](#persist-mutations)
    - [**Persisting Offline mutations**](#persisting-offline-mutations)
  - [TanStack Query - Example 1 - fetch the data](#tanstack-query---example-1---fetch-the-data)
  - [Tanstack Query - Example 2 - Fetch the data using a custom hook and dependecies](#tanstack-query---example-2---fetch-the-data-using-a-custom-hook-and-dependecies)
- [React Query DevTools](#react-query-devtools)
  - [Installation](#installation-5)
  - [Usage](#usage-2)
- [Global State Management](#global-state-management)
  - [Reducer](#reducer)
  - [Sharing a state](#sharing-a-state)
    - [**Sharing a state/data using React context**](#sharing-a-statedata-using-react-context)
    - [**Custom Providers - React Context**](#custom-providers---react-context)
    - [**Custom Hooks to access context providers**](#custom-hooks-to-access-context-providers)
    - [**Notes on React-Context:**](#notes-on-react-context)
- [Context vs Redux](#context-vs-redux)
  - [Redux](#redux)
  - [React Context](#react-context)
  - [Conclusion](#conclusion-1)
- [Zustand](#zustand)
  - [Zustand Example 1:](#zustand-example-1)
  - [Zustand - preventing unecessary re renders](#zustand---preventing-unecessary-re-renders)
  - [Simple Zustand Dev Tools](#simple-zustand-dev-tools)
- [What is Routing ?](#what-is-routing-)
  - [Example in Express.js](#example-in-expressjs)
- [Types of routing](#types-of-routing)
  - [1. **Path-Based Routing**](#1-path-based-routing)
  - [2. **Hash-Based Routing**](#2-hash-based-routing)
  - [3. **History-Based Routing (HTML5 PushState)**](#3-history-based-routing-html5-pushstate)
  - [4. **Query-Based Routing**](#4-query-based-routing)
  - [5. **Static vs. Dynamic Routing**](#5-static-vs-dynamic-routing)
  - [6. **Nested Routing**](#6-nested-routing)
  - [7. **Conditional Routing**](#7-conditional-routing)
- [React Router DOM](#react-router-dom)
  - [Some Important Hooks of react-router-dom](#some-important-hooks-of-react-router-dom)
    - [**1. useLocation**](#1-uselocation)
    - [**2. useNavigate**](#2-usenavigate)
    - [**3. useParams**](#3-useparams)
    - [**5. useRouteError**](#5-userouteerror)
- [Optimization in React](#optimization-in-react)
  - [1. **Memoization**:](#1-memoization)
  - [2. **Code Splitting**:](#2-code-splitting)
    - [**Benefits of Code Splitting**](#benefits-of-code-splitting)
    - [**How to Implement Code Splitting in React**](#how-to-implement-code-splitting-in-react)
  - [3. **Virtualization**:](#3-virtualization)
  - [4. **Avoid Anonymous Functions in JSX**:](#4-avoid-anonymous-functions-in-jsx)
  - [5. **Use Production Build**:](#5-use-production-build)
  - [6. **Optimize Images and Assets**:](#6-optimize-images-and-assets)
  - [7. **Efficient State Management**:](#7-efficient-state-management)
  - [8. **Debounce and Throttle**:](#8-debounce-and-throttle)
  - [9. **Use React Profiler**:](#9-use-react-profiler)
  - [10. **Avoid Reconciliation Pitfalls**:](#10-avoid-reconciliation-pitfalls)
- [Important Links](#important-links)
- [React Summary](#react-summary)


# General Programming Concepts & Libraries

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
4. [Chakra UI](https://chakra-ui.com/)

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

## Cache, Retry AJAX, Infinite Scrolling/Pagination library

1. [Tanstack Query](https://tanstack.com/query/latest/docs/framework/react/overview)

### Installation

```bash
npm i @tanstack/react-query
# or
pnpm add @tanstack/react-query
# or
yarn add @tanstack/react-query
```

### Additional dev tools

1. [@tanstack/react-query-devtools](https://tanstack.com/query/latest/docs/framework/react/devtools)

- **Installation**

```bash
npm i @tanstack/react-query-devtools
# or
pnpm add @tanstack/react-query-devtools
# or
yarn add @tanstack/react-query-devtools
```

- **Usage**

```tsx
import { ReactQueryDevtools } from "@tanstack/react-query-devtools";

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      {/* The rest of your application */}
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  );
}
```

## Infinite scrolling library - (to use with useInifiniteQuery of react-query)

1. [react-infinite-scroll-component](https://www.npmjs.com/package/react-infinite-scroll-component)

### Installation

```bash
  npm install --save react-infinite-scroll-component

  or

  yarn add react-infinite-scroll-component

```

## Client State Management library

1. [Zustand](https://github.com/pmndrs/zustand)

### Installation

```bash
npm install zustand
# or yarn add zustand or pnpm add zustand
```

### Additional Dev Tools

1. [simple-zustand-devtools](https://github.com/beerose/simple-zustand-devtools#readme)

**Installation**

```bash
npm i simple-zustand-devtools
```

## Routing Library

1. [react-router-dom](https://github.com/remix-run/react-router#readme)

**Installation:**

```bash
npm i react-router-dom
```

## Preferred backend stacks

1. Node + Express
2. C#/ASP.NET
3. Firebase

## Deploying app in GitHub Pages

- Install the gh-pages npm package and designate it as a development dependency

```bash
npm install gh-pages --save-dev
```

- Add the deployment commands in package.json > scripts

```json
    "predeploy": "npm run build",
    "deploy": "gh-pages -d dist",
```

- After successfully developing the app to a certain release version, build and deploy using below commands (we assume you already connected the local repo to a remote repo in GitHub)

```bash
npm run predeploy
npm run deploy
```
## Angular vs React

| Aspect                      | Angular                                      | React                                                               |
| --------------------------- | -------------------------------------------- | ------------------------------------------------------------------- |
| **Type**                    | Framework                                    | Library                                                             |
| **Developed by**            | Google                                       | Facebook                                                            |
| **Initial Release**         | 2010 (AngularJS), 2016 (Angular 2+)          | 2013                                                                |
| **Latest Version**          | Angular 12+ (as of 2023)                     | React 18+ (as of 2023)                                              |
| **Language**                | TypeScript (superset of JavaScript)          | JavaScript (with optional TypeScript)                               |
| **Architecture**            | MVC (Model-View-Controller)                  | Component-based                                                     |
| **DOM Handling**            | Real DOM                                     | Virtual DOM                                                         |
| **Data Binding**            | Two-way data binding                         | One-way data binding                                                |
| **State Management**        | Built-in with Services and RxJS              | External libraries (e.g., Redux, MobX)                              |
| **Component Communication** | @Input, @Output decorators                   | Props and state                                                     |
| **Performance**             | Slightly slower due to real DOM              | Faster due to virtual DOM                                           |
| **Learning Curve**          | Steeper due to comprehensive features        | Easier to get started, but more tools needed for full functionality |
| **Size**                    | Larger bundle size                           | Smaller core, but can increase with libraries                       |
| **Templating**              | HTML templates with Angular directives       | JSX (JavaScript XML)                                                |
| **Dependency Injection**    | Built-in                                     | Not built-in, achieved through libraries                            |
| **Routing**                 | Built-in with Angular Router                 | External libraries (e.g., React Router)                             |
| **Forms**                   | Template-driven and Reactive Forms           | Controlled and Uncontrolled components                              |
| **Testing**                 | Comprehensive testing tools (Karma, Jasmine) | Requires additional tools (Jest, Enzyme)                            |
| **Build System**            | Angular CLI (Command Line Interface)         | Create React App (CRA), Next.js, custom setups                      |
| **Ecosystem**               | Rich, integrated tools and libraries         | Rich, but relies on third-party libraries                           |
| **Community Support**       | Strong, with extensive documentation         | Strong, with extensive documentation                                |
| **Backward Compatibility**  | Breaking changes between major versions      | Generally maintains backward compatibility                          |
| **Learning Resources**      | Plentiful tutorials, official documentation  | Plentiful tutorials, official documentation                         |
| **Usage**                   | Enterprise-level applications, SPAs          | Wide range of applications, SPAs                                    |
| **Development Speed**       | Slower initially due to configuration        | Faster initially due to minimal setup                               |
| **Customization**           | Less flexible due to framework constraints   | Highly flexible, can choose libraries                               |
| **Mobile Development**      | NativeScript, Ionic                          | React Native                                                        |
| **Server-side Rendering**   | Angular Universal                            | Next.js                                                             |

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



# Setup Local Environment

1. Install Node LTS.
2. Install VS Code and extensions like "sublime babel" and "vscode icons."
3. Create a React app & run it in dev server mode using the command:

There are two ways to create react app

## 1. create-react-app

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

## 2. Vite

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

# JavaScript Concepts

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

# React Basics

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
- Forms
- Form Validation
- Data Updation
- Animations

Out of all the above concerns, react has opinion on only the UI part.
Also, react is agnostic to whether the app is web app (ReactDOM) or mobile (React Native).

- HTTP requets are handled by Axios
- Form validation using Zod
- Caching and data management (auto retry, auto refresh, paginated queries, infinite queries) is done with help of [TanStack Query](https://tanstack.com/query/latest)
- Global State Management with help of Reducers, Context, Providers, Zustand (actually Redux is no longer necessary with where React is now)
- Routing is done using [TanStack Router](https://tanstack.com/router/latest) (for multi page apps)
- Data Updation logic is done using [immer](https://github.com/immerjs/immer#readme)
- Forms can be simplified by using [React hook form](https://www.react-hook-form.com/)

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

# State

State makes UI interactive. UI = f(state). There are declarative and imperative approaches.

## Declarative State Programming

UI depends on the value of a state variable. Hooks help hook into state variables and read/modify them.

## Imperative State Programming

More primitive, used in early JS learning for frontend.

# State in React

In React, `state` is a fundamental concept that allows components to keep track of and manage their internal data. The `state` object is used to store and update information that can change over time, causing the component to re-render when the state is updated.

## **Understanding the state hook:**

- React updates states asynchronously
- State is stored outside of components (in memory)
- Use state hooks at the top level of your component

## **Choosing the state structure:**

- Avoid redundant state variables (eg: If you have firstname and lastname as states, don't go for a fullname state variable)
- Group related variables inside an object
- Avoid deeply nested structures

## **Keeping the components Pure:**

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

## **Understanding strictmode**

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

## **Updating state objects:**

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

## **Updating nested state objects:**

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

## **Updating Array states:**

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

## **Updating Array of object:**

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

## **Simplify update Logics using immer library:**

Immer can be used in any context in which immutable data structures need to be used. For example in combination with React state, React or Redux reducers, or configuration management. Immutable data structures allow for (efficient) change detection: if the reference to an object didn't change, the object itself did not change. In addition, it makes cloning relatively cheap: Unchanged parts of a data tree don't need to be copied and are shared in memory with older versions of the same state.

Generally speaking, these benefits can be achieved by making sure you never change any property of an object, array or map, but by always creating an altered copy instead. In practice this can result in code that is quite cumbersome to write, and it is easy to accidentally violate those constraints. Immer will help you to follow the immutable data paradigm by addressing these pain points:

1. Immer will detect accidental mutations and throw an error.
2. Immer will remove the need for the typical boilerplate code that is needed when creating deep updates to immutable objects: Without Immer, object copies need to be made by hand at every level. Typically by using a lot of `...` spread operations. When using Immer, changes are made to a draft object, that records the changes and takes care of creating the necessary copies, without ever affecting the original object.
3. When using Immer, you don't need to learn dedicated APIs or data structures to benefit from the paradigm. With Immer you'll use plain JavaScript data structures, and use the well-known mutable JavaScript APIs, but safely.

### **A quick example for comparison:**

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

### **Installing immer**

```bash
npm i immer
```

### **Using immer with React**

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

## **Sharing state between component:**

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

# Props vs State

| Props                       | State                       |
| --------------------------- | --------------------------- |
| Input passed to a component | Data managed by a component |
| Similar to function args    | Similar to local variables  |
| Immutable                   | Mutable                     |
| Cause a re-render           | Cause a re-render           |

# Hooks in React

Functions starting with 'use' are Hooks.
React Hooks are functions that allow functional components to use state and lifecycle features that were previously only available in class components. Introduced in React 16.8, Hooks provide a more direct way to interact with React's features in functional components, making it easier to reuse stateful logic and manage side effects.

React Hooks are functions that enable functional components to use state, lifecycle methods, and other React features without writing a class. Here are some of the most important React Hooks and their uses:

## 1. **useState**:

This hook allows functional components to manage local state.

- Usage: `const [state, setState] = useState(initialState);`
- Example: `const [count, setCount] = useState(0);`

## 2. **useEffect**:

This hook allows performing side effects in functional components, such as data fetching, subscriptions, or manually changing the DOM.

- Usage: `useEffect(() => { // side effect code }, [dependencies]);`
- Example:
  ```jsx
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, [count]);
  ```

## 3. **useContext**:

This hook allows functional components to consume a context created by the `React.createContext` API.

- Usage: `const value = useContext(MyContext);`
- Example:
  ```jsx
  const theme = useContext(ThemeContext);
  ```

## 4. **useReducer**:

This hook is an alternative to `useState` for managing more complex state logic. It accepts a reducer function and an initial state, and returns the current state and a dispatch function.

- Usage: `const [state, dispatch] = useReducer(reducer, initialState);`
- Example:
  ```jsx
  const [state, dispatch] = useReducer(reducer, initialState);
  ```

## 5. **useRef**:

This hook returns a mutable ref object whose `.current` property is initialized to the passed argument (initial value).

- Usage: `const refContainer = useRef(initialValue);`
- Example:
  ```jsx
  const inputRef = useRef();
  ```

## 6. **useCallback**:

This hook returns a memoized callback function that only changes if one of the dependencies has changed.

- Usage: `const memoizedCallback = useCallback(() => { // callback }, [dependencies]);`
- Example:
  ```jsx
  const memoizedCallback = useCallback(() => {
    doSomething(a, b);
  }, [a, b]);
  ```

## 7. **useMemo**:

This hook returns a memoized value that only recalculates when one of the dependencies has changed.

- Usage: `const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);`
- Example:
  ```jsx
  const memoizedValue = useMemo(() => {
    return computeExpensiveValue(a, b);
  }, [a, b]);
  ```

## 8. **useLayoutEffect**:

This hook is similar to `useEffect`, but it fires synchronously after all DOM mutations. It can be useful for measuring DOM elements.

- Usage: `useLayoutEffect(() => { // layout effect code }, [dependencies]);`
- Example:
  ```jsx
  useLayoutEffect(() => {
    const rect = inputRef.current.getBoundingClientRect();
    console.log("Rect:", rect);
  }, [inputRef]);
  ```

These are some of the most commonly used React Hooks, but React provides many more hooks for various purposes, such as custom hooks (`useCustomHook`) and hooks for working with forms, animations, and more. Understanding and mastering these hooks can significantly enhance your productivity and enable you to build powerful and maintainable React applications.

# Event Handling in React

In React, event handling is similar to handling events in HTML but with some differences due to the nature of React components. React provides a consistent way to handle events across different browsers and abstracts away some of the complexities.

## Using class components

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

## Using functions components

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

# React Event Pooling

- [Official Documentation](https://legacy.reactjs.org/docs/legacy-event-pooling.html)

      This content is only relevant for React 16 and earlier, and for React Native.
      React 17 on the web does not use event pooling.

Event pooling is a performance-enhancing feature used in React. It involves reusing event objects from a pool of previously allocated events, instead of creating a new event object for each event.

React uses `SyntheticEvent`, which is a wrapper for native browser events, to ensure they have consistent properties across different browsers. The event handlers in any React app are actually passed instances of `SyntheticEvent`.

When an event is triggered, React takes an instance from the pool and populates its properties and reuses it. Once the event handler has finished running, all properties will be nullified and the synthetic event instance is released back into the pool. This increases performance by avoiding the costly operation of garbage collection for every synthetic event wrapper that's created.

However, it's important to note that this means you cannot access the event in an asynchronous way. If you need to access event object’s properties after the event handler has run, you need to call `e.persist()`.

Also, since React 17, event pooling has been removed because it doesn't improve performance in modern browsers.

- Example: Won't work

```js
function handleChange(e) {
  // This won't work because the event object gets reused.
  setTimeout(() => {
    console.log(e.target.value); // Too late!
  }, 100);
}
```

- Example: Will Work

```js
function handleChange(e) {
  // Prevents React from resetting its properties:
  e.persist();

  setTimeout(() => {
    console.log(e.target.value); // Works
  }, 100);
}
```

# Life Cycle of a React Component

In React, the lifecycle of a component refers to the series of methods that are invoked at different stages of a component's existence, from its creation to its removal from the DOM. These lifecycle methods allow you to perform actions such as initializing state, fetching data, updating the UI, and cleaning up resources.

Here's an overview of the lifecycle methods in a React component:

1. **Mounting**:

   - These methods are called when an instance of a component is being created and inserted into the DOM.
   - `constructor(props)`: This method is called before a component is mounted. It's used for initializing state and binding event handlers. Avoid side effects or asynchronous tasks in the constructor.
   - `static getDerivedStateFromProps(props, state)`: This static method is called right before rendering when new props or state are received. It returns an object to update the state, or null to indicate no state update is necessary.
   - `render()`: This method is required and must return JSX or null. It describes what the UI of the component should look like based on its props and state.
   - `componentDidMount()`: This method is called after the component is mounted to the DOM. It's a good place to perform tasks like fetching data from a remote endpoint, setting up subscriptions, or initializing third-party libraries.

2. **Updating**:

   - These methods are called when a component is being re-rendered due to changes in props or state.
   - `static getDerivedStateFromProps(props, state)`: Similar to the mounting phase, this method is called right before rendering when new props or state are received. It returns an object to update the state, or null to indicate no state update is necessary.
   - `shouldComponentUpdate(nextProps, nextState)`: This method is called before rendering when new props or state are received. It returns a boolean value that determines whether the component should re-render. By default, it returns true.
   - `render()`: This method is required and must return JSX or null. It describes what the UI of the component should look like based on its props and state.
   - `getSnapshotBeforeUpdate(prevProps, prevState)`: This method is called right before changes from the virtual DOM are reflected in the DOM. It allows the component to capture some information from the DOM before it is potentially changed. The value returned by this method will be passed as a third parameter to `componentDidUpdate()`.
   - `componentDidUpdate(prevProps, prevState, snapshot)`: This method is called after the component is updated in the DOM. It's a good place to perform side effects in response to props or state changes, such as updating the DOM or making network requests.

3. **Unmounting**:
   - These methods are called when a component is being removed from the DOM.
   - `componentWillUnmount()`: This method is called immediately before a component is unmounted from the DOM. It's a good place to perform cleanup tasks like canceling network requests, clearing timers, or unsubscribing from event listeners.

Additionally, React provides methods like `componentDidCatch()` for handling errors within a component tree, and `getDerivedStateFromError()` for updating state in response to an error.

It's important to note that some lifecycle methods, such as `componentWillReceiveProps()` and `componentWillUpdate()`, have been deprecated in recent versions of React, and you should use their replacements (`static getDerivedStateFromProps()` and `getSnapshotBeforeUpdate()`) instead.

Understanding the lifecycle of a React component allows you to control its behavior and optimize performance by performing tasks at the appropriate stages of its existence.

## Life cycle implementation in Functional Component

In React functional components, you can achieve similar functionality to lifecycle methods in class components using React Hooks. Here's an example demonstrating how to replicate common lifecycle behavior using useEffect:

```jsx
import React, { useState, useEffect } from "react";

const MyComponent = () => {
  const [count, setCount] = useState(0);

  // componentDidMount equivalent
  useEffect(() => {
    console.log("Component mounted");
    // Cleanup function (componentWillUnmount equivalent)
    return () => {
      console.log("Component will unmount");
    };
  }, []); // Empty dependency array means this effect runs only once after initial render

  // componentDidUpdate equivalent
  useEffect(() => {
    console.log("Component updated");
    // Effect cleanup function (componentWillUnmount equivalent)
    return () => {
      console.log("Effect cleanup on update");
    };
  }); // No dependency array, so this effect runs on every update after render

  // componentWillUnmount equivalent
  useEffect(() => {
    return () => {
      console.log("Component will unmount");
    };
  }, []); // Empty dependency array means this effect runs only once when component unmounts

  // componentDidUpdate with specific prop change
  useEffect(() => {
    console.log("Count changed:", count);
    // Effect cleanup function (componentWillUnmount equivalent)
    return () => {
      console.log("Effect cleanup on count change");
    };
  }, [count]); // Runs when count state changes

  const handleClick = () => {
    setCount((prevCount) => prevCount + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleClick}>Increment Count</button>
    </div>
  );
};

export default MyComponent;
```

In this example:

- `useEffect` with an empty dependency array (`[]`) acts like `componentDidMount`, running once after the initial render.
- `useEffect` without a dependency array runs on every update after render, acting like `componentDidUpdate`.
- `useEffect` with a cleanup function can be used to perform cleanup when the component unmounts, similar to `componentWillUnmount`.
- By passing a dependency array to `useEffect`, you can trigger the effect only when certain dependencies change, mimicking `componentDidUpdate` with specific prop changes.

These `useEffect` hooks replicate common lifecycle behavior in functional components.

# Uni directional data flow

In React, the data flow is **uni-directional**. This means that data flows in a single direction, from parent components to child components. This unidirectional data flow is a core concept in React and helps to maintain a predictable data flow and application state.

## How Unidirectional Data Flow Works in React

1. **Props:**

   - Parent components pass data to child components via `props`. Props are read-only, meaning that child components cannot modify them.
   - This ensures that the source of truth is maintained at the top level of the component hierarchy.

2. **State:**

   - Components manage their own state internally. State can be updated using the `setState` function (in class components) or the `useState` hook (in functional components).
   - When the state changes, the component re-renders, and the new state is passed down to child components via props.

3. **Data Flow Example:**

```jsx
import React, { useState } from "react";

// Parent Component
function Parent() {
  const [message, setMessage] = useState("Hello from Parent");

  return (
    <div>
      <Child message={message} />
      <button onClick={() => setMessage("Message updated by Parent")}>
        Update Message
      </button>
    </div>
  );
}

// Child Component
function Child({ message }) {
  return <div>{message}</div>;
}

export default Parent;
```

In this example:

- The `Parent` component holds the state (`message`).
- The `message` state is passed down to the `Child` component via props.
- The `Child` component receives the `message` prop and renders it.
- When the button in the `Parent` component is clicked, it updates the `message` state, causing the `Parent` to re-render and pass the new message down to the `Child`.

## Benefits of Unidirectional Data Flow

1. **Predictability:**

   - With a single direction for data flow, it is easier to understand and predict how data changes affect the application.

2. **Debugging:**

   - Since data flows in one direction, it is easier to trace bugs and understand where data changes originate.

3. **Component Reusability:**

   - Components can be reused and composed more effectively because they depend on external data provided via props, not on internal state or context.

4. **Maintainability:**
   - Applications are easier to maintain because the flow of data is clear and consistent.

## **What happens when we use callback function as props?**

You can pass callback functions as props to child components, allowing data to be sent from child components back to the parent components. However, this mechanism still adheres to the principle of unidirectional data flow in React.

Here's why:

### How Callbacks Maintain Unidirectional Data Flow

1. **Parent Component Owns the State:**

   - The parent component owns and manages the state. It passes data down to child components via props.
   - The parent component also passes down a callback function via props, which the child component can call to send data back to the parent.

2. **Data Flow Direction:**
   - The data flow remains unidirectional: from parent to child for props and from child to parent through callbacks.
   - The child component does not directly change the parent's state. Instead, it communicates an event (like user input) back to the parent, which then updates the state.

### Example:

```jsx
import React, { useState } from "react";

// Parent Component
function Parent() {
  const [message, setMessage] = useState("Hello from Parent");

  const handleChildMessage = (childMessage) => {
    setMessage(childMessage);
  };

  return (
    <div>
      <h1>Parent Component</h1>
      <p>Message: {message}</p>
      <Child onMessageChange={handleChildMessage} />
    </div>
  );
}

// Child Component
function Child({ onMessageChange }) {
  const sendMessageToParent = () => {
    onMessageChange("Message from Child");
  };

  return (
    <div>
      <h2>Child Component</h2>
      <button onClick={sendMessageToParent}>Send Message to Parent</button>
    </div>
  );
}

export default Parent;
```

### Explanation:

- **Parent Component:**

  - Manages the state (`message`).
  - Passes the `handleChildMessage` callback function to the `Child` component via props.

- **Child Component:**
  - Receives the `onMessageChange` callback function as a prop.
  - When the button is clicked, the `sendMessageToParent` function calls the `onMessageChange` function, sending data back to the parent.

### Data Flow:

1. **Parent to Child:**

   - The parent passes data and functions to the child via props.

2. **Child to Parent:**
   - The child invokes the callback function, passing data to the parent.
   - The parent updates its state based on the data received from the child.

### Key Points:

- The actual data and state management still reside in the parent component.
- The child component only triggers a state update in the parent through the callback function.
- This approach maintains a clear and predictable flow of data, adhering to the unidirectional data flow principle.

### Summary

Even though child components can send data back to parent components using callback functions, this approach does not violate the unidirectional data flow principle. Instead, it enhances the interaction between components while keeping the data flow predictable and easy to manage.

## **What happens when someone uses Redux?**

When using Redux for state management in a React application, the unidirectional data flow principle remains intact, but the state management is centralized. Here's how it works:

1. **Centralized Store:** Redux maintains a single store that holds the entire application's state. This centralized state makes it easier to manage and debug the application.

2. **Actions:** Actions are plain JavaScript objects that describe what happened in the application. They are the only way to communicate with the Redux store to trigger a state change.

3. **Reducers:** Reducers are pure functions that take the current state and an action as arguments and return a new state. They specify how the state changes in response to actions.

4. **Dispatch:** Components use the `dispatch` function to send actions to the store. When an action is dispatched, the store calls the appropriate reducer to handle the action and update the state.

5. **Selectors:** Components use selectors to read specific pieces of state from the store. This abstracts the state structure and simplifies state access.

### Data Flow in Redux:

- **State Initialization:** The Redux store is initialized with an initial state.
- **Component Interaction:** Components subscribe to state changes and read state from the store using `useSelector` or `connect`. They also dispatch actions to the store.
- **Action Dispatching:** When an action is dispatched, it flows through any middleware and then reaches the reducer.
- **State Update:** The reducer processes the action and returns a new state, which replaces the old state in the store.
- **Component Re-Rendering:** Components subscribed to the state changes are re-rendered with the new state.

### Key Points:

- **Predictable State Updates:** Redux ensures state changes are predictable and traceable, as state updates are handled through pure functions (reducers).
- **Centralized State Management:** With Redux, all state is managed in one place, making it easier to handle complex state interactions.
- **Unidirectional Data Flow:** Despite the centralization, data flow remains unidirectional. The parent component dispatches actions to update the state, and the updated state flows down to child components via props.

This structure maintains a clear and predictable flow of data throughout the application, enhancing maintainability and debugging.

## Conclusion

Unidirectional data flow is a fundamental principle in React that contributes to its simplicity and predictability. By passing data from parent to child components through props, React ensures a clear and manageable flow of information throughout an application.

# Memoizing

Memoizing is a technique used in computer science and software engineering to optimize the performance of functions by caching the results of expensive function calls and returning the cached result when the same inputs occur again. The term "memoization" is derived from the word "memo," which means to remember or note down.

Here's how memoization works:

1. **Function Call**: When a function is called with certain inputs, it performs some computation and returns a result.

2. **Caching**: Before returning the result, the function checks if it has already computed and stored the result for the same inputs.

3. **Cache Hit**: If the function finds that the result for the given inputs is already cached, it returns the cached result instead of re-computing it.

4. **Cache Miss**: If the function doesn't find the result for the given inputs in the cache, it performs the computation, stores the result in the cache, and then returns the result.

Memoization can significantly improve the performance of functions that are called frequently with the same inputs, as it avoids redundant computations by reusing previously computed results. It's particularly useful for functions with expensive computations or calculations.

In JavaScript, memoization is often implemented using techniques like caching results in an object or using higher-order functions to wrap the original function and manage the cache. Libraries like Lodash provide utility functions for memoization, and in the context of React, hooks like `useMemo` can be used to memoize values within functional components.

Overall, memoization is a powerful optimization technique that can help improve the efficiency and responsiveness of applications by reducing unnecessary computations and improving performance.

# React Form

In React, forms are a crucial part of building interactive user interfaces. Managing form data and handling user input efficiently is essential for creating dynamic and responsive web applications. React provides a way to handle forms through controlled components, which means that form elements are controlled by the state of the React components.

Forms in React are essential for handling user input and managing form state. React provides several ways to create and manage forms, from simple controlled components to more complex form libraries. Here’s an overview of creating forms in React:

## 1. **Controlled Components**

In React, controlled components refer to form elements that are controlled by the component state. This means the form data is handled by the React component rather than the DOM itself.

### Example of a Simple Controlled Form:

```jsx
import React, { useState } from "react";

function SimpleForm() {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log("Name:", name);
    console.log("Email:", email);
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>
          Name:
          <input
            type="text"
            value={name}
            onChange={(e) => setName(e.target.value)}
          />
        </label>
      </div>
      <div>
        <label>
          Email:
          <input
            type="email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
          />
        </label>
      </div>
      <button type="submit">Submit</button>
    </form>
  );
}

export default SimpleForm;
```

## 2. **Uncontrolled Components**

Uncontrolled components use `refs` to access form values from the DOM directly, rather than keeping them in the component state.

### Example of an Uncontrolled Form:

```jsx
import React, { useRef } from "react";

function UncontrolledForm() {
  const nameRef = useRef();
  const emailRef = useRef();

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log("Name:", nameRef.current.value);
    console.log("Email:", emailRef.current.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>
          Name:
          <input type="text" ref={nameRef} />
        </label>
      </div>
      <div>
        <label>
          Email:
          <input type="email" ref={emailRef} />
        </label>
      </div>
      <button type="submit">Submit</button>
    </form>
  );
}

export default UncontrolledForm;
```

## 3. **Form Libraries**

For complex forms, it’s often beneficial to use a form library. Two popular libraries are **Formik** and **React Hook Form**.

### Example with Formik:

```jsx
import React from "react";
import { Formik, Form, Field, ErrorMessage } from "formik";
import * as Yup from "yup";

const SignupSchema = Yup.object().shape({
  name: Yup.string().required("Required"),
  email: Yup.string().email("Invalid email").required("Required"),
});

function FormikForm() {
  return (
    <Formik
      initialValues={{ name: "", email: "" }}
      validationSchema={SignupSchema}
      onSubmit={(values) => {
        console.log(values);
      }}
    >
      {({ isSubmitting }) => (
        <Form>
          <div>
            <label>Name:</label>
            <Field type="text" name="name" />
            <ErrorMessage name="name" component="div" />
          </div>
          <div>
            <label>Email:</label>
            <Field type="email" name="email" />
            <ErrorMessage name="email" component="div" />
          </div>
          <button type="submit" disabled={isSubmitting}>
            Submit
          </button>
        </Form>
      )}
    </Formik>
  );
}

export default FormikForm;
```

### Example with React Hook Form:

```jsx
import React from "react";
import { useForm } from "react-hook-form";

function HookForm() {
  const {
    register,
    handleSubmit,
    watch,
    formState: { errors },
  } = useForm();
  const onSubmit = (data) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <div>
        <label>Name:</label>
        <input {...register("name", { required: true })} />
        {errors.name && <span>This field is required</span>}
      </div>
      <div>
        <label>Email:</label>
        <input
          {...register("email", {
            required: true,
            pattern: /^[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,4}$/,
          })}
        />
        {errors.email && <span>Invalid email address</span>}
      </div>
      <button type="submit">Submit</button>
    </form>
  );
}

export default HookForm;
```

## Summary

- **Controlled Components:** Use state to manage form inputs.
- **Uncontrolled Components:** Use refs to access form inputs directly.
- **Form Libraries:** Formik and React Hook Form simplify handling complex forms with validation.

Each approach has its advantages, and the choice depends on the complexity of the form and specific requirements of your project.

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

## Controlled Components

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

## useState vs useRef

`useState` and `useRef` are both React hooks, but they serve different purposes when it comes to forms.

### **useState**:

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

### **useRef**:

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

## Managing forms with React Hook Form (library)

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

# React Form Validation

Form validation is an essential aspect of handling user inputs in React applications. There are several approaches to validating forms in React, ranging from basic manual validation to using specialized libraries that simplify the process. Here’s an overview of various methods for implementing form validation in React:

## 1. **Manual Validation**

You can manually validate forms by handling input events and checking the validity of each field within the component's state.

### Example of Manual Validation:

```jsx
import React, { useState } from "react";

function ManualValidationForm() {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");
  const [errors, setErrors] = useState({});

  const validate = () => {
    const errors = {};
    if (!name) errors.name = "Name is required";
    if (!email) {
      errors.email = "Email is required";
    } else if (!/\S+@\S+\.\S+/.test(email)) {
      errors.email = "Email is invalid";
    }
    return errors;
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    const validationErrors = validate();
    setErrors(validationErrors);
    if (Object.keys(validationErrors).length === 0) {
      console.log("Form submitted:", { name, email });
      // Submit form data
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>Name:</label>
        <input
          type="text"
          value={name}
          onChange={(e) => setName(e.target.value)}
        />
        {errors.name && <span>{errors.name}</span>}
      </div>
      <div>
        <label>Email:</label>
        <input
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
        {errors.email && <span>{errors.email}</span>}
      </div>
      <button type="submit">Submit</button>
    </form>
  );
}

export default ManualValidationForm;
```

## 2. **Using Formik**

Formik is a popular library for managing form state and validation in React. It simplifies form handling by providing components and hooks for form state management and validation.

### Example with Formik:

```jsx
import React from "react";
import { Formik, Form, Field, ErrorMessage } from "formik";
import * as Yup from "yup";

const validationSchema = Yup.object({
  name: Yup.string().required("Name is required"),
  email: Yup.string()
    .email("Invalid email address")
    .required("Email is required"),
});

function FormikValidationForm() {
  return (
    <Formik
      initialValues={{ name: "", email: "" }}
      validationSchema={validationSchema}
      onSubmit={(values) => {
        console.log("Form submitted:", values);
        // Submit form data
      }}
    >
      {({ isSubmitting }) => (
        <Form>
          <div>
            <label>Name:</label>
            <Field type="text" name="name" />
            <ErrorMessage name="name" component="div" />
          </div>
          <div>
            <label>Email:</label>
            <Field type="email" name="email" />
            <ErrorMessage name="email" component="div" />
          </div>
          <button type="submit" disabled={isSubmitting}>
            Submit
          </button>
        </Form>
      )}
    </Formik>
  );
}

export default FormikValidationForm;
```

## 3. **Using React Hook Form**

React Hook Form is another popular library for handling form validation. It leverages React hooks to manage form state and validation, providing a performant and flexible way to handle forms.

### Example with React Hook Form:

```jsx
import React from "react";
import { useForm } from "react-hook-form";

function HookFormValidation() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm();
  const onSubmit = (data) => {
    console.log("Form submitted:", data);
    // Submit form data
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <div>
        <label>Name:</label>
        <input {...register("name", { required: "Name is required" })} />
        {errors.name && <span>{errors.name.message}</span>}
      </div>
      <div>
        <label>Email:</label>
        <input
          {...register("email", {
            required: "Email is required",
            pattern: {
              value: /^\S+@\S+\.\S+$/,
              message: "Invalid email address",
            },
          })}
        />
        {errors.email && <span>{errors.email.message}</span>}
      </div>
      <button type="submit">Submit</button>
    </form>
  );
}

export default HookFormValidation;
```

## Summary

- **Manual Validation:** Allows full control but requires more boilerplate code.
- **Formik:** Simplifies form state management and validation using Yup for schema validation.
- **React Hook Form:** Provides a performant way to handle form state and validation using hooks, with less boilerplate and more flexibility.

Choosing the right method depends on the complexity of your form and your preference for handling form state and validation.

- Although `react-hook-form` provides validation, it is better to have a schema based validation rather than validating everything line by line in the TSX part
- Joi and Zod are two main validation libraries
- Joi can be used for JS based projects
- Zod is for TS based projects
- Since we use react with typescript, we can do validation using Zod

## Installing Zod

```bash
npm i zod
```

## To integrate react-hook-form with zod, we need @hookform/resolvers

```bash
npm i @hookform/resolvers
# Actually this library provides integration with various schema based validation libraries like Zod, Joi, Yup et.,
```

## Usage

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

# Connecting to a backend

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

## Reusable API Client & User Service

### **api-client.ts**

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

### **user-service.ts**

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

### **App.tsx**

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

## Generic Http service

- Can be used for different endpoint like users, posts, etc
- Write only once and it will work for multiple cases
- Must be initiated with endpoint and requests must use data types explicitly based on the endpoint

### **http-service.ts**

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

### **user-service.ts**

```ts
// src\services\user-service.ts

import create from "./http-service";

export interface UserData {
  id: number;
  name: string;
}

export default create("/users");
```

---

### **App.tsx**

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

## Custom data fetching (useUsers) hook for better state management

- If we have another component apart from App.tsx where we have to fetch the list of users, then the current implementation of useState s and useEffect s will require repititions.
- To avoid this we can define a custom hook which is just a function with all the current hooks implemented in App.tsx
- Custom Hooks can be used to share functionality across different components

### **useUsers.ts**

```ts
// src\hooks\useUsers.ts
import { useEffect, useState } from "react";
import userService, { UserData } from "../services/user-service";
import { AxiosError, CanceledError } from "../services/api-client";

const useUsers = () => {
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

  return {
    users,
    httpErrors,
    isLoading,
    setUsers,
    setHttpErrors,
    setIsLoading,
  };
};

export default useUsers;
```

---

### **App.tsx**

```tsx
import { AxiosError } from "./services/api-client";
import userService, { UserData } from "./services/user-service";
import useUsers from "./hooks/useUsers";

const App = () => {
  const { users, isLoading, httpErrors, setUsers, setHttpErrors } = useUsers();
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

# TanStack Query

Powerful asynchronous state management for TS/JS, React, Solid, Vue and Svelte.
Toss out that granular state management, manual refetching and endless bowls of async-spaghetti code. TanStack Query gives you declarative, always-up-to-date auto-managed queries and mutations that directly improve both your developer and user experiences.

[`Official Documentation`](https://tanstack.com/query/latest/docs/framework/react/overview)

## What is **`Redux?`**

A popular state management library for JavaScript applications. It allows us to store state or data in an application within a single global store (just a JS object in the user's browser). So a lot of people us it as a cache, as they fetch data from backend and store it in the browser and hence users can quickly access the data.

## Redux vs TanStack Query

| Feature                       | Redux                                                                        | TanStack Query                                                            |
| ----------------------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| **Purpose**                   | Primarily used for client-side state management.                             | Primarily used for server-side state management.                          |
| **Ease of Use**               | Can be complex due to the need to manage actions and reducers.               | More intuitive and lightweight compared to Redux.                         |
| **Data Fetching**             | Not built-in, requires additional middleware like redux-thunk or redux-saga. | Built-in data fetching, caching, and invalidation.                        |
| **Performance Optimizations** | Requires manual optimization.                                                | Comes with built-in performance optimizations and memoized query results. |
| **Integration with DevTools** | Fully integrated with Redux DevTools.                                        | Not mentioned.                                                            |
| **Usage with React**          | Can be used with React.                                                      | Originally developed for React, now available for other frameworks.       |

Please note that both libraries serve different purposes and can be used together in a project. The choice between Redux and TanStack Query depends on the specific needs of your project.

## Redux - should we use it ?

Redux is no longer needed unless we are maintaining an existing redux based app.

## Caching

The process of storing data in a place where it can be accessed more quickly and effeciently in the future.

## Problems with useEffect and direct Axios queries

When we use the useEffect Hook and Axios queries we face the following problems.

- No request cancellation when the request is unmounted. As a result, the requests will happen twice in `Strict TypeScript Mode`. So one must cancel the request using a cleanup function in useEffect
- No separation of concerns - The querying logic is leaked into the component (like App.tsx file). To prevent this we need to use a generic useData (custom hook).
- No retries of failed requests - In case of failure, with current approach, we just show the user an error and move on which is not the best approach.
- No automatic refresh - So if the data is changed while loading, the new data will not appear unless the page is refreshed.
- No Caching

Technically, we can solve all these problems by ourselves but would require more coding. This is where TanStack Query comes into the picture.

## Installation

```bash
npm i @tanstack/react-query
```

## Core Concepts of React (TanStack) Query:

1. [Queries](https://tanstack.com/query/latest/docs/framework/react/guides/queries)
2. [Mutations](https://tanstack.com/query/latest/docs/framework/react/guides/mutations)
3. [Query Invalidation](https://tanstack.com/query/latest/docs/framework/react/guides/query-invalidation)

## QueryClient and QueryClientProvider

`QueryClient` and `QueryClientProvider` are key components in the React Query library, which is part of the TanStack.

- **QueryClient**: This is an object that manages the query cache for your application. You create a new instance of the `QueryClient` like this:

```javascript
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      retry: 3,
      staleTime: 1000 * 60 * 10,
      gcTime: 1000 * 60 * 5,
      refetchOnWindowFocus: true,
      refetchOnReconnect: true,
      refetchOnMount: true,
    },
  },
});
```

- **QueryClientProvider**: This is a React component that provides the `QueryClient` instance to all child components in your application. You wrap your application component in a `QueryClientProvider` like this:

```javascript
<QueryClientProvider client={queryClient}>
  <YourApplication />
</QueryClientProvider>
```

In this setup, `YourApplication` represents the root component of your application.

These two components work together to enable the functionality of React Query in your application. The `QueryClient` manages the state and cache of your queries, while the `QueryClientProvider` makes the `QueryClient` available to all components in your application.

It's important to note that if you're using multiple versions of `react-query` (for example, due to dependencies on other packages), you might encounter issues. In such cases, it's recommended to ensure that all packages are using the same version of `react-query`.

## useQuery Hook

`useQuery` is a hook provided by the TanStack Query library for fetching, caching, synchronizing and updating server state in your React applications.

Here's a basic usage of `useQuery`:

```tsx
const App = () => {
  const fetchTodos = async () => {
    const resp = await axios.get<Todo[]>(
      "https://jsonplaceholder.typicode.com/todos"
    );
    return resp.data;
  };
  const { data, status, fetchStatus, error } = useQuery<
    Todo[],
    string,
    string,
    Error
  >({ queryKey: "todos", queryFn: fetchTodos });

  if (error) return <p>error?.message</p>;
  if (status === "loading") return <p>Data is Loading...</p>;
  return (
    <>
      <ul>
        {data.map((todo) => (
          <li key={todo.id}>{todo.title}</li>
        ))}
      </ul>
    </>
  );
};
```

In this example, 'todos' is the query key and `fetchTodos` is the function that will fetch your data.

The `useQuery` hook returns an object with several fields:

- `data`: The data returned by the query.
- `status`: The status of the query ('idle', 'loading', 'error', or 'success').

And many other fields like `error`, `isFetching`, `isStale`, etc., which provide more information about the state of the query.

You can also pass an options object to `useQuery` to configure things like retries, refetching, and placeholders.

Here's an example with options:

```javascript
const queryKey = "todos";
const queryFn = fetchTodos;
const config = {
  staleTime: 1000 * 60 * 5, // data is considered fresh for 5 minutes
  cacheTime: 1000 * 60 * 30, // unused data is removed from the cache after 30 minutes
  retry: 1, // if the query fails, retry once
};

const { data, status } = useQuery(queryKey, queryFn, config);
```

In this example, `staleTime` and `cacheTime` are used to control the freshness and lifetime of the cached data. The `retry` option controls how many times a failed query should be retried.

## useQuery and Pagination

- A good API provider will provide you with the following infomration
  - Count of total number of pages (or provides some metrics using which we can calculate)
  - Presence of next page
  - Presence of previous page
- By getting these information, we can set the pagination button behavior (like disabling prev button in first page)
- The pagination buttons can then use a state variable (that holds page number) to increment and decrement pages, and by passing this as a param and also as a dependency, we get the pagination + refresh on change of page function.

- Example

```ts
import { useQuery } from "@tanstack/react-query";
import { AxiosError, AxiosRequestConfig } from "../services/api-client";
import { HttpService } from "../services/http-service";

interface ResponseData<T> {
  count: number;
  results: T[];
  next: string | null;
  previous: string | null;
}

const useData = <T>(
  ServiceObject: HttpService,
  requestConfig?: AxiosRequestConfig,
  deps?: any[]
) => {
  const queryKey = deps || ["data"];

  const queryFn = async () => {
    const { resp } = await ServiceObject.get<ResponseData<T>>(requestConfig);

    return resp.data;
  };
  const staleTime = 1000 * 60 * 10; // The data will remain fresh until 10 mins
  const gcTime = 1000 * 60 * 10;
  const placeholderData = (prevData: ResponseData<T>) => prevData || [];

  const { data, error, isLoading, isFetching } = useQuery<
    ResponseData<T>,
    AxiosError
  >({
    queryKey,
    queryFn,
    staleTime,
    gcTime,
  });

  return {
    data: data?.results,
    httpErrors: error?.message,
    isLoading: isLoading || isFetching,
    nextPage: data?.next,
    prevPage: data?.previous,
    placeholderData,
  };
};

export default useData;
```

## useInfiniteQuery and Infinite loading queries

This hook is used for querying lists that can additively "load more" data onto an existing set of data or "infinite scroll".

```tsx
import { useInfiniteQuery } from "react-query";

function Projects() {
  const fetchProjects = ({ pageParam = 0 }) =>
    fetch("/api/projects?cursor=" + pageParam);

  const {
    data,
    error,
    fetchNextPage,
    hasNextPage,
    isFetching,
    isFetchingNextPage,
    status,
  } = useInfiniteQuery("projects", fetchProjects, {
    getNextPageParam: (lastPage, pages) => lastPage.nextCursor,
  });

  return status === "loading" ? (
    <p>Loading...</p>
  ) : status === "error" ? (
    <p>Error: {error.message}</p>
  ) : (
    <>
      {data.pages.map((group, i) => (
        <React.Fragment key={i}>
          {group.projects.map((project) => (
            <p key={project.id}>{project.name}</p>
          ))}
        </React.Fragment>
      ))}
      <div>
        <button
          onClick={() => fetchNextPage()}
          disabled={!hasNextPage || isFetchingNextPage}
        >
          {isFetchingNextPage
            ? "Loading more..."
            : hasNextPage
            ? "Load More"
            : "Nothing more to load"}
        </button>
      </div>
      <div>{isFetching && !isFetchingNextPage ? "Fetching..." : null}</div>
    </>
  );
}
```

## useMutation and Invalidate Cache, optimistic and pessimistic approaches

- Pessimistic

```tsx
import { FormEvent, useRef } from "react";

interface Person {
  name: string;
  age: number;
}

const Form = () => {
  const nameRef = useRef<HTMLInputElement>(null);
  const ageRef = useRef<HTMLInputElement>(null);

// useQueryClient

const queryClient = useQueryClient();

// useMutation<TData,TError,TVariable>
// TData => the data we get from the backend
// TError => the error
// TVariable => The data we send to backend
  const addPerson = useMutation<Person,Error,Person>({
  mutationFn: async (newPerson:Person)=>{
    const resp = await axios.post<Person>('backend_service/path/to/api/endpoint',newPerson);
    return resp.data;
  },
  mutationKey:['people'],
  onSuccess:(savedPerson, newPerson)=>{
    console.log('This is the object we sent: ',newPerson);
    console.log('This is the object server saved: ',savedPerson);
    // Approach 1 : Invalidate Cache
    queryClient.invalidateQueries({
      queryKey:['people']
    });

    // Approach 2 : Update Cache

    // IMPORTANT: IF WE UPDATE THE CACHE IN ON SUCCESS THEN IT IS PESSIMISTIC APPROACH
    // ELSE IF WE UPDATE CACHE IN ON MUTATE THEN IT IS OPTIMISTIC APPROACH

    // HOWEVER, IF WE DO OPTIMISTIC APPROACH, WE HAVE TO SET THE CACHE AGAIN IN ON SUCCESS ANYWAYS,
    // THIS IS BECAUSE, IN ON MUTATE, WE USE THE DATA WE SENT (VARIABLE) TO SERVER AS AN INPUT TO CACHE, THIS MAY BE WRONG
    // HENCE, AGAIN IN ON SUCCESS WE MUST RE UPDATE USING DATA WE RECEIVE (DATA) FROM SERVER
    queryClient.setQueryData<Person[]>(
      ['people'],
      (people)=>[savedPerson,...(people || [])]
    )
    // two ways of using setQueryData
    // setQueryData(queryKey, newData) ---> newData is value
    // setQueryData(queryKey, (oldData) => newData) ---> using a function


  }
})

  const person:Person = { name: "", age: 0 };

  const handleSubmit = (event: FormEvent) => {
    event.preventDefault();

    if (nameRef.current) person.name = nameRef.current.value;

    if (ageRef.current) person.age = parseInt(ageRef.current.value);

    // call our mutation fn
    addPerson.mutate(person);

  };

  return (
<>
{addPerson.error && <div  class='alert alert-danger'>{addperson.error.message}</div>}
<form onSubmit={handleSubmit}>
      <div className="mb-3">
        <label htmlFor="name" className="form-label">
          Name
        </label>
        <input ref={nameRef} id="name" type="text" className="form-control" />
        <label htmlFor="age" className="form-label">
          Age
        </label>
        <input ref={ageRef} id="age" type="number" className="form-control" />

        <!--Button will show loading when isLoading is true  -->
        <button type="submit" className="btn btn-primary">
          {addPerson.isLoading? 'Submitting...':'Submit'}
        </button>
      </div>
    </form>
    </>
  );
};

export default Form;
```

- Optimistic

```tsx
import { FormEvent, useRef } from "react";

interface Person {
  name: string;
  age: number;
}

interface addPersonContext{
  previousPeople: Person[];
}

const Form = () => {
  const nameRef = useRef<HTMLInputElement>(null);
  const ageRef = useRef<HTMLInputElement>(null);


const queryClient = useQueryClient();

  // 4th argument is the data type for context
  const addPerson = useMutation<Person,Error,Person,addPersonContext>({
  mutationFn: async (newPerson:Person)=>{
    const resp = await axios.post<Person>('backend_service/path/to/api/endpoint',newPerson);
    return resp.data;
  },
  mutationKey:['people'],
  onMutate: (newPerson:Person)=>{
    // We get previous cache data as a context and return it
    const previousPeople = queryClient.getQueryData<Person[]>(['people' ]) || [];
    queryClient.setQueryData<Person[]>(['people'],(people)=>[newPerson, ...(people | [])]);

    return {previousPeople}
  },
  onSuccess:(savedPerson, newPerson)=>{
    console.log('This is the object we sent: ',newPerson);
    console.log('This is the object server saved: ',savedPerson);

    queryClient.setQueryData<Person[]>(
      ['people'],
      (people)=>people?.map((person)=>person===newPerson?savedPerson:person)
    )
  },
  // context is an object that we create to pass data between our callbacks
  onError: (error,newPerson,context){
    if(!context)return;
    // rollback to previous data in case of any error
    queryClient.setQueryData<Person[]>(['people'],context.previousPeople);
  }

})

  const person:Person = { name: "", age: 0 };

  const handleSubmit = (event: FormEvent) => {
    event.preventDefault();

    if (nameRef.current) person.name = nameRef.current.value;

    if (ageRef.current) person.age = parseInt(ageRef.current.value);

    // call our mutation fn
    addPerson.mutate(person);

  };

  return (
<>
{addPerson.error && <div  class='alert alert-danger'>{addperson.error.message}</div>}
<form onSubmit={handleSubmit}>
      <div className="mb-3">
        <label htmlFor="name" className="form-label">
          Name
        </label>
        <input ref={nameRef} id="name" type="text" className="form-control" />
        <label htmlFor="age" className="form-label">
          Age
        </label>
        <input ref={ageRef} id="age" type="number" className="form-control" />

        <!--Button will show loading when isLoading is true  -->
        <button type="submit" className="btn btn-primary">
          {addPerson.isLoading? 'Submitting...':'Submit'}
        </button>
      </div>
    </form>
    </>
  );
};

export default Form;

```

## Query Basics

A query is a declarative dependency on an asynchronous source of data that is tied to a **unique key**. A query can be used with any Promise based method (including GET and POST methods) to fetch data from a server. If your method modifies data on the server, we recommend using [Mutations](https://tanstack.com/query/latest/docs/framework/react/guides/mutations) instead.

To subscribe to a query in your components or custom hooks, call the `useQuery` hook with at least:

- A **unique key for the query**
- A function that returns a promise that:
  - Resolves the data, or
  - Throws an error

[//]: # "Example"

```tsx
import { useQuery } from "@tanstack/react-query";

function App() {
  const info = useQuery({ queryKey: ["todos"], queryFn: fetchTodoList });
}
```

[//]: # "Example"

The **unique key** you provide is used internally for refetching, caching, and sharing your queries throughout your application.

The query result returned by `useQuery` contains all of the information about the query that you'll need for templating and any other usage of the data:

[//]: # "Example2"

```tsx
const result = useQuery({ queryKey: ["todos"], queryFn: fetchTodoList });
```

[//]: # "Example2"

The `result` object contains a few very important states you'll need to be aware of to be productive. A query can only be in one of the following states at any given moment:

- `isPending` or `status === 'pending'` - The query has no data yet
- `isError` or `status === 'error'` - The query encountered an error
- `isSuccess` or `status === 'success'` - The query was successful and data is available

Beyond those primary states, more information is available depending on the state of the query:

- `error` - If the query is in an `isError` state, the error is available via the `error` property.
- `data` - If the query is in an `isSuccess` state, the data is available via the `data` property.
- `isFetching` - In any state, if the query is fetching at any time (including background refetching) `isFetching` will be `true`.

For **most** queries, it's usually sufficient to check for the `isPending` state, then the `isError` state, then finally, assume that the data is available and render the successful state:

[//]: # "Example3"

```tsx
function Todos() {
  const { isPending, isError, data, error } = useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodoList,
  });

  if (isPending) {
    return <span>Loading...</span>;
  }

  if (isError) {
    return <span>Error: {error.message}</span>;
  }

  // We can assume by this point that `isSuccess === true`
  return (
    <ul>
      {data.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  );
}
```

[//]: # "Example3"

If booleans aren't your thing, you can always use the `status` state as well:

[//]: # "Example4"

```tsx
function Todos() {
  const { status, data, error } = useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodoList,
  });

  if (status === "pending") {
    return <span>Loading...</span>;
  }

  if (status === "error") {
    return <span>Error: {error.message}</span>;
  }

  // also status === 'success', but "else" logic works, too
  return (
    <ul>
      {data.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  );
}
```

[//]: # "Example4"

TypeScript will also narrow the type of `data` correctly if you've checked for `pending` and `error` before accessing it.

### **FetchStatus**

In addition to the `status` field, you will also get an additional `fetchStatus` property with the following options:

- `fetchStatus === 'fetching'` - The query is currently fetching.
- `fetchStatus === 'paused'` - The query wanted to fetch, but it is paused. Read more about this in the [Network Mode](./guides/network-mode) guide.
- `fetchStatus === 'idle'` - The query is not doing anything at the moment.

### **Why two different states?**

Background refetches and stale-while-revalidate logic make all combinations for `status` and `fetchStatus` possible. For example:

- a query in `success` status will usually be in `idle` fetchStatus, but it could also be in `fetching` if a background refetch is happening.
- a query that mounts and has no data will usually be in `pending` status and `fetching` fetchStatus, but it could also be `paused` if there is no network connection.

So keep in mind that a query can be in `pending` state without actually fetching data. As a rule of thumb:

- The `status` gives information about the `data`: Do we have any or not?
- The `fetchStatus` gives information about the `queryFn`: Is it running or not?

## Mutation basics

Unlike queries, mutations are typically used to create/update/delete data or perform server side-effects. For this purpose, TanStack Query exports a `useMutation` hook.

Here's an example of a mutation that adds a new todo to the server:

[//]: # "Example"

```tsx
function App() {
  const mutation = useMutation({
    mutationFn: (newTodo) => {
      return axios.post("/todos", newTodo);
    },
  });

  return (
    <div>
      {mutation.isPending ? (
        "Adding todo..."
      ) : (
        <>
          {mutation.isError ? (
            <div>An error occurred: {mutation.error.message}</div>
          ) : null}

          {mutation.isSuccess ? <div>Todo added!</div> : null}

          <button
            onClick={() => {
              mutation.mutate({ id: new Date(), title: "Do Laundry" });
            }}
          >
            Create Todo
          </button>
        </>
      )}
    </div>
  );
}
```

[//]: # "Example"

A mutation can only be in one of the following states at any given moment:

- `isIdle` or `status === 'idle'` - The mutation is currently idle or in a fresh/reset state
- `isPending` or `status === 'pending'` - The mutation is currently running
- `isError` or `status === 'error'` - The mutation encountered an error
- `isSuccess` or `status === 'success'` - The mutation was successful and mutation data is available

Beyond those primary states, more information is available depending on the state of the mutation:

- `error` - If the mutation is in an `error` state, the error is available via the `error` property.
- `data` - If the mutation is in a `success` state, the data is available via the `data` property.

In the example above, you also saw that you can pass variables to your mutations function by calling the `mutate` function with a **single variable or object**.

Even with just variables, mutations aren't all that special, but when used with the `onSuccess` option, the [Query Client's `invalidateQueries` method](./reference/QueryClient#queryclientinvalidatequeries) and the [Query Client's `setQueryData` method](./reference/QueryClient#queryclientsetquerydata), mutations become a very powerful tool.

[//]: # "Info1"

> IMPORTANT: The `mutate` function is an asynchronous function, which means you cannot use it directly in an event callback in **React 16 and earlier**. If you need to access the event in `onSubmit` you need to wrap `mutate` in another function. This is due to [React event pooling](https://reactjs.org/docs/legacy-event-pooling.html).

[//]: # "Info1"
[//]: # "Example2"

```tsx
// This will not work in React 16 and earlier
const CreateTodo = () => {
  const mutation = useMutation({
    mutationFn: (event) => {
      event.preventDefault();
      return fetch("/api", new FormData(event.target));
    },
  });

  return <form onSubmit={mutation.mutate}>...</form>;
};

// This will work
const CreateTodo = () => {
  const mutation = useMutation({
    mutationFn: (formData) => {
      return fetch("/api", formData);
    },
  });
  const onSubmit = (event) => {
    event.preventDefault();
    mutation.mutate(new FormData(event.target));
  };

  return <form onSubmit={onSubmit}>...</form>;
};
```

[//]: # "Example2"

### **Resetting Mutation State**

It's sometimes the case that you need to clear the `error` or `data` of a mutation request. To do this, you can use the `reset` function to handle this:

[//]: # "Example3"

```tsx
const CreateTodo = () => {
  const [title, setTitle] = useState("");
  const mutation = useMutation({ mutationFn: createTodo });

  const onCreateTodo = (e) => {
    e.preventDefault();
    mutation.mutate({ title });
  };

  return (
    <form onSubmit={onCreateTodo}>
      {mutation.error && (
        <h5 onClick={() => mutation.reset()}>{mutation.error}</h5>
      )}
      <input
        type="text"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
      />
      <br />
      <button type="submit">Create Todo</button>
    </form>
  );
};
```

[//]: # "Example3"

### **Mutation Side Effects**

`useMutation` comes with some helper options that allow quick and easy side-effects at any stage during the mutation lifecycle. These come in handy for both [invalidating and refetching queries after mutations](./guides/invalidations-from-mutations) and even [optimistic updates](./guides/optimistic-updates)

[//]: # "Example4"

```tsx
useMutation({
  mutationFn: addTodo,
  onMutate: (variables) => {
    // A mutation is about to happen!

    // Optionally return a context containing data to use when for example rolling back
    return { id: 1 };
  },
  onError: (error, variables, context) => {
    // An error happened!
    console.log(`rolling back optimistic update with id ${context.id}`);
  },
  onSuccess: (data, variables, context) => {
    // Boom baby!
  },
  onSettled: (data, error, variables, context) => {
    // Error or success... doesn't matter!
  },
});
```

[//]: # "Example4"

When returning a promise in any of the callback functions it will first be awaited before the next callback is called:

[//]: # "Example5"

```tsx
useMutation({
  mutationFn: addTodo,
  onSuccess: async () => {
    console.log("I'm first!");
  },
  onSettled: async () => {
    console.log("I'm second!");
  },
});
```

[//]: # "Example5"

You might find that you want to **trigger additional callbacks** beyond the ones defined on `useMutation` when calling `mutate`. This can be used to trigger component-specific side effects. To do that, you can provide any of the same callback options to the `mutate` function after your mutation variable. Supported options include: `onSuccess`, `onError` and `onSettled`. Please keep in mind that those additional callbacks won't run if your component unmounts _before_ the mutation finishes.

[//]: # "Example6"

```tsx
useMutation({
  mutationFn: addTodo,
  onSuccess: (data, variables, context) => {
    // I will fire first
  },
  onError: (error, variables, context) => {
    // I will fire first
  },
  onSettled: (data, error, variables, context) => {
    // I will fire first
  },
});

mutate(todo, {
  onSuccess: (data, variables, context) => {
    // I will fire second!
  },
  onError: (error, variables, context) => {
    // I will fire second!
  },
  onSettled: (data, error, variables, context) => {
    // I will fire second!
  },
});
```

[//]: # "Example6"

### **onMutate Side Effect**

The `onMutate` option in the `useMutation` hook is a function that is executed just before the mutation function (`mutationFn`) is called. This function receives the variables that are passed to the mutation function.

The purpose of `onMutate` is to allow for any side effects that should occur before the mutation happens. For example, you might want to update the UI optimistically, assuming that the mutation will succeed.

Here's an example of how you might use `onMutate` to optimistically update the UI:

```javascript
onMutate: (newTodo) => {
  // Save the current list of todos in case we need to roll back
  const previousTodos = queryClient.getQueryData("todos");

  // Optimistically update to the new value
  queryClient.setQueryData("todos", (old) => [...old, newTodo]);

  // Return the rollback function
  return () => queryClient.setQueryData("todos", previousTodos);
};
```

In this example, `onMutate` is used to immediately add the new todo to the UI, before the mutation has actually happened. If the mutation fails, the `onError` callback can use the rollback function provided by `onMutate` to restore the original list of todos. This provides a smoother user experience, as they see the result of their action immediately.

### **Consecutive mutations**

There is a slight difference in handling `onSuccess`, `onError` and `onSettled` callbacks when it comes to consecutive mutations. When passed to the `mutate` function, they will be fired up only _once_ and only if the component is still mounted. This is due to the fact that mutation observer is removed and resubscribed every time when the `mutate` function is called. On the contrary, `useMutation` handlers execute for each `mutate` call.

> Be aware that most likely, `mutationFn` passed to `useMutation` is asynchronous. In that case, the order in which mutations are fulfilled may differ from the order of `mutate` function calls.

[//]: # "Example7"

```tsx
useMutation({
  mutationFn: addTodo,
  onSuccess: (data, error, variables, context) => {
    // Will be called 3 times
  },
});

const todos = ["Todo 1", "Todo 2", "Todo 3"];
todos.forEach((todo) => {
  mutate(todo, {
    onSuccess: (data, error, variables, context) => {
      // Will execute only once, for the last mutation (Todo 3),
      // regardless which mutation resolves first
    },
  });
});
```

[//]: # "Example7"

### **Promises**

Use `mutateAsync` instead of `mutate` to get a promise which will resolve on success or throw on an error. This can for example be used to compose side effects.

[//]: # "Example8"

```tsx
const mutation = useMutation({ mutationFn: addTodo });

try {
  const todo = await mutation.mutateAsync(todo);
  console.log(todo);
} catch (error) {
  console.error(error);
} finally {
  console.log("done");
}
```

[//]: # "Example8"

### **Retry**

By default TanStack Query will not retry a mutation on error, but it is possible with the `retry` option:

[//]: # "Example9"

```tsx
const mutation = useMutation({
  mutationFn: addTodo,
  retry: 3,
});
```

[//]: # "Example9"

If mutations fail because the device is offline, they will be retried in the same order when the device reconnects.

### **Persist mutations**

Mutations can be persisted to storage if needed and resumed at a later point. This can be done with the hydration functions:

[//]: # "Example10"

```tsx
const queryClient = new QueryClient();

// Define the "addTodo" mutation
queryClient.setMutationDefaults(["addTodo"], {
  mutationFn: addTodo,
  onMutate: async (variables) => {
    // Cancel current queries for the todos list
    await queryClient.cancelQueries({ queryKey: ["todos"] });

    // Create optimistic todo
    const optimisticTodo = { id: uuid(), title: variables.title };

    // Add optimistic todo to todos list
    queryClient.setQueryData(["todos"], (old) => [...old, optimisticTodo]);

    // Return context with the optimistic todo
    return { optimisticTodo };
  },
  onSuccess: (result, variables, context) => {
    // Replace optimistic todo in the todos list with the result
    queryClient.setQueryData(["todos"], (old) =>
      old.map((todo) => (todo.id === context.optimisticTodo.id ? result : todo))
    );
  },
  onError: (error, variables, context) => {
    // Remove optimistic todo from the todos list
    queryClient.setQueryData(["todos"], (old) =>
      old.filter((todo) => todo.id !== context.optimisticTodo.id)
    );
  },
  retry: 3,
});

// Start mutation in some component:
const mutation = useMutation({ mutationKey: ["addTodo"] });
mutation.mutate({ title: "title" });

// If the mutation has been paused because the device is for example offline,
// Then the paused mutation can be dehydrated when the application quits:
const state = dehydrate(queryClient);

// The mutation can then be hydrated again when the application is started:
hydrate(queryClient, state);

// Resume the paused mutations:
queryClient.resumePausedMutations();
```

[//]: # "Example10"

### **Persisting Offline mutations**

If you persist offline mutations with the [persistQueryClient plugin](./plugins/persistQueryClient), mutations cannot be resumed when the page is reloaded unless you provide a default mutation function.

This is a technical limitation. When persisting to an external storage, only the state of mutations is persisted, as functions cannot be serialized. After hydration, the component that triggers the mutation might not be mounted, so calling `resumePausedMutations` might yield an error: `No mutationFn found`.

[//]: # "Example11"

```tsx
const persister = createSyncStoragePersister({
  storage: window.localStorage,
});
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      gcTime: 1000 * 60 * 60 * 24, // 24 hours
    },
  },
});

// we need a default mutation function so that paused mutations can resume after a page reload
queryClient.setMutationDefaults(["todos"], {
  mutationFn: ({ id, data }) => {
    return api.updateTodo(id, data);
  },
});

export default function App() {
  return (
    <PersistQueryClientProvider
      client={queryClient}
      persistOptions={{ persister }}
      onSuccess={() => {
        // resume mutations after initial restore from localStorage was successful
        queryClient.resumePausedMutations();
      }}
    >
      <RestOfTheApp />
    </PersistQueryClientProvider>
  );
}
```

[//]: # "Example11"

We also have an extensive [offline example](./examples/offline) that covers both queries and mutations.

## TanStack Query - Example 1 - fetch the data

```tsx
// GET https://api.github.com/repos/TanStack/query?repoData
// no useEffect needed. no useState needed.
import {
  QueryClient,
  QueryClientProvider,
  useQuery,
} from "@tanstack/react-query";

const queryClient = new QueryClient();

export default function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Example />
    </QueryClientProvider>
  );
}

function Example() {
  const { isPending, error, data } = useQuery({
    queryKey: ["repoData"],
    queryFn: () =>
      fetch("https://api.github.com/repos/TanStack/query").then((res) =>
        res.json()
      ),
  });

  if (isPending) return "Loading...";

  if (error) return "An error has occurred: " + error.message;

  return (
    <div>
      <h1>{data.name}</h1>
      <p>{data.description}</p>
      <strong>👀 {data.subscribers_count}</strong>{" "}
      <strong>✨ {data.stargazers_count}</strong>{" "}
      <strong>🍴 {data.forks_count}</strong>
    </div>
  );
}
```

## Tanstack Query - Example 2 - Fetch the data using a custom hook and dependecies

```ts
import { useQuery } from "@tanstack/react-query";
import { AxiosError, AxiosRequestConfig } from "../services/api-client";
import { HttpService } from "../services/http-service";

interface ResponseData<T> {
  count: number;
  results: T[];
}

const useData = <T>(
  ServiceObject: HttpService,
  requestConfig?: AxiosRequestConfig,
  deps?: any[]
) => {
  const queryKey = deps || ["data"];
  const queryFn = async () => {
    const { resp } = await ServiceObject.get<ResponseData<T>>(requestConfig);
    return resp.data.results;
  };
  const staleTime = 1000 * 60 * 10; // The data will remain fresh until 10 mins
  const gcTime = 1000 * 60 * 10;

  const { data, error, isLoading, isFetching } = useQuery<T[], AxiosError>({
    queryKey,
    queryFn,
    staleTime,
    gcTime,
  });

  return {
    data,
    httpErrors: error?.message,
    isLoading: isLoading || isFetching,
  };
};

export default useData;
```

# React Query DevTools

**React Query DevTools** is a development tool that helps visualize the inner workings of React Query and can save you hours of debugging.

## Installation

The DevTools are a separate package that you need to install. You can install it using npm, yarn, or pnpm:

```bash
npm i @tanstack/react-query-devtools
# or
pnpm add @tanstack/react-query-devtools
# or
yarn add @tanstack/react-query-devtools
```

By default, React Query DevTools are only included in bundles when `process.env.NODE_ENV === 'development'`, so you don't need to worry about excluding them during a production build.

## Usage

You can import the DevTools in your application like this:

```tsx
import { ReactQueryDevtools } from "@tanstack/react-query-devtools";
```

Then, you can add the `<ReactQueryDevtools />` component to your application. It's recommended to place this as high in your React app as you can. The closer it is to the root of the page, the better it will work.

```tsx
function App() {
  return (
    <QueryClientProvider client={queryClient}>
      {/* The rest of your application */}
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  );
}
```

In the above example, `initialIsOpen` is set to `false` which means the DevTools will not be open by default. You can set this to `true` if you want the DevTools to default to being open.

You can also customize the DevTools using various props such as `panelProps`, `closeButtonProps`, `toggleButtonProps`, `position`, `panelPosition`, etc. For example, you can add className, style (merge and override default style), onClick (extend default handler), etc.

Please note that for now, the DevTools do not support React Native.

# Global State Management

## Reducer

A function that allows us to centralize state updates in a component

- Example:
- before reducer

```tsx
// App.tsx
// without any reducers
import { useState } from "react";

const Counter = () => {
  const [value, setValue] = useState(0);
  return (
    <div>
      Counter ({value})
      <button onClick={() => setValue(value + 1)}>Increment</button>
      <button onClick={() => setValue(value - 1)}>Decrement</button>
    </div>
  );
};

export default Counter;
```

- after reducer

```ts
// CounterReducer.ts
interface Action {
  type: "INCREMENT" | "DECREMENT";
}

const counterReducer = (state: number, action: Action): number => {
  if (action.type === "INCREMENT") return state + 1;
  if (action.type === "DECREMENT") return state - 1;
  throw new Error("Action is not supported");
};

export default counterReducer;
```

```tsx
// App.tsx
import { useReducer, useState } from "react";
import counterReducer from "./example";

const Counter = () => {
  //   const [value, setValue] = useState(0);
  const [value, dispatch] = useReducer(counterReducer, 0);
  return (
    <div>
      Counter ({value})
      <button onClick={() => dispatch({ type: "INCREMENT" })}>Increment</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}>Decrement</button>
    </div>
  );
};

export default Counter;
```

- Example 2 : Complex actions

```ts
// authReducer.ts
interface LoginAction {
  type: "LOGIN";
  user: string;
}
interface LogoutAction {
  type: "LOGOUT";
}
type AuthAction = LoginAction | LogoutAction;

const authReducer = (state: string, action: AuthAction): string => {
  switch (action.type) {
    case "LOGIN":
      return action.user;
    case "LOGOUT":
      return "";
    default:
      return state;
  }
};

export default authReducer;
```

```tsx
// App.tsx

import React, { useReducer, useState } from "react";
import authReducer from "./example";

const LoginStatus = () => {
  //   const [user, setUser] = useState("");
  const [user, userDispatch] = useReducer(authReducer, "");
  if (user)
    return (
      <>
        <div>
          <span>{user}</span>
          <a
            href="#"
            onClick={() => {
              userDispatch({ type: "LOGOUT" });
            }}
          >
            Logout
          </a>
        </div>
      </>
    );

  return (
    <div>
      <a
        href="#"
        onClick={() => {
          userDispatch({ type: "LOGIN", user: "Narendran A I" });
        }}
      >
        Login
      </a>
    </div>
  );
};

export default LoginStatus;
```

## Sharing a state

Lift the state up to the closest parent and pass it down as props to child components.

However, in a complex app with many components becoming child to the top component, we need to set state at top and pass them down as props (again and again) until we reach the bottom component. This creates so much repition in code. This issue is known as `Prop Drilling`

### **Sharing a state/data using React context**

Allows sharing data without passing it down through many components in the middle.

A React Context acts as a transporter that carries state values and functions to the child components of the parent in which the state resides.

1. **`React.createContext`**:

   - The `createContext` function in React allows you to create a context object.
   - It provides an interface for sharing data across multiple components without passing data explicitly via props.
   - Example usage:
     ```jsx
     const defaultValue = { title: "Bag" };
     const CartContext = React.createContext(defaultValue);
     ```
   - The `CartContext` can be consumed using the `useContext` hook.

2. **`useContext`**:
   - The `useContext` hook is used to access data from a context within a functional component.
   - It takes the context object (created using `createContext`) as an argument.
   - Example usage:
     ```jsx
     function App() {
       const data = React.useContext(CartContext);
       return (
         <CartContext.Provider value={defaultValue}>
           {/* Other components */}
         </CartContext.Provider>
       );
     }
     ```
   - Components wrapped within the `CartContext.Provider` can access the shared data.

- Example:

  - Contexts

  ```ts
  // src\contexts\gameQueryContext.ts
  import { Dispatch, createContext } from "react";
  import { GameQuery } from "../interfaces/game.type";

  interface GameQueryContextType {
    gameQuery: GameQuery;
    setGameQuery: Dispatch<React.SetStateAction<GameQuery>>;
  }

  const GameQueryContext = createContext<GameQueryContextType>(
    {} as GameQueryContextType
  );

  export default GameQueryContext;
  ```

  ```ts
  // src\contexts\pageNumberContext.ts
  import { Dispatch, createContext } from "react";

  interface PageNumberContextType {
    pageNumber: number;
    setPageNumber: Dispatch<React.SetStateAction<number>>;
  }

  const PageNumberContext = createContext<PageNumberContextType>(
    {} as PageNumberContextType
  );

  export default PageNumberContext;
  ```

  - App.tsx (parent)

  ```tsx
  import { useState } from "react";
  import PlatformSelector from "./components/PlatformSelector";
  import OrderSelector from "./components/OrderSelector";
  import GameQueryContext from "./contexts/gameQueryContext";
  import PageNumberContext from "./contexts/pageNumberContext";
  import { GameQuery } from "./interfaces/game.type";

  function App() {
    // Use States
    const [pageNumber, setPageNumber] = useState(1);

    const [gameQuery, setGameQuery] = useState<GameQuery>({
      page: pageNumber,
    } as GameQuery);

    // Component TSX markup

    return (
      <GameQueryContext.Provider value={{ gameQuery, setGameQuery }}>
        <PageNumberContext.Provider value={{ pageNumber, setPageNumber }}>
          <OrderSelector />
          <PlatformSelector />
        </PageNumberContext.Provider>
      </GameQueryContext.Provider>
    );
  }
  export default App;
  ```

  - Child components

  ```tsx
  // src\components\PlatformSelector\PlatformSelector.tsx
  import {
    Button,
    Menu,
    MenuButton,
    MenuItem,
    MenuList,
    Text,
  } from "@chakra-ui/react";
  import { FaChevronDown } from "react-icons/fa";
  import usePlatform from "../../hooks/usePlatform";
  import { ParentPlatformData } from "../../interfaces/platform.type";
  import { useContext } from "react";
  import PageNumberContext from "../../contexts/pageNumberContext";
  import GameQueryContext from "../../contexts/gameQueryContext";

  const PlatformSelector = () => {
    const { platforms } = usePlatform();

    const { setPageNumber } = useContext(PageNumberContext);
    const { gameQuery, setGameQuery } = useContext(GameQueryContext);

    const handlePlatformSelect = (
      selectedPlatform: ParentPlatformData | null
    ) => {
      setGameQuery({
        ...gameQuery,
        platformName: selectedPlatform?.slug,
        platform: selectedPlatform,
        page: 1,
      });
      setPageNumber(1);
    };

    const selectedPlatform = gameQuery.platform;

    return (
      <Menu isLazy>
        <MenuButton as={Button} rightIcon={<FaChevronDown />}>
          Platforms :{" "}
          <Text display={"inline"} fontWeight={"bolder"}>
            {selectedPlatform ? selectedPlatform.name : "All"}
          </Text>
        </MenuButton>
        <MenuList maxH="8cm" overflowY="auto">
          <MenuItem
            onClick={() => {
              handlePlatformSelect(null);
            }}
          >
            All
          </MenuItem>
          {platforms?.map((platform) => (
            <MenuItem
              onClick={() => {
                handlePlatformSelect(platform);
              }}
              key={platform.id}
            >
              {platform.name}
            </MenuItem>
          ))}
        </MenuList>
      </Menu>
    );
  };

  export default PlatformSelector;
  ```

  ```tsx
  // src\components\OrderSelector\OrderSelector.tsx
  import {
    Badge,
    Button,
    HStack,
    Menu,
    MenuButton,
    MenuItem,
    MenuList,
    Text,
  } from "@chakra-ui/react";
  import { FaChevronDown } from "react-icons/fa";
  import { toCapitalize } from "../../services/utility";
  import { useContext } from "react";
  import GameQueryContext from "../../contexts/gameQueryContext";

  const OrderSelector = () => {
    const { gameQuery, setGameQuery } = useContext(GameQueryContext);

    const handleSortSelect = (selectedSortOption: string | null) => {
      if (selectedSortOption)
        setGameQuery({
          ...gameQuery,
          ordering: selectedSortOption,
          isAscending: true,
        });
      else {
        setGameQuery({
          ...gameQuery,
          ordering: null,
          isAscending: undefined,
        });
      }
    };

    const onAscDescToggle = () => {
      let newOrdering: string | null;
      if (gameQuery.isAscending) newOrdering = `-${gameQuery.ordering}`;
      else {
        newOrdering =
          gameQuery.ordering?.slice(0, 1) === "-"
            ? gameQuery.ordering?.slice(1)
            : null;
      }
      setGameQuery({
        ...gameQuery,
        isAscending: !gameQuery.isAscending,
        ordering: newOrdering,
      });
    };

    const selectedOption = gameQuery.ordering;
    const isAscending = gameQuery.isAscending;

    const sortByOptions: string[] = [
      "name",
      "released",
      "added",
      "created",
      "updated",
      "rating",
      "metacritic",
    ];

    return (
      <>
        <HStack>
          <Menu isLazy>
            <MenuButton as={Button} rightIcon={<FaChevronDown />}>
              Order by:{" "}
              <Text display={"inline"} fontWeight={"bolder"}>
                {selectedOption
                  ? toCapitalize(selectedOption.replace(/-/g, ""))
                  : "None"}
              </Text>
            </MenuButton>
            <MenuList maxH={"8cm"} overflowY={"auto"}>
              <MenuItem
                onClick={() => {
                  handleSortSelect(null);
                }}
              >
                None
              </MenuItem>
              {sortByOptions.map((option, index) => (
                <MenuItem
                  onClick={() => {
                    handleSortSelect(option);
                  }}
                  key={index}
                >
                  {toCapitalize(option)}
                </MenuItem>
              ))}
            </MenuList>
          </Menu>
          {selectedOption && (
            <Badge
              onClick={onAscDescToggle}
              as={Button}
              colorScheme="yellow"
              h={"20px"}
            >
              {isAscending ? "ASC" : "DESC"}
            </Badge>
          )}
        </HStack>
      </>
    );
  };

  export default OrderSelector;
  ```

### **Custom Providers - React Context**

- We can use custom providers to avoid prop drilling in App.tsx while using providers
- Example

  1. **`App.tsx`**: The main component that wraps everything.
  2. **`SomeContext.tsx`**: Contains the context provider (`SomeProvider`) and the context itself.
  3. **`MainExample.tsx`**: A component that consumes the context.

  Here's how you can structure your files:

  1. **`SomeContext.tsx`**:

  ```tsx
  // SomeContext.tsx
  import React from "react";

  // Create a context
  export const SomeContext = React.createContext<string | undefined>(undefined);

  // Custom provider component
  export const SomeProvider: React.FC = ({ children }) => {
    // Your state or data goes here
    const [someState, setSomeState] = React.useState<string | undefined>(
      undefined
    );

    return (
      <SomeContext.Provider value={{ someState, setSomeState }}>
        {children}
      </SomeContext.Provider>
    );
  };
  ```

  2. **`MainExample.tsx`**:

  ```tsx
  // MainExample.tsx
  import React from "react";
  import { useSomeContext } from "./SomeContext"; // Import your custom hook

  const MainExample: React.FC = () => {
    const { someState } = useSomeContext();

    return (
      <div>
        <h2>Main Example</h2>
        <p>Context value: {someState}</p>
      </div>
    );
  };

  export default MainExample;
  ```

  3. **`App.tsx`**:

  ```tsx
  // App.tsx
  import React from "react";
  import { SomeProvider } from "./SomeContext"; // Import your context provider
  import MainExample from "./MainExample"; // Import your component

  const App: React.FC = () => {
    return (
      <SomeProvider>
        <div>
          <h1>My App</h1>
          <MainExample />
        </div>
      </SomeProvider>
    );
  };

  export default App;
  ```

  Now, when you render `App`, it will provide the context to `MainExample`. You can access the context value using the `useSomeContext` hook within `MainExample`. Adjust the context names and state/data according to your specific use case.

### **Custom Hooks to access context providers**

Assuming you have already created a context using `React.createContext`, here's how you can create a custom hook to access that context:

1. **Create the Context**:
   - First, create your context (let's call it `UserContext` for this example):

```tsx
// UserContext.tsx
import React from "react";

const UserContext = React.createContext<string | undefined>(undefined);

export default UserContext;
```

2. **Create the Custom Hook**:
   - Next, create a custom hook that provides access to the context value:

```tsx
// useUserContext.ts
import { useContext } from "react";
import UserContext from "./UserContext"; // Import your context

export const useUserContext = () => {
  const user = useContext(UserContext);
  return user;
};
```

3. **Use the Custom Hook**:
   - Now you can use the `useUserContext` hook in any component to access the user context:

```tsx
// MyComponent.tsx
import React from "react";
import { useUserContext } from "./useUserContext"; // Import your custom hook

const MyComponent: React.FC = () => {
  const user = useUserContext();

  return <p>Hello, {user}!</p>;
};

export default MyComponent;
```

4. **Wrap Components with Context Provider**:
   - Finally, make sure to wrap your components with the `UserContext.Provider` in your main `App.tsx` or any other parent component:

```tsx
// App.tsx
import React from "react";
import UserContext from "./UserContext"; // Import your context
import MyComponent from "./MyComponent"; // Import your component

const App: React.FC = () => {
  const user = "John Doe"; // Set your user value here

  return (
    <UserContext.Provider value={user}>
      <div className="App">
        <MyComponent />
        {/* Other components */}
      </div>
    </UserContext.Provider>
  );
};

export default App;
```

Now, any component wrapped within the `UserContext.Provider` will have access to the user context using the `useUserContext` hook.
Adjust the context names and state/data according to your specific use case.

### **Notes on React-Context:**

- `Minimizing renders`: Split up a context into smaller and focused ones, each having a single responsibility.
- `When to use Context?`
  - Every react application has some state or data that needs to be managed and shared.
  - That state be either server (data fetched from backend) or client (data that represents state of UI) state.
  - We must avoid using contexts for server states as it may lead us to create multiple contexts.
  - In those cases, we must leave the state management to react query.
  - For client states, we can use Local State and React Context.
- `Other State Management Tools`:
  - Redux
  - MobX
  - Recoil
  - xState
  - Zustand (simplest of all these)

# Context vs Redux

## Redux

- A widely used state management library for JavaScript applications.
- It provides a centralized store to store the application states.
- Instead of storing local states in components, we store all the states in a global store and access it.
- In that way, we don't need to pass them around for using across application components.

- Redux can do
  - Cache the server state
  - Persist in local storage
  - Components can select pieces of state
  - Undo things
  - Use middleware and log actions
  - Decouple from React (and can be used in other framework/libraries like angular)
  - See state changes over time
  - Stores both client and server state

## React Context

- It is just a way to store the state somewhere else and transport it

## Conclusion

**So should we use Redux?**

- If your project really needs Redux with all its complexities, then yes. But if your goal is just server and client state management, then
  - React Query will take care of the server states
  - For Client States
    - Many apps won't have much client state
    - Define local states
    - Share it with context
    - Consolidate state logic with a reducer
    - Use a simpler state management tool (like Zustand)

# Zustand

**Zustand** is a **lightweight and straightforward state management library** for **React**. It provides an alternative to more complex solutions like **Redux**. Let's dive into what Zustand is and how it works:

1. **What is Zustand?**

   - Zustand is built on top of the **Context API** and uses the concept of **stores** to manage state.
   - A **store** is a container for a specific piece of state and any functions that modify that state.
   - Unlike Redux, Zustand doesn't require you to write complex reducers or action creators.

2. **Creating a Store:**

   - To use Zustand, you start by creating a store.
   - A store holds your application's state and provides methods for updating it.
   - Here's an example of creating a simple store with an initial state of `{ count: 0 }`:

     ```javascript
     import create from "zustand";

     const useCountStore = create((set) => ({
       count: 0,
     }));
     ```

     The `useCountStore` now holds the state with an initial count of 0.

3. **Updating State:**

   - You can update the state using the `set` method provided by the store.
   - The `set` method takes a function that receives the current state and returns the new state.
   - For example, to increment the count:
     ```javascript
     useCountStore.set((state) => ({
       count: state.count + 1,
     }));
     ```

4. **Consuming State:**

   - To access the state in your React components, use the `useStore` hook provided by Zustand.
   - The `useStore` hook takes a function that receives the current state and returns the state values you want to access.
   - Example:

     ```javascript
     import { useStore } from "zustand";

     function MyComponent() {
       const count = useStore((state) => state.count);
       return <div>{count}</div>;
     }
     ```

     In this example, we display the `count` value from the store.

5. **Benefits of Zustand:**
   - **Lightweight**: Zustand is minimalistic and doesn't introduce unnecessary complexity.
   - **Fast**: It performs well due to its simplicity.
   - **Easy to Learn**: Zustand's straightforward API makes it beginner-friendly.

In summary, Zustand simplifies global state management in React without sacrificing performance or readability.

## Zustand Example 1:

- An example of a **Zustand store** where we define functions while creating the store:

```javascript
import create from "zustand";

// Define the store type
type Store = {
  count: number,
  increment: () => void,
  decrement: () => void,
};

// Create the Zustand store
const useStore =
  create <
  Store >
  ((set) => ({
    count: 0,
    increment: () => set((state) => ({ count: state.count + 1 })),
    decrement: () => set((state) => ({ count: state.count - 1 })),
  }));

export default useStore;
```

In this example:

- We create a store with a `count` state variable and two functions: `increment` and `decrement`.
- The `increment` function increases the count by 1, and the `decrement` function decreases it by 1.
- We use the `create` function from Zustand, passing in a function that receives a `set` function as its argument.
- The `set` function is used to update the state, and we use ES6 arrow function syntax to define the increment and decrement functions.

You can now use this store in your components like this:

```javascript
import useStore from "./useStore";

function Counter() {
  const { count, increment, decrement } = useStore();

  return (
    <div>
      <button onClick={decrement}>-</button>
      <span>{count}</span>
      <button onClick={increment}>+</button>
    </div>
  );
}

export default Counter;
```

In the `Counter` component, we import the `useStore` hook and use it to access the `count` state variable and the `increment` and `decrement` functions. These values are then used in the JSX to render a simple counter component.

## Zustand - preventing unecessary re renders

To prevent unnecessary re-renders in your React app when using Zustand, you can leverage **selectors** and **shallow comparison**.

1. **Selectors in Zustand**:

   - When you need to subscribe to a computed state from a store, the recommended way is to use a **selector**.
   - A selector allows you to pick specific values from the store state.
   - The computed selector will cause a re-render if the output has changed according to `Object.is`.
   - To avoid unnecessary re-renders, you can use the `useShallow` hook.

2. **Shallow Comparison**:

   - Shallow comparison ensures that only relevant components re-render when specific state values change.
   - By default, Zustand compares picks by identity, which can lead to unnecessary re-renders.
   - To use shallow comparison, pass a shallow comparison function as the second argument to your selector.
   - For example:

     ```javascript
     import { useStore } from "zustand";
     import shallow from "zustand/shallow";

     function MyComponent() {
       const { bears, increment } = useStore(
         (state) => ({ bears: state.bears, increment: state.increment }),
         shallow
       );

       // Your component logic here...
     }
     ```

     In this example, only the relevant parts of the state (`bears` and `increment`) will trigger re-renders.

3. **Utility Function for Selectors** (Optional):

   - If you want to create a custom utility function for selectors, you can do so.
   - Here's an example of creating a `createStoreWithSelectors` function:

     ```javascript
     import { create } from "zustand";

     const bearStore = create((set) => ({
       bears: 0,
       increment: () => set((state) => ({ bears: state.bears + 1 })),
       // Other state and actions...
     }));

     export const createStoreWithSelectors = (store) => (keys) => {
       const useStore = (keys) => {
         return store((state) => {
           const selectedState = keys.reduce((acc, cur) => {
             acc[cur] = state[cur];
             return acc;
           }, {});
           return selectedState;
         }, shallow);
       };
       return useStore(keys);
     };

     export const useBearStore = createStoreWithSelectors(bearStore);
     ```

     Now you can use `useBearStore(['bears', 'increment'])` in your components.

The shallow comparison helps optimize performance by preventing unnecessary re-renders.

```plaintext
Make a note:
Each project is different and depending on the project we may have to change our global state management aproach.
If the project has components that are reusable, then we have to go with props and useReducer hooks to maintain and pass local states.
If the components are specific to the project and won't be re used, then a store like solution (eg: Zustand, Redux) can make the life simpler.

Avoid React Context most of the time.
Go with store like client state management or local states only.

For server state React Query already does wonders, so stick to it.


However, Redux can prove as a single stop solution for all of this, but bring alot of complexity and boiler plate codes.
Unless, we need to move stores from React to Anguler or perform some exception Redux based implementation, don't go for it.

Simply put, if you don't use advanced concepts of redux in your project, then redux is not for your project.
```

## Simple Zustand Dev Tools

- Usage Example

```ts
import create from "zustand";
import { mountStoreDevtool } from "simple-zustand-devtools";

// Define the store type
type Store = {
  count: number;
  increment: () => void;
  decrement: () => void;
};

// Create the Zustand store
const useStore = create<Store>((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
}));

if (process.env.NODE_ENV === "development")
  mountStoreDevtool("Counter Store", useStore);
export default useStore;
```

# What is Routing ?

Routing is the process of determining how requests are handled by an application, specifically how incoming client requests (such as those made via HTTP) are directed to the appropriate handler (e.g., a function, method, or controller).

In web development, routing typically refers to defining the paths or URLs that an application will respond to, and mapping those paths to specific code that should be executed. Here's a brief overview:

1. **Server-Side Routing**: This is handled on the server (e.g., in Node.js with Express.js). When a client makes a request to a URL, the server determines which code to run based on the route. For example, a request to `/about` might be routed to a function that sends an "About Us" page back to the client.

2. **Client-Side Routing**: This is used in single-page applications (SPAs) where the routing is handled within the browser, usually with a JavaScript framework like React, Angular, or Vue.js. Client-side routing allows the application to change the view without reloading the entire page, providing a smoother user experience.

## Example in Express.js

```javascript
const express = require("express");
const app = express();

// Define a route for the home page
app.get("/", (req, res) => {
  res.send("Welcome to the Home Page");
});

// Define a route for the about page
app.get("/about", (req, res) => {
  res.send("About Us");
});

// Start the server
app.listen(3000, () => {
  console.log("Server is running on port 3000");
});
```

In this example:

- A request to `http://localhost:3000/` will return "Welcome to the Home Page".
- A request to `http://localhost:3000/about` will return "About Us".

Routing is a fundamental concept in web development, crucial for directing users to different parts of an application.

# Types of routing

Routing in web development can be categorized into several types based on how URLs are structured and how navigation is handled. Here are the main types of routing:

## 1. **Path-Based Routing**
   - **Definition**: Uses the path segment of a URL to determine the route.
   - **Example URL**: `https://example.com/about`
   - **Use Case**: Common in both server-side and client-side routing.
   - **Characteristics**: 
     - Easy to read and understand.
     - SEO-friendly because each path represents a different URL.

   **Example (Express.js):**
   ```javascript
   app.get('/about', (req, res) => {
     res.send('About Us');
   });
   ```

## 2. **Hash-Based Routing**
   - **Definition**: Uses the hash (`#`) portion of the URL to determine the route. The part of the URL after the `#` is called the "hash" or "fragment".
   - **Example URL**: `https://example.com/#/about`
   - **Use Case**: Popular in older single-page applications (SPAs) to manage client-side routing without involving the server.
   - **Characteristics**: 
     - Does not cause a full page reload.
     - Not SEO-friendly, as search engines typically ignore the hash fragment.

   **Example (React):**
   ```javascript
   <HashRouter>
     <Route path="/about" component={About} />
   </HashRouter>
   ```

## 3. **History-Based Routing (HTML5 PushState)**
   - **Definition**: Uses the HTML5 History API to manipulate the browser history and change the URL path without causing a page reload.
   - **Example URL**: `https://example.com/about`
   - **Use Case**: Modern SPAs often use history-based routing for cleaner URLs.
   - **Characteristics**:
     - SEO-friendly because the full URL is used.
     - Allows for a more natural navigation experience.
  
   **Example (React with React Router):**
   ```javascript
   <BrowserRouter>
     <Route path="/about" component={About} />
   </BrowserRouter>
   ```

## 4. **Query-Based Routing**
   - **Definition**: Uses query parameters to determine the route or pass additional information to the route.
   - **Example URL**: `https://example.com/page?route=about`
   - **Use Case**: Often used in applications where a route needs to handle dynamic data.
   - **Characteristics**:
     - Not typically used for main navigation routes.
     - Useful for filtering, sorting, or paginating data.

   **Example (Express.js):**
   ```javascript
   app.get('/page', (req, res) => {
     const route = req.query.route;
     res.send(`You are at the ${route} page`);
   });
   ```

## 5. **Static vs. Dynamic Routing**
   - **Static Routing**: Routes are predefined and do not change. Each route corresponds to a specific URL.
   - **Dynamic Routing**: Routes can change based on conditions or parameters. Common in applications that use dynamic content or require parameters in the URL.
  
   **Example of Dynamic Routing (Express.js):**
   ```javascript
   app.get('/user/:id', (req, res) => {
     const userId = req.params.id;
     res.send(`User ID: ${userId}`);
   });
   ```

## 6. **Nested Routing**
   - **Definition**: Allows routes to be nested inside other routes, creating a hierarchy of routes.
   - **Use Case**: Useful in applications with complex structures, such as dashboards or admin panels.
   - **Characteristics**: 
     - Organizes routes in a parent-child relationship.
     - Helps manage complex views.

   **Example (React with React Router):**
   ```javascript
   <Route path="/user" component={User}>
     <Route path="profile" component={Profile} />
     <Route path="settings" component={Settings} />
   </Route>
   ```

## 7. **Conditional Routing**
   - **Definition**: Routes are conditionally rendered based on some logic, such as user authentication status.
   - **Use Case**: Often used to protect certain routes or redirect users based on their state.
   - **Characteristics**: 
     - Enables more control over access to routes.
     - Common in applications with role-based access control.

   **Example (React with React Router):**
   ```javascript
   <Route path="/dashboard" render={() => (
     isAuthenticated ? <Dashboard /> : <Redirect to="/login" />
   )}/>
   ```

Each type of routing serves different needs depending on the architecture of the application, user experience requirements, and technical constraints.

# React Router DOM

- [react-router-dom Website](https://reactrouter.com/en/main)
- `react-router-dom` is an npm package that provides bindings for using React Router in web applications. It enables you to implement dynamic routing in a web application. This allows you to create and manage routes, navigate, and access history in your React web app.
- Usage

```tsx
// src\pages\Layout.tsx

import NavBar from "../components/NavBar";
import { Outlet } from "react-router-dom";
const Layout = () => {
  return (
    <>
      <NavBar />
      <Outlet />
    </>
  );
};

export default Layout;
```

```tsx
// src\pages\HomePage.tsx
import { Box, Grid, GridItem, Show, Stack } from "@chakra-ui/react";
import Footer from "../components/Footer";
import GameGrid from "../components/GameGrid";
import GenreList from "../components/GenreList";
import Pagination from "../components/Pagination";
import ClearButton from "../components/ClearButton";
import Gameheading from "../components/GameHeading";
import OrderSelector from "../components/OrderSelector";
import PlatformSelector from "../components/PlatformSelector";

const HomePage = () => {
  return (
    <>
      {" "}
      <Box>
        <Stack
          display={"flex"}
          justifyContent={"space-between"}
          w={{
            base: "100%",
            md: "95%",
            lg: "80%",
            xl: "70%",
            "2xl": "60%",
          }}
          direction={{
            base: "column",
            md: "row",
          }}
          gap={2}
          mx={"auto"}
          my={3}
          px={2}
        >
          <OrderSelector />
          <Box
            display={"flex"}
            justifyContent={"space-between"}
            flexWrap={"wrap"}
          >
            <PlatformSelector />
            <ClearButton />
          </Box>
        </Stack>
        <Box m={4} textAlign={"center"}>
          <Gameheading />
        </Box>
      </Box>
      <Grid
        templateAreas={{
          base: ` "main main" "footer footer"`,
          md: ` "aside main" "footer footer"`,
        }}
        gridTemplateColumns={{
          base: "180px 1fr",
          lg: "200px 1fr",
          xl: "200px 1fr",
          "2xl": "250px 1fr",
        }}
      >
        <Show above="md">
          <GridItem
            area={"aside"}
            px={3}
            mb={"1cm"}
            position="sticky"
            left="0"
            top="212"
            zIndex="98" // Ensure it's behind the NavBar
            height="70vh"
            overflowY={"auto"}
          >
            <GenreList />
          </GridItem>
        </Show>
        <GridItem area={"main"} px={5} overflowY="auto">
          <GameGrid />
        </GridItem>

        <GridItem area="footer">
          <Pagination />
          <Footer />
        </GridItem>
      </Grid>
    </>
  );
};

export default HomePage;
```

```tsx
// src\routes.tsx

import { createBrowserRouter } from "react-router-dom";
import Layout from "./pages/Layout";
import HomePage from "./pages/HomePage";
import GameDetailsPage from "./pages/GameDetailsPage";
import ErrorPage from "./pages/ErrorPage";

const router = createBrowserRouter([
  {
    path: "/game-hub",
    element: <Layout />,
    errorElement: <ErrorPage />,
    children: [
      { index: true, element: <HomePage /> },
      { path: "games/:slug", element: <GameDetailsPage /> },
    ],
  },
]);

export default router;
```

```tsx
// src\main.tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { ChakraProvider, ColorModeScript } from "@chakra-ui/react";
import "./index.css";
import theme from "./theme.ts";
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { ReactQueryDevtools } from "@tanstack/react-query-devtools";
import { RouterProvider } from "react-router-dom";
import router from "./routes.tsx";

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      retry: 3,
      staleTime: 1000 * 60 * 10,
      gcTime: 1000 * 60 * 5,
      refetchOnWindowFocus: true,
      refetchOnReconnect: true,
      refetchOnMount: true,
    },
  },
});

ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <ChakraProvider theme={theme}>
      <ColorModeScript initialColorMode={theme.config.initialColorMode} />
      <QueryClientProvider client={queryClient}>
        <RouterProvider router={router} />
        <ReactQueryDevtools />
      </QueryClientProvider>
    </ChakraProvider>
  </React.StrictMode>
);
```

## Some Important Hooks of react-router-dom

### **1. useLocation**

This hook returns the current location object. This can be useful if you'd like to perform some side effect whenever the current location changes.

```tsx
import * as React from 'react';
import { useLocation } from 'react-router-dom';

function App() {
  let location = useLocation();

  React.useEffect(() => {
    // Google Analytics
    ga('send', 'pageview');
  }, [location]);

  return (
    // ...
  );
}
```

### **2. useNavigate**

The useNavigate hook returns a function that lets you navigate programmatically, for example in an effect:

```tsx
import { useNavigate } from "react-router-dom";

function useLogoutTimer() {
  const userIsInactive = useFakeInactiveUser();
  const navigate = useNavigate();

  useEffect(() => {
    if (userIsInactive) {
      fake.logout();
      navigate("/session-timed-out");
    }
  }, [userIsInactive]);
}
```

The navigate function has two signatures:

- Either pass a To value (same type as <Link to>) with an optional second options argument (similar to the props you can pass to <Link>), or
- Pass the delta you want to go in the history stack. For example, navigate(-1) is equivalent to hitting the back button

### **3. useParams**

The useParams hook returns an object of key/value pairs of the dynamic params from the current URL that were matched by the <Route path>. Child routes inherit all params from their parent routes.

```tsx
import * as React from 'react';
import { Routes, Route, useParams } from 'react-router-dom';

function ProfilePage() {
  // Get the userId param from the URL.
  let { userId } = useParams();
  // ...
}

function App() {
  return (
    <Routes>
      <Route path="users">
        <Route path=":userId" element={<ProfilePage />} />
        <Route path="me" element={...} />
      </Route>
    </Routes>
  );
}
```

### **5. useRouteError**

Inside of an errorElement, this hook returns anything thrown during an action, loader, or rendering. Note that thrown responses have special treatment, see isRouteErrorResponse for more information.

This feature only works if using a data router, see [Picking a Router](https://reactrouter.com/en/main/routers/picking-a-router)

```tsx
import { Box, Heading, Text } from "@chakra-ui/react";
import { isRouteErrorResponse, useRouteError } from "react-router-dom";
import NavBar from "../components/NavBar";

const ErrorPage = () => {
  const error = useRouteError();
  return (
    <>
      <NavBar />
      <Box textAlign={"center"}>
        <Heading>Oops! </Heading>
        <Text>
          {isRouteErrorResponse(error)
            ? "This page does not exist"
            : "An unexpected error occured"}
        </Text>
      </Box>
    </>
  );
};

export default ErrorPage;
```

# Optimization in React

Optimization in React involves improving the performance and efficiency of your application. Here are some key techniques:

## 1. **Memoization**:

- Use `React.memo` to prevent unnecessary re-renders of functional components.
- Use `useMemo` to memoize expensive calculations.
- Use `useCallback` to memoize callback functions.

## 2. **Code Splitting**:

- Use dynamic `import()` to split code into smaller bundles that can be loaded on demand.
- Tools like Webpack and libraries like React.lazy and Suspense can help with code splitting.
  Code splitting is a technique used to break up a large bundle of JavaScript into smaller chunks that can be loaded on demand. This helps improve the initial load time of an application by only loading the necessary code for the current user interaction. In React, code splitting can be achieved using dynamic `import()` statements, React.lazy, and React.Suspense.

### **Benefits of Code Splitting**

1. **Improved Performance**: By loading only the code needed for the current view, the initial load time is reduced.
2. **Reduced Bandwidth Usage**: Users only download the code they need, which can be especially beneficial for users with limited data plans.
3. **Better User Experience**: Faster initial load times can lead to a more responsive and pleasant user experience.

### **How to Implement Code Splitting in React**

1. **Using Dynamic `import()`**:
   You can use dynamic `import()` to split out parts of your application. This allows you to load modules on demand.

   ```javascript
   import React, { Component } from "react";

   class MyComponent extends Component {
     state = {
       MyModule: null,
     };

     loadModule = async () => {
       const { default: MyModule } = await import("./MyModule");
       this.setState({ MyModule });
     };

     render() {
       const { MyModule } = this.state;
       return (
         <div>
           <button onClick={this.loadModule}>Load Module</button>
           {MyModule && <MyModule />}
         </div>
       );
     }
   }

   export default MyComponent;
   ```

2. **Using `React.lazy` and `Suspense`**:
   React provides a built-in way to handle code splitting with `React.lazy` and `Suspense`.

   ```javascript
   import React, { Suspense, lazy } from "react";

   const LazyComponent = lazy(() => import("./LazyComponent"));

   function App() {
     return (
       <div>
         <Suspense fallback={<div>Loading...</div>}>
           <LazyComponent />
         </Suspense>
       </div>
     );
   }

   export default App;
   ```

   In this example, `LazyComponent` is loaded only when it is rendered. While it's being loaded, the `Suspense` component shows a fallback (like a loading spinner).

3. **Route-Based Code Splitting**:
   When using a router (e.g., React Router), you can split code based on routes to load only the components required for the current route.

   ```javascript
   import React, { Suspense, lazy } from "react";
   import { BrowserRouter as Router, Route, Switch } from "react-router-dom";

   const Home = lazy(() => import("./Home"));
   const About = lazy(() => import("./About"));

   function App() {
     return (
       <Router>
         <Suspense fallback={<div>Loading...</div>}>
           <Switch>
             <Route path="/about" component={About} />
             <Route path="/" component={Home} />
           </Switch>
         </Suspense>
       </Router>
     );
   }

   export default App;
   ```

   Here, the `Home` and `About` components are loaded only when their respective routes are accessed.

By using these techniques, you can make your React applications more efficient and responsive by leveraging code splitting.

## 3. **Virtualization**:

- Use libraries like `react-window` or `react-virtualized` to efficiently render large lists or tables.

## 4. **Avoid Anonymous Functions in JSX**:

- Passing anonymous functions directly in JSX can cause unnecessary re-renders.

## 5. **Use Production Build**:

- Ensure you are running a production build of React, which is optimized and stripped of development warnings.

## 6. **Optimize Images and Assets**:

- Compress and optimize images.
- Use SVGs for simple graphics.

## 7. **Efficient State Management**:

- Lift state up only when necessary.
- Use context wisely and avoid overusing it for global state.

## 8. **Debounce and Throttle**:

- Use techniques like debounce and throttle to limit the frequency of function execution, especially for input events.

## 9. **Use React Profiler**:

- Utilize the React Profiler to identify performance bottlenecks.

## 10. **Avoid Reconciliation Pitfalls**:

- Ensure that key props are stable and unique to avoid unnecessary re-renders.

Implementing these strategies can significantly enhance the performance of your React application.

# Important Links

1. [My Code Sample](https://codesandbox.io/s/introduction-to-jsx-forked-j5wyjn?file=/src/index.js)
2. [JSX Syntax](https://github.com/airbnb/javascript/blob/master/react/README.md)
3. [Practice: Keeper App](https://codesandbox.io/s/keeper-app-part-1-starting-forked-7ht3v2)
4. [HTML, CSS Notes](https://codepen.io/NarendranAI/pen/VwXENJO)

# [React Summary](ReactSummary.md)
