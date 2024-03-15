# Redux

Redux is an open-source JavaScript library primarily used for managing the state of applications. It is commonly used with libraries/frameworks like React, though it can be integrated with any JavaScript framework or library.

The core concept of Redux revolves around maintaining a single immutable state tree for the entire application. This state tree represents the entire state of the application and is managed in a predictable manner using pure functions called reducers. Redux follows a unidirectional data flow pattern, which means that data flows in a single direction within the application, making it easier to understand and debug.

## Installation

```bash
# Redux Core
npm install redux

# Redux Toolkit - official recommended approach for writing Redux logic. It wraps around the Redux core, and contains packages and functions that we think are essential for building a Redux app. Redux Toolkit builds in our suggested best practices, simplifies most Redux tasks, prevents common mistakes, and makes it easier to write Redux applications.

npm install @reduxjs/toolkit react-redux
```

# Pros and Cons of using Redux

## Pros:

1. Predictable state changes
2. Centralized state
3. Easy Debugging
4. Preserve Page State
5. Undo/Redo
6. Ecosystem of add-ons

## Cons:

1. Complexity
2. Verbosity

# When should we not use Redux ?

- Tight Budget
- Small to medium-size apps
- Simple UI/Data Flow
- Static Data

# Redux and Functional Programming

Redux is heavily influenced by functional programming principles. Here's how Redux aligns with functional programming concepts:

1. **Immutability**: In Redux, the state of the application is kept immutable. Instead of directly modifying the state, reducers create new state objects based on the previous state and the action dispatched. This prevents unintended side effects and makes state changes predictable, which is a fundamental aspect of functional programming.

2. **Pure functions**: Reducers in Redux are pure functions, meaning they produce the same output for the same input and do not have side effects. Given the same input, a reducer will always produce the same output, making it easy to reason about and test. This adherence to pure functions aligns with the functional programming paradigm.

3. **Avoiding mutable data**: Functional programming promotes the use of immutable data structures and avoiding mutable state. Redux encourages this by enforcing immutability in the state updates. Actions are dispatched to describe state changes, and reducers compute the new state based on the previous state and the action, without modifying the original state.

4. **Composition**: Functional programming emphasizes composing functions to create more complex behavior from simpler functions. In Redux, reducers are composed together to create the root reducer, which manages the entire state tree. This allows for modularization and reuse of reducer logic.

5. **Declarative programming**: Redux promotes a declarative approach to state management. Instead of imperatively modifying the state, developers describe how the state should change in response to actions through pure functions (reducers). This aligns with the declarative nature of functional programming, where programs describe what should be done rather than how to do it.

Overall, Redux's design principles and usage patterns are heavily influenced by functional programming concepts, making it a natural fit for applications developed using functional programming paradigms.

# Programming Paradigms

There are several programming paradigms, each offering different approaches to solving problems and structuring code. Here are some of the most prominent programming paradigms:

1. **Imperative Programming**: This paradigm focuses on describing how a program operates by giving a sequence of commands that change the program's state. It emphasizes the use of statements that change the state of variables and objects directly.

2. **Declarative Programming**: In contrast to imperative programming, declarative programming focuses on describing what the program should accomplish without specifying how it should achieve it. It emphasizes expressing the logic of a computation without describing its control flow.

3. **Procedural Programming**: This paradigm organizes code into procedures, also known as functions or subroutines, which perform specific tasks. It focuses on defining procedures that can be called to execute specific actions.

4. **Functional Programming**: Functional programming treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data. It emphasizes the use of pure functions, higher-order functions, and immutable data.

5. **Object-Oriented Programming (OOP)**: OOP organizes code into objects, which are instances of classes representing real-world entities. It emphasizes encapsulation, inheritance, and polymorphism as key concepts for structuring and managing code.

6. **Event-Driven Programming**: In this paradigm, the flow of the program is determined by events such as user actions, sensor outputs, or messages from other programs. It focuses on handling and responding to events asynchronously.

7. **Logic Programming**: Logic programming is based on formal logic. Programs are written in terms of relations and rules, and the execution model involves finding solutions by logical inference.

8. **Structured Programming**: This paradigm emphasizes the use of structured control flow constructs like loops, conditionals, and functions to improve the clarity, maintainability, and efficiency of code.

