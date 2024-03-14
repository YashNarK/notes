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
      - [3. **Hoisting:**](#3-hoisting)
      - [4. **Rest Operator and Arguments:**](#4-rest-operator-and-arguments)
      - [5. **Default Values for Parameters:**](#5-default-values-for-parameters)
      - [6. **Magic Function:**](#6-magic-function)
      - [7. **IIFE - Immediately Invoked Function Expression**:](#7-iife---immediately-invoked-function-expression)
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
    - [1. **Method Invocation:**](#1-method-invocation)
    - [2. **Function Invocation (Global Context in Browsers):**](#2-function-invocation-global-context-in-browsers)
    - [3. **Arrow Functions:**](#3-arrow-functions)
    - [4. **Passing `this` as an Argument:**](#4-passing-this-as-an-argument)
    - [5. **Arrow Functions in Callbacks:**](#5-arrow-functions-in-callbacks)
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
    - [Closures:](#closures)
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

#### 3. **Hoisting:**

- Function declarations are hoisted to the top of their scope, allowing them to be used before they are declared.

```javascript
console.log(greet("John")); // Outputs: "Hello, John!"

function greet(name) {
  return `Hello, ${name}!`;
}
```

#### 4. **Rest Operator and Arguments:**

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

#### 5. **Default Values for Parameters:**

- Parameters can have default values, ensuring they are assigned a value if none is provided during the function call.

```javascript
function greet(name = "Guest") {
  return `Hello, ${name}!`;
}

console.log(greet()); // Outputs: "Hello, Guest!"
```

#### 6. **Magic Function:**

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

#### 7. **IIFE - Immediately Invoked Function Expression**:

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

In JavaScript, the `this` keyword is a special variable that refers to the context in which a function is executed. The behavior of `this` is determined by how a function is called.

### 1. **Method Invocation:**

- When a function is called as a method of an object, `this` refers to the object itself.

```javascript
const person = {
  name: "John",
  sayHello: function () {
    console.log(`Hello, ${this.name}!`);
  },
};

person.sayHello(); // Outputs: "Hello, John!"
```

### 2. **Function Invocation (Global Context in Browsers):**

- When a function is not a method of an object, `this` refers to the global object (`window` in browsers, `global` in Node.js).

```javascript
function globalFunction() {
  console.log(this === window); // Outputs: true (in a browser)
}

globalFunction();
```

### 3. **Arrow Functions:**

- Arrow functions do not have their own `this`. They inherit the `this` value from the enclosing scope.

```javascript
const obj = {
  value: 42,
  getValue: function () {
    return () => console.log(this.value);
  },
};

const getValue = obj.getValue();
getValue(); // Outputs: 42
```

### 4. **Passing `this` as an Argument:**

- To explicitly pass the value of `this` to a function, you can use the `call()` or `apply()` methods.

```javascript
function greet() {
  console.log(`Hello, ${this.name}!`);
}

const person = { name: "John" };

greet.call(person); // Outputs: "Hello, John!"
```

### 5. **Arrow Functions in Callbacks:**

- Arrow functions are often used in callbacks to ensure that `this` retains its value from the enclosing scope.

```javascript
const obj = {
  value: 42,
  handleClick: function () {
    setTimeout(() => {
      console.log(this.value);
    }, 1000);
  },
};

obj.handleClick(); // Outputs: 42
```

Understanding the behavior of the `this` keyword is crucial in JavaScript, especially when working with different function contexts and callbacks. Arrow functions are often preferred in modern JavaScript development to avoid the pitfalls associated with the dynamic nature of `this`.

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

### Closures:

Closures occur when a function has access to variables from its outer scope, even after that scope has finished execution.

```javascript
function outerFunction() {
  const outerVariable = "I am outer";

  function innerFunction() {
    console.log(outerVariable);
  }

  return innerFunction;
}

const closureExample = outerFunction();
closureExample(); // Outputs: "I am outer"
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
