

# React

## Prerequisites

Before diving into React, make sure you're comfortable with the following:

- **[JavaScript](../../../../language/JavaScript.md)** — ES6+ features (arrow functions, destructuring, spread/rest, modules, Promises, async/await) are used extensively in React code.
- **[TypeScript](../../../../language/TypeScript.md)** — Most modern React projects use TypeScript. Understanding interfaces, generics, and utility types is essential for typed React development.

---

## Table of Contents

- [React](#react)
  - [Prerequisites](#prerequisites)
  - [Table of Contents](#table-of-contents)
  - [Setup Local Environment](#setup-local-environment)
  - [1. create-react-app](#1-create-react-app)
  - [2. Vite](#2-vite)
  - [Why Vite over Create React App?](#why-vite-over-create-react-app)
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
  - [JSX](#jsx)
  - [TSX](#tsx)
  - [Project Structure, Code Maintainability \& Style Guide](#project-structure-code-maintainability--style-guide)
    - [Folder Structure — Standalone React (Vite)](#folder-structure--standalone-react-vite)
    - [Feature-First vs Layer-First](#feature-first-vs-layer-first)
    - [Folder Structure — Next.js (App Router)](#folder-structure--nextjs-app-router)
    - [Naming Conventions](#naming-conventions)
    - [Component File Structure](#component-file-structure)
    - [ESLint Configuration](#eslint-configuration)
    - [Prettier Configuration](#prettier-configuration)
    - [Absolute Imports (Path Aliases)](#absolute-imports-path-aliases)
    - [Code Maintainability Rules](#code-maintainability-rules)
    - [TypeScript Strict Mode](#typescript-strict-mode)
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
  - [Custom Hooks](#custom-hooks)
  - [Rules of Hooks](#rules-of-hooks)
  - [useLocalStorage](#uselocalstorage)
  - [useFetch](#usefetch)
  - [useDebounce](#usedebounce)
  - [useMediaQuery](#usemediaquery)
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
  - [Usage](#usage)
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
  - [Installation](#installation)
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
  - [Installation](#installation-1)
  - [Usage](#usage-1)
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
    - [**4. useSearchParams**](#4-usesearchparams)
    - [**5. useRouteError**](#5-userouteerror)
  - [Advanced Patterns](#advanced-patterns)
  - [Compound Components](#compound-components)
  - [Higher-Order Components (HOC)](#higher-order-components-hoc)
  - [Render Props](#render-props)
  - [React Server Components (RSC)](#react-server-components-rsc)
    - [Server vs Client Components](#server-vs-client-components)
    - [Key RSC Rules](#key-rsc-rules)
  - [React 18+ Features](#react-18-features)
  - [createRoot](#createroot)
  - [Automatic Batching](#automatic-batching)
  - [Transitions (useTransition \& useDeferredValue)](#transitions-usetransition--usedeferredvalue)
    - [`useTransition`](#usetransition)
    - [`useDeferredValue`](#usedeferredvalue)
  - [`useId`](#useid)
  - [Suspense Improvements](#suspense-improvements)
  - [Error Boundaries](#error-boundaries)
  - [Class-Based Error Boundary](#class-based-error-boundary)
  - [react-error-boundary Library](#react-error-boundary-library)
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
  - [Testing React](#testing-react)
  - [Jest Setup](#jest-setup)
  - [React Testing Library Basics](#react-testing-library-basics)
    - [Common RTL Queries (Preference Order)](#common-rtl-queries-preference-order)
    - [Async Queries](#async-queries)
  - [Testing Hooks](#testing-hooks)
  - [General Programming Concepts \& Libraries](#general-programming-concepts--libraries)
  - [Cohesion and Coupling](#cohesion-and-coupling)
  - [Useful VS Code extensions](#useful-vs-code-extensions)
  - [Useful browser extensions](#useful-browser-extensions)
  - [Populer UI Libraries](#populer-ui-libraries)
  - [Icons for UI](#icons-for-ui)
    - [Installation](#installation-2)
    - [Usage](#usage-2)
    - [Icon Properties](#icon-properties)
  - [Update logic Libraries](#update-logic-libraries)
  - [React Form Libraries](#react-form-libraries)
  - [Validation Libraries](#validation-libraries)
    - [Joi vs Zod](#joi-vs-zod)
  - [HTTP request library](#http-request-library)
  - [CSS in JS styling libraries](#css-in-js-styling-libraries)
  - [TS types for JS library variables](#ts-types-for-js-library-variables)
  - [Cache, Retry AJAX, Infinite Scrolling/Pagination library](#cache-retry-ajax-infinite-scrollingpagination-library)
    - [Installation](#installation-3)
    - [Additional dev tools](#additional-dev-tools)
  - [Infinite scrolling library - (to use with useInifiniteQuery of react-query)](#infinite-scrolling-library---to-use-with-useinifinitequery-of-react-query)
    - [Installation](#installation-4)
  - [Client State Management library](#client-state-management-library)
    - [Installation](#installation-5)
    - [Additional Dev Tools](#additional-dev-tools-1)
  - [Routing Library](#routing-library)
  - [Preferred backend stacks](#preferred-backend-stacks)
  - [Deploying app in GitHub Pages](#deploying-app-in-github-pages)
  - [Angular vs React](#angular-vs-react)
  - [Important Links](#important-links)
  - [React Summary](#react-summary)

---
## Setup Local Environment

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

## Why Vite over Create React App?

CRA was the go-to for years, but the React community has largely moved on from it. Here's why:

| | Create React App | Vite |
|---|---|---|
| Dev server start | Slow (bundles everything first) | Instant (serves native ESM, no bundling) |
| Hot Module Replacement | Slow on large apps | Near-instant, only updates changed module |
| Build tool | Webpack | Rollup (faster, smaller output) |
| Config flexibility | Hidden behind `react-scripts` | Fully configurable `vite.config.ts` |
| TypeScript support | Via `--template typescript` | First-class, zero config |
| Maintenance status | ⚠️ Effectively unmaintained (deprecated by React team) | ✅ Actively maintained |
| Ecosystem | Webpack plugins only | Vite + Rollup plugin ecosystem |

**The key difference:** CRA uses Webpack which must **bundle all modules on startup** before the dev server is ready. Vite uses **native browser ES modules** — it only transforms a file when the browser requests it. On a large project, CRA can take 30–60 seconds to start; Vite takes under a second.

> The official React docs (react.dev) no longer recommend CRA. For new projects, use **Vite**, **Next.js** (if you need SSR), or **Remix**.

## JavaScript Concepts

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

## React Basics

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

React only has a strong opinion on the **UI** layer. Everything else is left to the ecosystem. Each concern has its own dedicated note:

| Concern                                    | Note                                               | Key Libraries                              |
| ------------------------------------------ | -------------------------------------------------- | ------------------------------------------ |
| [UI](React_UI.md)                          | [React_UI.md](React_UI.md)                         | MUI, Chakra, shadcn/ui, Tailwind, Radix UI |
| [Routing](React_Routing.md)                | [React_Routing.md](React_Routing.md)               | TanStack Router, React Router v6           |
| [HTTP & Data Fetching](React_HTTP.md)      | [React_HTTP.md](React_HTTP.md)                     | Axios, TanStack Query                      |
| [Managing App State](React_State.md)       | [React_State.md](React_State.md) · [Redux/Redux.md](../Redux/Redux.md) · [Redux/ReduxPatterns.md](../Redux/ReduxPatterns.md) | useState, useReducer, Context, Zustand, Redux (RTK) |
| [Internationalization](React_i18n.md)      | [React_i18n.md](React_i18n.md)                     | react-i18next, next-intl                   |
| [Forms](React_Forms.md)                    | [React_Forms.md](React_Forms.md)                   | React Hook Form, Server Actions            |
| [Form Validation](React_FormValidation.md) | [React_FormValidation.md](React_FormValidation.md) | Zod + RHF resolver                         |
| [Data Updation](React_DataUpdation.md)     | [React_DataUpdation.md](React_DataUpdation.md)     | Immer, TanStack Query mutations            |
| [Animations](React_Animations.md)          | [React_Animations.md](React_Animations.md)         | Framer Motion, React Spring                |

Also, React is agnostic to whether the app is a web app (ReactDOM) or mobile (React Native).

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
   const element = <h1 className="greeting" id="main-title">Hello, React!</h1>;
   // Transpiles to:
   const element = React.createElement(
     'h1',
     { className: 'greeting', id: 'main-title' },
     'Hello, React!'
   );
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





---

## Project Structure, Code Maintainability & Style Guide

A consistent project structure and code style guide is the foundation of a maintainable React
codebase. This section covers **standalone React (Vite)** and **Next.js (App Router)** setups.

### Folder Structure — Standalone React (Vite)

```
my-react-app/
├── public/                   # Static assets served as-is (favicon, robots.txt)
├── src/
│   ├── assets/               # Images, fonts, SVGs imported by components
│   ├── components/           # Globally reusable UI components
│   │   ├── ui/               # Atomic/primitive components (Button, Input, Modal)
│   │   └── layout/           # Structural components (Header, Footer, Sidebar)
│   ├── features/             # Feature-based modules — self-contained slices of the app
│   │   └── auth/
│   │       ├── components/   # Components used only by this feature
│   │       ├── hooks/        # Hooks used only by this feature
│   │       ├── services/     # API calls for this feature
│   │       ├── store/        # Feature-level state (slices / atoms)
│   │       └── types/        # TypeScript types for this feature
│   ├── hooks/                # Shared custom hooks (used across multiple features)
│   ├── pages/                # Route-level components (used with React Router)
│   ├── services/             # Global API clients and HTTP utilities
│   ├── store/                # Global state (Redux store or Zustand atoms)
│   ├── types/                # Shared TypeScript interfaces and types
│   ├── utils/                # Pure utility / helper functions (no side-effects)
│   ├── lib/                  # Third-party config instances (axios, query client)
│   ├── constants/            # App-wide constants (ROUTES, API_KEYS, etc.)
│   ├── styles/               # Global CSS, theme tokens, design variables
│   ├── App.tsx
│   └── main.tsx
├── .eslintrc.cjs
├── .prettierrc
├── tsconfig.json
└── vite.config.ts
```

### Feature-First vs Layer-First

**Layer-first** (avoid for large apps — everything mixed in flat folders):

```
src/
  components/   ← auth button, product card, checkout widget all mixed together
  hooks/        ← all hooks from all features in one pile
  services/     ← all API calls combined
```

**Feature-first** (recommended — each feature owns its code):

```
src/
  features/
    auth/       ← all auth: components, hooks, services, types
    products/   ← all product: components, hooks, services, types
    checkout/
  components/   ← only truly shared UI (Button, Modal, Input)
  hooks/        ← only truly shared hooks (useDebounce, useLocalStorage)
```

> **Rule of thumb:** if a component or hook is used by **more than one feature**, move it to
> the shared `components/` or `hooks/` folder. Otherwise keep it co-located with its feature.

### Folder Structure — Next.js (App Router)

```
my-next-app/
├── app/                          # App Router root (file-system routing)
│   ├── layout.tsx                # Root layout (wraps every page)
│   ├── page.tsx                  # Home page — renders at /
│   ├── globals.css
│   ├── (auth)/                   # Route group — no URL segment added
│   │   ├── login/
│   │   │   └── page.tsx          # renders at /login
│   │   └── register/
│   │       └── page.tsx          # renders at /register
│   ├── dashboard/
│   │   ├── layout.tsx            # Nested layout (only for /dashboard routes)
│   │   ├── page.tsx              # /dashboard
│   │   └── [id]/
│   │       └── page.tsx          # /dashboard/:id  (dynamic segment)
│   └── api/                      # Route Handlers (replaces pages/api/)
│       └── users/
│           └── route.ts          # GET /api/users  POST /api/users
├── components/                   # Shared UI (Client + Server Components)
│   ├── ui/                       # Primitive components
│   └── layout/                   # Structural components
├── features/                     # Feature modules (same pattern as standalone React)
├── hooks/                        # Client-side custom hooks
├── lib/                          # Server-side configs (Prisma client, Auth options)
├── services/                     # Server-side data fetching functions
├── store/                        # Client-side global state
├── types/                        # Shared TypeScript types
├── utils/
├── constants/
├── public/
├── .eslintrc.cjs
├── .prettierrc
├── next.config.js
└── tsconfig.json
```

> **Next.js gotcha:** Server Components (the default in App Router) **cannot** use hooks,
> browser APIs, or event handlers. Add `'use client'` at the top of any file that needs them.

### Naming Conventions

| Item | Convention | Example |
|---|---|---|
| Component files | PascalCase | `UserProfile.tsx` |
| Hook files | camelCase with `use` prefix | `useAuth.ts`, `useLocalStorage.ts` |
| Utility / service files | camelCase | `apiClient.ts`, `formatDate.ts` |
| CSS Modules | camelCase | `userProfile.module.css` |
| Constants | SCREAMING_SNAKE_CASE | `MAX_RETRY_COUNT`, `API_BASE_URL` |
| Types / Interfaces | PascalCase | `UserProfile`, `ApiResponse<T>` |
| Context files | PascalCase + `Context` suffix | `AuthContext.tsx`, `ThemeContext.tsx` |
| Event handlers | camelCase with `handle` prefix | `handleSubmit`, `handleInputChange` |
| Boolean props | `is`, `has`, `can` prefix | `isLoading`, `hasError`, `canEdit` |
| Test files | Same name + `.test` or `.spec` | `UserProfile.test.tsx` |

### Component File Structure

Follow this internal ordering within every component file for consistency:

```tsx
// 1. React imports first, then third-party, then local (keep groups separated by blank line)
import { useState, useEffect } from 'react';

import { useQuery } from '@tanstack/react-query';

import { Button } from '@/components/ui/Button';
import { fetchUser } from '@/services/userService';
import type { User } from '@/types';

// 2. Types / interfaces for THIS component only
interface UserCardProps {
  userId: string;
  onSelect?: (user: User) => void;
}

// 3. Module-level constants (if any)
const CARD_MAX_WIDTH = 320;

// 4. The component
export function UserCard({ userId, onSelect }: UserCardProps) {
  // 4a. Hooks: state → refs → context → queries (always at the top level, no conditions)
  const [isExpanded, setIsExpanded] = useState(false);
  const { data: user, isLoading } = useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetchUser(userId),
  });

  // 4b. Derived values (computed from state / props, no side-effects)
  const displayName = user ? `${user.firstName} ${user.lastName}` : '—';

  // 4c. Effects
  useEffect(() => {
    document.title = displayName;
  }, [displayName]);

  // 4d. Event handlers
  const handleClick = () => {
    setIsExpanded(prev => !prev);
    onSelect?.(user!);
  };

  // 4e. Early returns — guard clauses (loading, error, empty)
  if (isLoading) return <span>Loading…</span>;
  if (!user) return null;

  // 4f. The JSX return (always last)
  return (
    <div style={{ maxWidth: CARD_MAX_WIDTH }}>
      <h2>{displayName}</h2>
      <button onClick={handleClick}>
        {isExpanded ? 'Collapse' : 'Expand'}
      </button>
    </div>
  );
}
```

### ESLint Configuration

```bash
npm install -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin \
  eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-jsx-a11y \
  eslint-plugin-import eslint-config-prettier
```

`.eslintrc.cjs`:

```js
module.exports = {
  root: true,
  env: { browser: true, es2022: true },
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint', 'react', 'react-hooks', 'jsx-a11y', 'import'],
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',       // React 17+ — no need to import React in every file
    'plugin:react-hooks/recommended',
    'plugin:jsx-a11y/recommended',
    'plugin:import/recommended',
    'prettier',                       // MUST be last — disables conflicting ESLint format rules
  ],
  settings: {
    react: { version: 'detect' },
  },
  rules: {
    '@typescript-eslint/no-unused-vars': ['error', { argsIgnorePattern: '^_' }],
    'react/prop-types': 'off',        // TypeScript handles prop types
    'import/order': ['warn', {
      groups: ['builtin', 'external', 'internal', 'parent', 'sibling', 'index'],
      'newlines-between': 'always',
    }],
  },
};
```

### Prettier Configuration

```bash
npm install -D prettier eslint-config-prettier
```

`.prettierrc`:

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 100,
  "tabWidth": 2,
  "bracketSpacing": true,
  "arrowParens": "always",
  "endOfLine": "lf"
}
```

`.vscode/settings.json` — auto-format and auto-fix on every save:

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  }
}
```

### Absolute Imports (Path Aliases)

Avoid `../../../` path hell by aliasing `src/` as `@/`.

`vite.config.ts`:

```ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: { '@': path.resolve(__dirname, './src') },
  },
});
```

`tsconfig.json`:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": { "@/*": ["./src/*"] }
  }
}
```

Now write `import { Button } from '@/components/ui/Button'` instead of
`'../../../components/ui/Button'`.

> **Next.js:** The `@/` alias is built-in — no extra config required.

### Code Maintainability Rules

1. **One component per file** — multiple exports per file make refactoring and imports harder.
2. **Keep components small** — if a component exceeds ~150 lines, extract sub-components or a
   custom hook.
3. **Extract hooks for logic** — keep stateful logic in custom hooks; keep JSX in the component.
4. **Co-locate tests** — `UserCard.test.tsx` lives next to `UserCard.tsx`, not in a
   distant `__tests__/` folder.
5. **Barrel exports** — use `index.ts` in each folder for cleaner imports:

   ```ts
   // components/ui/index.ts
   export { Button } from './Button';
   export { Input } from './Input';
   export { Modal } from './Modal';

   // consumer:
   import { Button, Modal } from '@/components/ui';
   ```

6. **Avoid prop drilling deeper than 2 levels** — use Context, Zustand, or component
   composition instead.
7. **Never mutate props or state directly** — always return new objects / arrays.
8. **Memoize selectively** — only add `useMemo` / `useCallback` / `React.memo` after the
   React Profiler shows a real performance problem. Premature memoization adds noise.

### TypeScript Strict Mode

Enable the strictest TypeScript settings to catch the most bugs at compile time:

```json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true
  }
}
```

`strict: true` enables: `strictNullChecks`, `noImplicitAny`, `strictFunctionTypes`,
`strictBindCallApply` — the single most impactful TypeScript option for code correctness.

---
## State

State makes UI interactive. UI = f(state). There are declarative and imperative approaches.

## Declarative State Programming

UI depends on the value of a state variable. Hooks help hook into state variables and read/modify them.

## Imperative State Programming

More primitive, used in early JS learning for frontend.

## State in React

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

## Immutable State Updates, Immer & Sharing State

> These topics are covered in full in **[React_State.md](React_State.md)**.

Key concepts:

- **Immutable update patterns** — always return a new object/array; use spread (`...`) for flat objects and arrays; spread at every affected nesting level for deep objects
- **Immer** — lets you write apparently-mutating code (`draft.field = value`) that produces a new immutable state; use `useImmer` / `useImmerReducer` from `use-immer` to replace `useState` / `useReducer` for nested state
- **Sharing state (lifting state)** — move shared state to the closest common ancestor; pass as props; for many levels, lift into React Context (or Zustand)

→ See [React_State.md](React_State.md) § *Immutable Update Patterns*, *Immer*, *Sharing State*, *Pure Components & StrictMode*

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

React Hooks are functions that enable functional components to use state, lifecycle methods, and other React features without writing a class. Here are some of the most important React Hooks and their uses:

## 1. **useState**:

Manages local component state. When the setter is called React schedules a re-render with the new value.

**When to reach for it**: any UI-local value that, when changed, should cause the component to re-render.

**Object state pattern** — group related fields into one state object instead of five separate `useState` calls:

```tsx
interface TableFilters {
  search: string;
  status: "all" | "active" | "archived";
  page: number;
  pageSize: number;
  sortBy: string;
  sortDir: "asc" | "desc";
}

const DEFAULT_FILTERS: TableFilters = {
  search: "",
  status: "all",
  page: 1,
  pageSize: 20,
  sortBy: "createdAt",
  sortDir: "desc",
};

function UserTable() {
  const [filters, setFilters] = useState<TableFilters>(DEFAULT_FILTERS);

  // Partial update — spread existing state first, then override changed fields
  const updateFilter = <K extends keyof TableFilters>(
    key: K,
    value: TableFilters[K]
  ) => {
    setFilters((prev) => ({
      ...prev,
      [key]: value,
      // Reset to page 1 whenever a filter (not the page itself) changes
      page: key === "page" ? (value as number) : 1,
    }));
  };

  const reset = () => setFilters(DEFAULT_FILTERS);

  return (
    <div>
      <input
        value={filters.search}
        onChange={(e) => updateFilter("search", e.target.value)}
        placeholder="Search users…"
      />
      <select
        value={filters.status}
        onChange={(e) =>
          updateFilter("status", e.target.value as TableFilters["status"])
        }
      >
        <option value="all">All</option>
        <option value="active">Active</option>
        <option value="archived">Archived</option>
      </select>
      <button onClick={reset}>Reset filters</button>
    </div>
  );
}
```

> ⚠️ **Gotcha**: React state updates are **asynchronous and batched**. Reading `filters` immediately after `setFilters(...)` still gives you the old value. Use the functional updater form (`prev => ...`) whenever the new state depends on the previous state.

## 2. **useEffect**:

Runs a side-effect **after** the browser has painted. Return a cleanup function to cancel subscriptions, timers, or in-flight requests when the component unmounts or dependencies change.

**When to reach for it**: syncing with external systems (WebSockets, browser APIs, third-party libraries).

```tsx
import { useEffect, useRef, useState } from "react";

interface Notification {
  id: string;
  message: string;
  type: "info" | "warning" | "error";
}

function useNotificationStream(userId: string) {
  const [notifications, setNotifications] = useState<Notification[]>([]);
  const [connectionStatus, setConnectionStatus] = useState<
    "connecting" | "connected" | "disconnected"
  >("connecting");
  // useRef to hold the WebSocket instance — it's not display state, so we
  // don't need it in useState (which would cause an extra render on creation)
  const wsRef = useRef<WebSocket | null>(null);

  useEffect(() => {
    if (!userId) return;

    setConnectionStatus("connecting");
    const ws = new WebSocket(`wss://api.example.com/notifications/${userId}`);
    wsRef.current = ws;

    ws.onopen = () => setConnectionStatus("connected");

    ws.onmessage = (event: MessageEvent) => {
      const notification = JSON.parse(event.data) as Notification;
      setNotifications((prev) => [notification, ...prev].slice(0, 50)); // keep last 50
    };

    ws.onerror = () => setConnectionStatus("disconnected");
    ws.onclose = () => setConnectionStatus("disconnected");

    // Cleanup: close the socket when userId changes or component unmounts
    return () => {
      ws.close();
      wsRef.current = null;
    };
  }, [userId]); // re-runs only when userId changes

  const dismiss = (id: string) =>
    setNotifications((prev) => prev.filter((n) => n.id !== id));

  return { notifications, connectionStatus, dismiss };
}
```

> ⚠️ **Gotchas**:
> - Missing dependencies in the array causes stale-closure bugs that are hard to track down. Use the `eslint-plugin-react-hooks` exhaustive-deps rule.
> - **Always return a cleanup function** for subscriptions. Failing to do so leaks listeners and causes "Can't perform a React state update on an unmounted component" warnings.
> - Empty `[]` dependency array means "run once on mount" — it does **not** mean "run every render".

## 3. **useContext**:

Reads a value from the nearest matching `<Provider>` above in the tree. The consumer re-renders whenever the context value changes.

**When to reach for it**: cross-cutting concerns that many components need but that don't belong in a global store — auth user, theme, locale, feature flags.

```tsx
import {
  createContext,
  useContext,
  useState,
  useCallback,
  type ReactNode,
} from "react";

// ── Types ────────────────────────────────────────────────────────────────────
interface User {
  id: string;
  name: string;
  email: string;
  roles: string[];
}

interface AuthContextValue {
  user: User | null;
  isAuthenticated: boolean;
  hasRole: (role: string) => boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
}

// ── Context ──────────────────────────────────────────────────────────────────
// Initialise with `null` — the custom hook below guards against consuming
// outside the provider so the non-null assertion is safe there.
const AuthContext = createContext<AuthContextValue | null>(null);

// ── Provider ─────────────────────────────────────────────────────────────────
export function AuthProvider({ children }: { children: ReactNode }) {
  const [user, setUser] = useState<User | null>(null);

  const login = useCallback(async (email: string, password: string) => {
    const res = await fetch("/api/auth/login", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ email, password }),
    });
    if (!res.ok) throw new Error("Invalid credentials");
    const loggedInUser: User = await res.json();
    setUser(loggedInUser);
  }, []);

  const logout = useCallback(() => {
    fetch("/api/auth/logout", { method: "POST" });
    setUser(null);
  }, []);

  const hasRole = useCallback(
    (role: string) => user?.roles.includes(role) ?? false,
    [user]
  );

  const value: AuthContextValue = {
    user,
    isAuthenticated: user !== null,
    hasRole,
    login,
    logout,
  };

  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
}

// ── Consumer hook ─────────────────────────────────────────────────────────────
// Wrap useContext so consumers get a clear error if used outside the provider.
export function useAuth(): AuthContextValue {
  const ctx = useContext(AuthContext);
  if (!ctx) throw new Error("useAuth must be used inside <AuthProvider>");
  return ctx;
}

// ── Usage ─────────────────────────────────────────────────────────────────────
function AdminPanel() {
  const { user, hasRole, logout } = useAuth();

  if (!hasRole("admin")) return <p>Access denied.</p>;

  return (
    <div>
      <p>Welcome, {user!.name}</p>
      <button onClick={logout}>Sign out</button>
    </div>
  );
}
```

> ⚠️ **Gotcha**: every component that calls `useContext(MyCtx)` re-renders whenever **any** field in the context value changes. Split large contexts (e.g. `AuthContext` and `AuthDispatchContext`) or use `useMemo` for the value object to avoid unnecessary re-renders.

## 4. **useReducer**:

Alternative to `useState` for complex state where the next state depends on the previous state via well-defined transitions. Works best with TypeScript discriminated-union action types.

**When to reach for it**: multiple related state fields that change together, state machines, or when "what can happen" needs to be explicitly enumerable.

```tsx
import { useReducer } from "react";

// ── Domain types ─────────────────────────────────────────────────────────────
interface CartItem {
  productId: string;
  name: string;
  unitPrice: number;
  qty: number;
}

interface CartState {
  items: CartItem[];
  couponCode: string | null;
  discountPct: number;
}

// Discriminated union — each action is self-documenting
type CartAction =
  | { type: "ADD_ITEM"; payload: Omit<CartItem, "qty"> }
  | { type: "REMOVE_ITEM"; productId: string }
  | { type: "INCREMENT"; productId: string }
  | { type: "DECREMENT"; productId: string }
  | { type: "APPLY_COUPON"; code: string; discountPct: number }
  | { type: "CLEAR_CART" };

// ── Reducer ───────────────────────────────────────────────────────────────────
// Pure function — same inputs always produce the same output, no side effects
function cartReducer(state: CartState, action: CartAction): CartState {
  switch (action.type) {
    case "ADD_ITEM": {
      const exists = state.items.find(
        (i) => i.productId === action.payload.productId
      );
      if (exists) {
        return {
          ...state,
          items: state.items.map((i) =>
            i.productId === action.payload.productId
              ? { ...i, qty: i.qty + 1 }
              : i
          ),
        };
      }
      return {
        ...state,
        items: [...state.items, { ...action.payload, qty: 1 }],
      };
    }
    case "REMOVE_ITEM":
      return {
        ...state,
        items: state.items.filter((i) => i.productId !== action.productId),
      };
    case "INCREMENT":
      return {
        ...state,
        items: state.items.map((i) =>
          i.productId === action.productId ? { ...i, qty: i.qty + 1 } : i
        ),
      };
    case "DECREMENT":
      return {
        ...state,
        items: state.items
          .map((i) =>
            i.productId === action.productId ? { ...i, qty: i.qty - 1 } : i
          )
          .filter((i) => i.qty > 0), // auto-remove when qty hits 0
      };
    case "APPLY_COUPON":
      return {
        ...state,
        couponCode: action.code,
        discountPct: action.discountPct,
      };
    case "CLEAR_CART":
      return { items: [], couponCode: null, discountPct: 0 };
    default:
      return state; // TypeScript exhaustiveness: unreachable if all actions handled
  }
}

const INITIAL_CART: CartState = { items: [], couponCode: null, discountPct: 0 };

// ── Component ─────────────────────────────────────────────────────────────────
function ShoppingCart() {
  const [cart, dispatch] = useReducer(cartReducer, INITIAL_CART);

  const subtotal = cart.items.reduce(
    (sum, i) => sum + i.unitPrice * i.qty,
    0
  );
  const total = subtotal * (1 - cart.discountPct / 100);

  return (
    <div>
      {cart.items.map((item) => (
        <div key={item.productId}>
          <span>{item.name} × {item.qty}</span>
          <button onClick={() => dispatch({ type: "INCREMENT", productId: item.productId })}>+</button>
          <button onClick={() => dispatch({ type: "DECREMENT", productId: item.productId })}>−</button>
          <button onClick={() => dispatch({ type: "REMOVE_ITEM", productId: item.productId })}>✕</button>
        </div>
      ))}
      <p>Total: ${total.toFixed(2)}</p>
      <button onClick={() => dispatch({ type: "CLEAR_CART" })}>Clear cart</button>
    </div>
  );
}
```

> ⚠️ **Gotcha**: the reducer must be a **pure function**. Never mutate `state` directly or call APIs inside it. Side effects (API calls, logging) belong in `useEffect` or event handlers, not in the reducer.

## 5. **useRef**:

Returns a stable object `{ current: T }` that persists across renders without causing re-renders when mutated. Has two distinct use-cases:

1. **DOM access** — attach to a JSX element to read/manipulate it imperatively
2. **Mutable instance variable** — store values that need to survive re-renders but shouldn't trigger them (timers, previous values, WebSocket instances)

```tsx
import { useRef, useEffect, useState, useCallback } from "react";

// ── Use-case 1: DOM ref — programmatic focus management ───────────────────────
function SearchDialog({ isOpen }: { isOpen: boolean }) {
  const inputRef = useRef<HTMLInputElement>(null);

  // Auto-focus the search field whenever the dialog opens
  useEffect(() => {
    if (isOpen) {
      inputRef.current?.focus();
    }
  }, [isOpen]);

  return (
    <dialog open={isOpen}>
      <input ref={inputRef} type="search" placeholder="Search…" />
    </dialog>
  );
}

// ── Use-case 2: Mutable instance variable — tracking previous value ────────────
function usePrevious<T>(value: T): T | undefined {
  const ref = useRef<T | undefined>(undefined);
  useEffect(() => {
    ref.current = value; // runs after render, so ref.current is the PREVIOUS value during render
  });
  return ref.current;
}

// ── Use-case 3: Holding a timer ID to cancel it ───────────────────────────────
function AutoSaveEditor() {
  const [content, setContent] = useState("");
  const timerRef = useRef<ReturnType<typeof setTimeout> | null>(null);
  const [saveStatus, setSaveStatus] = useState<"idle" | "saving" | "saved">("idle");

  const handleChange = useCallback((text: string) => {
    setContent(text);
    if (timerRef.current) clearTimeout(timerRef.current);
    timerRef.current = setTimeout(async () => {
      setSaveStatus("saving");
      await fetch("/api/draft", { method: "PUT", body: JSON.stringify({ content: text }) });
      setSaveStatus("saved");
    }, 2000); // auto-save 2 s after last keystroke
  }, []);

  useEffect(() => () => { if (timerRef.current) clearTimeout(timerRef.current); }, []);

  return (
    <div>
      <textarea value={content} onChange={(e) => handleChange(e.target.value)} />
      <span>{saveStatus === "saving" ? "Saving…" : saveStatus === "saved" ? "Saved ✓" : ""}</span>
    </div>
  );
}
```

> ⚠️ **Gotcha**: mutating `ref.current` does **not** trigger a re-render. If you need the UI to update, use `useState` instead. `useRef` is for values the UI doesn't need to observe.

## 6. **useCallback**:

Returns a **stable function reference** that only changes when its dependencies change. Primarily useful when a function is passed as a prop to a `React.memo`-wrapped child — without it, the child re-renders on every parent render because a new function object is created each time.

**When to reach for it**: a function is in a `useEffect` dependency array, or it is passed as a prop to an expensive memoized child.

```tsx
import { useState, useCallback, memo } from "react";

// ── Child is memoized — will only re-render if props actually change ───────────
const ProductRow = memo(function ProductRow({
  product,
  onAddToCart,
  onWishlist,
}: {
  product: { id: string; name: string; price: number };
  onAddToCart: (id: string) => void;
  onWishlist: (id: string) => void;
}) {
  console.log(`ProductRow "${product.name}" rendered`); // watch how often this fires
  return (
    <tr>
      <td>{product.name}</td>
      <td>${product.price}</td>
      <td>
        <button onClick={() => onAddToCart(product.id)}>Add to cart</button>
        <button onClick={() => onWishlist(product.id)}>♡</button>
      </td>
    </tr>
  );
});

// ── Parent ─────────────────────────────────────────────────────────────────────
function ProductList({ products }: { products: { id: string; name: string; price: number }[] }) {
  const [cartIds, setCartIds] = useState<Set<string>>(new Set());
  const [wishlistIds, setWishlistIds] = useState<Set<string>>(new Set());

  // Without useCallback, these functions are recreated on every render,
  // breaking memo() and causing ALL ProductRow children to re-render.
  const handleAddToCart = useCallback((id: string) => {
    setCartIds((prev) => new Set(prev).add(id));
    fetch("/api/cart", { method: "POST", body: JSON.stringify({ productId: id }) });
  }, []); // stable — doesn't depend on any changing variable

  const handleWishlist = useCallback((id: string) => {
    setWishlistIds((prev) => {
      const next = new Set(prev);
      prev.has(id) ? next.delete(id) : next.add(id);
      return next;
    });
  }, []);

  return (
    <table>
      <tbody>
        {products.map((p) => (
          <ProductRow
            key={p.id}
            product={p}
            onAddToCart={handleAddToCart}
            onWishlist={handleWishlist}
          />
        ))}
      </tbody>
    </table>
  );
}
```

> ⚠️ **Gotcha**: `useCallback` is only useful when the function is passed to a `memo()`-wrapped child or appears in a `useEffect` dependency array. Wrapping every function in `useCallback` "just in case" adds overhead without benefit.

## 7. **useMemo**:

Returns a **memoized computed value** that is only recalculated when its dependencies change. Avoids re-running expensive derivations on every render.

**When to reach for it**: deriving a value from props/state that involves sorting, filtering, or aggregating large arrays, or creating objects/arrays passed to memoized children.

```tsx
import { useState, useMemo } from "react";

interface Order {
  id: string;
  customerId: string;
  customerName: string;
  status: "pending" | "processing" | "shipped" | "delivered" | "cancelled";
  total: number;
  createdAt: string;
}

interface SortConfig {
  field: keyof Order;
  dir: "asc" | "desc";
}

function OrdersDashboard({ orders }: { orders: Order[] }) {
  const [search, setSearch] = useState("");
  const [statusFilter, setStatusFilter] = useState<Order["status"] | "all">("all");
  const [sort, setSort] = useState<SortConfig>({ field: "createdAt", dir: "desc" });

  // This derivation runs only when orders, search, statusFilter, or sort changes —
  // NOT on every render (e.g. when an unrelated piece of state changes).
  const processedOrders = useMemo(() => {
    let result = orders;

    // 1. Filter by status
    if (statusFilter !== "all") {
      result = result.filter((o) => o.status === statusFilter);
    }

    // 2. Filter by search
    if (search.trim()) {
      const q = search.toLowerCase();
      result = result.filter(
        (o) =>
          o.customerName.toLowerCase().includes(q) ||
          o.id.toLowerCase().includes(q)
      );
    }

    // 3. Sort
    result = [...result].sort((a, b) => {
      const aVal = a[sort.field];
      const bVal = b[sort.field];
      const cmp = aVal < bVal ? -1 : aVal > bVal ? 1 : 0;
      return sort.dir === "asc" ? cmp : -cmp;
    });

    return result;
  }, [orders, search, statusFilter, sort]);

  // Aggregate stats — also memoized separately so a search change doesn't
  // recompute stats (which only depend on the full orders list).
  const stats = useMemo(
    () => ({
      total: orders.length,
      revenue: orders.reduce((s, o) => s + o.total, 0),
      pending: orders.filter((o) => o.status === "pending").length,
    }),
    [orders]
  );

  return (
    <div>
      <p>Total revenue: ${stats.revenue.toLocaleString()} | Pending: {stats.pending}</p>
      <input value={search} onChange={(e) => setSearch(e.target.value)} placeholder="Search…" />
      {/* render processedOrders */}
      <p>{processedOrders.length} results</p>
    </div>
  );
}
```

> ⚠️ **Gotcha**: `useMemo` is an **optimisation hint**, not a guarantee. React may discard the cache in low-memory situations. Never use it to enforce a side effect or produce a non-deterministic value.

## 8. **useLayoutEffect**:

Fires **synchronously after DOM mutations but before the browser paints**. This means you can read layout measurements and apply corrections in the same frame, avoiding visible flicker.

**When to reach for it**: measuring DOM geometry (size, position) to position a tooltip, popover, or custom scroll indicator.  
**When NOT to reach for it**: data fetching or anything that doesn't need layout measurements — use `useEffect` for that to avoid blocking the paint.

```tsx
import { useRef, useState, useLayoutEffect, type ReactNode } from "react";

interface TooltipPosition {
  top: number;
  left: number;
}

function Tooltip({
  anchor,
  children,
}: {
  anchor: HTMLElement | null;
  children: ReactNode;
}) {
  const tooltipRef = useRef<HTMLDivElement>(null);
  const [pos, setPos] = useState<TooltipPosition | null>(null);

  // useLayoutEffect so the tooltip renders in the right position in the same
  // frame it first appears — no flash of incorrectly positioned tooltip.
  useLayoutEffect(() => {
    if (!anchor || !tooltipRef.current) return;

    const anchorRect = anchor.getBoundingClientRect();
    const tooltipRect = tooltipRef.current.getBoundingClientRect();
    const viewportWidth = window.innerWidth;

    // Default: above the anchor, centred horizontally
    let top = anchorRect.top - tooltipRect.height - 8;
    let left = anchorRect.left + anchorRect.width / 2 - tooltipRect.width / 2;

    // Flip to below if not enough space above
    if (top < 0) {
      top = anchorRect.bottom + 8;
    }

    // Clamp to viewport edges
    left = Math.max(8, Math.min(left, viewportWidth - tooltipRect.width - 8));

    setPos({ top, left });
  }, [anchor]); // recalculate whenever the anchor element changes

  return (
    <div
      ref={tooltipRef}
      role="tooltip"
      style={{
        position: "fixed",
        top: pos?.top ?? -9999, // hide off-screen until positioned
        left: pos?.left ?? -9999,
        zIndex: 1000,
        background: "#1a1a1a",
        color: "#fff",
        padding: "4px 8px",
        borderRadius: 4,
        fontSize: 12,
        pointerEvents: "none",
      }}
    >
      {children}
    </div>
  );
}
```

> ⚠️ **Gotcha**: `useLayoutEffect` runs **synchronously** — heavy work inside it blocks the browser paint and degrades perceived performance. Keep it limited to layout reads/writes. On the server (SSR/Next.js), `useLayoutEffect` is a no-op and triggers a warning; use `useEffect` or guard with `typeof window !== 'undefined'`.

These are some of the most commonly used React Hooks. Understanding when and why to apply each one — not just the syntax — is what separates beginner from production-quality React code.

## Custom Hooks

Custom hooks extract reusable stateful logic into standalone functions. Any function starting with `use` that calls React hooks is a custom hook.

## Rules of Hooks

1. Only call hooks at the **top level** — never inside loops, conditions, or nested functions
2. Only call hooks from **React function components** or other custom hooks
3. Custom hooks must start with **`use`**

## useLocalStorage

```tsx
import { useState, useEffect } from "react";

function useLocalStorage<T>(key: string, initialValue: T) {
  const [value, setValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? (JSON.parse(item) as T) : initialValue;
    } catch {
      return initialValue;
    }
  });

  useEffect(() => {
    try {
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch {
      console.warn(`Failed to save "${key}" to localStorage`);
    }
  }, [key, value]);

  return [value, setValue] as const;
}

// Usage
const [theme, setTheme] = useLocalStorage<"light" | "dark">("theme", "light");
```

## useFetch

```tsx
import { useState, useEffect, useRef } from "react";

interface FetchState<T> {
  data: T | null;
  isLoading: boolean;
  error: Error | null;
}

function useFetch<T>(url: string): FetchState<T> {
  const [state, setState] = useState<FetchState<T>>({
    data: null,
    isLoading: true,
    error: null,
  });
  const abortRef = useRef<AbortController>(null);

  useEffect(() => {
    abortRef.current = new AbortController();

    setState({ data: null, isLoading: true, error: null });

    fetch(url, { signal: abortRef.current.signal })
      .then((res) => {
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        return res.json() as Promise<T>;
      })
      .then((data) => setState({ data, isLoading: false, error: null }))
      .catch((err) => {
        if (err.name !== "AbortError") {
          setState({ data: null, isLoading: false, error: err });
        }
      });

    return () => abortRef.current?.abort(); // cancel on unmount or url change
  }, [url]);

  return state;
}

// Usage
const { data: users, isLoading, error } = useFetch<User[]>("/api/users");
```

## useDebounce

```tsx
import { useState, useEffect } from "react";

function useDebounce<T>(value: T, delay: number = 500): T {
  const [debouncedValue, setDebouncedValue] = useState<T>(value);

  useEffect(() => {
    const timer = setTimeout(() => setDebouncedValue(value), delay);
    return () => clearTimeout(timer);
  }, [value, delay]);

  return debouncedValue;
}

// Usage — only fires search API call 500ms after user stops typing
function SearchBar() {
  const [query, setQuery] = useState("");
  const debouncedQuery = useDebounce(query, 500);

  useEffect(() => {
    if (debouncedQuery) fetchResults(debouncedQuery);
  }, [debouncedQuery]);

  return <input value={query} onChange={(e) => setQuery(e.target.value)} />;
}
```

## useMediaQuery

```tsx
import { useState, useEffect } from "react";

function useMediaQuery(query: string): boolean {
  const [matches, setMatches] = useState(() =>
    typeof window !== "undefined"
      ? window.matchMedia(query).matches
      : false
  );

  useEffect(() => {
    const mql = window.matchMedia(query);
    const handler = (e: MediaQueryListEvent) => setMatches(e.matches);
    mql.addEventListener("change", handler);
    setMatches(mql.matches);
    return () => mql.removeEventListener("change", handler);
  }, [query]);

  return matches;
}

// Usage
const isMobile = useMediaQuery("(max-width: 768px)");
const prefersDark = useMediaQuery("(prefers-color-scheme: dark)");
```

## React.memo

`React.memo` is a **Higher Order Component** that memoizes the rendered output of a functional component. React skips re-rendering the component and reuses the last rendered result as long as its props have not changed (by shallow comparison).

**When to reach for it**: a child component re-renders frequently due to its parent updating, but the child's own props rarely change and the render is non-trivial.

### Basic usage

```tsx
import { memo } from "react";

interface AvatarProps {
  userId: string;
  displayName: string;
  avatarUrl: string;
  size: "sm" | "md" | "lg";
}

// Without memo: re-renders every time the parent renders, even if props are identical.
// With memo: React shallow-compares each prop; skips re-render if all are ===.
const Avatar = memo(function Avatar({ userId, displayName, avatarUrl, size }: AvatarProps) {
  const px = size === "sm" ? 32 : size === "md" ? 48 : 64;
  return (
    <img
      key={userId}
      src={avatarUrl}
      alt={displayName}
      width={px}
      height={px}
      style={{ borderRadius: "50%" }}
    />
  );
});

export default Avatar;
```

### The second argument — `arePropsEqual`

`React.memo` accepts an optional second argument:

```ts
memo(Component, arePropsEqual?)
```

`arePropsEqual(prevProps, nextProps): boolean`

- Return **`true`** → props are considered equal → **skip re-render**
- Return **`false`** → props changed → **re-render**

This is the inverse of `shouldComponentUpdate` in class components (which returned `true` to re-render).

**Use-case: ignore noisy props that don't affect the output**

```tsx
interface DataGridRowProps {
  row: { id: string; name: string; value: number };
  isSelected: boolean;
  onSelect: (id: string) => void;
  // `meta` carries analytics data — it changes every render but doesn't
  // affect what the row displays. Shallow comparison would always fail.
  meta: Record<string, unknown>;
}

const DataGridRow = memo(
  function DataGridRow({ row, isSelected, onSelect }: DataGridRowProps) {
    return (
      <tr
        style={{ background: isSelected ? "#e8f0fe" : "transparent" }}
        onClick={() => onSelect(row.id)}
      >
        <td>{row.name}</td>
        <td>{row.value}</td>
      </tr>
    );
  },
  // Custom equality: compare only the props that actually affect rendering.
  // Ignore `meta` entirely.
  (prev, next) =>
    prev.row.id === next.row.id &&
    prev.row.name === next.row.name &&
    prev.row.value === next.row.value &&
    prev.isSelected === next.isSelected &&
    prev.onSelect === next.onSelect
);
```

**Use-case: deep-compare a specific nested object**

```tsx
interface ChartProps {
  config: {
    title: string;
    color: string;
    dataKeys: string[];
  };
  data: number[];
}

// config is rebuilt as a new object reference on every parent render
// (e.g. `config={{ title, color, dataKeys }}` inline in JSX).
// Shallow comparison sees a new object reference → always re-renders.
// The custom comparator deep-checks only what the chart actually uses.
const Chart = memo(
  function Chart({ config, data }: ChartProps) {
    /* expensive D3 render */
    return <canvas />;
  },
  (prev, next) =>
    prev.config.title === next.config.title &&
    prev.config.color === next.config.color &&
    prev.config.dataKeys.join(",") === next.config.dataKeys.join(",") &&
    prev.data.length === next.data.length &&
    prev.data.every((v, i) => v === next.data[i])
);
```

### Why shallow comparison fails with objects and functions

```tsx
// ❌ Creates a NEW object reference on every render → memo always re-renders
<DataGridRow row={{ id: "1", name: "Alice", value: 42 }} ... />

// ✅ Stable reference from state/selector → memo works
const row = useMemo(() => ({ id: "1", name: "Alice", value: 42 }), [id, name, value]);
<DataGridRow row={row} ... />

// ❌ Inline arrow function is a new reference every render → breaks memo
<DataGridRow onSelect={(id) => handleSelect(id)} ... />

// ✅ Stable reference via useCallback → memo works
const handleSelect = useCallback((id: string) => { ... }, []);
<DataGridRow onSelect={handleSelect} ... />
```

### The trio: memo + useMemo + useCallback

These three work together:

| Tool | What it stabilises | Without it |
|---|---|---|
| `memo` | The **component** — skips re-render if props unchanged | Child re-renders on every parent render |
| `useCallback` | A **function prop** — same reference between renders | `memo` sees new function → re-renders anyway |
| `useMemo` | An **object/array prop** — same reference between renders | `memo` sees new object → re-renders anyway |

`memo` alone is useless if the parent passes new function or object references every render. All three must be applied together for the optimization to work.

### When NOT to use React.memo

- **Trivial renders** — wrapping a `<span>{name}</span>` in `memo` adds more overhead (comparator call) than it saves.
- **Props change on every render anyway** — `memo` skips nothing and just wastes the comparison.
- **Before profiling** — use React DevTools Profiler to confirm a component is a hot spot before wrapping it.

> ⚠️ **Gotcha**: `memo` does **not** prevent re-renders caused by `useContext`. If a memoized component consumes a context that changes, it still re-renders — `memo` only compares props, not context values.

---

## Event Handling in React

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

## React Event Pooling

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

## Life Cycle of a React Component

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

## Uni directional data flow

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

## Memoizing

Memoizing is a technique used in computer science and software engineering to optimize the performance of functions by caching the results of expensive function calls and returning the cached result when the same inputs occur again. The term "memoization" is derived from the word "memo," which means to remember or note down.

Here's how memoization works:

1. **Function Call**: When a function is called with certain inputs, it performs some computation and returns a result.

2. **Caching**: Before returning the result, the function checks if it has already computed and stored the result for the same inputs.

3. **Cache Hit**: If the function finds that the result for the given inputs is already cached, it returns the cached result instead of re-computing it.

4. **Cache Miss**: If the function doesn't find the result for the given inputs in the cache, it performs the computation, stores the result in the cache, and then returns the result.

Memoization can significantly improve the performance of functions that are called frequently with the same inputs, as it avoids redundant computations by reusing previously computed results. It's particularly useful for functions with expensive computations or calculations.

In JavaScript, memoization is often implemented using techniques like caching results in an object or using higher-order functions to wrap the original function and manage the cache. Libraries like Lodash provide utility functions for memoization, and in the context of React, hooks like `useMemo` can be used to memoize values within functional components.

Overall, memoization is a powerful optimization technique that can help improve the efficiency and responsiveness of applications by reducing unnecessary computations and improving performance.

## React Forms

> Forms, validation, and form libraries are covered in full in the dedicated opinion files.

Key concepts:

- **Controlled components** — `value` + `onChange` drive input via `useState`; re-renders on every keystroke
- **Uncontrolled components** — use `useRef` to read the value only on submit (fewer re-renders)
- **React Hook Form** — preferred library; uncontrolled under the hood, excellent TypeScript support, integrates with Zod/Yup via `@hookform/resolvers`
- **Formik** — alternative; controlled components, built-in Yup `validationSchema`
- **Validation** — schema-based (Zod, Yup), field-level, async server validation, server-side (Next.js Server Actions)

→ See [React_Forms.md](React_Forms.md) for controlled/uncontrolled, RHF, Formik, Server Actions  
→ See [React_FormValidation.md](React_FormValidation.md) for Zod integration, async validation, server-side errors

## Connecting to a Backend & Data Fetching

> HTTP patterns, the service layer, and TanStack Query are covered in full in the dedicated opinion file.

Key concepts:

- **Raw Axios** — `axios.get/post/put/delete` inside `useEffect` with `AbortController` for cancellation; handle `CanceledError` in catch
- **Optimistic vs Pessimistic updates** — optimistic: update UI first, revert on failure; pessimistic: wait for server before updating UI
- **Service layer pattern** — shared `api-client.ts` (Axios instance) + per-resource service class (`user-service.ts`) keeps HTTP logic out of components
- **Generic `HttpService`** — one class for any entity type; instantiate with `create('/endpoint')`
- **Custom data hooks** — `useUsers()` extracts `useEffect` + `useState` logic into a reusable hook
- **TanStack Query** — declarative server-state management; `useQuery` for reads, `useMutation` for writes; handles caching, background refetch, pagination, infinite scroll

→ See [React_HTTP.md](React_HTTP.md) for full coverage including Service Layer, Generic HttpService, custom hooks, TanStack Query

## Global State Management

> Advanced Context patterns, Zustand, and Redux are covered in full in the dedicated opinion file.

Key concepts:

- **`useReducer`** — centralises state update logic in a pure reducer function; ideal when multiple actions modify the same state
- **React Context** — shares state across the component tree without prop-drilling; split into multiple contexts for performance; best for low-frequency values (theme, locale, auth)
- **Zustand** — lightweight store; no provider needed; use `useShallow` to prevent unnecessary re-renders when selecting multiple values
- **Redux** — full-featured; worth the complexity only for cross-framework portability or advanced middleware pipelines

→ See [React_State.md](React_State.md) § *Multi-Context Pattern*, *Custom Provider*, *Zustand*, *Zustand DevTools*  
→ See [Redux/Redux.md](../Redux/Redux.md) for full Redux coverage

## Routing

> Routing concepts, React Router DOM, TanStack Router, and Next.js App Router are covered in the dedicated opinion file.

Key concepts:

- **Hash routing** — URL anchor-based (`/#/page`); works without a server; history never leaves the client
- **History API / browser routing** — clean URLs (`/page`); requires server to serve `index.html` for all paths (or use `createBrowserRouter`)
- **React Router DOM v6** — `createBrowserRouter`, nested routes, `<Outlet>`, `errorElement`; hooks: `useNavigate`, `useParams`, `useSearchParams`, `useLocation`, `useRouteError`
- **TanStack Router** — fully type-safe routes; file-based or code-based; integrates natively with TanStack Query
- **Next.js App Router** — file-system routing; `page.tsx`, `layout.tsx`, `loading.tsx`, `error.tsx`; both static and dynamic segments

→ See [React_Routing.md](React_Routing.md) for full coverage including router hooks and production setup

## Advanced Patterns

## Compound Components

Compound components share implicit state via context, giving consumers a composable API — like `<select>` and `<option>`.

```tsx
import { createContext, useContext, useState, ReactNode } from "react";

interface TabsContextType {
  activeTab: string;
  setActiveTab: (id: string) => void;
}

const TabsContext = createContext<TabsContextType | null>(null);

function useTabs() {
  const ctx = useContext(TabsContext);
  if (!ctx) throw new Error("Must be used within <Tabs>");
  return ctx;
}

// Parent manages state
function Tabs({ children, defaultTab }: { children: ReactNode; defaultTab: string }) {
  const [activeTab, setActiveTab] = useState(defaultTab);
  return (
    <TabsContext.Provider value={{ activeTab, setActiveTab }}>
      <div className="tabs">{children}</div>
    </TabsContext.Provider>
  );
}

// Sub-components consume shared state
function TabList({ children }: { children: ReactNode }) {
  return <div className="tab-list" role="tablist">{children}</div>;
}

function Tab({ id, children }: { id: string; children: ReactNode }) {
  const { activeTab, setActiveTab } = useTabs();
  return (
    <button
      role="tab"
      aria-selected={activeTab === id}
      onClick={() => setActiveTab(id)}
    >
      {children}
    </button>
  );
}

function TabPanel({ id, children }: { id: string; children: ReactNode }) {
  const { activeTab } = useTabs();
  if (activeTab !== id) return null;
  return <div role="tabpanel">{children}</div>;
}

// Attach sub-components as static properties (namespace pattern)
Tabs.List  = TabList;
Tabs.Tab   = Tab;
Tabs.Panel = TabPanel;

// Consumer — clean, declarative API
function App() {
  return (
    <Tabs defaultTab="home">
      <Tabs.List>
        <Tabs.Tab id="home">Home</Tabs.Tab>
        <Tabs.Tab id="about">About</Tabs.Tab>
      </Tabs.List>
      <Tabs.Panel id="home"><HomePage /></Tabs.Panel>
      <Tabs.Panel id="about"><AboutPage /></Tabs.Panel>
    </Tabs>
  );
}
```

## Higher-Order Components (HOC)

An HOC is a function that takes a component and returns a new enhanced component. Useful for cross-cutting concerns (auth, logging, theming).

```tsx
import { ComponentType } from "react";

// HOC that adds authentication guard
function withAuth<P extends object>(WrappedComponent: ComponentType<P>) {
  return function AuthenticatedComponent(props: P) {
    const { user, isLoading } = useAuth();

    if (isLoading) return <Spinner />;
    if (!user) return <Navigate to="/login" replace />;

    return <WrappedComponent {...props} />;
  };
}

// HOC that injects loading state
function withLoading<P extends object>(WrappedComponent: ComponentType<P>) {
  return function WithLoadingComponent({
    isLoading,
    ...props
  }: P & { isLoading: boolean }) {
    if (isLoading) return <div>Loading...</div>;
    return <WrappedComponent {...(props as P)} />;
  };
}

// Usage — compose HOCs
const ProtectedDashboard = withAuth(withLoading(Dashboard));
```

> **Prefer custom hooks over HOCs** when possible — hooks are simpler and don't add to the component tree depth.

## Render Props

A component accepts a function as a prop (or `children`) and calls it with internal state. Gives consumers full control over rendering.

```tsx
interface MousePosition { x: number; y: number }

function MouseTracker({
  render,
}: {
  render: (pos: MousePosition) => React.ReactNode;
}) {
  const [pos, setPos] = useState<MousePosition>({ x: 0, y: 0 });

  return (
    <div
      onMouseMove={(e) => setPos({ x: e.clientX, y: e.clientY })}
      style={{ height: "100vh" }}
    >
      {render(pos)}
    </div>
  );
}

// Usage
<MouseTracker
  render={({ x, y }) => (
    <p>Mouse is at ({x}, {y})</p>
  )}
/>
```

> **Modern alternative:** Extract to a custom hook (`useMouse`) — same logic, cleaner code.

---

## React Server Components (RSC)

React Server Components (introduced with Next.js 13+ App Router) run **exclusively on the server**. They can directly access databases, file systems, and secrets — without sending those to the browser.

### Server vs Client Components

| | Server Component | Client Component |
|---|---|---|
| Default in Next.js App Router | ✅ Yes | ❌ No (opt-in with `"use client"`) |
| Runs on | Server only | Browser (+ server for hydration) |
| Can use hooks (useState, etc.) | ❌ No | ✅ Yes |
| Can access DB / filesystem | ✅ Yes | ❌ No |
| Bundle size impact | Zero (not sent to browser) | Adds to JS bundle |
| Can accept/render Server Components as children | ✅ Yes | ✅ Yes |

```tsx
// app/users/page.tsx — Server Component (default in Next.js App Router)
import { db } from "@/lib/db";

export default async function UsersPage() {
  const users = await db.user.findMany(); // direct DB access, no API needed

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

// app/users/UserCard.tsx — Client Component (needs interactivity)
"use client";

import { useState } from "react";

export function UserCard({ user }: { user: User }) {
  const [expanded, setExpanded] = useState(false);

  return (
    <div onClick={() => setExpanded((e) => !e)}>
      <h3>{user.name}</h3>
      {expanded && <p>{user.email}</p>}
    </div>
  );
}
```

### Key RSC Rules

1. Never `"use client"` on a component that accesses secrets/DB — those would leak to the browser
2. Client Components can import Server Components as `children` props (pass-through is safe)
3. Client Components **cannot** import Server Components directly
4. Use `Suspense` around Server Components for streaming/loading states

---

## React 18+ Features

React 18 (released March 2022) introduced a new concurrent rendering engine and several new APIs.

## createRoot

React 18 replaced `ReactDOM.render` with `createRoot`. This opts your app into concurrent features.

```tsx
// Before (React 17)
import ReactDOM from "react-dom";
ReactDOM.render(<App />, document.getElementById("root"));

// After (React 18)
import { createRoot } from "react-dom/client";
const root = createRoot(document.getElementById("root")!);
root.render(<App />);
```

## Automatic Batching

In React 17, state updates inside async callbacks (setTimeout, Promises, native event handlers) were NOT batched — each caused a separate re-render. React 18 batches all updates automatically.

```tsx
// React 18 — only ONE re-render, even in async callbacks
setTimeout(() => {
  setCount((c) => c + 1);
  setFlag((f) => !f);
  // Previously: 2 renders. Now: 1 render.
}, 1000);

// Opt-out if needed
import { flushSync } from "react-dom";
flushSync(() => setCount((c) => c + 1));
flushSync(() => setFlag((f) => !f));
// Forces 2 separate renders
```

## Transitions (useTransition & useDeferredValue)

Transitions mark updates as **non-urgent**. React can interrupt them to keep the UI responsive for urgent updates (typing, clicking).

### `useTransition`

```tsx
import { useState, useTransition } from "react";

function SearchPage() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState<string[]>([]);
  const [isPending, startTransition] = useTransition();

  const handleSearch = (e: React.ChangeEvent<HTMLInputElement>) => {
    const value = e.target.value;
    setQuery(value); // urgent: update input immediately

    startTransition(() => {
      // non-urgent: heavy search/filter can lag
      setResults(performExpensiveSearch(value));
    });
  };

  return (
    <>
      <input value={query} onChange={handleSearch} placeholder="Search..." />
      {isPending && <span>Loading...</span>}
      <ResultsList results={results} />
    </>
  );
}
```

### `useDeferredValue`

Defer the rendering of a slow child component without blocking the parent:

```tsx
import { useState, useDeferredValue } from "react";

function App() {
  const [text, setText] = useState("");
  const deferredText = useDeferredValue(text); // lags behind `text`

  return (
    <>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <SlowList query={deferredText} /> {/* re-renders only when idle */}
    </>
  );
}
```

## `useId`

Generate unique, stable IDs that work with server-side rendering. Never use `Math.random()` for IDs in React.

```tsx
import { useId } from "react";

function FormField({ label }: { label: string }) {
  const id = useId(); // e.g., ":r0:", ":r1:", ...

  return (
    <div>
      <label htmlFor={id}>{label}</label>
      <input id={id} type="text" />
    </div>
  );
}
```

## Suspense Improvements

React 18 extended Suspense to work with data fetching (not just lazy-loaded components):

```tsx
import { Suspense, lazy } from "react";

// Lazy-loaded component
const Dashboard = lazy(() => import("./Dashboard"));

function App() {
  return (
    <Suspense fallback={<LoadingSpinner />}>
      <Dashboard />
    </Suspense>
  );
}

// Nested Suspense — granular loading states
function Page() {
  return (
    <Suspense fallback={<PageSkeleton />}>
      <Header />
      <Suspense fallback={<FeedSkeleton />}>
        <Feed />
      </Suspense>
      <Suspense fallback={<SidebarSkeleton />}>
        <Sidebar />
      </Suspense>
    </Suspense>
  );
}
```

---

## Error Boundaries

Error Boundaries catch JavaScript errors anywhere in their child component tree, log them, and display a fallback UI instead of crashing the whole app.

> **Error Boundaries do NOT catch:** event handlers, async code, server-side rendering, errors thrown in the boundary itself.

## Class-Based Error Boundary

Error boundaries must be class components (no hook equivalent exists yet):

```tsx
import { Component, ErrorInfo, ReactNode } from "react";

interface Props {
  fallback: ReactNode;
  children: ReactNode;
}

interface State {
  hasError: boolean;
  error: Error | null;
}

class ErrorBoundary extends Component<Props, State> {
  state: State = { hasError: false, error: null };

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, info: ErrorInfo) {
    // Log to error tracking service (e.g., Sentry)
    console.error("Caught by ErrorBoundary:", error, info.componentStack);
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback;
    }
    return this.props.children;
  }
}

// Usage
function App() {
  return (
    <ErrorBoundary fallback={<h2>Something went wrong. Please refresh.</h2>}>
      <RiskyComponent />
    </ErrorBoundary>
  );
}
```

## react-error-boundary Library

The community package `react-error-boundary` provides a convenient functional API:

```bash
npm install react-error-boundary
```

```tsx
import { ErrorBoundary, useErrorBoundary } from "react-error-boundary";

function ErrorFallback({ error, resetErrorBoundary }: {
  error: Error;
  resetErrorBoundary: () => void;
}) {
  return (
    <div role="alert">
      <p>Something went wrong:</p>
      <pre style={{ color: "red" }}>{error.message}</pre>
      <button onClick={resetErrorBoundary}>Try again</button>
    </div>
  );
}

function App() {
  return (
    <ErrorBoundary
      FallbackComponent={ErrorFallback}
      onError={(error, info) => logErrorToService(error, info)}
      onReset={() => resetAppState()}
    >
      <MyComponent />
    </ErrorBoundary>
  );
}

// Throw errors from hooks/async handlers
function MyComponent() {
  const { showBoundary } = useErrorBoundary();

  async function fetchData() {
    try {
      await api.getData();
    } catch (e) {
      showBoundary(e); // triggers the nearest error boundary
    }
  }
}
```

---

## Optimization in React

Optimization in React involves improving the performance and efficiency of your application. Here are some key techniques:

## 1. **Memoization**:

- Use `React.memo` to prevent unnecessary re-renders of functional components. Supports a second `arePropsEqual` argument for custom comparison. See the [React.memo section above](#reactmemo) for full patterns.
- Use `useMemo` to memoize expensive derived values (filtering, sorting, aggregating large arrays).
- Use `useCallback` to stabilise function references passed as props to `memo()`-wrapped children.
- All three work as a trio — `memo` alone is ineffective if the parent still passes new object/function references on every render.

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

---

## Testing React

## Jest Setup

For projects using Vite + TypeScript, use Vitest (Jest-compatible) or Jest with ts-jest:

```bash
# Option 1: Vitest (recommended for Vite projects)
npm install --save-dev vitest @testing-library/react @testing-library/jest-dom @testing-library/user-event jsdom

# Option 2: Jest
npm install --save-dev jest @types/jest ts-jest @testing-library/react @testing-library/jest-dom @testing-library/user-event jest-environment-jsdom
```

**vitest.config.ts:**
```typescript
import { defineConfig } from "vitest/config";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,
    environment: "jsdom",
    setupFiles: ["./src/test/setup.ts"],
  },
});
```

**src/test/setup.ts:**
```typescript
import "@testing-library/jest-dom";
```

## React Testing Library Basics

RTL encourages testing behavior (what the user sees/does) over implementation details.

```tsx
import { render, screen, fireEvent, waitFor } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { Counter } from "./Counter";

describe("Counter", () => {
  it("renders with initial count of 0", () => {
    render(<Counter />);
    expect(screen.getByText("Count: 0")).toBeInTheDocument();
  });

  it("increments count when button is clicked", async () => {
    const user = userEvent.setup();
    render(<Counter />);

    await user.click(screen.getByRole("button", { name: /increment/i }));

    expect(screen.getByText("Count: 1")).toBeInTheDocument();
  });

  it("displays error when fetch fails", async () => {
    // Mock global fetch
    global.fetch = vi.fn().mockResolvedValue({
      ok: false,
      status: 500,
    });

    render(<DataComponent />);

    await waitFor(() => {
      expect(screen.getByRole("alert")).toHaveTextContent("Failed to load");
    });
  });
});
```

### Common RTL Queries (Preference Order)

| Priority | Query | When to Use |
|---|---|---|
| 1st | `getByRole` | Most elements — buttons, headings, inputs |
| 2nd | `getByLabelText` | Form inputs with labels |
| 3rd | `getByPlaceholderText` | Inputs with placeholder |
| 4th | `getByText` | Non-interactive text elements |
| 5th | `getByDisplayValue` | Current value of select/input |
| 6th | `getByAltText` | Images |
| 7th | `getByTitle` | Elements with title attr |
| Last | `getByTestId` | Last resort — add `data-testid` to element |

### Async Queries

```tsx
// waitFor — retries assertion until it passes or times out
await waitFor(() => expect(screen.getByText("Done")).toBeInTheDocument());

// findBy* — returns a promise, waits for element to appear
const element = await screen.findByText("Loaded Data");

// queryBy* — returns null if not found (for testing absence)
expect(screen.queryByText("Error")).not.toBeInTheDocument();
```

## Testing Hooks

Use `renderHook` from RTL to test custom hooks in isolation:

```tsx
import { renderHook, act } from "@testing-library/react";
import { useCounter } from "./useCounter";

describe("useCounter", () => {
  it("starts at the initial value", () => {
    const { result } = renderHook(() => useCounter(10));
    expect(result.current.count).toBe(10);
  });

  it("increments", () => {
    const { result } = renderHook(() => useCounter(0));

    act(() => result.current.increment());

    expect(result.current.count).toBe(1);
  });

  it("resets to initial value", () => {
    const { result } = renderHook(() => useCounter(5));

    act(() => result.current.increment());
    act(() => result.current.reset());

    expect(result.current.count).toBe(5);
  });
});
```

---

## General Programming Concepts & Libraries

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

## Important Links

1. [My Code Sample](https://codesandbox.io/s/introduction-to-jsx-forked-j5wyjn?file=/src/index.js)
2. [JSX Syntax](https://github.com/airbnb/javascript/blob/master/react/README.md)
3. [Practice: Keeper App](https://codesandbox.io/s/keeper-app-part-1-starting-forked-7ht3v2)
4. [HTML, CSS Notes](https://codepen.io/NarendranAI/pen/VwXENJO)

## [React Summary](ReactSummary.md)