9. **Aspect-Oriented Programming (AOP)**: AOP aims to increase modularity by allowing the separation of cross-cutting concerns, such as logging, authentication, and error handling, from the main program logic.

10. **Concurrent Programming**: Concurrent programming deals with the execution of multiple tasks simultaneously. It involves techniques for managing shared resources, synchronization, and communication between concurrent tasks.

These paradigms can be used individually or in combination, depending on the requirements of a given problem and the preferences of the developers.

# Functional Programming

Functional programming (FP) is a programming paradigm that treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data. It emphasizes the use of pure functions, higher-order functions, and immutable data. Here's a more detailed explanation of key concepts in functional programming:

1. **Pure Functions**: Pure functions are functions that produce the same output for the same input and have no side effects. They do not rely on or modify external state, making them deterministic and easier to reason about. Pure functions are key to achieving referential transparency, which means that an expression can be replaced with its value without changing the program's behavior.

2. **Immutability**: Functional programming encourages the use of immutable data structures, where once a data structure is created, its state cannot be changed. Instead of modifying existing data, new data structures are created with the desired modifications. Immutable data structures help prevent unintended side effects and make it easier to reason about code.

3. **Higher-Order Functions**: Higher-order functions are functions that can take other functions as arguments or return functions as results. They enable the composition of functions and provide a powerful mechanism for abstraction and code reuse. Examples of higher-order functions include map, filter, and reduce.

4. **First-Class Functions**: In functional programming languages, functions are treated as first-class citizens, meaning they can be assigned to variables, passed as arguments to other functions, and returned as values from other functions. This allows functions to be manipulated and used just like any other data type.

5. **Function Composition**: Functional programming promotes the composition of functions, where the output of one function becomes the input of another function. This allows for the creation of complex behavior by combining simpler functions together. Function composition enables code to be concise, modular, and easier to understand.

6. **Recursion**: Recursion is a fundamental concept in functional programming, where functions call themselves to solve smaller instances of the same problem. Recursive functions are often used to process recursive data structures like trees and lists. Tail recursion, where the recursive call is the last operation performed by the function, is commonly used in functional programming languages to optimize recursive algorithms.

7. **Lazy Evaluation**: Lazy evaluation is a technique where expressions are not evaluated until their results are actually needed. This can help improve performance by avoiding unnecessary computations. Functional programming languages often use lazy evaluation to delay the evaluation of expressions until they are demanded by the program.

8. **Pattern Matching**: Pattern matching is a technique used to match data structures against patterns and extract components according to the match. It is commonly used in functional programming languages for tasks like destructuring data structures and implementing algorithms based on case analysis.

Functional programming languages like Haskell, Lisp, Clojure, and Scala provide built-in support for these concepts and encourage developers to write code in a functional style. However, functional programming concepts can also be applied in languages that support higher-order functions and immutable data structures, even if they are not purely functional languages.

## Higher Order Function

A higher-order function is a function that can take other functions as arguments or return functions as results. In other words, it treats functions as first-class citizens. This concept is fundamental to functional programming and enables a wide range of powerful programming techniques, including function composition, abstraction, and code reuse.

Here's an explanation of higher-order functions and how they are used:

1. **Functions as Arguments**: A higher-order function can accept other functions as arguments. This allows you to pass behavior as a parameter, making functions more flexible and customizable. For example:

```javascript
// Higher-order function that applies a function to each element of an array
function map(array, fn) {
  const result = [];
  for (const element of array) {
    result.push(fn(element));
  }
  return result;
}

// Function to double a number
function double(x) {
  return x * 2;
}

const numbers = [1, 2, 3, 4];
const doubledNumbers = map(numbers, double);
console.log(doubledNumbers); // Output: [2, 4, 6, 8]
```

In this example, the `map` function is a higher-order function that accepts an array and a function `fn`. It applies the function `fn` to each element of the array and returns a new array containing the results. The `double` function is passed as an argument to `map`, causing each number in the `numbers` array to be doubled.

2. **Functions as Return Values**: A higher-order function can also return another function as its result. This allows for the creation of functions on the fly based on certain conditions or parameters. For example:

