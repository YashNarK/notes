# React JS
## Table of contents
- [Local Environment Setup](#setup-local-environment)
- [Basics](#basics)
- [Useful JS functions](#useful-js-functions)
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

## Important Links
1) My code sample: https://codesandbox.io/s/introduction-to-jsx-forked-j5wyjn?file=/src/index.js
2) JSX syntax: https://github.com/airbnb/javascript/blob/master/react/README.md
3) Practice: Keeper App: https://codesandbox.io/s/keeper-app-part-1-starting-forked-7ht3v2 
4) HTML, CSS notes: https://codepen.io/NarendranAI/pen/VwXENJO
