# JavaScript

## Table of contents
- [Introduction on JS](#introduction-to-js)
- [Features of JavaScript](#features-of-javascript)
- [Applications of JavaScript](#applications-of-javascript)
- [Limitations of JavaScript](#limitations-of-javascript)
- [Why JavaScript is known as a lightweight programming language ?](#why-javascript-is-known-as-a-lightweight-programming-language)
- [Is JavaScript Compiled or Interpreted or both ?](#is-javascript-compiled-or-interpreted-or-both)
- [Data Types](#data-types)
- [Operators](#operators)
- [Loops in JS](#loops)
- [Truthy and Falsy](#truthy-and-falsy)
- [Short Circuiting](#short-circuiting)
- [Strings](#strings)
- [Arrays](#arrays)
- [Functions](#functions-in-javascript---detailed-overview)
- [Getters and Setters]
- [Exception handling]
- [This - keyword]
- [Object Oriented Programming]
- [Mixins]
- [Important Resources]

## Introduction to JS
JavaScript is a lightweight, cross-platform, single-threaded, and interpreted compiled programming language. It is also known as the scripting language for webpages. It is well-known for the development of web pages, and many non-browser environments also use it.

JavaScript is a weakly typed (*allows implicit type conversions on operation involves mismatched types, instead of throwing type errors*) and dynamically (*also called loosely typed; variables are not directly associated with any particular type*) typed language. JavaScript can be used for Client-side developments as well as Server-side developments. JavaScript is both an imperative and declarative type of language. JavaScript contains a standard library of objects, like Array, Date, and Math, and a core set of language elements like operators, control structures, and statements. 

- **Client-side**: It supplies objects to control a browser and its Document Object Model (DOM). Like if client-side extensions allow an application to place elements on an HTML form and respond to user events such as mouse clicks, form input, and page navigation. Useful libraries for the client side are AngularJS, ReactJS, VueJS, and so many others.
- **Server-side**: It supplies objects relevant to running JavaScript on a server. For if the server-side extensions allow an application to communicate with a database, and provide continuity of information from one invocation to another of the application, or perform file manipulations on a server. The useful framework which is the most famous these days is node.js.
- **Imperative language** – In this type of language we are mostly concerned about how it is to be done. It simply controls the flow of computation. The procedural programming approach, object, oriented approach comes under this as async await we are thinking about what is to be done further after the async call.
- **Declarative programming** – In this type of language we are concerned about how it is to be done, basically here logical computation requires. Her main goal is to describe the desired result without direct dictation on how to get it as the arrow function does.

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
- **Security risks:** JavaScript can be used to fetch data using AJAX or by manipulating tags that load data such as <img>, <object>, <script>. These attacks are called cross-site script attacks. They inject JS that is not part of the site into the visitor’s browser thus fetching the details. 
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

3. **Multiplication (*):**
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

8. **Compound Assignment (+=, -=, *=, /=):**
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
    ``` javascript
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
   const person = { name: 'John', age: 30 };
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
   numbers.forEach(function(number) {
     console.log(number);
   });
   ```

### 7. **Map Method:**
   - Another method for arrays that creates a new array by applying a function to each element of the original array.

   ```javascript
   const numbers = [1, 2, 3, 4, 5];
   const squaredNumbers = numbers.map(function(number) {
     return number * number;
   });
   ```

### 8. **Filter Method:**
   - Filters elements in an array based on a provided function.

   ```javascript
    const numbers = [1, 2, 3, 4, 5];
    const evenNumbers = numbers.filter(function(number) {
        return number % 2 === 0;
    });
   ```
### 9. **Reduce method:**
   - Iterates over each element in an array and accumulates a single result, often by applying a provided function to each element.

   ```javascript
    const numbers = [1, 2, 3, 4, 5];
    const sum = numbers.reduce(function(accumulator, currentValue) {
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
   let singleQuotes = 'Hello, World!';
   let doubleQuotes = "Hello, World!";
   ```

2. **String Concatenation:**
   - Strings can be concatenated using the `+` operator.

   ```javascript
   let firstName = 'John';
   let lastName = 'Doe';
   let fullName = firstName + ' ' + lastName; // "John Doe"
   ```

3. **String Length:**
   - The `length` property gives the number of characters in a string.

   ```javascript
   let message = 'Hello, World!';
   console.log(message.length); // 13
   ```

4. **Accessing Characters:**
   - Characters in a string can be accessed using square brackets.

   ```javascript
   let str = 'JavaScript';
   console.log(str[0]); // "J"
   ```

5. **String Methods:**
   - JavaScript provides various built-in methods for string manipulation, such as `toUpperCase()`, `toLowerCase()`, `charAt()`, `substring()`, `split()`, and more.

   ```javascript
   let text = 'Hello, World!';
   console.log(text.toUpperCase()); // "HELLO, WORLD!"
   ```

### Primitive vs Object Strings:

#### 1. **Primitive Strings:**
   - Strings are primitive data types in JavaScript.
   - Immutable: Once a string is created, its value cannot be changed.
   - Comparisons are done by value.

   ```javascript
   let str1 = 'Hello';
   let str2 = 'Hello';

   console.log(str1 === str2); // true
   ```

#### 2. **Object Strings:**
   - Strings can also be created using the `String` constructor, creating a string object.
   - Objects are mutable, and methods can be added to their prototype.
   - Comparisons are done by reference.

   ```javascript
   let strObj1 = new String('Hello');
   let strObj2 = new String('Hello');

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
   let fruits = ['Apple', 'Banana', 'Orange'];
   console.log(fruits[0]); // Outputs: 'Apple'
   ```

3. **Dynamic Size:**
   - Arrays in JavaScript are dynamic, meaning their size can change during runtime. You can add or remove elements as needed.

   ```javascript
   let numbers = [1, 2, 3];
   numbers.push(4); // Add an element
   numbers.pop();   // Remove the last element
   ```

4. **Heterogeneous Elements:**
   - Arrays can contain elements of different data types, including numbers, strings, objects, or even other arrays.

   ```javascript
   let mixedArray = [1, 'Hello', { key: 'value' }, [1, 2, 3]];
   ```

5. **Array Methods:**
   - JavaScript provides a variety of built-in methods for array manipulation, including `push()`, `pop()`, `shift()`, `unshift()`, `splice()`, `slice()`, `concat()`, and more.

   ```javascript
   let numbers = [1, 2, 3];
   numbers.push(4);          // Adds 4 to the end
   numbers.pop();            // Removes the last element
   numbers.shift();          // Removes the first element
   numbers.unshift(0);       // Adds 0 to the beginning
   numbers.splice(1, 0, 1.5); // Adds 1.5 at index 1
   ```

6. **Iterating Over Arrays:**
   - Arrays can be iterated using loops (e.g., `for`, `while`) or array methods like `forEach()`.

   ```javascript
   let fruits = ['Apple', 'Banana', 'Orange'];
   for (let i = 0; i < fruits.length; i++) {
     console.log(fruits[i]);
   }

   // or using forEach
   fruits.forEach(fruit => console.log(fruit));
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
   let fruits = ['Apple', 'Banana', 'Orange'];
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
   let index = fruits.findIndex(fruit => fruit === 'Banana');
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
   let allPositive = numbers.every(num => num > 0);

   // Some: checks if at least one element passes a condition
   let hasNegative = numbers.some(num => num < 0);

   // Filter: creates a new array with elements that pass a condition
   let positiveNumbers = numbers.filter(num => num > 0);

   // forEach: iterates over each element in an array
   numbers.forEach(num => console.log(num));

   // Map: creates a new array by applying a function to each element
   let squaredNumbers = numbers.map(num => num * num);

   // Reduce: accumulates a single result by applying a function to each element
   let sum = numbers.reduce((acc, num) => acc + num, 0);
   ```

Understanding these array operations is essential for effective JavaScript programming, as arrays are a fundamental data structure in the language.

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
     let greet = function(name) {
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
   console.log(greet('John')); // Outputs: "Hello, John!"

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

   greetAll('John', 'Jane', 'Doe');
   ```

#### 5. **Default Values for Parameters:**
   - Parameters can have default values, ensuring they are assigned a value if none is provided during the function call.

   ```javascript
   function greet(name = 'Guest') {
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