```javascript
// Higher-order function that returns a function to calculate the power of a number
function power(exponent) {
  return function (base) {
    return Math.pow(base, exponent);
  };
}

const square = power(2);
console.log(square(3)); // Output: 9 (3^2 = 9)

const cube = power(3);
console.log(cube(3)); // Output: 27 (3^3 = 27)
```

In this example, the `power` function is a higher-order function that takes an `exponent` as an argument and returns a new function. This returned function calculates the power of a given `base` raised to the specified `exponent`. By invoking `power` with different exponents, we can create new functions like `square` and `cube` that calculate the square and cube of a number, respectively.

Higher-order functions provide a powerful mechanism for abstraction, modularity, and code reuse in functional programming languages. They enable developers to write more concise and expressive code by treating functions as first-class citizens.

## Function Composition

Function composition is a fundamental concept in functional programming where the output of one function is passed as the input to another function, creating a chain of functions that are applied sequentially. It allows you to combine multiple functions to create new functions, enabling a concise and modular approach to solving problems.

In mathematical notation, function composition is denoted by the symbol "∘". If you have two functions f and g, the composition of f and g is written as f ∘ g, and it represents the function that applies g first and then applies f to the result.

Here's an example in JavaScript:

```javascript
// Function to double a number
const double = (x) => x * 2;

// Function to increment a number
const increment = (x) => x + 1;

// Function composition: double(increment(x))
const doubleAfterIncrement = (x) => double(increment(x));

console.log(doubleAfterIncrement(3)); // Output: 8 (2 * (3 + 1) = 8)
```

In the example above, the function `doubleAfterIncrement` is created by composing the `double` function with the `increment` function. This means that the input value `x` is first incremented by 1 using the `increment` function, and then the result is doubled using the `double` function.

Function composition can be further generalized to compose an arbitrary number of functions. In functional programming languages, higher-order functions like `compose` or `pipe` are often provided to facilitate function composition. Here's how you could implement a `compose` function in JavaScript:

```javascript
// Function to compose two functions
const compose = (f, g) => (x) => f(g(x));

// Compose three functions
const triple = (x) => x * 3;
const tripleAfterDoubleIncrement = compose(triple, doubleAfterIncrement);

console.log(tripleAfterDoubleIncrement(3)); // Output: 24 ((2 * (3 + 1)) * 3 = 24)
```

In this example, `tripleAfterDoubleIncrement` is created by composing the `triple` function with the previously composed function `doubleAfterIncrement`. The `compose` function takes two functions `f` and `g` and returns a new function that first applies `g` to the input and then applies `f` to the result. This allows for the chaining of multiple functions in a modular and composable manner.

### Composition using Lodash

Lodash is a popular JavaScript utility library that provides a wide range of functions to facilitate common programming tasks. Among its many features, Lodash includes functions for working with arrays, objects, strings, functions, and more. Two functions provided by Lodash, `_.compose` and `_.pipe`, are used for function composition.

1. **\_.compose**: The `_.compose` function in Lodash creates a new function that is the composition of the provided functions. It combines multiple functions into a single function, where the output of one function is passed as the input to the next function in the chain.

   Here's an example of how `_.compose` works:

   ```javascript
   import { compose } from "lodash/fp";

   // Define two functions
   const add = (x) => x + 1;
   const double = (x) => x * 2;

   // Create a composed function that first adds 1 and then doubles the result
   const addAndDouble = compose(double, add);

   console.log(addAndDouble(3)); // Output: 8 ((3 + 1) * 2 = 8)
   ```

   In this example, `addAndDouble` is a composed function that first applies the `add` function and then applies the `double` function to the result.

2. **\_.pipe**: Similar to `_.compose`, the `_.pipe` function in Lodash creates a new function that is the composition of the provided functions. However, `_.pipe` applies the functions in left-to-right order, whereas `_.compose` applies them in right-to-left order.

   Here's an example of how `_.pipe` works:

   ```javascript
   import { pipe } from "lodash/fp";

   // Define two functions
   const add = (x) => x + 1;
   const double = (x) => x * 2;

   // Create a piped function that first adds 1 and then doubles the result
   const addAndDouble = pipe(add, double);

   console.log(addAndDouble(3)); // Output: 8 ((3 + 1) * 2 = 8)
   ```

   In this example, `addAndDouble` is a piped function that first applies the `add` function and then applies the `double` function to the result.

