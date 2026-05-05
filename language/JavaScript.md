# JavaScript

## Table of contents

- [JavaScript](#javascript)
  - [Table of contents](#table-of-contents)
  - [Introduction to JS](#introduction-to-js)
    - [Imperative Programming](#imperative-programming)
    - [Declarative Programming](#declarative-programming)
  - [Features of JavaScript](#features-of-javascript)
  - [Applications of JavaScript](#applications-of-javascript)
  - [Limitations of JavaScript](#limitations-of-javascript)
  - [Why JavaScript is known as a lightweight programming language ?](#why-javascript-is-known-as-a-lightweight-programming-language-)
  - [Is JavaScript Compiled or Interpreted or both ?](#is-javascript-compiled-or-interpreted-or-both-)
  - [Data Types](#data-types)
    - [Primitive Data Types:](#primitive-data-types)
    - [Object Data Types:](#object-data-types)
    - [Special Data Type:](#special-data-type)
- [Operators](#operators)
    - [Arithmetic Operators:](#arithmetic-operators)
    - [Assignment Operators:](#assignment-operators)
    - [Comparison Operators:](#comparison-operators)
    - [Logical Operators:](#logical-operators)
    - [Conditional (Ternary) Operator:](#conditional-ternary-operator)
    - [Rest Operator:](#rest-operator)
    - [Other Operators:](#other-operators)
  - [Loops](#loops)
    - [1. **For Loop:**](#1-for-loop)
    - [2. **While Loop:**](#2-while-loop)
    - [3. **Do-While Loop:**](#3-do-while-loop)
    - [4. **For...In Loop:**](#4-forin-loop)
    - [5. **For...Of Loop:**](#5-forof-loop)
    - [6. **forEach Method:**](#6-foreach-method)
    - [7. **Map Method:**](#7-map-method)
    - [8. **Filter Method:**](#8-filter-method)
    - [9. **Reduce method:**](#9-reduce-method)
  - [Truthy and Falsy](#truthy-and-falsy)
    - [Truthy Values:](#truthy-values)
    - [Falsy Values:](#falsy-values)
  - [Short Circuiting](#short-circuiting)
    - [Logical AND (`&&`) Short-Circuiting:](#logical-and--short-circuiting)
    - [Logical OR (`||`) Short-Circuiting:](#logical-or--short-circuiting)
  - [Strings](#strings)
    - [String Basics:](#string-basics)
    - [Primitive vs Object Strings:](#primitive-vs-object-strings)
      - [1. **Primitive Strings:**](#1-primitive-strings)
      - [2. **Object Strings:**](#2-object-strings)
  - [Arrays](#arrays)
    - [Arrays in JavaScript - Detailed Overview](#arrays-in-javascript---detailed-overview)
      - [1. **Creating Arrays:**](#1-creating-arrays)
      - [2. **Adding Elements:**](#2-adding-elements)
      - [3. **Removing Elements:**](#3-removing-elements)
      - [4. **Finding Elements:**](#4-finding-elements)
      - [5. **Emptying Arrays:**](#5-emptying-arrays)
      - [6. **Combining Arrays:**](#6-combining-arrays)
      - [7. **Slicing Arrays:**](#7-slicing-arrays)
      - [8. **Spread Operator for Concatenation:**](#8-spread-operator-for-concatenation)
      - [9. **Array Functions:**](#9-array-functions)
  - [Array Functions - Types](#array-functions---types)
  - [Functions in JavaScript - Detailed Overview](#functions-in-javascript---detailed-overview)
      - [1. **Function Description:**](#1-function-description)
      - [2. **Types of Functions:**](#2-types-of-functions)
      - [3. **Rest Operator and Arguments:**](#3-rest-operator-and-arguments)
      - [4. **Default Values for Parameters:**](#4-default-values-for-parameters)
      - [5. **Magic Function:**](#5-magic-function)
      - [6. **IIFE - Immediately Invoked Function Expression**:](#6-iife---immediately-invoked-function-expression)
  - [Hoisting](#hoisting)
    - [What actually gets hoisted](#what-actually-gets-hoisted)
    - [`var` — hoisted and initialized to `undefined`](#var--hoisted-and-initialized-to-undefined)
    - [`let` and `const` — Temporal Dead Zone (TDZ)](#let-and-const--hoisted-but-in-the-temporal-dead-zone-tdz)
    - [Function declarations — fully hoisted](#function-declarations--fully-hoisted-body-and-all)
    - [Function expressions and arrow functions](#function-expressions-and-arrow-functions--only-the-variable-is-hoisted)
    - [Class declarations — TDZ](#class-declarations--hoisted-but-in-tdz-like-let)
    - [Hoisting inside functions](#hoisting-inside-functions-var-is-function-scoped)
  - [Getters and Setters](#getters-and-setters)
    - [Getters:](#getters)
    - [Setters:](#setters)
    - [Using Getters and Setters in Classes:](#using-getters-and-setters-in-classes)
  - [Exception handling](#exception-handling)
    - [Try-Catch Statement:](#try-catch-statement)
    - [Multiple Catch Blocks:](#multiple-catch-blocks)
    - [Finally Block:](#finally-block)
    - [Throwing Exceptions:](#throwing-exceptions)
    - [Custom Exceptions:](#custom-exceptions)
  - [This - keyword](#this---keyword)
    - [What is `this`? (Layman explanation)](#what-is-this-layman-explanation)
    - [The 4 Binding Rules (in priority order)](#the-4-binding-rules-in-priority-order)
    - [`this` Inside Classes](#this-inside-classes)
    - [Lexical `this` — Arrow Functions](#lexical-this--arrow-functions)
    - [Quick Decision Guide](#quick-decision-guide-which-this-will-i-get)
    - [`this` Summary Table](#this-summary-table)
  - [Object-Oriented Programming (OOP) in JavaScript:](#object-oriented-programming-oop-in-javascript)
    - [Basics of OOP:](#basics-of-oop)
      - [1. **Objects:**](#1-objects)
      - [2. **Encapsulation:**](#2-encapsulation)
      - [3. **Abstraction:**](#3-abstraction)
      - [4. **Inheritance:**](#4-inheritance)
      - [5. **Polymorphism:**](#5-polymorphism)
    - [ES6 Classes:](#es6-classes)
    - [Prototypes:](#prototypes)
    - [Creating Objects: Object Literals, Factory Functions, and Constructor Functions:](#creating-objects-object-literals-factory-functions-and-constructor-functions)
    - [Factory Function vs Constructor Function:](#factory-function-vs-constructor-function)
    - [Private Properties and Methods:](#private-properties-and-methods)
    - [Intermediate Function Inheritance:](#intermediate-function-inheritance)
    - [Super Constructor Calling:](#super-constructor-calling)
    - [Closures](#closures)
      - [What is a Closure? (Layman explanation)](#what-is-a-closure-layman-explanation)
      - [How Closures Work — The Scope Chain](#how-closures-work--the-scope-chain)
      - [Practical Use Cases](#practical-use-cases)
      - [The Classic Loop Closure Bug](#the-classic-loop-closure-bug)
      - [Memory Considerations](#memory-considerations)
      - [Closure vs Global Variable](#closure-vs-global-variable--when-to-use-which)
    - [Instance, Prototype, and Static Members:](#instance-prototype-and-static-members)
    - [Method Overriding and Polymorphism:](#method-overriding-and-polymorphism)
    - [Object Property Attributes:](#object-property-attributes)
    - [Points to Remember:](#points-to-remember)
  - [ES6 Features](#es6-features)
  - [Mixins](#mixins)
    - [Mixins in JavaScript:](#mixins-in-javascript)
    - [Example:](#example)
    - [Benefits of Mixins:](#benefits-of-mixins)
    - [Applying Mixins:](#applying-mixins)
    - [Points to Remember:](#points-to-remember-1)
  - [Synchronous and Asynchronous Programming in JavaScript:](#synchronous-and-asynchronous-programming-in-javascript)
    - [Synchronous Programming:](#synchronous-programming)
    - [Asynchronous Programming:](#asynchronous-programming)
    - [Key Differences:](#key-differences)
  - [Callbacks, promises and Async/Await in detail](#callbacks-promises-and-asyncawait-in-detail)
    - [Callbacks in JavaScript:](#callbacks-in-javascript)
    - [Promises in JavaScript:](#promises-in-javascript)
    - [Async/Await in JavaScript:](#asyncawait-in-javascript)
    - [Key Points:](#key-points)
  - [Important resources](#important-resources)
    - [Practice Platforms:](#practice-platforms)
    - [Documentation:](#documentation)
    - [Frontend Code Archive:](#frontend-code-archive)
    - [Emmet Plugins Documentation:](#emmet-plugins-documentation)
  - [Iterating Collections](#iterating-collections)
    - [Array — value + index](#array--value--index)
    - [Map — key + value](#map--key--value)
    - [Object — key + value](#object--key--value)
    - [Set — value only (no index)](#set--value-only-no-index)
    - [Quick Comparison Table](#quick-comparison-table)
  - [Type Checking](#type-checking)
    - [The `typeof` Operator and Its Pitfalls](#the-typeof-operator-and-its-pitfalls)
    - [Checking for `null`](#checking-for-null)
    - [Checking for `undefined`](#checking-for-undefined)
    - [Checking for `NaN`](#checking-for-nan)
    - [Checking for an Array](#checking-for-an-array)
  - [Copying Objects and Arrays](#copying-objects-and-arrays)
    - [Shallow Copy](#shallow-copy)
    - [Deep Copy](#deep-copy)
    - [Method Comparison Table](#method-comparison-table)
    - [What structuredClone Cannot Copy](#what-structuredclone-cannot-copy)
    - [Checking for a plain Object](#checking-for-a-plain-object)
    - [Checking for a Map](#checking-for-a-map)
    - [Checking for a Set](#checking-for-a-set)
    - [Checking for a Function](#checking-for-a-function)
    - [Universal type tag via `Object.prototype.toString`](#universal-type-tag-via-objectprototypetostring)
    - [Type-checking utility reference table](#type-checking-utility-reference-table)

## Introduction to JS

JavaScript is a lightweight, cross-platform, single-threaded, and interpreted compiled programming language. It is also known as the scripting language for webpages. It is well-known for the development of web pages, and many non-browser environments also use it.

JavaScript is a weakly typed (_allows implicit type conversions on operation involves mismatched types, instead of throwing type errors_) and dynamically (_also called loosely typed; variables are not directly associated with any particular type_) typed language. JavaScript can be used for Client-side developments as well as Server-side developments. JavaScript is both an imperative and declarative type of language. JavaScript contains a standard library of objects, like Array, Date, and Math, and a core set of language elements like operators, control structures, and statements.

- **Client-side**: It supplies objects to control a browser and its Document Object Model (DOM). Like if client-side extensions allow an application to place elements on an HTML form and respond to user events such as mouse clicks, form input, and page navigation. Useful libraries for the client side are AngularJS, ReactJS, VueJS, and so many others.
- **Server-side**: It supplies objects relevant to running JavaScript on a server. For if the server-side extensions allow an application to communicate with a database, and provide continuity of information from one invocation to another of the application, or perform file manipulations on a server. The useful framework which is the most famous these days is node.js.
- **Imperative language** – Imperative programming is a programming paradigm that focuses on describing how a program should achieve a certain goal through a sequence of statements that change the program's state.
- **Declarative programming** – Declarative programming is a programming paradigm that focuses on describing what a program should accomplish, rather than explicitly detailing how to achieve it through a sequence of statements.

### Imperative Programming

Imperative programming is a programming paradigm that focuses on describing _how_ a program should achieve a certain goal through a sequence of statements that change the program's state. In imperative programming, developers explicitly write each step of the computation, detailing the exact actions the program should take to accomplish its task.

Key characteristics of imperative programming include:

1. **State Modification:** Imperative programs maintain mutable state, and instructions directly manipulate this state. Programmers specify the sequence of operations that transform the program's state from its initial state to the desired outcome.

2. **Control Flow:** Imperative programs use control structures such as loops (`for`, `while`) and conditional statements (`if`, `else`) to define the order of execution and handle branching logic based on certain conditions.

3. **Procedural Abstraction:** Imperative programs often organize code into procedures or functions, which encapsulate sequences of operations that perform specific tasks. These procedures can be called multiple times from different parts of the program.

4. **Focus on Details:** Imperative programming emphasizes the low-level details of computation, requiring developers to explicitly manage variables, loops, and conditional logic.

Example of imperative programming in JavaScript:

```javascript
// Imperative approach to summing the elements of an array
function sumArray(arr) {
  let sum = 0;
  for (let i = 0; i < arr.length; i++) {
    sum += arr[i];
  }
  return sum;
}

const numbers = [1, 2, 3, 4, 5];
console.log(sumArray(numbers)); // Output: 15
```

In this example, the `sumArray` function follows an imperative style by iterating over each element of the array and updating the `sum` variable to accumulate the total sum. Imperative programming allows developers precise control over program behavior, but it can lead to complex and verbose code, making it harder to reason about and maintain as programs grow in size and complexity.

### Declarative Programming

Declarative programming is a programming paradigm that focuses on describing *what* a program should accomplish, rather than explicitly detailing *how* to achieve it through a sequence of statements. In declarative programming, developers define the desired result or outcome, and the underlying implementation takes care of the necessary steps to achieve that result.

Key characteristics of declarative programming include:

1. **Focus on Descriptions:** Declarative programs emphasize the description of the problem and its solution rather than the step-by-step process of reaching the solution. Developers specify the desired end state or behavior, leaving the implementation details to the language or framework.

2. **Expression of Logic:** Declarative programs often use higher-level abstractions and specialized syntax to express complex logic concisely. This can include domain-specific languages (DSLs), configuration files, or specialized constructs provided by libraries or frameworks.

3. **Immutability:** Declarative programming favors immutable data structures and values, where changes to state create new instances rather than modifying existing ones. This promotes predictable behavior and makes it easier to reason about code.

4. **Delegated Control:** Declarative programming delegates control over program execution to underlying systems or libraries, which handle the implementation details and optimize the execution based on the defined logic.

Example of declarative programming in React (using JSX):

```jsx
// Declarative approach to rendering a list of items in React
function ListComponent({ items }) {
    return (
        <ul>
            {items.map(item => (
                <li key={item.id}>{item.name}</li>
            ))}
        </ul>
    );
}

const items = [
    { id: 1, name: 'Item 1' },
    { id: 2, name: 'Item 2' },
    { id: 3, name: 'Item 3' }
];

ReactDOM.render(<ListComponent items={items} />, document.getElementById('root'));
```

In this React example, the `ListComponent` function takes an array of items and returns a declarative description of how to render them as an unordered list. React handles the process of generating the actual DOM elements based on this description, abstracting away the imperative DOM manipulation typically required in traditional web development. Declarative programming in React makes it easier to create and maintain UI components by focusing on the desired UI structure rather than the individual DOM manipulation steps.

## Features of JavaScript

According to a recent survey conducted by Stack Overflow, JavaScript is the most popular language on earth.
With advances in browser technology and JavaScript having moved into the server with Node.js and other frameworks, JavaScript is capable of so much more. Here are a few things that we can do with JavaScript:

- JavaScript was created in the first place for DOM manipulation. Earlier websites were mostly static, after JS was created dynamic Web sites were made.
- Functions in JS are objects. They may have properties and methods just like other objects. They can be passed as arguments in other functions.
- Can handle date and time.
- Performs Form Validation although the forms are created using HTML.
- No compiler is needed.

## Applications of JavaScript

- **Web Development:** Adding interactivity and behavior to static sites JavaScript was invented to do this in 1995. By using AngularJS that can be achieved so easily.
- **Web Applications:** With technology, browsers have improved to the extent that a language was required to create robust web applications. When we explore a map in Google Maps then we only need to click and drag the mouse. All detailed view is just a click away, and this is possible only because of JavaScript. It uses Application Programming Interfaces(APIs) that provide extra power to the code. The Electron and React are helpful in this department.
- **Server Applications:** With the help of Node.js, JavaScript made its way from client to server and Node.js is the most powerful on the server side.
- **Games:** Not only in websites, but JavaScript also helps in creating games for leisure. The combination of JavaScript and HTML 5 makes JavaScript popular in game development as well. It provides the EaseJS library which provides solutions for working with rich graphics.
- **Smartwatches:** JavaScript is being used in all possible devices and applications. It provides a library PebbleJS which is used in smartwatch applications. This framework works for applications that require the Internet for their functioning.
- **Art:** Artists and designers can create whatever they want using JavaScript to draw on HTML 5 canvas, and make the sound more effective also can be used p5.js library.
- **Machine Learning:** This JavaScript ml5.js library can be used in web development by using machine learning.
- **Mobile Applications:** JavaScript can also be used to build an application for non-web contexts. The features and uses of JavaScript make it a powerful tool for creating mobile applications. This is a Framework for building web and mobile apps using JavaScript. Using React Native, we can build mobile applications for different operating systems. We do not require to write code for different systems. Write once use it anywhere!

## Limitations of JavaScript

- **Security risks:** JavaScript can be used to fetch data using AJAX or by manipulating tags that load data such as `<img>, <object>, <script>`. These attacks are called cross-site script attacks. They inject JS that is not part of the site into the visitor’s browser thus fetching the details.
- **Performance:** JavaScript does not provide the same level of performance as offered by many traditional languages as a complex program written in JavaScript would be comparatively slow. But as JavaScript is used to perform simple tasks in a browser, so performance is not considered a big restriction in its use.
- **Complexity:** To master a scripting language, programmers must have a thorough knowledge of all the programming concepts, core language objects, and client and server-side objects otherwise it would be difficult for them to write advanced scripts using JavaScript.
- **Weak error handling and type checking facilities:** It is a weakly typed language as there is no need to specify the data type of the variable. So wrong type checking is not performed by compile.

## Why JavaScript is known as a lightweight programming language ?

JavaScript is considered lightweight due to the fact that it has low CPU usage, is easy to implement, and has a minimalist syntax. Minimalist syntax as in, has no data types. Everything is treated here as an object. It is very easy to learn because of its syntax similar to C++ and Java.

A lightweight language does not consume much of your CPU’s resources. It doesn’t put excess strain on your CPU or RAM. JavaScript runs in the browser even though it has complex paradigms and logic which means it uses fewer resources than other languages. For example, NodeJs, a variation of JavaScript not only performs faster computations but also uses fewer resources than its counterparts such as Dart or Java.

Additionally, when compared with other programming languages, it has fewer in-built libraries or frameworks, contributing as another reason for it being lightweight. However, this brings a drawback in that we need to incorporate external libraries and frameworks.

## Is JavaScript Compiled or Interpreted or both ?

JavaScript is both compiled and interpreted. In the earlier versions of JavaScript, it used only the interpreter that executed code line by line and shows the result immediately. But with time the performance became an issue as interpretation is quite slow. Therefore, in the newer versions of JS, probably after the V8, the JIT compiler was also incorporated to optimize the execution and display the result more quickly. This JIT compiler generates a bytecode that is relatively easier to code. This bytecode is a set of highly optimized instructions.
The V8 engine initially uses an interpreter, to interpret the code. On further executions, the V8 engine finds patterns such as frequently executed functions, and frequently used variables, and compiles them to improve performance.

JavaScript is best known for web page development but it is also used in a variety of non-browser environments.

## Data Types

JavaScript has several data types that can be broadly categorized into two main groups: primitive data types and object data types. Here's an overview of the different types:

### Primitive Data Types:

1. **String:**

   - Represents textual data.
   - Example: `"Hello, World!"`.

2. **Number:**

   - Represents numeric data, both integers and floating-point numbers.
   - Example: `42` or `3.14`.

3. **Boolean:**

   - Represents either `true` or `false`.

4. **Undefined:**

   - Represents a variable that has been declared but not assigned a value.

5. **Null:**

   - Represents the absence of a value.

6. **Symbol:**
   - Introduced in ECMAScript 6 (ES6).
   - Represents a unique identifier.

### Object Data Types:

1. **Object:**

   - Represents a collection of key-value pairs.
   - Example: `{ name: "John", age: 25 }`.

2. **Array:**

   - Represents an ordered list of values.
   - Example: `[1, 2, 3, 4]`.

3. **Function:**
   - Represents a reusable block of code.
   - Example: `function add(a, b) { return a + b; }`.

### Special Data Type:

1. **BigInt:**
   - Introduced in ECMAScript 2020 (ES11).
   - Represents whole numbers larger than `2^53 - 1` (the maximum value representable with the Number primitive).

These data types play a crucial role in JavaScript, and understanding them is fundamental for effective programming in the language. Keep in mind that JavaScript is a loosely typed language, meaning variables can change types as the program runs.

# Operators

JavaScript supports a variety of operators that allow you to perform operations on variables and values. Here's an overview of some of the common operators in JavaScript:

### Arithmetic Operators:

1. **Addition (+):**

   - Adds two operands.
   - Example: `let sum = 5 + 3;`

2. **Subtraction (-):**

   - Subtracts the right operand from the left operand.
   - Example: `let difference = 7 - 2;`

3. **Multiplication (\*):**

   - Multiplies two operands.
   - Example: `let product = 4 * 6;`

4. **Division (/):**

   - Divides the left operand by the right operand.
   - Example: `let quotient = 8 / 2;`

5. **Modulus (%):**
   - Returns the remainder of the division of the left operand by the right operand.
   - Example: `let remainder = 9 % 4;`

### Assignment Operators:

6. **Assignment (=):**

   - Assigns a value to a variable.
   - Example: `let x = 10;`

7. **Increment (++) and Decrement (--):**

   - Increase or decrease the value of a variable by 1.
   - Example: `let count = 5; count++;`

8. **Compound Assignment (+=, -=, \*=, /=):**
   - Performs an operation and assigns the result to the variable.
   - Example: `let total = 20; total += 5;` (equivalent to `total = total + 5;`)

### Comparison Operators:

9. **Equal (==) and Strict Equal (===):**

   - Compares two values for equality.
   - `==` performs type coercion, while `===` requires both value and type to be the same.

10. **Not Equal (!=) and Strict Not Equal (!==):**

    - Compares two values for inequality.

11. **Greater Than (>), Less Than (<), Greater Than or Equal To (>=), Less Than or Equal To (<=):**
    - Compare numerical or string values.

### Logical Operators:

12. **Logical AND (&&), Logical OR (||), Logical NOT (!):**
    - Perform logical operations on Boolean values.

### Conditional (Ternary) Operator:

13. **Ternary Operator (a ? b : c):**
    - A shorthand for an if-else statement.

### Rest Operator:

14. **Rest Operator (...):**

    - Collects the remaining arguments into an array.
    - Example:
      ```javascript
      function sum(...numbers) {
        return numbers.reduce((acc, num) => acc + num, 0);
      }
      console.log(sum(1, 2, 3, 4)); // Output: 10
      ```
    - Another Example:

    ```javascript
    function exampleFunction(firstArg, secondArg, ...restArgs) {
      console.log("First argument:", firstArg);
      console.log("Second argument:", secondArg);
      console.log("Rest of the arguments:", restArgs);
    }

    exampleFunction(1, 2, 3, 4, 5);
    ```

The rest operator (`...`) is particularly useful in functions when you want to handle a variable number of arguments. It allows you to represent an indefinite number of arguments as an array.

### Other Operators:

15. **Typeof Operator:**

    - Returns a string indicating the type of a variable.

16. **Instanceof Operator:**
    - Checks if an object is an instance of a particular class or constructor.

These are some of the fundamental operators in JavaScript. Understanding how to use them is essential for effective programming in the language.

## Loops

JavaScript supports various types of loops that allow you to repeatedly execute a block of code. Here are the main types of loops in JavaScript:

### 1. **For Loop:**

- The `for` loop repeats a block of code a specified number of times.

```javascript
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

### 2. **While Loop:**

- The `while` loop repeats a block of code as long as a specified condition is true.

```javascript
let i = 0;
while (i < 5) {
  console.log(i);
  i++;
}
```

### 3. **Do-While Loop:**

- Similar to the `while` loop, but it always executes the block of code at least once, even if the condition is initially false.

```javascript
let i = 0;
do {
  console.log(i);
  i++;
} while (i < 5);
```

### 4. **For...In Loop:**

- Used to iterate over the properties of an object.

```javascript
const person = { name: "John", age: 30 };
for (let key in person) {
  console.log(key, person[key]);
}
```

### 5. **For...Of Loop:**

- Introduced in ECMAScript 6 (ES6), it iterates over iterable objects (arrays, strings, etc.).

```javascript
const numbers = [1, 2, 3, 4, 5];
for (let number of numbers) {
  console.log(number);
}
```

### 6. **forEach Method:**

- A method available for arrays that executes a provided function once for each array element.

```javascript
const numbers = [1, 2, 3, 4, 5];
numbers.forEach(function (number) {
  console.log(number);
});
```

### 7. **Map Method:**

- Another method for arrays that creates a new array by applying a function to each element of the original array.

```javascript
const numbers = [1, 2, 3, 4, 5];
const squaredNumbers = numbers.map(function (number) {
  return number * number;
});
```

### 8. **Filter Method:**

- Filters elements in an array based on a provided function.

```javascript
const numbers = [1, 2, 3, 4, 5];
const evenNumbers = numbers.filter(function (number) {
  return number % 2 === 0;
});
```

### 9. **Reduce method:**

- Iterates over each element in an array and accumulates a single result, often by applying a provided function to each element.

```javascript
const numbers = [1, 2, 3, 4, 5];
const sum = numbers.reduce(function (accumulator, currentValue) {
  return accumulator + currentValue;
}, 0);
```

These loops provide different ways to iterate over data structures or execute code repeatedly based on specific conditions. Choose the appropriate loop based on your use case.

## Truthy and Falsy

In JavaScript, values are broadly classified as either "truthy" or "falsy" based on their inherent boolean interpretation. When a non-boolean value is used in a boolean context, it is treated as either true or false.

Here's a quick overview of truthy and falsy values in JavaScript:

### Truthy Values:

1. **Non-empty Strings:**

   - Any string with at least one character is truthy.

   ```javascript
   if ("Hello") {
     // This block will be executed
   }
   ```

2. **Numbers:**

   - Any non-zero number (positive or negative) is truthy.

   ```javascript
   if (42) {
     // This block will be executed
   }
   ```

3. **Objects:**

   - Any object (including arrays and functions) is truthy.

   ```javascript
   if ({ key: "value" }) {
     // This block will be executed
   }
   ```

4. **Arrays:**

   - Any array, even if it's empty, is truthy.

   ```javascript
   if ([]) {
     // This block will be executed
   }
   ```

5. **Truthy Expressions:**

   - Expressions that evaluate to true are considered truthy.

   ```javascript
   if (5 > 2) {
     // This block will be executed
   }
   ```

### Falsy Values:

1. **Empty String:**

   - An empty string (`""`) is falsy.

   ```javascript
   if ("") {
     // This block will NOT be executed
   }
   ```

2. **Zero:**

   - The number `0` is falsy.

   ```javascript
   if (0) {
     // This block will NOT be executed
   }
   ```

3. **NaN (Not a Number):**

   - NaN is falsy.

   ```javascript
   if (isNaN("Not a Number")) {
     // This block will NOT be executed
   }
   ```

4. **null:**

   - The value `null` is falsy.

   ```javascript
   if (null) {
     // This block will NOT be executed
   }
   ```

5. **undefined:**

   - The value `undefined` is falsy.

   ```javascript
   if (undefined) {
     // This block will NOT be executed
   }
   ```

6. **false:**

   - The boolean value `false` is, of course, falsy.

   ```javascript
   if (false) {
     // This block will NOT be executed
   }
   ```

Understanding truthy and falsy values is crucial in JavaScript, especially when working with conditional statements, such as `if` statements and ternary operators. It allows you to write concise and expressive code.

## Short Circuiting

In JavaScript, short-circuiting is a behavior in logical expressions where the evaluation of the second operand is skipped if the result can be determined by the first operand alone. This is based on the logical operators `&&` (logical AND) and `||` (logical OR). Here's how short-circuiting works:

### Logical AND (`&&`) Short-Circuiting:

The `&&` operator returns the first falsy operand, or the last operand if all are truthy. If the first operand is falsy, the second operand is not evaluated because the result is already determined.

```javascript
// Example 1: Short-circuiting due to falsy first operand
let result = false && someFunction(); // someFunction() is not called

// Example 2: Short-circuiting due to truthy first operand
let name = "John";
let greeting = name && "Hello, " + name; // "Hello, John"
```

### Logical OR (`||`) Short-Circuiting:

The `||` operator returns the first truthy operand, or the last operand if all are falsy. If the first operand is truthy, the second operand is not evaluated because the result is already determined.

```javascript
// Example 1: Short-circuiting due to truthy first operand
let result = true || someFunction(); // someFunction() is not called

// Example 2: Short-circuiting due to falsy first operand
let defaultName = "Guest";
let enteredName = "";
let userName = enteredName || defaultName; // "Guest"
```

Short-circuiting is commonly used for concise conditional expressions and can be leveraged to write more readable and efficient code. However, it's essential to understand its implications, especially when using expressions with side effects (such as function calls), as the skipped evaluation may affect the program's behavior.

## Strings

Strings in JavaScript are used to represent and manipulate sequences of characters. They can be created using string literals or the `String` constructor. Here's an overview of strings in JavaScript, including the difference between primitive and object strings:

### String Basics:

1. **String Literals:**

   - Strings can be created using single or double quotes.

   ```javascript
   let singleQuotes = "Hello, World!";
   let doubleQuotes = "Hello, World!";
   ```

2. **String Concatenation:**

   - Strings can be concatenated using the `+` operator.

   ```javascript
   let firstName = "John";
   let lastName = "Doe";
   let fullName = firstName + " " + lastName; // "John Doe"
   ```

3. **String Length:**

   - The `length` property gives the number of characters in a string.

   ```javascript
   let message = "Hello, World!";
   console.log(message.length); // 13
   ```

4. **Accessing Characters:**

   - Characters in a string can be accessed using square brackets.

   ```javascript
   let str = "JavaScript";
   console.log(str[0]); // "J"
   ```

5. **String Methods:**

   - JavaScript provides various built-in methods for string manipulation, such as `toUpperCase()`, `toLowerCase()`, `charAt()`, `substring()`, `split()`, and more.

   ```javascript
   let text = "Hello, World!";
   console.log(text.toUpperCase()); // "HELLO, WORLD!"
   ```

### Primitive vs Object Strings:

#### 1. **Primitive Strings:**

- Strings are primitive data types in JavaScript.
- Immutable: Once a string is created, its value cannot be changed.
- Comparisons are done by value.

```javascript
let str1 = "Hello";
let str2 = "Hello";

console.log(str1 === str2); // true
```

#### 2. **Object Strings:**

- Strings can also be created using the `String` constructor, creating a string object.
- Objects are mutable, and methods can be added to their prototype.
- Comparisons are done by reference.

```javascript
let strObj1 = new String("Hello");
let strObj2 = new String("Hello");

console.log(strObj1 === strObj2); // false
```

While using string literals (primitive) is more common and efficient, string objects (created using the `String` constructor) are rarely used due to their complexity and potential pitfalls. The primitive form is generally recommended for most use cases.

Understanding the distinction between primitive and object strings is important for effective string manipulation in JavaScript. In most scenarios, using primitive strings is preferred for simplicity and performance reasons.

## Arrays

Arrays in JavaScript are versatile and widely used data structures that allow you to store and organize multiple values. Here are some key characteristics and features of arrays in JS:

1. **Ordered Collection:**

   - Arrays maintain the order of elements, meaning the sequence in which elements are added is preserved.

2. **Zero-Based Indexing:**

   - Elements in an array are accessed using a zero-based index, where the first element has an index of 0, the second has an index of 1, and so on.

   ```javascript
   let fruits = ["Apple", "Banana", "Orange"];
   console.log(fruits[0]); // Outputs: 'Apple'
   ```

3. **Dynamic Size:**

   - Arrays in JavaScript are dynamic, meaning their size can change during runtime. You can add or remove elements as needed.

   ```javascript
   let numbers = [1, 2, 3];
   numbers.push(4); // Add an element
   numbers.pop(); // Remove the last element
   ```

4. **Heterogeneous Elements:**

   - Arrays can contain elements of different data types, including numbers, strings, objects, or even other arrays.

   ```javascript
   let mixedArray = [1, "Hello", { key: "value" }, [1, 2, 3]];
   ```

5. **Array Methods:**

   - JavaScript provides a variety of built-in methods for array manipulation, including `push()`, `pop()`, `shift()`, `unshift()`, `splice()`, `slice()`, `concat()`, and more.

   ```javascript
   let numbers = [1, 2, 3];
   numbers.push(4); // Adds 4 to the end
   numbers.pop(); // Removes the last element
   numbers.shift(); // Removes the first element
   numbers.unshift(0); // Adds 0 to the beginning
   numbers.splice(1, 0, 1.5); // Adds 1.5 at index 1
   ```

6. **Iterating Over Arrays:**

   - Arrays can be iterated using loops (e.g., `for`, `while`) or array methods like `forEach()`.

   ```javascript
   let fruits = ["Apple", "Banana", "Orange"];
   for (let i = 0; i < fruits.length; i++) {
     console.log(fruits[i]);
   }

   // or using forEach
   fruits.forEach((fruit) => console.log(fruit));
   ```

7. **Length Property:**

   - The `length` property returns the number of elements in an array.

   ```javascript
   let numbers = [1, 2, 3, 4, 5];
   console.log(numbers.length); // Outputs: 5
   ```

Understanding these characteristics is crucial for effectively working with arrays in JavaScript, as they play a fundamental role in many programming tasks.

### Arrays in JavaScript - Detailed Overview

#### 1. **Creating Arrays:**

- Arrays are created using square brackets `[]` and can hold various data types.

```javascript
let numbers = [1, 2, 3, 4, 5];
let fruits = ["Apple", "Banana", "Orange"];
```

#### 2. **Adding Elements:**

- Elements can be added at the end, beginning, or middle of an array.

```javascript
// Add at the end
numbers.push(6);

// Add at the beginning
numbers.unshift(0);

// Add in the middle
numbers.splice(2, 0, 1.5);
```

#### 3. **Removing Elements:**

- Elements can be removed from the end, beginning, or middle of an array.

```javascript
// Remove from the end
let removedElement = numbers.pop();

// Remove from the beginning
let removedElement = numbers.shift();

// Remove from the middle
let removedElements = numbers.splice(2, 2);
```

#### 4. **Finding Elements:**

- Finding elements in arrays for both primitive and reference types.

```javascript
// For primitives
let index = numbers.indexOf(3);

// For reference types
let index = fruits.findIndex((fruit) => fruit === "Banana");
```

#### 5. **Emptying Arrays:**

- Different ways to empty an array.

```javascript
// Reassign
numbers = [];

// Set length to 0
numbers.length = 0;

// Using splice
numbers.splice(0, numbers.length);

// Using pop in a loop
while (numbers.length > 0) {
  numbers.pop();
}
```

#### 6. **Combining Arrays:**

- Combining arrays using methods like `concat` or the spread operator.

```javascript
let combinedArray = numbers.concat(fruits);

// Using the spread operator
let combinedArray = [...numbers, ...fruits];
```

#### 7. **Slicing Arrays:**

- Creating a new array by extracting a portion of an existing array.

```javascript
let slicedArray = numbers.slice(1, 4); // Elements at index 1, 2, 3
```

#### 8. **Spread Operator for Concatenation:**

- Using the spread operator to concatenate arrays.

```javascript
let newArray = [...numbers, 6, 7, 8];
```

#### 9. **Array Functions:**

- Array methods like `every`, `some`, `filter`, `forEach`, `map`, and `reduce`.

```javascript
// Every: checks if all elements pass a condition
let allPositive = numbers.every((num) => num > 0);

// Some: checks if at least one element passes a condition
let hasNegative = numbers.some((num) => num < 0);

// Filter: creates a new array with elements that pass a condition
let positiveNumbers = numbers.filter((num) => num > 0);

// forEach: iterates over each element in an array
numbers.forEach((num) => console.log(num));

// Map: creates a new array by applying a function to each element
let squaredNumbers = numbers.map((num) => num * num);

// Reduce: accumulates a single result by applying a function to each element
let sum = numbers.reduce((acc, num) => acc + num, 0);
```

Understanding these array operations is essential for effective JavaScript programming, as arrays are a fundamental data structure in the language.

## Array Functions - Types

JavaScript array functions can be broadly categorized into two types based on their behavior: functions that modify the existing array in-place and functions that create a new array without modifying the original one. Here's a breakdown:

1. Functions that modify the existing array (in-place):

   - `push()`: Adds one or more elements to the end of an array and returns the new length of the array.
   - `pop()`: Removes the last element from an array and returns that element.
   - `shift()`: Removes the first element from an array and returns that element.
   - `unshift()`: Adds one or more elements to the beginning of an array and returns the new length of the array.
   - `splice()`: Adds or removes elements from an array.
   - `reverse()`: Reverses the order of the elements in an array.
   - `sort()`: Sorts the elements of an array in place and returns the sorted array.

2. Functions that create a new array without modifying the original one:
   - `concat()`: Returns a new array comprised of the original array joined with other array(s) and/or value(s).
   - `slice()`: Returns a shallow copy of a portion of an array into a new array.
   - `filter()`: Creates a new array with all elements that pass the test implemented by the provided function.
   - `map()`: Creates a new array populated with the results of calling a provided function on every element in the calling array.
   - `reduce()`: Executes a reducer function on each element of the array, resulting in a single output value.
   - `reduceRight()`: Same as `reduce()`, but works from right to left.
   - `forEach()`: Executes a provided function once for each array element.
   - `every()`: Tests whether all elements in the array pass the test implemented by the provided function.
   - `some()`: Tests whether at least one element in the array passes the test implemented by the provided function.
   - `indexOf()`: Returns the first index at which a given element can be found in the array, or -1 if it is not present.
   - `lastIndexOf()`: Returns the last index at which a given element can be found in the array, or -1 if it is not present.

These are the commonly used array functions in JavaScript along with their behavior regarding modifying the existing array or creating a new array.

## Functions in JavaScript - Detailed Overview

#### 1. **Function Description:**

- A function is a reusable block of code that performs a specific task. It helps organize code, promote reusability, and improve maintainability.

#### 2. **Types of Functions:**

- **Declaration (Named Function):**

  ```javascript
  function greet(name) {
    return `Hello, ${name}!`;
  }
  ```

  - A function declared with the `function` keyword. Can be used before its declaration (hoisted).

- **Expression (Anonymous Function):**

  ```javascript
  let greet = function (name) {
    return `Hello, ${name}!`;
  };
  ```

  - A function assigned to a variable. Must be defined before it is used.

- **Arrow Function:**
  ```javascript
  let greet = (name) => `Hello, ${name}!`;
  ```
  - A concise form of function expression introduced in ECMAScript 6 (ES6).

#### 3. **Rest Operator and Arguments:**

- The rest operator (`...`) allows a function to accept a variable number of arguments as an array.

```javascript
function sum(...numbers) {
  return numbers.reduce((acc, num) => acc + num, 0);
}

console.log(sum(1, 2, 3, 4)); // Outputs: 10
```

- The `arguments` object is an array-like object available in function scope, containing all passed arguments.

```javascript
function greetAll() {
  for (let i = 0; i < arguments.length; i++) {
    console.log(`Hello, ${arguments[i]}!`);
  }
}

greetAll("John", "Jane", "Doe");
```

#### 4. **Default Values for Parameters:**

- Parameters can have default values, ensuring they are assigned a value if none is provided during the function call.

```javascript
function greet(name = "Guest") {
  return `Hello, ${name}!`;
}

console.log(greet()); // Outputs: "Hello, Guest!"
```

#### 5. **Magic Function:**

- A simple example demonstrating the power of functions.

```javascript
function magicFunction(base) {
  return function (exponent) {
    return Math.pow(base, exponent);
  };
}

let square = magicFunction(2);
console.log(square(3)); // Outputs: 8
```

- The `magicFunction` returns another function that calculates the power of a base number. It showcases the concept of closures.

Understanding these aspects of functions in JavaScript is crucial for effective programming, as functions play a central role in structuring code and implementing logic.

#### 6. **IIFE - Immediately Invoked Function Expression**:

An **Immediately Invoked Function Expression (IIFE)** is a unique JavaScript construct that combines the power of function expressions, closures, and immediate execution. Let's break it down:

1. **Definition**:
   - An IIFE is a function expression that is defined and invoked immediately after its declaration.
   - It's pronounced "iffy," which stands for "Immediately Invoked Function Expression."

2. **Syntax**:
   - The basic syntax for an IIFE looks like this:

    ```javascript
    (function () {
        // Your code here
    })();
    ```

   - Alternatively, you can use an arrow function for concise syntax:

    ```javascript
    (() => {
        // Your code here
    })();
    ```

3. **Why Use IIFEs?**:
   - **Encapsulation**: IIFEs create a new scope, allowing you to encapsulate variables and functions.
   - **Avoid Global Pollution**: Variables declared inside an IIFE are not added to the global scope, preventing unintended global pollution.
   - **Module-Like Behavior**: Before ES modules, IIFEs were commonly used for modular programming in JavaScript.

4. **Example**:
   Suppose you want to create a counter that increments each time the page loads. You can use an IIFE like this:

    ```javascript
    (function () {
        let count = 0;

        function incrementCounter() {
            count++;
            console.log(`Counter: ${count}`);
        }

        incrementCounter();
    })();
    ```

   The `count` variable and `incrementCounter` function are scoped within the IIFE, keeping them private and preventing conflicts with other code.

Remember, IIFEs are less common nowadays due to the widespread adoption of ES modules, but understanding their behavior is still valuable! 


## Hoisting

Hoisting is JavaScript's behaviour of moving **declarations** (not initializations) to the top of their scope during the compilation phase. Different declarations hoist differently.

### What actually gets hoisted

| Declaration | Hoisted? | Initialized to | Usable before declaration? |
|---|---|---|---|
| `var` | ✅ Yes | `undefined` | ⚠️ Yes, but value is `undefined` |
| `let` | ✅ Yes | ❌ Uninitialized (TDZ) | ❌ `ReferenceError` |
| `const` | ✅ Yes | ❌ Uninitialized (TDZ) | ❌ `ReferenceError` |
| Function declaration | ✅ Yes | Full function body | ✅ Yes — fully usable |
| Function expression (`var fn = function`) | ✅ var part only | `undefined` | ❌ `TypeError` (not a function yet) |
| Arrow function (`const fn = () =>`) | ✅ const part only | TDZ | ❌ `ReferenceError` |
| Class declaration | ✅ Yes | TDZ | ❌ `ReferenceError` |

---

### `var` — hoisted and initialized to `undefined`

```js
console.log(x); // undefined (NOT ReferenceError)
var x = 5;
console.log(x); // 5

// What the engine actually sees:
var x;          // declaration hoisted to top of scope
console.log(x); // undefined
x = 5;          // assignment stays in place
console.log(x); // 5
```

### `let` and `const` — hoisted but in the Temporal Dead Zone (TDZ)

The variable exists in scope from the top of the block but **cannot be read or written** until the declaration line is reached. Accessing it before that throws a `ReferenceError`.

```js
console.log(y); // ❌ ReferenceError: Cannot access 'y' before initialization
let y = 10;

console.log(PI); // ❌ ReferenceError
const PI = 3.14;

// TDZ in action inside a block
{
  // TDZ for 'z' starts here
  console.log(z); // ❌ ReferenceError
  let z = 99;     // TDZ ends here
}
```

### Function declarations — fully hoisted (body and all)

```js
console.log(greet('Alice')); // ✅ 'Hello, Alice!' — works before declaration

function greet(name) {
  return `Hello, ${name}!`;
}
```

### Function expressions and arrow functions — only the variable is hoisted

```js
// var function expression — variable hoisted as undefined
console.log(sayHi);   // undefined — no error, but not callable yet
console.log(sayHi()); // ❌ TypeError: sayHi is not a function
var sayHi = function() { return 'hi'; };

// const arrow function — TDZ applies
console.log(sayBye); // ❌ ReferenceError
const sayBye = () => 'bye';
```

### Class declarations — hoisted but in TDZ (like `let`)

```js
const p = new Person('Alice'); // ❌ ReferenceError: Cannot access 'Person' before initialization

class Person {
  constructor(name) { this.name = name; }
}
```

### Hoisting inside functions (`var` is function-scoped)

`var` hoists to the **top of the nearest function**, not to the top of a block. `let`/`const` are block-scoped.

```js
function example() {
  console.log(a); // undefined — var is function-scoped, hoisted to top of fn
  if (true) {
    var a = 1;    // var ignores the if-block boundary
  }
  console.log(a); // 1
}

function example2() {
  console.log(b); // ❌ ReferenceError — let is block-scoped, stays inside the if-block
  if (true) {
    let b = 2;
  }
}
```

### Key takeaway

> Prefer `const`/`let` over `var`. The TDZ converts subtle silent-`undefined` bugs into explicit `ReferenceError`s that are caught immediately — making hoisting a feature, not a footgun.

## Getters and Setters

In JavaScript, getters and setters are special methods that allow you to control the access and modification of object properties. They provide a way to define the behavior of reading and writing values to an object's properties.

### Getters:

A getter is a method that gets the value of a specific property. It is defined using the `get` keyword followed by the property name. Getters are used to compute a value on the fly or perform some actions when retrieving a property.

```javascript
const person = {
  firstName: "John",
  lastName: "Doe",
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
};

console.log(person.fullName); // Outputs: "John Doe"
```

In this example, `fullName` is a getter that concatenates `firstName` and `lastName` when accessed.

### Setters:

A setter is a method that sets the value of a specific property. It is defined using the `set` keyword followed by the property name. Setters are used to perform validation or execute some actions when assigning a value to a property.

```javascript
const person = {
  _age: 25, // Convention to indicate it's a private variable
  set age(newAge) {
    if (newAge > 0 && newAge < 150) {
      this._age = newAge;
    } else {
      console.log("Invalid age value");
    }
  },
  get age() {
    return this._age;
  },
};

person.age = 30;
console.log(person.age); // Outputs: 30

person.age = 200; // Outputs: Invalid age value
```

In this example, the `age` property is controlled by a setter that performs validation to ensure the age is within a valid range.

### Using Getters and Setters in Classes:

Getters and setters are often used in classes to encapsulate the internal state of an object. Here's an example:

```javascript
class Circle {
  constructor(radius) {
    this._radius = radius;
  }

  get diameter() {
    return this._radius * 2;
  }

  set diameter(diameter) {
    this._radius = diameter / 2;
  }

  get area() {
    return Math.PI * this._radius ** 2;
  }
}

const myCircle = new Circle(5);
console.log(myCircle.diameter); // Outputs: 10
console.log(myCircle.area); // Outputs: 78.54

myCircle.diameter = 12;
console.log(myCircle.radius); // Outputs: 6
```

In this example, the `Circle` class uses getters and setters to control access to the `diameter` property and calculate the `area` property.

## Exception handling

Exception handling in JavaScript is done using `try`, `catch`, `finally`, and optionally `throw` statements. This mechanism allows you to handle errors gracefully, preventing them from crashing your program.

### Try-Catch Statement:

The `try` block contains the code that might throw an exception. If an exception occurs, it is caught by the `catch` block, which contains the code to handle the exception.

```javascript
try {
  // Code that might throw an exception
  let result = someUndefinedVariable + 5;
} catch (error) {
  // Code to handle the exception
  console.error("An error occurred:", error.message);
}
```

In this example, if `someUndefinedVariable` is not defined, a `ReferenceError` will be thrown and caught by the `catch` block.

### Multiple Catch Blocks:

You can have multiple `catch` blocks to handle different types of exceptions.

```javascript
try {
  // Code that might throw an exception
  let result = someUndefinedVariable + 5;
} catch (referenceError) {
  console.error('ReferenceError:', referenceError.message);
} catch (error) {
  console.error('An error occurred:', error.message);
}
```

### Finally Block:

The `finally` block, if present, is executed regardless of whether an exception is thrown or caught. It's often used for cleanup operations.

```javascript
try {
  // Code that might throw an exception
  let result = someUndefinedVariable + 5;
} catch (error) {
  // Code to handle the exception
  console.error("An error occurred:", error.message);
} finally {
  // Code that always executes, regardless of exceptions
  console.log("Finally block executed");
}
```

### Throwing Exceptions:

You can use the `throw` statement to manually throw an exception. This is useful when you want to signal an error condition.

```javascript
function divide(x, y) {
  if (y === 0) {
    throw new Error("Cannot divide by zero");
  }
  return x / y;
}

try {
  let result = divide(10, 0);
  console.log(result);
} catch (error) {
  console.error("Error:", error.message);
}
```

In this example, calling `divide(10, 0)` will throw an error, and it will be caught in the `catch` block.

### Custom Exceptions:

You can create custom exception objects by extending the `Error` class or using plain objects.

```javascript
class CustomError extends Error {
  constructor(message) {
    super(message);
    this.name = "CustomError";
  }
}

try {
  throw new CustomError("This is a custom error");
} catch (error) {
  console.error("Error:", error.message);
}
```

This is a basic overview of exception handling in JavaScript. Proper error handling is crucial for robust and maintainable code, helping to identify and address issues during development and runtime.

## This - keyword

### What is `this`? (Layman explanation)

Think of `this` as the answer to: **"Who called me?"**

When a function runs, JavaScript automatically creates a variable called `this` inside it. `this` points to the **object that invoked the function** — it's the context the function is operating in.

The trick: **`this` is not decided when the function is defined. It's decided at the moment the function is called.**

```js
// Same function, different `this` depending on WHO calls it
function introduce() {
  console.log(`I am ${this.name}`);
}

const alice = { name: 'Alice', introduce };
const bob   = { name: 'Bob',   introduce };

alice.introduce(); // 'I am Alice'  — this = alice
bob.introduce();   // 'I am Bob'    — this = bob
introduce();       // 'I am undefined' — this = window/global (no caller object)
```

---

### The 4 Binding Rules (in priority order)

#### Rule 1 — Default binding (lowest priority)

When a function is called as a plain function (no object, no `new`, no explicit binding), `this` is the global object (`window` in browsers, `global` in Node.js). In strict mode it is `undefined`.

```js
function show() {
  console.log(this);
}

show();              // window (browser) / global (Node)

'use strict';
function showStrict() {
  console.log(this); // undefined — strict mode prevents global default
}
showStrict();
```

#### Rule 2 — Implicit binding

When a function is called as a method (`obj.fn()`), `this` is the object to the **left of the dot**.

```js
const user = {
  name: 'Alice',
  greet() {
    console.log(`Hi, I am ${this.name}`);
  },
};

user.greet(); // 'Hi, I am Alice'  — this = user

// Only the immediate left-of-dot object matters
const admin = { name: 'Admin', greet: user.greet };
admin.greet(); // 'Hi, I am Admin' — this = admin, not user
```

**⚠️ The classic "lost `this`" bug — implicit binding lost:**

```js
const user = {
  name: 'Alice',
  greet() { console.log(this.name); },
};

// Assigning the method to a variable detaches it from user
const fn = user.greet;
fn(); // undefined — this is now global/undefined, not user!

// Common real-world trap with callbacks:
setTimeout(user.greet, 1000); // undefined — setTimeout detaches the method
```

#### Rule 3 — Explicit binding (`call`, `apply`, `bind`)

You manually tell JavaScript what `this` should be.

```js
function greet(greeting, punctuation) {
  console.log(`${greeting}, ${this.name}${punctuation}`);
}

const person = { name: 'Alice' };

// call — invoke immediately, args passed individually
greet.call(person, 'Hello', '!');   // 'Hello, Alice!'

// apply — invoke immediately, args passed as an array
greet.apply(person, ['Hi', '?']);   // 'Hi, Alice?'

// bind — returns a NEW function with this permanently bound (doesn't invoke)
const aliceGreet = greet.bind(person, 'Hey');
aliceGreet('.');   // 'Hey, Alice.'
aliceGreet('!');   // 'Hey, Alice!'  — reusable

// Fix the lost-this bug with bind:
setTimeout(user.greet.bind(user), 1000); // ✅ 'Alice' — this is locked to user
```

| | Invokes immediately? | Args | Returns |
|---|---|---|---|
| `call(ctx, a, b)` | ✅ Yes | Individual | Result of fn |
| `apply(ctx, [a,b])` | ✅ Yes | Array | Result of fn |
| `bind(ctx, a)` | ❌ No | Individual (partial) | New bound function |

#### Rule 4 — `new` binding (highest priority)

When a function is called with `new`, JavaScript creates a brand-new empty object, sets `this` to it, and returns it automatically.

```js
function Person(name) {
  // 'this' is the new object being constructed
  this.name = name;
  this.greet = function() { console.log(`Hi, I am ${this.name}`); };
}

const alice = new Person('Alice');
alice.greet(); // 'Hi, I am Alice'
```

---

### `this` Inside Classes

In a class, `this` refers to the instance (works like `new` binding).

```js
class Counter {
  count = 0;

  increment() {
    this.count++;
    console.log(this.count);
  }
}

const c = new Counter();
c.increment(); // 1 ✅

// ⚠️  Detaching a class method loses this — same trap as objects
const inc = c.increment;
inc(); // TypeError or NaN — this is undefined in strict mode (classes always strict)

// Fix 1: bind in constructor
class Counter2 {
  count = 0;
  constructor() {
    this.increment = this.increment.bind(this);
  }
  increment() { this.count++; }
}

// Fix 2: class field arrow function (most common in React)
class Counter3 {
  count = 0;
  increment = () => { this.count++; }; // arrow — lexical this, always bound
}
```

---

### Lexical `this` — Arrow Functions

#### What is "lexical this"? (Layman explanation)

Regular functions ask **"who called me?"** every time they run.

Arrow functions ask **"where was I written?"** — they permanently borrow `this` from the surrounding code at the time they were defined. This is called **lexical** `this` (lexical = "determined by where it appears in the source code").

> **Analogy:** A regular function is like a freelancer who adapts to each new client (the caller). An arrow function is like a full-time employee who always belongs to the company they joined (the enclosing scope).

```js
const team = {
  name: 'Frontend',

  // Regular function — this is dynamic (depends on caller)
  regularGreet: function() {
    console.log(this.name); // 'Frontend' — called as team.regularGreet()
  },

  // Arrow function — this is lexical (captured from surrounding scope)
  arrowGreet: () => {
    console.log(this.name); // undefined — 'this' here is the module/global scope,
                            // NOT the team object, because the arrow was defined
                            // at module level (outside any function)
  },
};
```

#### Where arrow functions shine — callbacks

```js
const timer = {
  seconds: 0,

  start() {
    // 'this' here is timer (called as timer.start())
    setInterval(function() {
      // ❌ Regular function — 'this' is window/undefined, NOT timer
      this.seconds++;
    }, 1000);
  },

  startFixed() {
    setInterval(() => {
      // ✅ Arrow function — 'this' is lexically captured from startFixed()
      //    which has this = timer
      this.seconds++;
      console.log(this.seconds);
    }, 1000);
  },
};

timer.startFixed(); // works correctly
```

#### Arrow functions CANNOT be used as methods (when you need dynamic `this`)

```js
const person = {
  name: 'Alice',

  // ❌ Arrow as object method — this is NOT person, it's the outer scope
  greet: () => {
    console.log(this.name); // undefined
  },

  // ✅ Regular function as method — this IS person
  greetCorrect() {
    console.log(this.name); // 'Alice'
  },
};
```

#### `this` binding rules for arrow functions

Arrow functions:
- **Cannot be bound** with `call`, `apply`, or `bind` (the `this` arg is ignored)
- **Cannot be used with `new`** (they have no own `this` to construct with)
- **Cannot be used as generators** (`function*`)

```js
const arrow = () => console.log(this);

arrow.call({ name: 'Alice' }); // ignores the context — still logs outer this
new arrow();                   // TypeError: arrow is not a constructor
```

---

### Quick Decision Guide: Which `this` will I get?

```
How is the function called?
│
├─ new fn()              → brand-new object (new binding)
├─ fn.call(ctx) / bind   → explicitly provided ctx (explicit binding)
├─ obj.fn()              → obj (implicit binding)
├─ fn() (plain call)
│   ├─ strict mode       → undefined (default)
│   └─ non-strict        → global object (window/global)
└─ Arrow function        → this from enclosing lexical scope (always)
```

---

### `this` Summary Table

| Call style | `this` value | Notes |
|---|---|---|
| `obj.method()` | `obj` | Implicit binding |
| `fn()` (non-strict) | `window` / `global` | Default binding |
| `fn()` (strict) | `undefined` | Default binding |
| `fn.call(ctx)` | `ctx` | Explicit |
| `fn.apply(ctx, args)` | `ctx` | Explicit |
| `fn.bind(ctx)()` | `ctx` | Explicit, new fn returned |
| `new Fn()` | new object | New binding |
| Arrow `() => {}` | Inherited from enclosing scope | Lexical — permanent |
| Class method | instance | Implicit (like `new`) |

## Object-Oriented Programming (OOP) in JavaScript:

Object-Oriented Programming is a paradigm that enables structuring code around objects, which represent real-world entities and encapsulate data and behavior. JavaScript, despite being a prototype-based language, supports OOP principles through various mechanisms.

### Basics of OOP:

#### 1. **Objects:**

- Objects in OOP are instances of classes or prototypes, encapsulating properties (data) and methods (behavior).

#### 2. **Encapsulation:**

- Encapsulation bundles data and methods that operate on the data within a single unit, preventing external access and modification.

#### 3. **Abstraction:**

- Abstraction simplifies complex systems by representing the essential features and ignoring unnecessary details.

#### 4. **Inheritance:**

- Inheritance allows a new class or prototype to inherit properties and methods from an existing class or prototype.

#### 5. **Polymorphism:**

- Polymorphism enables a single interface to represent different types or classes, allowing for flexible and modular code.

### ES6 Classes:

Introduced in ECMAScript 2015 (ES6), classes in JavaScript provide a more structured syntax for implementing OOP concepts.

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  makeSound() {
    console.log("Some generic sound");
  }
}

class Dog extends Animal {
  makeSound() {
    console.log("Bark!");
  }
}
```

### Prototypes:

JavaScript is a prototype-based language, and objects can inherit properties and methods from other objects through their prototype chain.

```javascript
function Animal(name) {
  this.name = name;
}

Animal.prototype.makeSound = function () {
  console.log("Some generic sound");
};

function Dog(name) {
  Animal.call(this, name);
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.makeSound = function () {
  console.log("Bark!");
};
```

### Creating Objects: Object Literals, Factory Functions, and Constructor Functions:

- **Object Literals:**

  ```javascript
  const person = {
    name: "John",
    sayHello: function () {
      console.log(`Hello, ${this.name}!`);
    },
  };
  ```

- **Factory Function:**

  ```javascript
  function createPerson(name) {
    return {
      name,
      sayHello() {
        console.log(`Hello, ${this.name}!`);
      },
    };
  }

  const john = createPerson("John");
  ```

- **Constructor Function:**

  ```javascript
  function Person(name) {
    this.name = name;
    this.sayHello = function () {
      console.log(`Hello, ${this.name}!`);
    };
  }

  const john = new Person("John");
  ```

### Factory Function vs Constructor Function:

| Feature                | Factory Function                              | Constructor Function                                                 |
| ---------------------- | --------------------------------------------- | -------------------------------------------------------------------- |
| **Return Object**      | Returns an object directly.                   | Does not need explicit return (creates instance automatically).      |
| **`this` Binding**     | `this` is bound to the new object.            | `this` is bound to the new object.                                   |
| **Usage**              | Commonly used for creating instances.         | Used when creating instances with the `new` keyword.                 |
| **Multiple Instances** | Requires a new object for each instance.      | Requires a new object for each instance.                             |
| **Memory Consumption** | More memory-efficient for multiple instances. | Each instance has its own method, potentially less memory-efficient. |

### Private Properties and Methods:

Encapsulation and private variables can be achieved using closures.

```javascript
function createPerson(name) {
  let privateAge = 30;

  return {
    name,
    getAge() {
      return privateAge;
    },
    setAge(newAge) {
      if (newAge > 0 && newAge < 150) {
        privateAge = newAge;
      } else {
        console.log("Invalid age value");
      }
    },
  };
}

const john = createPerson("John");
```

### Intermediate Function Inheritance:

An intermediate function acts as a step in the inheritance chain.

```javascript
function extend(Child, Parent) {
  function Temp() {}
  Temp.prototype = Parent.prototype;
  Child.prototype = new Temp();
  Child.prototype.constructor = Child;
}

function Animal(name) {
  this.name = name;
}

function Dog(name, breed) {
  Animal.call(this, name);
  this.breed = breed;
}

extend(Dog, Animal);
```

### Super Constructor Calling:

Used in classes to call the constructor of the parent class.

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Call the constructor of the parent class
    this.breed = breed;
  }
}
```

### Closures

#### What is a Closure? (Layman explanation)

Imagine you write a letter, seal it in an envelope, and mail it. The letter was written in your room, so it carries the context of your room — even after you've left. The letter "closes over" your room's information.

That's a closure. A function carries the variables from where it was **born** (its outer scope), even after that outer scope has finished running and is "gone".

> **Backpack analogy:** Every function carries an invisible backpack. When the function is created, it stuffs into that backpack all the variables available in its surrounding scope. Wherever the function travels and whenever it gets called later, it can always unzip the backpack and use those variables.

```js
function makeCounter() {
  let count = 0;            // lives in the backpack of the returned function

  return function() {       // this inner function IS the closure
    count++;
    return count;
  };
}

const counter = makeCounter();  // outerFunction has finished — count is "gone"...
counter(); // 1   ...but NOT really gone — the closure still has it in its backpack
counter(); // 2
counter(); // 3

const counter2 = makeCounter(); // fresh closure — its OWN private count
counter2(); // 1  — independent from counter
```

---

#### How Closures Work — The Scope Chain

When a function is defined, JavaScript captures the entire **scope chain** at that moment — not just a snapshot of the values, but **live references** to the variables.

```js
function outer() {
  let x = 10;

  function inner() {
    console.log(x);  // inner can see x from outer's scope
  }

  x = 99;            // change x AFTER defining inner
  inner();           // logs 99, not 10 — it's a live reference, not a copy!
}

outer(); // 99
```

---

#### Practical Use Cases

**1. Data privacy / encapsulation**

JavaScript has no built-in `private` keyword for plain objects. Closures are the classic way to hide data.

```js
function createBankAccount(initialBalance) {
  let balance = initialBalance;  // private — nobody outside can touch this directly

  return {
    deposit(amount) {
      balance += amount;
      console.log(`Deposited ${amount}. Balance: ${balance}`);
    },
    withdraw(amount) {
      if (amount > balance) { console.log('Insufficient funds'); return; }
      balance -= amount;
      console.log(`Withdrew ${amount}. Balance: ${balance}`);
    },
    getBalance() { return balance; },
  };
}

const account = createBankAccount(100);
account.deposit(50);    // Balance: 150
account.withdraw(30);   // Balance: 120
console.log(account.balance); // undefined — balance is truly private!
```

**2. Factory functions — creating specialised versions of functions**

```js
function makeMultiplier(factor) {
  return (number) => number * factor;  // factor is closed over
}

const double = makeMultiplier(2);
const triple = makeMultiplier(3);
const tenX   = makeMultiplier(10);

double(5);  // 10
triple(5);  // 15
tenX(5);    // 50
```

**3. Memoization — caching expensive results**

```js
function memoize(fn) {
  const cache = {};          // closed over — persists between calls

  return function(...args) {
    const key = JSON.stringify(args);
    if (key in cache) {
      console.log('cache hit');
      return cache[key];
    }
    cache[key] = fn(...args);
    return cache[key];
  };
}

const expensiveSquare = memoize((n) => {
  console.log('computing...');
  return n * n;
});

expensiveSquare(4);  // 'computing...' → 16
expensiveSquare(4);  // 'cache hit' → 16 (no recompute)
expensiveSquare(5);  // 'computing...' → 25
```

**4. Event handlers / callbacks that remember context**

```js
function setupButton(buttonId, message) {
  const btn = document.getElementById(buttonId);

  btn.addEventListener('click', function() {
    // message is closed over — still accessible on every click
    alert(message);
  });
}

setupButton('btn1', 'Hello from button 1');
setupButton('btn2', 'Hello from button 2');
// Each button's handler remembers its own message independently
```

**5. Partial application / currying**

```js
function add(a) {
  return function(b) {    // closes over 'a'
    return a + b;
  };
}

const add5  = add(5);
const add10 = add(10);

add5(3);   // 8
add10(3);  // 13
```

---

#### The Classic Loop Closure Bug

This is one of the most famous JavaScript interview questions:

```js
// ❌ BUG — all handlers print 3, not 0, 1, 2
for (var i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);     // all three callbacks close over the SAME 'i'
  }, 1000);
}
// Output after 1s: 3, 3, 3
```

**Why?** `var` is function-scoped, so there is only **one** `i` variable for all three iterations. By the time the callbacks run (1s later), the loop has finished and `i` is `3`. All closures share that one `i`.

**Fix 1 — use `let` (block-scoped, creates a new `i` per iteration):**

```js
// ✅ let creates a fresh binding per loop iteration
for (let i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);   // each callback has its OWN 'i'
  }, 1000);
}
// Output: 0, 1, 2 ✅
```

**Fix 2 — IIFE to create a new scope per iteration (pre-ES6 workaround):**

```js
for (var i = 0; i < 3; i++) {
  (function(j) {        // IIFE captures current value of i as 'j'
    setTimeout(function() {
      console.log(j);   // j is a new variable per iteration
    }, 1000);
  })(i);
}
// Output: 0, 1, 2 ✅
```

**Fix 3 — `bind` to snapshot the value:**

```js
for (var i = 0; i < 3; i++) {
  setTimeout(console.log.bind(null, i), 1000);
}
// Output: 0, 1, 2 ✅
```

---

#### Memory Considerations

Closures keep the outer scope's variables alive as long as the closure itself is alive. This is useful by design, but can cause **memory leaks** if closures are unintentionally kept around.

```js
function heavyOperation() {
  const largeData = new Array(1_000_000).fill('x');  // 1 million strings

  return function() {
    // Even if we only use one value, largeData is kept alive because
    // the closure holds a reference to the entire outer scope
    return largeData[0];
  };
}

const fn = heavyOperation();
// largeData stays in memory as long as 'fn' is referenced

fn = null;  // ✅ release the closure → largeData can now be garbage-collected
```

---

#### Closure vs Global Variable — When to use which

| | Closure variable | Global variable |
|---|---|---|
| Accessibility | Only to the closure and its inner functions | Anywhere in the program |
| Lifetime | Alive as long as the closure is alive | Alive for the whole program |
| Privacy | ✅ Truly private | ❌ Anyone can read/overwrite |
| Multiple independent instances | ✅ Each closure gets its own copy | ❌ Shared — one copy for all |
| Risk | Memory leak if closure held too long | Naming collisions, accidental mutation |

> **Rule of thumb:** If you need persistent state that is private to a function or module, use a closure. Only reach for globals when data truly needs to be shared app-wide.

---

#### Quick Mental Model

```
DEFINE a function inside another function
         ↓
The inner function gets a BACKPACK
         ↓
The backpack contains LIVE REFERENCES to outer variables
         ↓
Even when the outer function FINISHES and returns,
the backpack stays attached to the inner function
         ↓
Every time the inner function is CALLED,
it can open the backpack and use / update those variables
```

### Instance, Prototype, and Static Members:

- **Instance Members:** Properties or methods attached to an instance of a class or object.
- **Prototype Members:** Properties or methods attached to the prototype of a class or object.
- **Static Members:** Properties or

methods attached to the class itself, not its instances.

### Method Overriding and Polymorphism:

- **Method Overriding:** When a subclass provides a specific implementation for a method already defined in its superclass.

  ```javascript
  class Animal {
    makeSound() {
      console.log("Some generic sound");
    }
  }

  class Dog extends Animal {
    makeSound() {
      console.log("Bark!");
    }
  }
  ```

- **Polymorphism:** The ability to use a single interface to represent different types or classes.

  ```javascript
  function makeAnimalSound(animal) {
    animal.makeSound();
  }

  const genericAnimal = new Animal();
  const barkingDog = new Dog();

  makeAnimalSound(genericAnimal); // Outputs: "Some generic sound"
  makeAnimalSound(barkingDog); // Outputs: "Bark!"
  ```

### Object Property Attributes:

Every attribute of an object in JS will have properties like `writable`, `enumerable`, `configurable`, etc.

- **Writable:**

  - Determines if the property's value can be changed.

- **Enumerable:**

  - Determines if the property will be returned in a `for...in` loop or `Object.keys()`.

- **Configurable:**
  - Determines if the property can be deleted, and if its attributes (other than value and writable) can be changed.

### Points to Remember:

1. **Avoid Inheritance Unless Necessary:**

   - Inheritance can lead to tight coupling and make the code harder to maintain.

2. **Limit Inheritance Levels:**

   - Limit the depth of the inheritance hierarchy to avoid complex and hard-to-understand code.

3. **Avoid Inheritance Hierarchies:**

   - Favor composition (using mixins or other techniques) over deep inheritance hierarchies.

4. **Favor Composition Over Inheritance:**
   - Composition allows for more flexible and reusable code by combining simple components.

Understanding these concepts, including object property attributes, is essential for writing clean, maintainable, and efficient object-oriented JavaScript code.

## ES6 Features

ECMAScript 2015 (also known as ES6) introduced numerous features and enhancements to the JavaScript language. Some of the key features introduced in ES6 include:

1. **Arrow Functions:**
   Arrow functions provide a concise syntax for writing function expressions. They have a more intuitive syntax and lexically bind the `this` value.

   ```javascript
   // ES5 function
   function add(a, b) {
     return a + b;
   }

   // ES6 arrow function
   const add = (a, b) => a + b;
   ```

2. **Let and Const Declarations:**
   `let` and `const` provide block-scoped variable declarations, replacing the traditional `var` keyword.

   ```javascript
   // ES5 variable declaration
   var x = 10;

   // ES6 let and const declarations
   let y = 20;
   const PI = 3.14;
   ```

3. **Template Literals:**
   Template literals allow for easy string interpolation and multiline strings using backticks (`).

   ```javascript
   // ES5 string concatenation
   var name = "John";
   var greeting = "Hello, " + name + "!";

   // ES6 template literals
   const name = "John";
   const greeting = `Hello, ${name}!`;
   ```

4. **Destructuring Assignment:**
   Destructuring assignment provides a concise way to extract values from arrays or objects and assign them to variables.

   ```javascript
   // ES6 array destructuring
   const [x, y] = [1, 2];

   // ES6 object destructuring
   const { name, age } = { name: "John", age: 30 };
   ```

5. **Default Parameters:**
   Default parameters allow function parameters to have default values if no argument is provided.

   ```javascript
   // ES6 default parameters
   function greet(name = "World") {
     return `Hello, ${name}!`;
   }
   ```

6. **Classes:**
   ES6 introduced the `class` syntax for creating classes, providing a more familiar and intuitive way to implement object-oriented programming in JavaScript.

   ```javascript
   // ES6 class syntax
   class Person {
     constructor(name) {
       this.name = name;
     }

     greet() {
       return `Hello, ${this.name}!`;
     }
   }
   ```

7. **Modules:**
   ES6 modules allow for better code organization and encapsulation by providing a standardized syntax for importing and exporting modules.

   ```javascript
   // ES6 module export
   export function add(a, b) {
     return a + b;
   }

   // ES6 module import
   import { add } from "./math";
   ```

These are just some of the many features introduced in ECMAScript 2015 (ES6). ES6 laid the foundation for modern JavaScript development and significantly improved the language's expressiveness, readability, and maintainability. Subsequent ECMAScript versions have continued to build upon these features and introduce new ones to further enhance JavaScript development.

## Mixins

Mixins in JavaScript are a way to share methods among multiple objects or to create reusable pieces of code that can be mixed into different classes or objects. They provide a means of achieving composition, allowing you to combine the functionality of multiple objects without the need for traditional inheritance. Here's an explanation along with an example:

### Mixins in JavaScript:

Mixins are a way to encapsulate and share functionality between objects in a flexible and reusable manner. Unlike classical inheritance, which involves a hierarchy of classes, mixins allow you to combine functionalities from multiple sources without creating a strict parent-child relationship.

### Example:

Let's create a simple mixin for logging functionality and apply it to two different objects.

```javascript
// Define a Logger mixin
const LoggerMixin = {
  log(message) {
    console.log(`[Log]: ${message}`);
  },
};

// Create objects with the mixin applied
const user = {
  name: "John",
};

const admin = {
  name: "Admin",
};

// Mixin the Logger functionality into the objects
Object.assign(user, LoggerMixin);
Object.assign(admin, LoggerMixin);

// Now both objects have the log method
user.log("User logged in");
admin.log("Admin action performed");
```

In this example, `LoggerMixin` is a simple object with a `log` method. The `Object.assign()` method is used to mix this functionality into the `user` and `admin` objects. As a result, both objects can now use the `log` method.

### Benefits of Mixins:

1. **Code Reusability:**

   - Mixins allow you to reuse specific functionalities across different objects, promoting a more modular and maintainable codebase.

2. **Flexibility:**

   - Objects can mix in functionalities dynamically, providing more flexibility than traditional inheritance.

3. **Avoiding Inheritance Complexity:**

   - Mixins help avoid the complexities of deep inheritance hierarchies by promoting a flat structure.

4. **Encapsulation:**
   - Mixins encapsulate specific functionalities, making it easier to reason about and modify individual pieces of code.

### Applying Mixins:

Mixins can be applied in various ways, including manual copying of methods, using helper functions, or leveraging libraries/frameworks designed for this purpose. The example above manually applies the mixin, but more advanced scenarios might benefit from utility functions or dedicated libraries.

```javascript
function applyMixin(target, mixin) {
  Object.assign(target, mixin);
}

// Applying LoggerMixin to an object
applyMixin(user, LoggerMixin);
```

### Points to Remember:

- Mixins are a powerful tool for achieving code reuse and composability in JavaScript.
- They provide a way to share functionality without creating a rigid class hierarchy.
- Mixins can be applied manually or using utility functions, depending on the specific use case and preference.

## Synchronous and Asynchronous Programming in JavaScript:

JavaScript is a single-threaded, non-blocking, asynchronous language. Understanding the concepts of synchronous and asynchronous programming is crucial for developing efficient and responsive applications. Let's delve into each concept:

### Synchronous Programming:

1. **Definition:**

   - In synchronous programming, each operation or task is executed one after the other, in a sequential order. The program waits for each task to complete before moving on to the next one.

2. **Execution Flow:**

   - The flow of execution is predictable and follows a top-to-bottom order.
   - Blocking operations can cause the entire program to pause until the operation completes.

3. **Example:**
   ```javascript
   console.log("Start");
   console.log("Task 1");
   console.log("Task 2");
   console.log("End");
   ```
   Output:
   ```
   Start
   Task 1
   Task 2
   End
   ```

### Asynchronous Programming:

1. **Definition:**

   - In asynchronous programming, tasks are initiated, but the program doesn't wait for their completion. Instead, it continues with the next tasks. When an asynchronous task completes, a callback or some mechanism is used to handle the result.

2. **Execution Flow:**

   - Non-blocking operations allow the program to continue executing other tasks while waiting for certain operations to complete.
   - Callbacks, Promises, and async/await are common mechanisms for handling asynchronous operations.

3. **Example using Callbacks:**

   ```javascript
   console.log("Start");

   setTimeout(function () {
     console.log("Async Task");
   }, 1000);

   console.log("End");
   ```

   Output:

   ```
   Start
   End
   Async Task
   ```

4. **Example using Promises:**

   ```javascript
   console.log("Start");

   const asyncTask = new Promise((resolve) => {
     setTimeout(() => {
       resolve("Async Task");
     }, 1000);
   });

   asyncTask.then((result) => {
     console.log(result);
     console.log("End");
   });
   ```

   Output:

   ```
   Start
   End
   Async Task
   ```

5. **Example using Async/Await:**

   ```javascript
   async function example() {
     console.log("Start");

     function asyncTask() {
       return new Promise((resolve) => {
         setTimeout(() => {
           resolve("Async Task");
         }, 1000);
       });
     }

     const result = await asyncTask();
     console.log(result);
     console.log("End");
   }

   example();
   ```

   Output:

   ```
   Start
   Async Task
   End
   ```

### Key Differences:

- **Execution Model:**

  - Synchronous: Executes tasks sequentially, blocking the program until each task completes.
  - Asynchronous: Initiates tasks and continues with other operations without waiting for completion.

- **Concurrency:**

  - Synchronous: Single-threaded, one task at a time.
  - Asynchronous: Supports concurrency by allowing multiple tasks to run concurrently without blocking.

- **Handling Complexity:**

  - Synchronous: Easier to reason about and debug due to a predictable execution flow.
  - Asynchronous: Requires careful handling of callbacks or other mechanisms to manage asynchronous tasks and avoid callback hell.

- **Examples of Asynchronous Operations:**
  - Network requests (AJAX)
  - File I/O operations
  - setTimeout/setInterval functions
  - Promises, async/await in modern JavaScript

Understanding when to use synchronous or asynchronous programming is crucial for building responsive and scalable applications, especially in scenarios involving I/O operations or dealing with external services.

## Callbacks, promises and Async/Await in detail

### Callbacks in JavaScript:

1. **Definition:**

   - A callback is a function passed as an argument to another function. It allows that function to be executed once the operation it is associated with is complete.

2. **Example:**

   ```javascript
   function fetchData(callback) {
     // Simulating an asynchronous operation (e.g., fetching data)
     setTimeout(() => {
       const data = "Some data";
       callback(data);
     }, 1000);
   }

   // Using the callback
   fetchData((result) => {
     console.log(result);
   });
   ```

3. **Common Use Cases:**
   - Handling asynchronous operations (e.g., AJAX requests, file I/O).
   - Event handling in the browser.
   - Managing control flow in asynchronous code.

---

### Promises in JavaScript:

1. **Definition:**

   - A Promise is an object that represents the eventual completion or failure of an asynchronous operation, and its resulting value.

2. **States:**

   - **Pending:** The initial state; the promise is neither fulfilled nor rejected.
   - **Fulfilled:** The operation completed successfully, and the promise has a resulting value.
   - **Rejected:** The operation failed, and the promise has a reason for the failure.

3. **Example:**

   ```javascript
   function fetchData() {
     return new Promise((resolve, reject) => {
       // Simulating an asynchronous operation
       setTimeout(() => {
         const success = true;

         if (success) {
           resolve("Some data");
         } else {
           reject("Error fetching data");
         }
       }, 1000);
     });
   }

   // Using the Promise
   fetchData()
     .then((result) => {
       console.log(result);
     })
     .catch((error) => {
       console.error(error);
     });
   ```

4. **Common Use Cases:**
   - Handling asynchronous operations with more structured and readable code.
   - Chaining multiple asynchronous operations.
   - Replacing callback-based patterns.

---

### Async/Await in JavaScript:

1. **Definition:**

   - Async/Await is a syntax for writing asynchronous code that looks and behaves like synchronous code. It is built on top of Promises and provides a more concise and readable way to work with asynchronous operations.

2. **Example:**

   ```javascript
   async function fetchData() {
     return new Promise((resolve, reject) => {
       // Simulating an asynchronous operation
       setTimeout(() => {
         const success = true;

         if (success) {
           resolve("Some data");
         } else {
           reject("Error fetching data");
         }
       }, 1000);
     });
   }

   // Using Async/Await
   async function fetchDataWrapper() {
     try {
       const result = await fetchData();
       console.log(result);
     } catch (error) {
       console.error(error);
     }
   }

   fetchDataWrapper();
   ```

3. **Common Use Cases:**
   - Simplifying asynchronous code and making it more readable.
   - Handling errors using try/catch.
   - Awaiting multiple asynchronous operations sequentially.

### Key Points:

- **Callbacks:**

  - Pros: Simple and widely supported.
  - Cons: Callback hell, difficult error handling, can lead to less readable and maintainable code.

- **Promises:**

  - Pros: Improved readability, better error handling with `.catch()`, and chaining of asynchronous operations.
  - Cons: Still involves chaining, and handling parallel operations might be complex.

- **Async/Await:**
  - Pros: Clean and concise syntax, easier error handling with try/catch, and sequential execution of asynchronous code.
  - Cons: Requires a good understanding of Promises, may not be supported in older environments.

Choosing between callbacks, promises, or async/await depends on the specific use case, project requirements, and the level of support needed for older environments. Async/Await has become the preferred choice in modern JavaScript development due to its readability and ease of use.

## Important resources

### Practice Platforms:

1. **CodePen:**
   - [CodePen](https://codepen.io/) is a popular online platform for practicing HTML, CSS, and JavaScript. It provides a real-time coding environment and allows you to see the results instantly.

### Documentation:

2. **Mozilla Developer Network (MDN):**

   - [MDN Web Docs](https://developer.mozilla.org/) is a comprehensive resource for web developers, offering detailed documentation on HTML, CSS, JavaScript, and other web technologies.

3. **W3Schools:**

   - [W3Schools](https://www.w3schools.com/) is a beginner-friendly resource that provides tutorials and references on various web development technologies, including HTML, CSS, and JavaScript.

4. **DevDocs:**
   - [DevDocs](https://devdocs.io/) is an online documentation platform that aggregates documentation for various programming languages and web technologies in one place.

### Frontend Code Archive:

5. **Web Archive (web.archive.org):**
   - [Web Archive](https://web.archive.org/) is a digital archive of the World Wide Web. It allows you to access and view web pages as they appeared at different points in the past.

### Emmet Plugins Documentation:

6. **Emmet Cheat Sheet:**
   - [Emmet Cheat Sheet](https://docs.emmet.io/cheat-sheet/) provides quick reference documentation for Emmet, a toolkit for web developers that greatly improves HTML and CSS workflow.

These resources cover a wide range of topics and cater to different learning styles. Whether you're looking to practice coding, explore documentation, or access archived versions of websites, you've got a solid set of tools at your disposal. Don't hesitate to explore additional resources as you continue your web development journey.


---

## let and const

| Keyword | Scope | Re-assignable | Re-declarable | Hoisted |
|---|---|---|---|---|
| `var` | Function | ✅ | ✅ | ✅ (undefined) |
| `let` | Block | ✅ | ❌ | ✅ (TDZ) |
| `const` | Block | ❌ | ❌ | ✅ (TDZ) |

```js
// Temporal Dead Zone (TDZ) — accessing before declaration throws ReferenceError
console.log(x); // ReferenceError
let x = 5;

const obj = { a: 1 };
obj.a = 2;        // ✅ — property mutation is fine
obj = { a: 3 };   // ❌ TypeError — reassignment blocked
```

> **Rule:** Use `const` by default. Use `let` only when the value needs to change. Never use `var`.

---

## Arrow Functions

```js
// Traditional
function add(a, b) { return a + b; }

// Arrow
const add = (a, b) => a + b;

// Multi-line body
const fetchUser = async (id) => {
  const res = await fetch(`/api/users/${id}`);
  return res.json();
};

// Returning object literal — wrap in parentheses
const makeUser = (name) => ({ name, active: true });
```

**Key difference — `this` binding:**
```js
// Arrow functions do NOT have their own `this`
function Timer() {
  this.count = 0;
  setInterval(() => {
    this.count++;   // `this` = Timer instance ✅
  }, 1000);
}

// Regular function would need .bind(this) or self = this
```

---

## Template Literals

```js
const name = 'Alice';
const role = 'Developer';

// String interpolation
console.log(`Hello, ${name}! You are a ${role}.`);

// Multi-line strings
const html = `
  <div class="card">
    <h2>${name}</h2>
    <p>${role}</p>
  </div>
`;

// Tagged templates
function highlight(strings, ...values) {
  return strings.reduce((result, str, i) =>
    `${result}${str}${values[i] ? `<b>${values[i]}</b>` : ''}`, '');
}
const output = highlight`Hello ${name}, you are a ${role}!`;
// "Hello <b>Alice</b>, you are a <b>Developer</b>!"
```

---

## Destructuring

**Array destructuring:**
```js
const [first, second, ...rest] = [1, 2, 3, 4, 5];
// first=1, second=2, rest=[3,4,5]

// Skipping elements
const [,, third] = [1, 2, 3];   // third=3

// Default values
const [a = 10, b = 20] = [5];    // a=5, b=20

// Swapping variables
let x = 1, y = 2;
[x, y] = [y, x];                 // x=2, y=1
```

**Object destructuring:**
```js
const user = { name: 'Alice', age: 30, role: 'admin' };

const { name, age, role } = user;

// Rename on destructure
const { name: userName, role: userRole } = user;

// Default values
const { name, theme = 'light' } = user;   // theme='light'

// Nested destructuring
const { address: { city, zip } } = { address: { city: 'NYC', zip: '10001' } };

// Function parameter destructuring
function greet({ name, age = 0 }) {
  return `${name} is ${age}`;
}
greet(user);
```

---

## Spread and Rest Operators

Both use `...` but in different contexts.

**Spread — expand an iterable:**
```js
// Arrays
const a = [1, 2, 3];
const b = [4, 5, 6];
const merged = [...a, ...b];             // [1,2,3,4,5,6]
const copy = [...a];                     // shallow copy

// Objects (ES2018)
const base = { theme: 'light', lang: 'en' };
const config = { ...base, lang: 'fr', debug: true };
// { theme:'light', lang:'fr', debug:true }

// Function calls
Math.max(...[1, 2, 3]);                  // 3
```

**Rest — collect remaining elements:**
```js
// Array rest
const [head, ...tail] = [1, 2, 3, 4];   // head=1, tail=[2,3,4]

// Object rest
const { name, ...rest } = { name: 'Alice', age: 30, role: 'admin' };
// rest = { age:30, role:'admin' }

// Function rest parameters
function sum(...nums) {
  return nums.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3, 4);   // 10
```

---

## Default Parameters

```js
function createUser(name, role = 'user', active = true) {
  return { name, role, active };
}

createUser('Alice');                    // { name:'Alice', role:'user', active:true }
createUser('Bob', 'admin');             // { name:'Bob', role:'admin', active:true }
createUser('Carol', undefined, false);  // undefined triggers default for role

// Default can reference earlier params
function padStart(str, len = str.length * 2) { ... }
```

---

## Modules (import / export)

```js
// Named exports
export const API_URL = 'https://api.example.com';
export function fetchUser(id) { ... }
export class UserService { ... }

// Default export (one per file)
export default function App() { ... }

// Named imports
import { API_URL, fetchUser } from './api.js';

// Rename on import
import { fetchUser as getUser } from './api.js';

// Default import
import App from './App.jsx';

// Mix default and named
import App, { API_URL } from './app.js';

// Re-export (barrel file)
// index.js
export { UserCard } from './UserCard.jsx';
export { UserList } from './UserList.jsx';
export { default as UserService } from './UserService.js';
```

**Dynamic import (code splitting):**
```js
// Lazy load on demand
const { default: Chart } = await import('./Chart.js');

// In React
const Chart = React.lazy(() => import('./Chart'));
```

---

## Promises

```js
// Creating a promise
const delay = (ms) =>
  new Promise((resolve, reject) => {
    if (ms < 0) reject(new Error('Delay must be positive'));
    setTimeout(resolve, ms);
  });

// Chaining
fetch('/api/users')
  .then(res => {
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    return res.json();
  })
  .then(users => console.log(users))
  .catch(err => console.error(err))
  .finally(() => setLoading(false));

// Static methods
Promise.all([p1, p2, p3])         // resolves when ALL resolve; rejects if any rejects
Promise.allSettled([p1, p2, p3])  // resolves with all results (fulfilled or rejected)
Promise.race([p1, p2, p3])        // resolves/rejects with the FIRST settled promise
Promise.any([p1, p2, p3])         // resolves with FIRST fulfilled; rejects if all reject
Promise.resolve(value)            // immediately resolved
Promise.reject(error)             // immediately rejected
```

---

## Async / Await

Syntactic sugar over Promises — makes async code read like synchronous code.

```js
async function loadUserData(userId) {
  try {
    const userRes = await fetch(`/api/users/${userId}`);
    if (!userRes.ok) throw new Error('User not found');
    const user = await userRes.json();

    const postsRes = await fetch(`/api/users/${userId}/posts`);
    const posts = await postsRes.json();

    return { user, posts };
  } catch (err) {
    console.error('Failed:', err);
    throw err;  // re-throw so caller knows it failed
  } finally {
    setLoading(false);
  }
}

// Parallel requests (don't await sequentially if independent)
async function loadAll(userId) {
  const [user, posts] = await Promise.all([
    fetch(`/api/users/${userId}`).then(r => r.json()),
    fetch(`/api/posts?userId=${userId}`).then(r => r.json()),
  ]);
  return { user, posts };
}
```

---

## Optional Chaining

Access deeply nested properties without manual null checks.

```js
const user = { profile: { address: null } };

// Without optional chaining
const city = user && user.profile && user.profile.address && user.profile.address.city;

// With optional chaining
const city = user?.profile?.address?.city;   // undefined (no throw)

// With arrays
const first = arr?.[0];

// With function calls
const result = obj.method?.();

// Combined with nullish coalescing
const displayName = user?.profile?.name ?? 'Anonymous';
```

---

## Nullish Coalescing

`??` returns the right-hand side when the left is **null or undefined** (not other falsy values).

```js
// ?? vs ||
0 || 'default'    // 'default'  (0 is falsy)
0 ?? 'default'    // 0          (0 is not null/undefined)

'' || 'default'   // 'default'
'' ?? 'default'   // ''

null ?? 'default'      // 'default'
undefined ?? 'default' // 'default'
false ?? 'default'     // false
```

---

## Logical Assignment Operators

```js
// ||=  assign if value is falsy
user.name ||= 'Anonymous';     // same as: user.name = user.name || 'Anonymous'

// &&=  assign if value is truthy
user.profile &&= sanitize(user.profile);

// ??=  assign if value is null/undefined
config.timeout ??= 5000;
```

---

## Classes

```js
class Animal {
  #name;                        // private field (ES2022)
  static count = 0;             // static field

  constructor(name, sound) {
    this.#name = name;
    this.sound = sound;
    Animal.count++;
  }

  speak() {
    return `${this.#name} says ${this.sound}`;
  }

  get name() { return this.#name; }            // getter
  set name(val) { this.#name = val.trim(); }   // setter

  static create(name, sound) {                 // static method
    return new Animal(name, sound);
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name, 'Woof');
  }

  fetch(item) {
    return `${this.name} fetches ${item}`;
  }
}

const dog = new Dog('Rex');
dog.speak();    // "Rex says Woof"
```

---

## Symbols

Unique, immutable primitive values — useful as object keys to avoid naming collisions.

```js
const id = Symbol('id');
const id2 = Symbol('id');
id === id2;   // false — always unique

const user = {
  [id]: 123,
  name: 'Alice',
};

user[id];   // 123
// Symbol keys don't appear in for...in or Object.keys()

// Well-known Symbols
class MyArray {
  [Symbol.iterator]() {   // makes object iterable
    let i = 0;
    return {
      next: () => i < 3
        ? { value: i++, done: false }
        : { done: true }
    };
  }
}
```

---

## Iterators and Generators

**Iterator protocol:** An object with a `next()` method returning `{ value, done }`.

**Generator function:** Produces an iterator using `function*` and `yield`.

```js
function* range(start, end, step = 1) {
  for (let i = start; i < end; i += step) {
    yield i;
  }
}

for (const n of range(0, 10, 2)) {
  console.log(n);   // 0, 2, 4, 6, 8
}

// Infinite sequence
function* naturalNumbers() {
  let n = 1;
  while (true) yield n++;
}

const nums = naturalNumbers();
nums.next().value;  // 1
nums.next().value;  // 2

// Async generators
async function* fetchPages(url) {
  let page = 1;
  while (true) {
    const res = await fetch(`${url}?page=${page++}`);
    const data = await res.json();
    if (!data.length) return;
    yield data;
  }
}

for await (const page of fetchPages('/api/items')) {
  process(page);
}
```

---

## Proxy and Reflect

**Proxy** wraps an object and intercepts operations (get, set, delete, etc.).

```js
const validator = {
  set(target, key, value) {
    if (key === 'age' && (typeof value !== 'number' || value < 0)) {
      throw new TypeError('Age must be a non-negative number');
    }
    target[key] = value;
    return true;
  },
  get(target, key) {
    console.log(`Accessing ${key}`);
    return Reflect.get(target, key);
  }
};

const user = new Proxy({}, validator);
user.age = 25;    // ✅
user.age = -1;    // ❌ TypeError

// Reactive state (basis of Vue 3)
function reactive(obj) {
  return new Proxy(obj, {
    set(target, key, value) {
      target[key] = value;
      render(); // trigger re-render
      return true;
    }
  });
}
```

---

## WeakMap and WeakSet

Hold **weak references** — entries are garbage-collected when the key object has no other references.

```js
// WeakMap: private data, caching, metadata
const cache = new WeakMap();

function processUser(user) {
  if (cache.has(user)) return cache.get(user);
  const result = heavyComputation(user);
  cache.set(user, result);
  return result;
}
// When `user` is GC'd, its cache entry is automatically removed

// WeakSet: track without preventing GC
const seen = new WeakSet();
function process(obj) {
  if (seen.has(obj)) return;
  seen.add(obj);
  // ...
}
```

---

## Array Methods (ES6+)

```js
// find / findIndex
arr.find(x => x.id === 5)         // first match or undefined
arr.findIndex(x => x.id === 5)    // first index or -1
arr.findLast(x => x.active)       // ES2023: search from end
arr.findLastIndex(x => x.active)

// flat / flatMap
[1, [2, [3]]].flat()              // [1, 2, [3]] — 1 level
[1, [2, [3]]].flat(Infinity)      // [1, 2, 3]
arr.flatMap(x => [x, x * 2])      // map + flat(1)

// Array.from
Array.from({ length: 5 }, (_, i) => i)    // [0,1,2,3,4]
Array.from(new Set([1, 2, 2, 3]))          // [1,2,3]
Array.from('hello')                        // ['h','e','l','l','o']

// Array.at() — negative indexing (ES2022)
arr.at(-1)    // last element
arr.at(-2)    // second to last

// includes
[1, 2, NaN].includes(NaN)    // true (unlike indexOf)

// fill
new Array(5).fill(0)             // [0,0,0,0,0]
arr.fill('x', 2, 4)             // fill from index 2 to 3

// toSorted / toReversed / toSpliced / with (ES2023 — non-mutating)
arr.toSorted((a, b) => a - b)    // returns new sorted array
arr.toReversed()                  // returns new reversed array
arr.with(2, 'new')               // returns copy with index 2 replaced
```

---

## Object Methods (ES6+)

```js
// Object.assign — shallow merge
Object.assign({}, defaults, overrides)

// Object.keys / values / entries
Object.keys(obj)      // ['key1', 'key2']
Object.values(obj)    // [val1, val2]
Object.entries(obj)   // [['key1', val1], ['key2', val2]]

// Object.fromEntries — inverse of Object.entries
Object.fromEntries([['a', 1], ['b', 2]])    // { a:1, b:2 }
Object.fromEntries(new Map([['x', 10]]))    // { x:10 }

// Transform object values
const doubled = Object.fromEntries(
  Object.entries(prices).map(([k, v]) => [k, v * 2])
);

// Object.freeze / Object.seal
Object.freeze(obj)   // no add/modify/delete
Object.seal(obj)     // can modify existing, can't add/delete

// Object.hasOwn (ES2022 — preferred over hasOwnProperty)
Object.hasOwn(user, 'name')    // true/false

// Object.getOwnPropertyDescriptor
Object.getOwnPropertyDescriptor(obj, 'name')
// { value, writable, enumerable, configurable }

// Computed property names
const key = 'dynamic';
const obj = { [key]: 'value', [`${key}_id`]: 1 };
```

---

## Error Handling Patterns

```js
// Custom error classes
class ApiError extends Error {
  constructor(message, status, code) {
    super(message);
    this.name = 'ApiError';
    this.status = status;
    this.code = code;
  }
}

// Result pattern (avoid thrown errors for expected failures)
async function safeParseJSON(text) {
  try {
    return { ok: true, data: JSON.parse(text) };
  } catch {
    return { ok: false, error: 'Invalid JSON' };
  }
}

const { ok, data, error } = await safeParseJSON(response);

// Error cause (ES2022)
try {
  await fetchUser(id);
} catch (err) {
  throw new Error('Failed to load dashboard', { cause: err });
}
```


---

## DOM Manipulation

The **Document Object Model (DOM)** is a tree representation of an HTML document that JavaScript can read and modify.

```js
// Selecting elements
document.getElementById('app')
document.querySelector('.card')           // first match
document.querySelectorAll('.card')        // NodeList of all matches
document.querySelector('ul > li:first-child')

// Creating and inserting elements
const div = document.createElement('div');
div.className = 'card';
div.textContent = 'Hello';
div.setAttribute('data-id', '42');

// Insertion methods
parent.append(div)            // end of parent (accepts strings too)
parent.prepend(div)           // start of parent
parent.insertBefore(div, ref) // before a reference node
ref.before(div)               // before ref (modern)
ref.after(div)                // after ref (modern)
ref.replaceWith(div)          // replace ref with div

// Reading/writing
el.textContent = 'Safe text'       // no HTML parsing (XSS-safe)
el.innerHTML = '<b>Bold</b>'       // parses HTML (⚠️ XSS risk)
el.innerText                       // visible text only (respects CSS)

// Classes
el.classList.add('active', 'visible')
el.classList.remove('hidden')
el.classList.toggle('open')
el.classList.contains('active')   // boolean
el.classList.replace('old', 'new')

// Styles
el.style.color = 'red'
el.style.cssText = 'color:red; font-size:16px'
getComputedStyle(el).color         // computed value

// Attributes
el.getAttribute('href')
el.setAttribute('aria-label', 'Close')
el.removeAttribute('disabled')
el.hasAttribute('hidden')
el.dataset.userId                  // <div data-user-id="5">

// DOM traversal
el.parentElement
el.children                        // HTMLCollection of child elements
el.firstElementChild
el.lastElementChild
el.nextElementSibling
el.previousElementSibling
el.closest('.card')                // nearest ancestor matching selector

// Removing elements
el.remove()
```

---

## Events

```js
// Adding listeners
el.addEventListener('click', handler)
el.addEventListener('click', handler, { once: true })     // fires once
el.addEventListener('click', handler, { passive: true })  // can't preventDefault (perf)
el.addEventListener('click', handler, { capture: true })  // capture phase

// Removing listeners — must pass same function reference
el.removeEventListener('click', handler)

// Event object
el.addEventListener('click', (e) => {
  e.preventDefault()         // prevent default browser action
  e.stopPropagation()        // stop bubbling to parent
  e.stopImmediatePropagation() // also stop other listeners on same element
  e.target                   // element that triggered the event
  e.currentTarget            // element the listener is attached to
  e.type                     // 'click'
  e.clientX, e.clientY       // mouse position relative to viewport
  e.key, e.code              // keyboard events
});

// Event delegation — one listener handles many children
document.querySelector('#list').addEventListener('click', (e) => {
  const item = e.target.closest('li');
  if (!item) return;
  console.log('Clicked:', item.dataset.id);
});

// Custom events
const event = new CustomEvent('user:login', {
  detail: { userId: 42 },
  bubbles: true,
  cancelable: true,
});
document.dispatchEvent(event);

document.addEventListener('user:login', (e) => {
  console.log(e.detail.userId);
});

// Common events
// Mouse: click, dblclick, mousedown, mouseup, mouseover, mouseout, mousemove
// Keyboard: keydown, keyup, keypress
// Form: submit, change, input, focus, blur, reset
// Window: load, DOMContentLoaded, resize, scroll, beforeunload
// Touch: touchstart, touchend, touchmove
// Pointer: pointerdown, pointerup, pointermove (unifies mouse/touch/pen)
```

---

## Fetch API

The modern way to make HTTP requests.

```js
// Basic GET
const res = await fetch('/api/users');
if (!res.ok) throw new Error(`HTTP error: ${res.status}`);
const users = await res.json();

// POST with JSON body
const res = await fetch('/api/users', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name: 'Alice', role: 'admin' }),
});

// File upload
const formData = new FormData();
formData.append('avatar', fileInput.files[0]);
await fetch('/api/upload', { method: 'POST', body: formData });

// With AbortController (cancellation)
const controller = new AbortController();
const timeout = setTimeout(() => controller.abort(), 5000);

try {
  const res = await fetch('/api/data', { signal: controller.signal });
  const data = await res.json();
} catch (err) {
  if (err.name === 'AbortError') console.log('Request cancelled');
} finally {
  clearTimeout(timeout);
}

// Reading response types
res.json()        // parse as JSON
res.text()        // parse as string
res.blob()        // parse as Blob (images, files)
res.arrayBuffer() // parse as ArrayBuffer
res.formData()    // parse as FormData

// Response properties
res.ok            // true if status 200-299
res.status        // 200, 404, etc.
res.headers.get('Content-Type')
res.url
res.redirected

// Streaming responses (large files, SSE)
const reader = res.body.getReader();
while (true) {
  const { done, value } = await reader.read();
  if (done) break;
  process(value);   // Uint8Array chunk
}
```

---

## AJAX with XMLHttpRequest

The older approach — still relevant to understand, but prefer Fetch.

```js
const xhr = new XMLHttpRequest();
xhr.open('GET', '/api/users');
xhr.setRequestHeader('Accept', 'application/json');

xhr.onload = function () {
  if (xhr.status === 200) {
    const data = JSON.parse(xhr.responseText);
  }
};
xhr.onerror = () => console.error('Network error');
xhr.onprogress = (e) => {
  if (e.lengthComputable) {
    const percent = (e.loaded / e.total) * 100;
  }
};

xhr.send();

// POST
xhr.open('POST', '/api/users');
xhr.setRequestHeader('Content-Type', 'application/json');
xhr.send(JSON.stringify({ name: 'Alice' }));
```

---

## Web Storage

Client-side key-value storage. Data persists across sessions (localStorage) or only for the current session (sessionStorage).

```js
// localStorage — persists until manually cleared
localStorage.setItem('theme', 'dark');
localStorage.getItem('theme');          // 'dark'
localStorage.removeItem('theme');
localStorage.clear();

// Store objects (must serialize)
localStorage.setItem('user', JSON.stringify({ name: 'Alice' }));
const user = JSON.parse(localStorage.getItem('user'));

// sessionStorage — cleared when tab/window closes
sessionStorage.setItem('token', 'abc123');

// Storage event (fired in OTHER tabs of the same origin)
window.addEventListener('storage', (e) => {
  console.log(e.key, e.oldValue, e.newValue);
});
```

| Feature | localStorage | sessionStorage | Cookie |
|---|---|---|---|
| Capacity | ~5MB | ~5MB | ~4KB |
| Expiry | Never | Tab close | Configurable |
| Accessible from JS | ✅ | ✅ | ✅ (if not HttpOnly) |
| Sent with requests | ❌ | ❌ | ✅ automatically |

---

## Cookies

```js
// Set a cookie
document.cookie = 'user=Alice; expires=Fri, 31 Dec 2025 23:59:59 GMT; path=/; SameSite=Lax';

// Read cookies
document.cookie   // "user=Alice; theme=dark" — all as one string

function getCookie(name) {
  const match = document.cookie.match(new RegExp(`(^|; )${name}=([^;]*)`));
  return match ? decodeURIComponent(match[2]) : null;
}

// Delete a cookie
document.cookie = 'user=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/';
```

**Cookie flags:**
- `HttpOnly` — not accessible via JS (set server-side, XSS protection)
- `Secure` — HTTPS only
- `SameSite=Strict/Lax/None` — CSRF protection
- `Path=/` — scope to a path

---

## WebSockets

Full-duplex real-time communication between client and server.

```js
const ws = new WebSocket('wss://api.example.com/ws');

ws.onopen = () => {
  console.log('Connected');
  ws.send(JSON.stringify({ type: 'subscribe', channel: 'prices' }));
};

ws.onmessage = (event) => {
  const data = JSON.parse(event.data);
  updateUI(data);
};

ws.onerror = (err) => console.error('WebSocket error', err);

ws.onclose = (event) => {
  console.log(`Closed: ${event.code} ${event.reason}`);
  // implement reconnect logic here
};

ws.close();   // close the connection
ws.readyState // 0=CONNECTING, 1=OPEN, 2=CLOSING, 3=CLOSED
```

**vs. Server-Sent Events (SSE):**
```js
// SSE — server pushes to client only (one-way, HTTP)
const es = new EventSource('/api/events');
es.onmessage = (e) => console.log(e.data);
es.addEventListener('userJoined', (e) => updateList(JSON.parse(e.data)));
es.close();
```

---

## Service Workers

A script running in the background, separate from the web page. Enables offline support, push notifications, and background sync.

```js
// Register a service worker
if ('serviceWorker' in navigator) {
  const reg = await navigator.serviceWorker.register('/sw.js');
  console.log('SW registered:', reg.scope);
}

// sw.js — basic cache-first strategy
const CACHE_NAME = 'app-v1';
const ASSETS = ['/', '/index.html', '/app.js', '/style.css'];

self.addEventListener('install', (e) => {
  e.waitUntil(
    caches.open(CACHE_NAME).then(cache => cache.addAll(ASSETS))
  );
  self.skipWaiting();
});

self.addEventListener('activate', (e) => {
  e.waitUntil(
    caches.keys().then(keys =>
      Promise.all(keys.filter(k => k !== CACHE_NAME).map(k => caches.delete(k)))
    )
  );
  self.clients.claim();
});

self.addEventListener('fetch', (e) => {
  e.respondWith(
    caches.match(e.request).then(cached => cached || fetch(e.request))
  );
});
```

**Use cases:** Offline apps (PWA), background data sync, push notifications.

---

## Intersection Observer

Efficiently observe when elements enter/leave the viewport — avoids scroll event listeners.

```js
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
      entry.target.src = entry.target.dataset.src;  // lazy load image
      observer.unobserve(entry.target);              // stop observing
    }
  });
}, {
  root: null,           // viewport
  rootMargin: '0px 0px -100px 0px',  // trigger 100px before bottom
  threshold: 0.1        // 10% of element visible
});

document.querySelectorAll('.lazy').forEach(el => observer.observe(el));
```

**Use cases:** Lazy loading images/components, infinite scroll, animations on scroll, analytics (impression tracking).

---

## MutationObserver

Watch for changes in the DOM tree.

```js
const observer = new MutationObserver((mutations) => {
  mutations.forEach(mutation => {
    if (mutation.type === 'childList') {
      console.log('Children changed:', mutation.addedNodes, mutation.removedNodes);
    }
    if (mutation.type === 'attributes') {
      console.log(`${mutation.attributeName} changed`);
    }
  });
});

observer.observe(targetEl, {
  childList: true,       // watch for child node add/remove
  attributes: true,      // watch for attribute changes
  characterData: true,   // watch for text changes
  subtree: true,         // watch entire subtree
});

observer.disconnect();   // stop observing
```

---

## ResizeObserver

Observe element size changes (replaces window resize event for element-level tracking).

```js
const observer = new ResizeObserver((entries) => {
  entries.forEach(entry => {
    const { width, height } = entry.contentRect;
    if (width < 600) applyMobileLayout();
    else applyDesktopLayout();
  });
});

observer.observe(document.querySelector('.container'));
observer.unobserve(el);
observer.disconnect();
```

---

## Clipboard API

```js
// Copy text
await navigator.clipboard.writeText('Copied text!');

// Read text
const text = await navigator.clipboard.readText();

// Copy image/rich content
const blob = await fetch('/image.png').then(r => r.blob());
await navigator.clipboard.write([
  new ClipboardItem({ 'image/png': blob })
]);
```

---

## Geolocation API

```js
// One-time position
navigator.geolocation.getCurrentPosition(
  (pos) => {
    const { latitude, longitude, accuracy } = pos.coords;
    showOnMap(latitude, longitude);
  },
  (err) => console.error(err.message),
  { enableHighAccuracy: true, timeout: 10000, maximumAge: 0 }
);

// Watch position (real-time tracking)
const watchId = navigator.geolocation.watchPosition(successFn, errorFn);
navigator.geolocation.clearWatch(watchId);
```

---

## Web Workers

Run scripts in background threads — keeps UI responsive during heavy computation.

```js
// main.js
const worker = new Worker('/worker.js');
worker.postMessage({ data: largeDataset });

worker.onmessage = (e) => {
  console.log('Result:', e.data);
};
worker.onerror = (err) => console.error(err);
worker.terminate();

// worker.js
self.onmessage = (e) => {
  const result = heavyComputation(e.data);
  self.postMessage(result);
};
```

---

## History API

Manipulate browser history without reloading the page (foundation of client-side routing).

```js
// Push a new state
history.pushState({ page: 1 }, 'Title', '/about');

// Replace current state
history.replaceState({ page: 2 }, 'Title', '/profile');

// Navigate
history.back()
history.forward()
history.go(-2)

// Listen for back/forward
window.addEventListener('popstate', (e) => {
  console.log('State:', e.state, 'Path:', location.pathname);
  renderPage(location.pathname);
});
```

---

## URL and URLSearchParams

```js
const url = new URL('https://example.com/search?q=react&page=2');
url.hostname   // 'example.com'
url.pathname   // '/search'
url.search     // '?q=react&page=2'
url.hash       // ''

const params = url.searchParams;
params.get('q')          // 'react'
params.get('page')       // '2'
params.set('page', '3')
params.append('filter', 'latest')
params.delete('q')
params.has('q')          // false after delete

// Convert back
url.toString()           // updated full URL
params.toString()        // 'page=3&filter=latest'

// Parse from current page
const params = new URLSearchParams(window.location.search);
```

---

## Performance API

```js
// High-resolution timing
performance.now()           // ms since page load (microsecond precision)

// Mark and measure custom timings
performance.mark('start:render');
renderDashboard();
performance.mark('end:render');
performance.measure('render', 'start:render', 'end:render');

const [measure] = performance.getEntriesByName('render');
console.log(`Render took ${measure.duration}ms`);

// Navigation timing
const nav = performance.getEntriesByType('navigation')[0];
nav.domContentLoadedEventEnd - nav.startTime   // DOMContentLoaded time
nav.loadEventEnd - nav.startTime               // Page load time

// Resource timing
performance.getEntriesByType('resource').forEach(r => {
  console.log(r.name, r.duration);
});
```

---

## Broadcast Channel API

Communicate between tabs/windows of the same origin.

```js
// All tabs listen to the same channel
const channel = new BroadcastChannel('app-state');

// Send
channel.postMessage({ type: 'THEME_CHANGED', theme: 'dark' });

// Receive
channel.onmessage = (e) => {
  if (e.data.type === 'THEME_CHANGED') applyTheme(e.data.theme);
};

channel.close();
```

**Use case:** Sync authentication state, theme changes, or cart updates across multiple open tabs.

---

## Iterating Collections

### Array — value + index

```js
const fruits = ['apple', 'banana', 'cherry'];

// 1. Classic for loop — full index control
for (let i = 0; i < fruits.length; i++) {
  console.log(i, fruits[i]);       // 0 'apple', 1 'banana', 2 'cherry'
}

// 2. forEach — callback receives (value, index, array)
fruits.forEach((value, index) => {
  console.log(index, value);
});

// 3. for...of with entries() — cleanest modern syntax
for (const [index, value] of fruits.entries()) {
  console.log(index, value);
}

// 4. map — returns a new array (use when you need a transformed result)
const result = fruits.map((value, index) => `${index}: ${value}`);
// ['0: apple', '1: banana', '2: cherry']

// 5. for...in — iterates keys (strings!) — avoid for arrays
for (const key in fruits) {
  console.log(key, fruits[key]);   // '0' 'apple' — key is a string, not number
}
// ⚠️  for...in also picks up prototype properties; don't use on arrays.
```

### Map — key + value

```js
const scores = new Map([
  ['Alice', 95],
  ['Bob',   87],
  ['Carol', 91],
]);

// 1. for...of — most idiomatic
for (const [key, value] of scores) {
  console.log(key, value);         // 'Alice' 95 ...
}

// 2. forEach — callback receives (value, key, map)  ← note: value first!
scores.forEach((value, key) => {
  console.log(key, value);
});

// 3. Separate key/value iterators
for (const key   of scores.keys())   { console.log(key);   }
for (const value of scores.values()) { console.log(value); }
for (const entry of scores.entries()) { console.log(entry); }  // [key, value] pair
```

> **Gotcha:** `Map.forEach` callback signature is `(value, key)` — the opposite of `Array.forEach`'s `(value, index)`.

### Object — key + value

```js
const user = { name: 'Alice', age: 30, role: 'admin' };

// 1. for...in — iterates all enumerable keys including inherited ones
for (const key in user) {
  if (Object.hasOwn(user, key)) {          // guard against prototype keys
    console.log(key, user[key]);
  }
}

// 2. Object.entries() — recommended; gives [key, value] pairs
for (const [key, value] of Object.entries(user)) {
  console.log(key, value);                 // 'name' 'Alice', 'age' 30 ...
}

// 3. Object.keys() / Object.values() — when you only need one side
Object.keys(user).forEach(key   => console.log(key));
Object.values(user).forEach(val => console.log(val));

// 4. forEach via entries
Object.entries(user).forEach(([key, value]) => {
  console.log(`${key} = ${value}`);
});
```

> **Note:** `Object.entries/keys/values` only return **own enumerable** properties.  
> Symbol-keyed properties are excluded — use `Object.getOwnPropertySymbols()` if needed.

### Set — value only (no index)

```js
const tags = new Set(['js', 'react', 'css', 'js']); // duplicate 'js' ignored
// Set {'js', 'react', 'css'}

// 1. for...of — most common
for (const value of tags) {
  console.log(value);
}

// 2. forEach — callback receives (value, value, set)  ← second arg is also value (no index)
tags.forEach((value) => {
  console.log(value);
});

// 3. Spread to array if you need indices
[...tags].forEach((value, index) => {
  console.log(index, value);          // 0 'js', 1 'react', 2 'css'
});

// 4. Iterators
for (const v of tags.values()) { console.log(v); }
for (const e of tags.entries()) { console.log(e); }  // [value, value] — same twice
```

> **Why no index?** Sets are unordered by conceptual design (iteration order is insertion order as an implementation detail, but sets don't expose positional semantics).

### Quick Comparison Table

| Collection | Get value | Get key/index | Best loop | `forEach` signature |
|---|---|---|---|---|
| `Array` | `arr[i]` | index (number) | `for...of arr.entries()` | `(value, index, arr)` |
| `Map` | `map.get(key)` | key (any type) | `for...of map` | `(value, key, map)` ⚠️ value first |
| `Object` | `obj[key]` | key (string/Symbol) | `Object.entries(obj)` | N/A — use `.forEach` after `entries()` |
| `Set` | value itself | ❌ N/A | `for...of set` | `(value, value, set)` |

---

## Type Checking

JavaScript's type system has several well-known traps (`typeof null === 'object'`, `typeof [] === 'object'`). This section covers reliable patterns for every case.

### The `typeof` Operator and Its Pitfalls

```js
typeof 42            // 'number'
typeof 'hello'       // 'string'
typeof true          // 'boolean'
typeof undefined     // 'undefined'
typeof Symbol()      // 'symbol'
typeof 42n           // 'bigint'
typeof function(){}  // 'function'  ← works for functions

// ⚠️  Everything else returns 'object' — including traps:
typeof null          // 'object'  ← historical bug, unfixable
typeof []            // 'object'
typeof {}            // 'object'
typeof new Map()     // 'object'
typeof new Set()     // 'object'
```

### Checking for `null`

```js
// Only reliable pattern — strict equality
const x = null;

x === null               // ✅ true
typeof x === 'object' && x === null  // ✅ verbose but explicit

// ❌ typeof alone doesn't work:
typeof x === 'object'    // true for both null AND real objects
```

### Checking for `undefined`

```js
let y;

y === undefined          // ✅ true
typeof y === 'undefined' // ✅ true — also safe for undeclared variables

// typeof is safer for undeclared variables:
typeof undeclaredVar     // 'undefined' (no ReferenceError)
undeclaredVar === undefined  // ❌ ReferenceError if not declared
```

### Checking for `NaN`

```js
const n = NaN;

Number.isNaN(n)          // ✅ true — most reliable
isNaN(n)                 // ✅ true — but coerces strings first (isNaN('hello') → true)
n !== n                  // ✅ true — NaN is the only value not equal to itself

// ❌ Avoid:
typeof n === 'number' && isNaN(n)  // works but verbose; use Number.isNaN
```

### Checking for an Array

```js
const arr = [1, 2, 3];

Array.isArray(arr)       // ✅ true — the canonical way
arr instanceof Array     // ✅ true — but fails across iframes (different Array refs)

// ❌ Unreliable:
typeof arr               // 'object'
```

### Checking for a plain Object

A "plain object" is `{}` — created by `Object.create(null)`, an object literal, or `new Object()`. It excludes Arrays, Maps, Sets, class instances, etc.

```js
function isPlainObject(val) {
  if (typeof val !== 'object' || val === null) return false;
  const proto = Object.getPrototypeOf(val);
  return proto === Object.prototype || proto === null;
}

isPlainObject({})              // ✅ true
isPlainObject({ a: 1 })        // ✅ true
isPlainObject(Object.create(null)) // ✅ true
isPlainObject([])              // ❌ false
isPlainObject(new Map())       // ❌ false
isPlainObject(new MyClass())   // ❌ false — has custom prototype
```

### Checking for a Map

```js
const m = new Map();

m instanceof Map                           // ✅ true
Object.prototype.toString.call(m)          // '[object Map]'

// Cross-realm safe (e.g. across iframes):
Object.prototype.toString.call(m) === '[object Map]'  // ✅
```

### Checking for a Set

```js
const s = new Set();

s instanceof Set                           // ✅ true
Object.prototype.toString.call(s)          // '[object Set]'
Object.prototype.toString.call(s) === '[object Set]'  // ✅
```

### Checking for a Function

```js
const fn = () => {};

typeof fn === 'function'           // ✅ true — most reliable
fn instanceof Function             // ✅ true
Object.prototype.toString.call(fn) // '[object Function]'

// Works for all callable forms:
typeof function(){} === 'function' // ✅
typeof class Foo {}  === 'function' // ✅ — classes are functions too
typeof fn.bind(null) === 'function' // ✅
```

### Universal type tag via `Object.prototype.toString`

The most reliable way to distinguish built-in types. Returns `'[object <Tag>]'`.

```js
const tag = (val) => Object.prototype.toString.call(val);

tag(null)          // '[object Null]'
tag(undefined)     // '[object Undefined]'
tag(42)            // '[object Number]'
tag('hello')       // '[object String]'
tag(true)          // '[object Boolean]'
tag([])            // '[object Array]'
tag({})            // '[object Object]'
tag(new Map())     // '[object Map]'
tag(new Set())     // '[object Set]'
tag(() => {})      // '[object Function]'
tag(new Date())    // '[object Date]'
tag(/regex/)       // '[object RegExp]'
tag(new Promise(() => {})) // '[object Promise]'
tag(Symbol())      // '[object Symbol]'
tag(42n)           // '[object BigInt]'
```

> **Caveat:** Custom classes can override `Symbol.toStringTag` to spoof any tag:
> ```js
> class Fake { get [Symbol.toStringTag]() { return 'Map'; } }
> tag(new Fake())  // '[object Map]' — spoofed!
> ```
> For your own code this is rarely a problem, but it matters in security-sensitive contexts.

### Type-checking utility reference table

| Value | `typeof` | `instanceof` | `Array.isArray` | `=== null` | `Number.isNaN` | `toString` tag |
|---|---|---|---|---|---|---|
| `null` | `'object'` ⚠️ | — | — | ✅ `true` | — | `[object Null]` |
| `undefined` | `'undefined'` ✅ | — | — | — | — | `[object Undefined]` |
| `42` | `'number'` ✅ | — | — | — | `false` | `[object Number]` |
| `NaN` | `'number'` ⚠️ | — | — | — | ✅ `true` | `[object Number]` |
| `'hi'` | `'string'` ✅ | — | — | — | — | `[object String]` |
| `true` | `'boolean'` ✅ | — | — | — | — | `[object Boolean]` |
| `[]` | `'object'` ⚠️ | `Array` ✅ | ✅ `true` | — | — | `[object Array]` |
| `{}` | `'object'` ⚠️ | `Object` ✅ | `false` | — | — | `[object Object]` |
| `new Map()` | `'object'` ⚠️ | `Map` ✅ | — | — | — | `[object Map]` |
| `new Set()` | `'object'` ⚠️ | `Set` ✅ | — | — | — | `[object Set]` |
| `() => {}` | `'function'` ✅ | `Function` ✅ | — | — | — | `[object Function]` |
| `new Date()` | `'object'` ⚠️ | `Date` ✅ | — | — | — | `[object Date]` |

---

## Copying Objects and Arrays

### Shallow Copy

A **shallow copy** duplicates the top-level structure but nested objects/arrays are still **shared by reference** — mutating a nested value in the copy also mutates the original.

```js
const original = { name: 'Alice', address: { city: 'Chennai' } };

// ── Spread operator ──────────────────────────────────────────────
const copy1 = { ...original };
copy1.name = 'Bob';           // ✅ original.name still 'Alice'
copy1.address.city = 'Delhi'; // ❌ original.address.city ALSO becomes 'Delhi'!

// ── Object.assign ─────────────────────────────────────────────────
const copy2 = Object.assign({}, original);
// Same behaviour as spread — nested objects are still shared refs

// ── Array spread / slice ──────────────────────────────────────────
const arr = [1, [2, 3], { x: 4 }];
const arrCopy = [...arr];          // or arr.slice()
arrCopy[0] = 99;                   // ✅ independent
arrCopy[1].push(99);               // ❌ arr[1] also modified (shared ref)

// ── Array.from ───────────────────────────────────────────────────
const arrCopy2 = Array.from(arr);  // also shallow
```

> **Rule of thumb:** If the value contains only primitives (strings, numbers, booleans) at every level, a shallow copy is safe. As soon as there are nested objects or arrays, use a deep copy.

### Deep Copy

A **deep copy** recursively duplicates every nested structure. The copy is completely independent.

#### `structuredClone()` — the modern standard (ES2022)

Built-in, fast, handles circular references, and works in all modern browsers and Node.js ≥ 17.

```js
const original = {
  name: 'Alice',
  address: { city: 'Chennai', coords: [13.08, 80.27] },
  tags: ['dev', 'js'],
  joined: new Date('2022-01-01'),
};

const deep = structuredClone(original);

deep.address.city = 'Delhi';
deep.tags.push('ts');
console.log(original.address.city); // 'Chennai' ✅ untouched
console.log(original.tags);         // ['dev', 'js'] ✅ untouched
console.log(deep.joined instanceof Date); // true ✅ Date is properly cloned
```

**Handles correctly:**
- Nested objects and arrays
- `Date`, `Map`, `Set`, `ArrayBuffer`, `RegExp`, `Blob`
- Circular references (no infinite loop)

```js
// Circular reference — no problem
const a = { val: 1 };
a.self = a;
const clone = structuredClone(a);
console.log(clone.self === clone); // true — circularity preserved
```

#### `JSON.parse(JSON.stringify())` — legacy approach

Works for plain data but has significant limitations:

```js
const deep2 = JSON.parse(JSON.stringify(original));

// ❌ Limitations:
// - Date becomes a string:   deep2.joined → '2022-01-01T00:00:00.000Z'
// - undefined values dropped: { a: undefined } → {}
// - Functions dropped:        { fn: () => {} } → {}
// - NaN / Infinity → null
// - Map / Set → {} (empty object)
// - Circular references → throws TypeError
```

Use `JSON.parse(JSON.stringify())` only when you are certain the data is plain JSON-safe.

#### Manual recursive clone (rare — only if customization needed)

```js
function deepClone(value, seen = new Map()) {
  if (value === null || typeof value !== 'object') return value;
  if (seen.has(value)) return seen.get(value);      // handle circular refs

  if (value instanceof Date)   return new Date(value);
  if (value instanceof RegExp) return new RegExp(value.source, value.flags);
  if (value instanceof Map) {
    const clone = new Map();
    seen.set(value, clone);
    value.forEach((v, k) => clone.set(deepClone(k, seen), deepClone(v, seen)));
    return clone;
  }
  if (value instanceof Set) {
    const clone = new Set();
    seen.set(value, clone);
    value.forEach(v => clone.add(deepClone(v, seen)));
    return clone;
  }
  if (Array.isArray(value)) {
    const clone = [];
    seen.set(value, clone);
    value.forEach((v, i) => { clone[i] = deepClone(v, seen); });
    return clone;
  }

  const clone = Object.create(Object.getPrototypeOf(value));
  seen.set(value, clone);
  for (const key of Reflect.ownKeys(value)) {
    clone[key] = deepClone(value[key], seen);
  }
  return clone;
}
```

### Method Comparison Table

| Method | Depth | Handles `Date` | Handles `Map`/`Set` | Handles circular refs | Functions | Speed |
|---|---|---|---|---|---|---|
| `{ ...obj }` / `Object.assign` | Shallow | N/A | N/A | N/A | Copied (ref) | ⚡ Fastest |
| `[...arr]` / `arr.slice()` | Shallow | N/A | N/A | N/A | Copied (ref) | ⚡ Fastest |
| `structuredClone()` | **Deep** | ✅ Date | ✅ Map/Set | ✅ Yes | ❌ Dropped | ✅ Fast |
| `JSON.parse(JSON.stringify())` | **Deep** | ❌ → string | ❌ → `{}` | ❌ Throws | ❌ Dropped | 🐢 Slow |
| Lodash `_.cloneDeep()` | **Deep** | ✅ | ✅ | ✅ | ✅ Cloned | Medium |
| Manual `deepClone()` | **Deep** | Custom | Custom | Custom | Custom | Varies |

### What `structuredClone` Cannot Copy

```js
// ❌ Functions are silently dropped
structuredClone({ fn: () => 42 });     // → {}

// ❌ Symbol-keyed properties are dropped
const sym = Symbol('id');
structuredClone({ [sym]: 1 });         // → {}

// ❌ DOM nodes throw DataCloneError
structuredClone(document.body);        // TypeError: Failed to execute 'structuredClone'

// ❌ Class instances lose their prototype chain
class User { greet() { return 'hi'; } }
const u = new User();
const cloned = structuredClone(u);
cloned instanceof User;  // false — plain object, greet() is gone

// ✅ Workaround for class instances: clone the data, then reconstruct
const clonedUser = Object.assign(new User(), structuredClone({ ...u }));
```
