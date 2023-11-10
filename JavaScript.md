# JavaScript

## Table of contents
- [Introduction on JS](#introduction-to-js)
- [Features of JavaScript](#features-of-javascript)
- [Applications of JavaScript](#applications-of-javascript)
- [Limitations of JavaScript](#limitations-of-javascript)
- [Why JavaScript is known as a lightweight programming language ?](#why-javascript-is-known-as-a-lightweight-programming-language)
- [Is JavaScript Compiled or Interpreted or both ?](#is-javascript-compiled-or-interpreted-or-both)
- [Data Types]
- [Operators]
- [Loops in JS]
- [JS: Special concepts]
- [Important Functions]
- [Strings]
- [Arrays]
- [Functions]
- [Getters and Setters]
- [Exception handling]
- [This - keyword]
- [Object Oriented Programming]
- [Mixins]
- [Important Resources]

## Introduction to JS
JavaScript is a lightweight, cross-platform, single-threaded, and interpreted compiled programming language. It is also known as the scripting language for webpages. It is well-known for the development of web pages, and many non-browser environments also use it.

JavaScript is a weakly typed (allows implicit type conversions on operation involves mismatched types, instead of throwing type errors) and dynamically (also called loosely typed; variables are not directly associated with any particular type) typed language. JavaScript can be used for Client-side developments as well as Server-side developments. JavaScript is both an imperative and declarative type of language. JavaScript contains a standard library of objects, like Array, Date, and Math, and a core set of language elements like operators, control structures, and statements. 

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