Both `_.compose` and `_.pipe` are useful for creating composed functions that combine multiple functions into a single function, enabling a more modular and expressive coding style. The choice between `_.compose` and `_.pipe` depends on whether you prefer to specify the function composition in right-to-left order or left-to-right order, respectively.

### Currying

Currying is a technique used in functional programming to transform a function with multiple arguments into a sequence of nested functions, each taking a single argument. The process of currying allows for partial application of a function, where some of its arguments are fixed, resulting in a new function that takes fewer arguments.

Here's a simple example to illustrate currying in JavaScript:

```javascript
// Regular function with multiple arguments
function add(x, y) {
  return x + y;
}

console.log(add(2, 3)); // Output: 5

// Curried version of the add function
function curryAdd(x) {
  return function (y) {
    return x + y;
  };
}

const add2 = curryAdd(2); // Partial application: fix the first argument to 2
console.log(add2(3)); // Output: 5
```

In this example:

- The `add` function takes two arguments `x` and `y` and returns their sum.
- The `curryAdd` function is a curried version of `add`, where the first argument `x` is fixed, and the function returns another function that takes the second argument `y`.
- By calling `curryAdd(2)`, we partially apply the `add` function, fixing `x` to 2 and returning a new function.
- The `add2` function is the result of partially applying `curryAdd(2)`, so it only requires one argument (`y`), and it adds `2` to whatever value of `y` is provided.

Currying enables a more modular and flexible way of working with functions. It allows for the creation of specialized versions of functions with certain arguments pre-configured, making it easier to reuse and compose functions in various contexts. Additionally, curried functions can be composed more easily with other functions, as they accept and return functions, which aligns well with the principles of functional programming.

## Pure Functions

Pure functions are a fundamental concept in functional programming.
They must not deal with `random values`, `current date and time`, `global state (DOM, Files, DB)`, `mutation of paramenters`
They are functions that have two main characteristics:

1. **Determinism**: A pure function, given the same input, will always return the same output, without any variation. This property ensures predictability and reliability in the codebase.

2. **No Side Effects**: Pure functions do not cause any observable side effects outside of their scope, such as modifying global variables, mutating input parameters, or performing I/O operations. They only rely on their input parameters to compute the output and do not alter external state.

Here's an example of a pure function:

```javascript
function add(a, b) {
  return a + b;
}
```

This `add` function takes two parameters and returns their sum. It satisfies the characteristics of a pure function because:

- It always returns the same result for the same input (e.g., `add(2, 3)` will always return `5`).
- It does not modify any external state or variables outside of its scope.

In contrast, here's an example of an impure function:

```javascript
let total = 0;

function addToTotal(value) {
  total += value;
  return total;
}
```

This `addToTotal` function adds a value to the `total` variable, which is external to the function's scope. It has side effects because it modifies external state (`total`) and does not strictly rely on its input parameters to compute the output.

Benefits of Pure Functions:

`Self-documenting, Easily testable, Concurrency and Cacheable`

1. **Predictability**: Pure functions make code more predictable and easier to reason about since they always produce the same output for the same input, regardless of the context in which they are called.

2. **Testability**: Testing pure functions is straightforward because they are isolated from external state and side effects. You can easily mock or stub their input parameters and assert their return values.

3. **Reusability**: Pure functions are composable and reusable since they only depend on their input parameters. They can be safely reused in different parts of the codebase without worrying about unintended side effects.

4. **Parallelism**: Pure functions are inherently thread-safe and can be executed concurrently without the risk of race conditions or data corruption.

By adhering to the principles of pure functions, developers can write cleaner, more maintainable, and bug-resistant code, especially in functional programming paradigms.

## Immutability

Immutability refers to the characteristic of data that once created cannot be changed or modified. In programming, immutability is an important concept, particularly in functional programming paradigms, and it contrasts with mutable data, which can be modified after creation.

Here are some key points about immutability:

1. **Immutable Data**: Immutable data cannot be changed once it's created. Any operation that seems to modify immutable data actually creates a new copy of the data with the desired changes.

2. **Immutable Objects**: In many programming languages, data types like strings, numbers, and tuples are immutable by default. Once created, their values cannot be modified.

3. **Immutable Collections**: Immutable data structures like lists, sets, and maps are commonly used in functional programming languages. These collections provide methods for creating new collections with modified data while leaving the original collection unchanged.

4. **Benefits of Immutability**:

   - **Predictability**: Immutable data ensures that the state of an object remains consistent over time, which makes programs easier to understand and reason about.
   - **Concurrency**: Immutable data is inherently thread-safe, as multiple threads can access it concurrently without the risk of race conditions or data corruption.
   - **Debugging**: Immutability reduces the chances of bugs related to unexpected changes in data. Since data cannot be modified after creation, there are fewer opportunities for unintended side effects.
   - **Functional Programming**: Immutability is a core principle of functional programming, where functions operate on immutable data and produce new immutable data as output. This enables pure functions and facilitates reasoning about code.

5. **Copying vs. Mutation**: When working with immutable data, instead of mutating the existing data, you create new copies of the data with the desired modifications. This approach can be achieved efficiently through techniques like structural sharing, where only the parts of the data that change are copied, while the rest is shared between the old and new data structures.

Here's a simple example in JavaScript:

```javascript
const originalArray = [1, 2, 3];

// Modifying originalArray would be considered mutable
// Instead, create a new array with the desired modifications
const modifiedArray = [...originalArray, 4]; // [1, 2, 3, 4]

console.log(originalArray); // Output: [1, 2, 3]
console.log(modifiedArray); // Output: [1, 2, 3, 4]
```

In this example, `originalArray` remains unchanged, and a new array `modifiedArray` is created with the additional element `4`. This demonstrates the principle of immutability in action.

### Immutability using Immer

Immer is a library that simplifies the process of working with immutable data structures, particularly in the context of state management in JavaScript applications. It allows you to write code that appears to be mutating data while ensuring immutability behind the scenes. Immer achieves this by using a technique called "structural sharing," where only the parts of the data that change are copied, while the rest is shared between the old and new data structures.

Here's how you can use Immer to work with immutable data:

1. **Install Immer**:
   First, you need to install Immer in your project. You can do this via npm or yarn:

   ```bash
   npm install immer
   ```

   or

   ```bash
   yarn add immer
   ```

2. **Use `produce` Function**:
   Immer provides a `produce` function that allows you to create a draft of your data, make changes to it as if it were mutable, and then automatically produce an immutable copy of the data with the modifications applied.

   Here's an example:

   ```javascript
   import produce from "immer";

   // Original immutable data
   const originalState = {
     todos: [
       { id: 1, text: "Learn React", completed: false },
       { id: 2, text: "Write Code", completed: true },
     ],
   };

   // Produce a new immutable copy with modifications
   const newState = produce(originalState, (draft) => {
     draft.todos.push({ id: 3, text: "Read Documentation", completed: false });
     draft.todos[0].completed = true;
   });

   console.log(originalState); // Original state remains unchanged
   console.log(newState); // New state with modifications
   ```

   In this example, `produce` takes the original immutable `originalState` and a callback function that modifies a "draft" of the state as if it were mutable. Immer then automatically generates an immutable copy of the modified state and returns it as `newState`.

Using Immer, you can write code that resembles mutable updates, which can be easier to understand and maintain, while still benefiting from the advantages of immutability, such as predictability and concurrency safety.

# Redux Architecture

Redux is a predictable state container for JavaScript applications, primarily used with frameworks like React for building user interfaces. Redux follows a unidirectional data flow architecture, which helps manage the state of an application in a predictable and maintainable way. Here's an overview of the Redux architecture:

1. **Store**:

   - The central piece of Redux architecture is the store. It holds the entire state tree of the application.
   - The state in Redux is represented as a plain JavaScript object, often referred to as the "state tree."
   - You create the store using the `createStore` function provided by Redux. The store is typically created at the entry point of the application.

2. **Actions**:

   - Actions are plain JavaScript objects that represent events or payloads of information that describe changes in the application state.
   - Actions must have a `type` property that describes the type of action being performed.
   - You can create action creators, which are functions that return action objects. Action creators help encapsulate the process of creating actions.

3. **Reducers**:

   - Reducers are pure functions responsible for specifying how the application's state changes in response to actions.
   - A reducer takes the current state and an action as arguments and returns the new state based on the action.
   - Reducers should be pure functions with no side effects, meaning they should not mutate the state directly.

4. **Dispatch**:

   - Actions are dispatched to the store using the `dispatch` method. Dispatching an action triggers the state change process.
   - The store passes the current state and the action to the root reducer, which returns the new state.
   - The store notifies all subscribed components of the state change, triggering re-rendering as necessary.

5. **Selectors**:

   - Selectors are functions used to extract specific pieces of data from the state tree.
   - Selectors help in organizing and optimizing access to the state, especially for complex state structures.

6. **Middleware**:
   - Middleware provides a third-party extension point between dispatching an action and the moment it reaches the reducer.
   - Middleware can intercept actions, perform asynchronous tasks, or dispatch additional actions.

Overall, Redux provides a predictable and centralized way to manage application state, making it easier to develop and maintain complex applications, especially those with a large number of components and data interactions. By following a strict unidirectional data flow and maintaining immutability, Redux helps ensure application state changes are predictable and traceable, which aids in debugging and reasoning about the application's behavior.

# Important Redux functions

## createStore()

1. **Purpose of `createStore`**:

   - The **`createStore`** function is a core part of Redux. It creates a Redux store that holds the complete state tree of your application.
   - The store is where your application's state lives, and it's the single source of truth for your data.

2. **Usage**:

   - You can use `createStore` to manually configure your store by providing the following arguments:
     - **`reducer`**: A root reducer function that calculates the next state tree based on the current state and an action.
     - **`preloadedState`** (optional): The initial state of your application. You can use this to hydrate the state from the server or restore a serialized user session.
     - **`enhancer`** (optional): A store enhancer that adds third-party capabilities like middleware, time travel, or persistence. The most common enhancer is `applyMiddleware()` for middleware support.

3. **Example**:

   ```javascript
   import { createStore } from "redux";

   function todos(state = [], action) {
     switch (action.type) {
       case "ADD_TODO":
         return state.concat([action.text]);
       default:
         return state;
     }
   }

   const store = createStore(todos, ["Use Redux"]);

   store.dispatch({ type: "ADD_TODO", text: "Read the docs" });
   console.log(store.getState()); // Output: [ 'Use Redux', 'Read the docs' ]
   ```

4. **Deprecation and Migration**:

   - The original `createStore` method is **deprecated** in favor of modern Redux Toolkit APIs.
   - We recommend using **`configureStore`** from Redux Toolkit, which provides better defaults and simplifies store setup.
   - Additionally, consider using Redux Toolkit's **`createSlice`** method for writing reducer logic.

5. **Migration Options**:
   - To migrate your existing codebase:
     1. **Switch to Redux Toolkit**: Use `configureStore` and `createSlice`.
     2. **Do Nothing**: The deprecation is visual (a strikethrough), and it won't affect your code behavior.
     3. **Continue Using `createStore`**: It will still work indefinitely, but we encourage adopting modern Redux Toolkit practices.

Remember, Redux Toolkit provides a more streamlined and efficient way to set up your Redux store.

## configureStore()

1. **Purpose and Behavior**:

   - `configureStore` simplifies the process of creating a Redux store by providing good defaults for development.
   - It internally uses the low-level Redux core `createStore` method but wraps it to enhance the store setup experience.
   - The typical Redux store setup involves multiple configuration steps, such as combining slice reducers, adding middleware, and setting up DevTools. `configureStore` handles all of this for you.

2. **What `configureStore` Does**:

   - Combines your slice reducers into the root reducer function.
   - Automatically adds the thunk middleware (or other side effects middleware) using `applyMiddleware`.
   - In development mode, it also adds additional middleware to check for common mistakes (e.g., accidental state mutations).
   - Sets up the Redux DevTools Extension connection.
   - Calls `createStore` to create the Redux store with the specified configuration options.

3. **Improved API and Usage Patterns**:

   - `configureStore` accepts a single configuration object parameter with named fields:
     - **`reducer`**: A single reducer function or an object of slice reducers.
     - **`middleware`**: An array of Redux middleware (defaults to middleware returned by `getDefaultMiddleware`).
     - **`devTools`**: Enables or disables Redux DevTools integration.
     - **`preloadedState`**: The initial state (similar to `createStore`).

4. **TypeScript Type Inference**:
   - `configureStore` provides better TypeScript type inference compared to the original `createStore`.
   - This makes it easier to work with type-safe Redux code.
5. **Example**:

   ```ts
   // store.ts

   import { configureStore, getDefaultMiddleware } from "@reduxjs/toolkit";
   import rootReducer from "./reducers"; // Your root reducer
   import loggerMiddleware from "./middleware/logger"; // Custom logger middleware
   import monitorReducerEnhancer from "./enhancers/monitorReducer"; // Custom enhancer

   // Create the store
   const store = configureStore({
     reducer: rootReducer, // Your root reducer
     middleware: [
       ...getDefaultMiddleware(), // Default middleware (including thunk)
       loggerMiddleware, // Custom logger middleware
     ],
     enhancers: [monitorReducerEnhancer], // Custom enhancer
   });

   export default store;
   ```

In summary, if you're starting a new Redux project or migrating an existing one, consider using `configureStore` for a more streamlined and efficient store setup experience.

# Redux Implementation Steps:

1. Design the store
2. Define the actions
3. Create one or more reducers
4. Setup the store

## Design the Store

```ts
// src\interface\Store.type.ts

export type TStore = {
  bugs: TBug[];
  currentUser: TUser;
};

export type TBug = {
  id: number;
  description: string;
  resolved: boolean;
};

export type TUser = {
  id: number;
  username: string;
};
```

## Define the actions

- **Only related to bugs**

1. Add a bug
2. Mark as resolved
3. Delete a bug

Make sure that our payload of action has only minimial information to perform the action.

```ts
// src\interface\Store.type.ts

export type TStore = {
  bugs: TBug[];
  currentUser: TUser;
};

export type TBug = {
  id: number;
  description: string;
  resolved: boolean;
};

export type TUser = {
  id: number;
  username: string;
};

export type TActions =
  | { type: "bugAdded"; payload: Omit<TBug, "id" | "resolved"> }
  | { type: "bugRemoved"; payload: Pick<TBug, "id"> }
  | { type: "bugResolved"; payload: Pick<TBug, "id"> };
```

## Create Reducer

- **Without using Immer**

```ts
// src\reducers\StoreReducer.ts
import { TActions, TBug } from "../interface/Store.type";

let bugIdCounter = 0;
function generateBugId(): number {
  return ++bugIdCounter;
}

const initialState: TBug[] = []; // Initialize with an empty array

const storeReducer = (
  state: TBug[] = initialState,
  action: TActions
): TBug[] => {
  if (action.type === "bugAdded") {
    const newBug: TBug = {
      id: generateBugId(),
      description: action.payload.description,
      resolved: false,
    };
    return [...state, newBug];
  }

  if (action.type === "bugRemoved") {
    return state.filter((bug) => bug.id !== action.payload.id);
  }

  if (action.type === "bugResolved") {
    return state.map((bug) =>
      bug.id === action.payload.id ? { ...bug, resolved: true } : bug
    );
  }

  throw new Error(`Action not supported. Current state:${state}`);
};

export default storeReducer;
```

- **With Immer**

```ts
// src\reducers\StoreReducer.ts
import { produce } from "immer";
import { TActions, TBug } from "../interface/Store.type";

let bugIdCounter = 0;
function generateBugId(): number {
  return ++bugIdCounter;
}

const initialState: TBug[] = []; // Initialize with an empty array

const storeReducer = (
  state: TBug[] = initialState,
  action: TActions
): TBug[] => {
  if (action.type === "bugAdded") {
    const newBug: TBug = {
      id: generateBugId(),
      description: action.payload.description,
      resolved: false,
    };
    return [...state, newBug];
  }

  if (action.type === "bugRemoved") {
    return state.filter((bug) => bug.id !== action.payload.id);
  }

  if (action.type === "bugResolved") {
    return produce(state, (draft) => {
      draft.map((bug) =>
        bug.id === action.payload.id ? (bug.resolved = false) : bug
      );
    });
  }

  throw new Error(`Action not supported. Current state:${state}`);
};

export default storeReducer;
```
