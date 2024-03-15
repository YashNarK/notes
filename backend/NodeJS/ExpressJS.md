# Table of Contents

- [Table of Contents](#table-of-contents)
- [Express JS](#express-js)
- [Express Instance - properties and methods](#express-instance---properties-and-methods)
  - [Methods:](#methods)
  - [Properties:](#properties)
  - [Example:](#example)
- [Automatic change detection and re-serving of node app - using Nodemon](#automatic-change-detection-and-re-serving-of-node-app---using-nodemon)
  - [How Nodemon Works:](#how-nodemon-works)
  - [How to Use Nodemon:](#how-to-use-nodemon)
- [Environment Variables](#environment-variables)
  - [Setting Environment Variables:](#setting-environment-variables)
  - [Accessing Environment Variables in Node.js:](#accessing-environment-variables-in-nodejs)
  - [Best Practices for Using Environment Variables:](#best-practices-for-using-environment-variables)
  - [Example](#example-1)
  - [Environment of node / express](#environment-of-node--express)
  - [Configuration management - using config](#configuration-management---using-config)
  - [Using config and env for storing both sensitive and non sensitive data](#using-config-and-env-for-storing-both-sensitive-and-non-sensitive-data)
- [Request Object in Express Instance](#request-object-in-express-instance)
- [Response Object in Express Instance](#response-object-in-express-instance)
- [Middlewares](#middlewares)
  - [1. **Middleware Functions**:](#1-middleware-functions)
  - [2. **Middleware Execution Order**:](#2-middleware-execution-order)
  - [3. **Types of Middleware**:](#3-types-of-middleware)
  - [4. **Example Middleware Usage**:](#4-example-middleware-usage)
  - [5. **Middleware Chains and Order**:](#5-middleware-chains-and-order)
  - [6. **Custom Middleware**:](#6-custom-middleware)
- [Some Important middlewares](#some-important-middlewares)
  - [Express.json() Middleware](#expressjson-middleware)
  - [Express.urlencoded() Middleware](#expressurlencoded-middleware)
    - [URL Encoded vs Query Params](#url-encoded-vs-query-params)
  - [Express.static() Middleware](#expressstatic-middleware)
  - [Helmet Middleware](#helmet-middleware)
  - [Morgan Middleware](#morgan-middleware)
- [Debug using debug package](#debug-using-debug-package)
- [Validation - using JOI](#validation---using-joi)
  - [JOI - implementation](#joi---implementation)
  - [Handling Error message](#handling-error-message)
  - [Argument of a middleware and error](#argument-of-a-middleware-and-error)
- [Templating Engines](#templating-engines)
- [Routing](#routing)
- [Database Integration](#database-integration)

# Express JS

**Express.js**, also known simply as **Express**, is a **back-end web application framework** for building **RESTful APIs** with **Node.js**. It's released as **free and open-source software** under the **MIT License**. Here are some key points about Express:

1. **Purpose**: Express is designed for creating **web applications** and **APIs**.
2. **Features**: It provides a **robust set of features** for web and mobile applications.
3. **Popularity**: Express has been called the **de facto standard server framework** for Node.js.
4. **Minimalistic**: It's fast, **unopinionated**, and minimalist, allowing developers to build applications without unnecessary constraints.
5. **HTTP Utility Methods**: Express offers a myriad of **HTTP utility methods** and middleware, making it easy to create a robust API.
6. **Performance**: It provides a thin layer of fundamental web application features without obscuring the Node.js features you know and love.
7. **Frameworks**: Many popular frameworks are based on Express.

To get started with Express, you can install it using npm:

```bash
$ npm install express --save
```

Whether you're building a simple web app or a complex API, Express is a powerful choice. Explore the [official documentation](http://expressjs.com/) to learn more!

# Express Instance - properties and methods

An instance of Express in a Node.js application exposes several methods and properties that allow you to configure routes, middleware, settings, and more.

```js
// Import Express
const express = require("express");
// Initialize Express Instance
const app = express();
```

Here's an overview of the most commonly used methods and properties of an Express application instance:

## Methods:

1. **HTTP Methods for Routing**:

   - `app.get(path, callback)`: Defines a route for handling GET requests.
   - `app.post(path, callback)`: Defines a route for handling POST requests.
   - `app.put(path, callback)`: Defines a route for handling PUT requests.
   - `app.delete(path, callback)`: Defines a route for handling DELETE requests.
   - `app.use([path], callback)`: Mounts middleware functions at the specified path or all paths.

2. **Router Methods**:

   - `app.route(path)`: Returns an instance of Express Router to handle route methods for the specified path.
   - `app.param(name, callback)`: Registers middleware to be executed for parameters in route paths.

3. **Error Handling**:

   - `app.use([error,] callback)`: Defines error-handling middleware.

4. **Server Management**:

   - `app.listen(port[, hostname][, backlog][, callback])`: Starts the Express server to listen for incoming connections on the specified port and hostname.
   - `app.close([callback])`: Stops the Express server from accepting new connections.

5. **Settings and Configuration**:

   - `app.set(setting, value)`: Sets the value of the specified setting for the application.

6. **View Rendering**:
   - `app.engine(ext, callback)`: Registers the template engine for the specified file extension.
   - `app.set('view engine', engine)`: Sets the default view engine for rendering templates.
   - `app.set('views', path)`: Sets the directory for storing view templates.

## Properties:

1. **Router**:

   - `app._router`: Contains the Express Router instance, which handles routing for the application.

2. **Settings**:

   - `app.locals`: An object representing application local variables.

3. **Mounting Points**:

   - `app.mountpath`: The path pattern(s) on which the app is mounted, if mounted as middleware.

4. **Environment**:
   - `app.get('env')`: Retrieves the value of the NODE_ENV environment variable.

## Example:

```javascript
const express = require("express");
const app = express();

// Define routes
app.get("/", (req, res) => {
  res.send("Hello, World!");
});

// Start the server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

In this example, `app` is an instance of Express. We use methods like `app.get()` to define routes and `app.listen()` to start the server. These methods and properties provide the foundation for building robust and scalable web applications with Express.js.

# Automatic change detection and re-serving of node app - using Nodemon

`nodemon` is a utility for Node.js that monitors changes in your application's source files and automatically restarts the server when changes are detected. It's commonly used during the development process to streamline the development workflow by eliminating the need to manually stop and restart the server after making changes to the codebase.

Here's how `nodemon` works and how you can use it:

## How Nodemon Works:

1. **Monitoring File Changes**:

   - Nodemon continuously monitors the files in your Node.js application's directory (and subdirectories) for any changes.
   - It watches for changes in JavaScript files, JSON files, or any other files specified in the nodemon configuration.

2. **Restarting the Server**:

   - When a change is detected in any of the monitored files, nodemon automatically restarts the Node.js server.
   - It terminates the current server process and starts a new one, ensuring that the latest changes in the codebase are reflected in the running server.

3. **Customization and Configuration**:
   - Nodemon provides options to customize its behavior through configuration files or command-line arguments.
   - You can specify additional directories or file types to monitor, ignore specific files or directories, and configure other settings according to your project requirements.

## How to Use Nodemon:

1. **Installation**:

   - You can install nodemon globally or locally in your project's directory using npm or yarn.
   - Global Installation: `npm install -g nodemon`
   - Local Installation (recommended for project-specific usage): `npm install --save-dev nodemon`

2. **Running Your Application**:

   - Instead of running your Node.js application with the `node` command, use `nodemon` followed by the entry file of your application.
   - For example: `nodemon index.js` or `nodemon app.js`

3. **Automatic Restart**:

   - Nodemon will start your application and monitor changes in the source files.
   - Whenever you save changes to a file, nodemon will automatically restart the server to reflect those changes.

4. **Customization**:

   - You can create a `nodemon.json` or `nodemon.config.js` file in your project's root directory to specify nodemon configuration options.
   - Configuration options include specifying additional directories to monitor, ignoring specific files or directories, setting delay before restarting, etc.

5. **Usage with npm Scripts**:
   - You can also add nodemon as a script in your `package.json` file for easier usage.
   - For example:
     ```json
     "scripts": {
       "start": "nodemon index.js"
     }
     ```

Using `nodemon` can significantly improve your development workflow by automating the process of restarting the server whenever changes are made to your codebase. It's a valuable tool for Node.js developers, especially during the iterative process of writing and testing code.

# Environment Variables

Environment variables are dynamic values that are set outside of an application and can be accessed by the application during runtime. They are used to configure the behavior of applications and provide a flexible way to customize settings without modifying the source code. Environment variables are commonly used to store sensitive information, such as API keys, database credentials, or configuration settings, that should not be hardcoded into the application.

Here's how environment variables work and how they are typically used in applications:

## Setting Environment Variables:

1. **Shell Environment**:

   - Environment variables can be set in the shell or command-line interface using export (Unix-like systems) or set (Windows) commands.
   - Example (Unix-like systems): `export API_KEY=your-api-key`

2. **Environment Files**:
   - Environment variables can also be defined in environment configuration files (e.g., .env files) for easier management and consistency.
   - Example (.env file):
     ```
     API_KEY=your-api-key
     DB_HOST=localhost
     ```

## Accessing Environment Variables in Node.js:

1. **Using `process.env`**:
   - In Node.js, environment variables are accessed using the `process.env` object.
   - Example: `const apiKey = process.env.API_KEY;`

## Best Practices for Using Environment Variables:

1. **Sensitive Information**:

   - Environment variables should be used to store sensitive information like API keys, passwords, and access tokens instead of hardcoding them in the source code.
   - This helps prevent accidental exposure of sensitive data and improves security.

2. **Configuration Settings**:

   - Environment variables can be used to configure application settings such as database connection strings, server ports, logging levels, etc.
   - This allows for easy configuration of the application across different environments (development, staging, production) without modifying the source code.

3. **Consistency**:

   - Use consistent naming conventions for environment variables to improve readability and maintainability.
   - For example, use uppercase letters and underscores to separate words in variable names (e.g., `DATABASE_URL`, `API_KEY`).

4. **Environment-Specific Variables**:

   - Define environment-specific variables to customize behavior based on the current environment (e.g., development, production).
   - Use default values or fallbacks for variables that may not be set in all environments.

5. **Secure Handling**:
   - Avoid storing sensitive information directly in environment files or version control systems.
   - Use tools like `.env` files or environment management libraries to securely manage environment variables.

Environment variables provide a flexible and secure way to configure applications and manage sensitive information. By following best practices for using environment variables, you can improve the security, flexibility, and maintainability of your Node.js applications.

## Example

- index.js

```js
// importing modules
const express = require("express");

const app = express();

app.get("/", (req, res) => {
  res.send("hello");
});
// api listening using PORT value set by env
port = process.env.PORT || 3000;
app.listen(port, () =>
  debug(`server is listening on http://localhost:${port}`)
);
```

- on console

```bash
$ PORT=4000 nodemon app.js

# OUTPUT
# [nodemon] 3.1.0
# [nodemon] to restart at any time, enter `rs`
# [nodemon] watching path(s): *.*
# [nodemon] watching extensions: js,mjs,cjs,json
# [nodemon] starting `node app.js`
# server is listening on http://localhost:4000
```

## Environment of node / express

Differences between `process.env.NODE_ENV` and `app.get('env')` in the context of **Express.js**.

1. **`process.env.NODE_ENV`**:

   - `process.env` is a global Node.js object that contains environment variables.
   - `NODE_ENV` is a specific environment variable used to determine the application's environment (e.g., development, production, testing).
   - By convention, its value is set to `'development'` by default if not explicitly defined.
   - Developers can set it manually based on their deployment environment (e.g., in a `.env` file or server configuration).

2. **`app.get('env')`**:

   - In Express.js, `app.get('env')` returns the current environment mode set for the application.
   - By default, it returns `'development'` if `NODE_ENV` is not explicitly defined.
   - You can customize it using `app.set('env', 'production')` or other values.

3. **When to Use Which**:

   - Use `process.env.NODE_ENV` when you want to access the raw environment variable directly (e.g., for conditional logic in your code).
   - Use `app.get('env')` when you want to retrieve the environment mode specifically set for your Express application.

4. **Example Usage**:

   - Suppose you want to enable certain features only in development mode:
     ```javascript
     if (process.env.NODE_ENV === "development") {
       // Enable additional logging or debugging features
     }
     ```

5. **Best Practices**:
   - Set `NODE_ENV` explicitly in your deployment environment (e.g., production server, testing environment).
   - Use `app.get('env')` to access the environment mode within your Express routes and middleware.

In summary, both `process.env.NODE_ENV` and `app.get('env')` serve similar purposes, but the latter is specific to Express and provides a convenient way to access the application's environment mode.

For more information, refer to the [official Express.js documentation on app settings](http://expressjs.com/en/api.html#app.settings) .

## Configuration management - using config

- **Non sensitive data management for different environments**
- **Sensitive data must always be managed using env variables or .env files**
  The **`config`** package is a powerful tool for managing configuration settings in your Node.js applications, including those built with Express.js. Let's explore how it works and how you can use it:

1. **What Is `config`?**

   - The **`config`** package provides a convenient way to organize and manage configuration settings for your Node.js applications.
   - It allows you to define configuration parameters in a structured manner, separate from your application code.
   - With `config`, you can easily switch between different environments (development, production, testing) without modifying your code.

2. **How `config` Works**:

   - `config` reads configuration files (usually in JSON or YAML format) from a specified directory.
   - It merges settings from multiple files (e.g., default, environment-specific, custom overrides) into a single configuration object.
   - You can access these settings programmatically within your application.

3. **Installation**:

   - Install the `config` package using npm or yarn:
     ```
     npm install config
     ```

4. **Configuration Files**:

   - Create a directory (usually named `config`) in your project.
   - Inside this directory, define configuration files (e.g., `default.json`, `production.json`, `custom.json`).
   - Example configuration file (`default.json`):
     ```json
     {
       "apiKey": "your-api-key",
       "port": 3000
     }
     ```

5. **Usage in Your Application**:

   - Load the configuration in your Node.js or Express application:

     ```javascript
     const config = require("config");

     // Access configuration settings
     console.log(config.get("apiKey"));
     ```

6. **Benefits of Using `config`**:

   - Centralized configuration management: Keep all your settings in one place.
   - Easily switch between different environments by using separate configuration files.
   - Avoid hardcoding sensitive information (e.g., API keys) directly in your code.

7. **Customization**:
   - Customize the configuration file names (e.g., `myapp.json`) and file formats (JSON, YAML, etc.).
   - Define default values for settings in your code.

In summary, the `config` package simplifies configuration management by providing a clean and consistent way to handle settings across different environments. It's a great choice for managing configuration in your Express.js applications!

For more information, check out the [official `config` documentation](https://www.npmjs.com/package/config) ..

## Using config and env for storing both sensitive and non sensitive data

To load sensitive keys from a `.env` file into your `development.json` and `production.json` configuration files using the `config` package, you can follow these steps:

1. Install the `dotenv` package:

   ```
   npm install dotenv
   ```

2. Create a `.env` file in the root directory of your project and add your sensitive keys:

   ```
   SENSITIVE_KEY_1=your_sensitive_value_1
   SENSITIVE_KEY_2=your_sensitive_value_2
   ```

3. Create or update your `development.json` and `production.json` configuration files. In these files, you can access the sensitive keys using `process.env` after loading them with `dotenv`.

   Example `development.json`:

   ```json
   {
     "development": {
       "sensitive_key_1": "${process.env.SENSITIVE_KEY_1}",
       "sensitive_key_2": "${process.env.SENSITIVE_KEY_2}",
       "other_config_key": "other_config_value"
     }
   }
   ```

   Example `production.json`:

   ```json
   {
     "production": {
       "sensitive_key_1": "${process.env.SENSITIVE_KEY_1}",
       "sensitive_key_2": "${process.env.SENSITIVE_KEY_2}",
       "other_config_key": "other_config_value"
     }
   }
   ```

4. Load the environment variables from the `.env` file at the beginning of your application or before loading the configuration files. This ensures that the sensitive keys are available in the `process.env` object.

   Example in your application entry point (`app.js` or `index.js`):

   ```javascript
   require("dotenv").config(); // Load environment variables from .env file

   // Now you can load your configuration files using the config package
   const config = require("config");
   ```

With this setup, the `development.json` and `production.json` configuration files will contain the sensitive keys loaded from the `.env` file using `process.env`. This approach keeps your sensitive keys secure while allowing you to manage configuration files for different environments using the `config` package. Remember to keep your `.env` file out of version control to prevent sensitive information from being exposed.

# Request Object in Express Instance

```js
// ....
app.get("/", (req, res) => {
  res.send("hello");
});
// ....
```

In Express.js, `req` is an object that represents the HTTP request sent by the client to the server. It contains various properties and methods that provide information about the incoming request and allow you to access its data.

Here's a detailed explanation of the `req` object:

1. **req.method**:

   - Contains the HTTP method used by the client to make the request (e.g., GET, POST, PUT, DELETE).

2. **req.url**:

   - Contains the URL path of the request (excluding the domain name and query parameters).

3. **req.params**:

   - Contains an object representing the route parameters extracted from the URL path. For example, if your route is defined as `/users/:userId`, and the URL is `/users/123`, `req.params.userId` will be `'123'`.

4. **req.query**:

   - Contains an object representing the query parameters of the URL. For example, if the URL is `/search?q=node`, `req.query.q` will be `'node'`.

5. **req.headers**:

   - Contains an object representing the HTTP headers sent in the request. You can access specific headers using property names (e.g., `req.headers['content-type']`).

6. **req.body**:

   - Contains the parsed body of the request, typically used with POST or PUT requests where data is sent in the request body. This property is populated by middleware such as `body-parser`.

7. **req.cookies**:

   - Contains an object representing the cookies sent by the client with the request. This property is populated by middleware such as `cookie-parser`.

8. **req.ip**:

   - Contains the IP address of the client making the request.

9. **req.path**:

   - Similar to `req.url`, contains the URL path of the request.

10. **req.protocol**:

    - Contains the protocol (HTTP or HTTPS) used by the client to make the request.

11. **req.hostname**:

    - Contains the hostname of the server where the request was received.

12. **req.xhr**:
    - Indicates whether the request was made via XMLHttpRequest (true) or not (false).

These are just a few of the properties and methods available on the `req` object. Depending on the specific needs of your application, you may find other properties and methods provided by Express or third-party middleware that can be accessed via `req`. The `req` object allows you to access various aspects of the incoming HTTP request and process them accordingly in your Express.js application.

# Response Object in Express Instance

```js
// ....
app.get("/", (req, res) => {
  res.send("hello");
});
// ....
```

In Express.js, `res` is an object representing the HTTP response that the server sends back to the client. It contains various methods and properties that allow you to control the response sent to the client.

Here's a detailed explanation of the `res` object:

1. **res.status(code)**:

   - Sets the HTTP status code of the response. For example, `res.status(200)` sets the status code to 200 (OK).

2. **res.send([body])**:

   - Sends the HTTP response to the client with the specified body. The body can be a string, buffer, JSON object, or array. If no body is provided, it sends an empty response.

3. **res.json([body])**:

   - Sends a JSON response to the client. It serializes the JSON object or array passed as the body and sets the appropriate Content-Type header.

4. **res.sendFile(path[, options][, callback])**:

   - Sends a file as the response to the client. It sets the appropriate Content-Type header based on the file extension.

5. **res.redirect([status,] path)**:

   - Redirects the client to the specified URL path with an optional status code. If no status code is provided, it defaults to 302 (Found).

6. **res.setHeader(name, value)**:

   - Sets a single header on the response. For example, `res.setHeader('Content-Type', 'text/html')` sets the Content-Type header to 'text/html'.

7. **res.getHeaders()**:

   - Returns an object containing all the headers that have been set on the response.

8. **res.cookie(name, value[, options])**:

   - Sets a cookie on the client's browser. You can specify options such as the expiration date, domain, and secure flag.

9. **res.clearCookie(name[, options])**:

   - Clears a cookie previously set on the client's browser.

10. **res.statusMessage**:

    - Contains the HTTP status message associated with the status code set using `res.status()`.

11. **res.locals**:

    - An object that contains local variables scoped to the request. These variables are available to the view engine when rendering templates.

12. **res.end([data])**:
    - Ends the response process. If data is provided, it sends it as the last chunk of the response body.

These are just a few of the methods and properties available on the `res` object. Depending on the specific requirements of your application, you may find other methods and properties provided by Express.js or third-party middleware that can be accessed via `res`. The `res` object allows you to control the HTTP response sent back to the client in your Express.js application.

# Middlewares

In the context of Express.js, **middlewares** are essential components that handle requests and responses during the lifecycle of an HTTP request. They sit between the server receiving a request and sending a response back to the client. Here are the key points about middlewares:

## 1. **Middleware Functions**:

- Middleware functions are JavaScript functions that have access to three important objects:
  - **Request Object (`req`)**: Represents the incoming HTTP request.
  - **Response Object (`res`)**: Represents the response that will be sent back to the client.
  - **Next Function (`next`)**: A special function that allows you to pass control to the next middleware in the stack.
- Middleware functions can:
  - Execute custom code.
  - Modify the request and response objects.
  - End the request-response cycle.
  - Call the next middleware function.

## 2. **Middleware Execution Order**:

- Express applications are essentially a series of middleware function calls.
- The order in which you define and use middleware matters. Middleware functions are executed in the order they are added to the application.

## 3. **Types of Middleware**:

- Express supports several types of middleware:
  - **Application-level Middleware**: Bound to the entire Express application using `app.use()` or specific HTTP methods (`app.get()`, `app.post()`, etc.). These execute for every request.
  - **Router-level Middleware**: Associated with specific routes or route groups. You can mount them using `router.use()` or directly within route handlers.
  - **Error-handling Middleware**: Used to handle errors (e.g., 404 Not Found, server errors). These are defined with four parameters (including `err`) and placed at the end of the middleware chain.
  - **Built-in Middleware**: Provided by Express itself (e.g., `express.json()`, `express.urlencoded()`).
  - **Third-party Middleware**: Developed by the community (e.g., `morgan` for logging, `helmet` for security).

## 4. **Example Middleware Usage**:

- Let's look at some examples:

  ```javascript
  const express = require("express");
  const app = express();

  // Application-level middleware (executed for every request)
  app.use((req, res, next) => {
    console.log("Time:", Date.now());
    next(); // Pass control to the next middleware
  });

  // Router-level middleware (mounted on /user/:id path)
  app.use("/user/:id", (req, res, next) => {
    console.log("Request Type:", req.method);
    next();
  });

  // Route handler (GET request to /user/:id)
  app.get("/user/:id", (req, res, next) => {
    res.send("USER");
  });

  // Middleware sub-stack (multiple middleware functions)
  app.use(
    "/user/:id",
    (req, res, next) => {
      console.log("Request URL:", req.originalUrl);
      next();
    },
    (req, res, next) => {
      console.log("Request Type:", req.method);
      next();
    }
  );
  ```

## 5. **Middleware Chains and Order**:

- Middleware functions are executed in the order they are defined.
- If a middleware function doesn't end the request-response cycle (by sending a response or calling `next()`), it must call `next()` to pass control to the next middleware.
- The first middleware in the chain is executed first, followed by subsequent ones.

## 6. **Custom Middleware**:

- You can create your own custom middleware functions to handle authentication, logging, error handling, and more.
- Middleware allows you to modularize your application logic and keep your route handlers clean.

Remember, mastering middleware is crucial for building robust Express applications. Dive into the details, experiment, and create your own custom middleware to enhance your Express.js projects!

For more in-depth examples and practical use cases, check out the [official Express.js documentation on using middleware](http://expressjs.com/en/guide/using-middleware.html) . Additionally, explore community articles and tutorials to deepen your understanding .

# Some Important middlewares

## Express.json() Middleware

1. **Purpose**:

   - The `express.json()` middleware is a built-in function in Express.js.
   - It parses incoming HTTP requests with **JSON payloads** (i.e., requests containing data in JSON format).
   - It is based on the popular middleware library called **body-parser**.

2. **Functionality**:

   - When a client sends a request with a JSON payload (usually in the request body), `express.json()` parses that data and converts it into a JavaScript object.
   - This middleware is commonly used for handling data submitted via forms, APIs, or AJAX requests.

3. **Usage**:

   - To use `express.json()`, add it to your Express application as middleware:

     ```javascript
     const express = require("express");
     const app = express();

     // Add express.json() middleware
     app.use(express.json());

     // Your route handlers go here...
     ```

4. **Options**:

   - The `express.json()` function accepts an optional `options` parameter with various properties. Some common ones include:
     - **`inflate`**: Determines whether to decompress the request body if it's compressed (e.g., with gzip). Default is `true`.
     - **`limit`**: Sets the maximum request body size (in bytes). Default is `'100kb'`.
     - **`type`**: Specifies the content type to parse. Default is `'application/json'`.

5. **Example**:

   - Suppose you receive a POST request with the following JSON payload:
     ```json
     {
       "name": "Alice",
       "age": 30
     }
     ```
   - Using `express.json()`, you can access this data in your route handler:
     ```javascript
     app.post("/user", (req, res) => {
       const { name, age } = req.body;
       console.log(`Received user: ${name}, age: ${age}`);
       res.send("User data received successfully!");
     });
     ```

6. **Note**:
   - Ensure that the client sets the `Content-Type` header to `'application/json'` when sending the request.
   - If the request body is not valid JSON, `express.json()` will throw an error.

In summary, `express.json()` simplifies handling JSON data in your Express application. It's a powerful tool for processing incoming requests with JSON payloads.

## Express.urlencoded() Middleware

1. **Purpose**:

   - The `express.urlencoded()` middleware is a built-in function in Express.js.
   - It parses incoming HTTP requests with **URL-encoded payloads** (usually form data submitted via HTML forms).
   - It is based on the popular middleware library called **body-parser**.

2. **Functionality**:

   - When a client sends a request with URL-encoded data (e.g., form submissions), `express.urlencoded()` parses that data and converts it into a JavaScript object.
   - This middleware is commonly used for handling form submissions, query parameters, and other data sent via `application/x-www-form-urlencoded` content type.

3. **Usage**:

   - To use `express.urlencoded()`, add it to your Express application as middleware:

     ```javascript
     const express = require("express");
     const app = express();

     // Add express.urlencoded() middleware
     app.use(express.urlencoded({ extended: true }));

     // Your route handlers go here...
     ```

4. **Options**:

   - The `express.urlencoded()` function accepts an optional `options` parameter with various properties. Some common ones include:
     - **`extended`**: Determines whether to use the `querystring` library (`false`) or the `qs` library (`true`) for parsing. Default is `true`.
     - **`limit`**: Sets the maximum request body size (in bytes). Default is `'100kb'`.

5. **Example**:

   - Suppose you receive a POST request with the following URL-encoded data:
     ```
     Name=Pikachu&Type=Banana&Number+In+Stable=12
     ```
   - Using `express.urlencoded()`, you can access this data in your route handler:
     ```javascript
     app.post("/profile", (req, res) => {
       const { Name, Type, "Number In Stable": NumberInStable } = req.body;
       console.log(
         `Received profile: Name=${Name}, Type=${Type}, NumberInStable=${NumberInStable}`
       );
       res.send("Profile data received successfully!");
     });
     ```

6. **Note**:
   - Ensure that the client sets the `Content-Type` header to `'application/x-www-form-urlencoded'` when sending the request.
   - If the request body is not valid URL-encoded data, `express.urlencoded()` will throw an error.

In summary, `express.urlencoded()` simplifies handling form data and URL-encoded payloads in your Express application. It's a powerful tool for processing incoming requests with form submissions.

### URL Encoded vs Query Params

Query parameters and URL-encoded data are two common ways to pass data from a client to a server in an HTTP request, but they serve different purposes and have different formats:

1. **Query Parameters**:

   - **Purpose**: Query parameters are used to append data to the URL of an HTTP request.
   - **Format**: Query parameters are key-value pairs separated by an ampersand (`&`) and appended to the URL after a question mark (`?`). Each key-value pair is separated by an equals sign (`=`). For example: `http://example.com/api/users?id=123&name=John`.
   - **Usage**: Query parameters are typically used for filtering, sorting, or specifying additional information about the requested resource. They are commonly used in GET requests.
   - **Example**:
     ```
     GET /api/users?id=123&name=John HTTP/1.1
     Host: example.com
     ```

2. **URL-Encoded Data**:

   - **Purpose**: URL-encoded data is used to send form data in the body of an HTTP request.
   - **Format**: URL-encoded data consists of key-value pairs encoded in the body of the request using the `application/x-www-form-urlencoded` MIME type. Each key-value pair is separated by an ampersand (`&`), and the key and value are separated by an equals sign (`=`). For example: `id=123&name=John`.
   - **Usage**: URL-encoded data is commonly used in form submissions, where form data is sent to the server as part of a POST request. It allows clients to send structured data in the body of the request.
   - **Example**:

     ```
     POST /api/users HTTP/1.1
     Host: example.com
     Content-Type: application/x-www-form-urlencoded

     id=123&name=John
     ```

In summary, query parameters are appended to the URL of an HTTP request and are typically used for filtering or specifying additional information about the requested resource, while URL-encoded data is sent in the body of the request and is commonly used for submitting form data to the server. Both methods allow clients to send data to the server, but they serve different purposes and are used in different contexts.

## Express.static() Middleware

1. **Purpose**:

   - The `express.static()` middleware is a built-in function in Express.js.
   - Its primary purpose is to **serve static files** (such as images, CSS files, and JavaScript files) to clients (browsers, mobile apps, etc.) via HTTP requests.
   - It allows you to make these files accessible to users without any additional server-side processing.

2. **How It Works**:

   - When a client requests a static file (e.g., an image or a CSS file), `express.static()` looks for that file in a specified directory on the server.
   - If the file exists, it sends it back to the client as the HTTP response.
   - If the file doesn't exist, it passes control to the next middleware in the pipeline.

3. **Usage**:

   - To use `express.static()`, you specify the **root directory** from which to serve static assets:

     ```javascript
     const express = require("express");
     const app = express();

     // Serve static files from the 'public' directory
     app.use(express.static("public"));

     // Your route handlers go here...
     ```

4. **Example**:

   - Suppose you have an image file named `logo.png` in a directory called `public`.
   - By using `express.static()`, you can serve this image to clients:
     ```javascript
     // Serve files from the 'public' directory
     app.use(express.static("public"));
     ```
   - Now, clients can access the image via the URL: `http://example.com/logo.png`.

5. **Virtual Paths**:

   - You can create a **virtual path prefix** (where the path doesn't actually exist in the file system) for files served by `express.static()`.
   - For example:
     ```javascript
     // Serve files from the 'public' directory under the '/static' path
     app.use("/static", express.static("public"));
     ```
   - Now, clients can access the image using the URL: `http://example.com/static/logo.png`.

6. **Best Practices**:
   - Use a **reverse proxy cache** to improve performance when serving static assets.
   - If you run your Express app from a different directory, use an **absolute path** for the directory you want to serve:
     ```javascript
     const path = require("path");
     app.use("/static", express.static(path.join(__dirname, "public")));
     ```

In summary, `express.static()` is a powerful middleware for serving static files in Express.js applications. It simplifies the process of making assets (images, stylesheets, scripts) accessible to clients.

## Helmet Middleware

```bash
# Third party middleware so needs installation
npm i helmet
```

1. **What Is Helmet?**

   - **Helmet** is a collection of smaller middleware functions for Express.js.
   - Its primary purpose is to enhance the security of your Express applications by setting various **HTTP response headers**.
   - While it's not a silver bullet, Helmet helps protect your app from common web vulnerabilities.

2. **Why Use Helmet?**

   - By default, Express applications do not include security HTTP headers.
   - Helmet automatically adds or removes headers to comply with web security standards.
   - It mitigates risks related to security threats such as **Cross-Site Scripting (XSS)** and **clickjacking attacks**.

3. **How to Use Helmet**:

   - To use Helmet, add it to your Express application as middleware:

     ```javascript
     const express = require("express");
     const helmet = require("helmet");
     const app = express();

     // Use Helmet middleware
     app.use(helmet());

     // Your route handlers go here...
     ```

4. **What Does Helmet Do?**

   - Helmet includes 11 smaller middleware functions (such as **Content Security Policy**, **Strict Transport Security**, and **Referrer Policy**).
   - These functions are enabled by default when you use `app.use(helmet())`.
   - You can also disable specific middleware functions or customize their options.

5. **Example Usage**:

   - Suppose you want to set a **Content Security Policy (CSP)** header:
     ```javascript
     app.use(
       helmet.contentSecurityPolicy({
         directives: {
           defaultSrc: ["'self'"],
           scriptSrc: ["'self'", "trusted-cdn.com"],
           // Add more directives as needed
         },
       })
     );
     ```

6. **Additional Headers Set by Helmet**:
   - Apart from CSP, Helmet also sets headers like **Strict-Transport-Security**, **X-Content-Type-Options**, and **X-Frame-Options**.
   - These headers improve security by preventing certain types of attacks.

In summary, Helmet is a powerful middleware for securing your Express applications. By using it, you enhance your app's resilience against common security threats.

For more information, refer to the [official Helmet documentation](https://helmetjs.github.io/docs/) .

## Morgan Middleware

1. **What Is Morgan?**

   - **Morgan** is a popular **HTTP request logger middleware** for Node.js and Express.
   - Its primary purpose is to log information about incoming HTTP requests, making it useful for debugging, monitoring, and analyzing server activity.

2. **How Morgan Works**:

   - When a client sends an HTTP request to your Express server, Morgan captures relevant details about that request.
   - It logs information such as the request method, URL, response status, response time, and more.
   - Morgan can be customized to log in various formats, from concise to detailed.

3. **Usage**:

   - To use Morgan, add it to your Express application as middleware:

     ```javascript
     const express = require("express");
     const morgan = require("morgan");
     const app = express();

     // Use Morgan middleware
     app.use(morgan("tiny"));

     // Your route handlers go here...
     ```

4. **Predefined Formats**:

   - Morgan provides several predefined formats (or **tokens**) for logging. Some common ones include:
     - **`tiny`**: Minimal output (e.g., `GET / 200 5ms - 123b`).
     - **`combined`**: Standard Apache combined log output.
     - **`dev`**: Colored output for development use.
     - **`short`**: Shorter than default, including response time.
     - **`common`**: Standard Apache common log output.

5. **Customization**:

   - You can create custom formats by defining your own tokens.
   - For example, to log the client's IP address:
     ```javascript
     morgan.token("client-ip", (req) => req.ip);
     app.use(
       morgan(
         ":client-ip - :method :url :status :res[content-length] - :response-time ms"
       )
     );
     ```

6. **Security Considerations**:
   - Be cautious when using Morgan in production environments. Avoid logging sensitive information.
   - Use Morgan with appropriate log levels (e.g., debug, info, error) based on your application's needs.

In summary, Morgan is a valuable tool for understanding your Express server's traffic and diagnosing issues. It's like having a traffic monitor for your application!

For more information, refer to the [official Morgan documentation](https://github.com/expressjs/morgan).

# Debug using debug package

To use the `debug` package with Express.js for debugging, you can follow these steps:

1. Install the `debug` package:

   ```
   npm install debug
   ```

2. In your Express application, require the `debug` package and initialize it with a namespace specific to your application. You can choose any namespace you like, but it's common to use a namespace related to your project or specific modules within your project.

   Example:

   ```javascript
   const debug = require("debug")("myapp:server");
   ```

3. Place debug statements strategically throughout your codebase to log useful information during development. You can use `debug` to log messages, variables, or any other information that helps you debug your application.

   Example:

   ```javascript
   debug("Starting server...");
   ```

4. Run your application with the `DEBUG` environment variable set to the namespace you specified when initializing `debug`.

   On Unix-like systems (Linux, macOS):

   ```
   DEBUG=myapp:server node app.js
   ```

   On Windows:

   ```
   set DEBUG=myapp:server
   node app.js
   ```

   Alternatively, you can set the `DEBUG` environment variable directly in your code before requiring the `debug` package:

   ```javascript
   process.env.DEBUG = "myapp:server";
   const debug = require("debug")("myapp:server");
   ```

5. When you run your application with the `DEBUG` environment variable set, the `debug` package will output debug messages to the console with the specified namespace. This allows you to filter debug output based on namespaces, which can be helpful for debugging specific parts of your application.

   Example output:

   ```
   myapp:server Starting server...
   ```

By using the `debug` package with Express.js, you can easily add debug statements to your codebase and control debug output based on namespaces, making it easier to debug and troubleshoot your application during development.

# Validation - using JOI

**Joi** is a powerful schema description language and data validator for **JavaScript**. It ensures that users input valid data into forms or any type of input in applications. Here are some key points about **Joi**:

1. **Purpose**: Joi compels users to provide valid data by verifying that all required fields are present and rejecting unspecified or unwanted objects in the data.

2. **Usage**: It is commonly used to validate data sent from API endpoints, allowing you to create blueprints for the data types you want to accept.

3. **Installation**: To start using Joi in your project, run `npm i joi`.

4. **Documentation and Support**: Visit the [joi.dev Developer Portal](https://joi.dev/) for tutorials, documentation, and support.

Whether you're building a Node.js application with Express or working on other JavaScript projects, Joi offers a flexible API for defining and validating various data types, including HTTP request parameters, query parameters, and request bodies.

## JOI - implementation

Below is an example of how to use **Joi** for request schema validation in a **Node.js** + **Express** API. We'll create middleware functions to validate incoming data.

1. **Install Joi**:
   First, make sure you have **Joi** installed in your project. You can do this by running:

   ```
   npm install joi
   ```

   or using **Yarn**:

   ```
   yarn add joi
   ```

2. **Create Middleware for Account Creation**:
   The following middleware validates a request to create a new account. It defines schema rules using **Joi** and checks if the request data adheres to those rules:

   ```javascript
   const Joi = require("joi");

   function createAccountSchema(req, res, next) {
     const schema = Joi.object({
       title: Joi.string().required(),
       firstName: Joi.string().required(),
       lastName: Joi.string().required(),
       email: Joi.string().email().required(),
       password: Joi.string().min(6).required(),
       confirmPassword: Joi.string().valid(Joi.ref("password")).required(),
       role: Joi.string().valid("Admin", "User").required(),
     });

     const options = {
       abortEarly: false, // Include all errors
       allowUnknown: true, // Ignore unknown properties
       stripUnknown: true, // Remove unknown properties
     };

     const { error, value } = schema.validate(req.body, options);

     if (error) {
       // On validation failure, return comma-separated errors
       next(
         `Validation error: ${error.details.map((x) => x.message).join(", ")}`
       );
     } else {
       // On success, replace req.body with validated value and trigger next middleware
       req.body = value;
       next();
     }
   }

   // Usage in your route:
   app.post("/create-account", createAccountSchema, (req, res) => {
     // Handle account creation logic here
     // req.body now contains validated data
   });
   ```

3. **Create Middleware for Account Update**:
   Similar to the account creation schema, here's an example for updating an existing account:

   ```javascript
   function updateAccountSchema(req, res, next) {
     const schema = Joi.object({
       title: Joi.string().empty(""), // All properties are optional
       firstName: Joi.string().empty(""),
       lastName: Joi.string().empty(""),
       email: Joi.string().email().empty(""),
       password: Joi.string().min(6).empty(""),
       confirmPassword: Joi.string().valid(Joi.ref("password")).empty(""),
       role: Joi.string().valid("Admin", "User").empty(""),
     });

     const options = {
       abortEarly: false, // Include all errors
       allowUnknown: true, // Ignore unknown properties
       stripUnknown: true, // Remove unknown properties
     };

     const { error, value } = schema.validate(req.body, options);

     if (error) {
       next(
         `Validation error: ${error.details.map((x) => x.message).join(", ")}`
       );
     } else {
       req.body = value;
       next();
     }
   }

   // Usage in your route:
   app.put("/update-account/:accountId", updateAccountSchema, (req, res) => {
     // Handle account update logic here
     // req.body now contains validated data
   });
   ```

Remember to adapt these examples to your specific use case and customize the schema rules according to your application's requirements. Happy coding!

## Handling Error message

When an error occurs during request validation using **Joi** (or any other middleware), the responsibility of handling the error message depends on the context of your application. Here are some common approaches:

1. **Logging to Console (Server-side)**:

   - If you want to log the error message to the server console (for debugging purposes or monitoring), you can do so within your middleware function. For example:
     ```javascript
     function createAccountSchema(req, res, next) {
       // ... (validation logic)
       if (error) {
         console.error(
           "Validation error:",
           error.details.map((x) => x.message).join(", ")
         );
         // Continue with next middleware or return an error response
       } else {
         // ...
       }
     }
     ```

2. **Sending Error Response to Frontend (Client-side)**:

   - If you want to send the error message back to the client (frontend), you can handle it in your route handler after the validation middleware. For example:
     ```javascript
     app.post("/create-account", createAccountSchema, (req, res) => {
       // Handle account creation logic here
       // req.body now contains validated data
       // ...
     });
     ```
     If an error occurs during validation, you can send an error response to the frontend:
     ```javascript
     app.post("/create-account", createAccountSchema, (req, res) => {
       // Handle account creation logic here
       // req.body now contains validated data
       // ...
       if (error) {
         return res
           .status(400)
           .json({ error: "Validation error", details: error.details });
       }
       // ...
     });
     ```

3. **Custom Error Middleware (Centralized Handling)**:

   - You can create a custom error-handling middleware that catches errors from all routes and sends a consistent error response to the client. This middleware should be placed after all other route handlers.
     ```javascript
     // Custom error middleware
     app.use((err, req, res, next) => {
       console.error("Error:", err); // Log the error
       res.status(500).json({ error: "Internal server error" }); // Send a generic error response
     });
     ```

4. **Integration with Express Error Handling**:

   - Express provides built-in error handling middleware. You can use `next(err)` to pass an error to the next middleware in the chain. If you don't handle it explicitly, Express will automatically invoke the default error handler.

     ```javascript
     app.post("/create-account", createAccountSchema, (req, res, next) => {
       // Handle account creation logic here
       // req.body now contains validated data
       // ...
       if (error) {
         next(error); // Pass the error to the default error handler
       }
       // ...
     });

     // Default error handler (placed at the end of your middleware chain)
     app.use((err, req, res, next) => {
       console.error("Error:", err); // Log the error
       res.status(500).json({ error: "Internal server error" }); // Send a generic error response
     });
     ```

Remember to adapt these approaches based on your application's architecture, error handling strategy, and the level of detail you want to expose to the client. Choose the one that best fits your use case!

## Argument of a middleware and error

1. **Error Handling in Middleware**:

   - In the middleware function, when you encounter a validation error (using **Joi** or any other validation library), you can create an error object with a descriptive message.
   - The `next()` function is used to pass control to the next middleware in the chain. By convention, if you pass an argument to `next()`, Express interprets it as an error.
   - The argument you pass to `next()` (in this case, the error message) is then handled by the next middleware or the default error handler.

2. **Custom Error Message**:

   - In your example, you're constructing a custom error message using template literals:
     ```javascript
     next(
       `Validation error: ${error.details.map((x) => x.message).join(", ")}`
     );
     ```
   - Here, `error` is not a predefined variable; it's just a placeholder for the error object that you create within your middleware.

3. **Express Error Handling**:

   - When you call `next()` with an argument (an error), Express looks for a middleware function specifically designed to handle errors.
   - If you have a custom error-handling middleware (usually defined after all your route handlers), it will catch this error and handle it.
   - If you don't have a custom error handler, Express will use its default error handler, which logs the error to the console and sends a generic error response to the client.

4. **Custom Error Middleware Example**:

   - Here's an example of a custom error middleware that logs the error and sends a consistent error response to the client:
     ```javascript
     app.use((err, req, res, next) => {
       console.error("Error:", err); // Log the error
       res.status(500).json({ error: "Internal server error" }); // Send a generic error response
     });
     ```
   - In this middleware, `err` is the error object passed from the previous middleware (your validation middleware).

5. **Handling Errors in Route Handlers**:
   - In your route handlers, you can check for errors and handle them accordingly:
     ```javascript
     app.post("/create-account", createAccountSchema, (req, res) => {
       if (error) {
         // Handle the error (e.g., send an error response to the client)
         return res
           .status(400)
           .json({ error: "Validation error", details: error.details });
       }
       // Proceed with account creation logic
     });
     ```

In summary, the `error` variable is created within your middleware, and you pass it to `next()` to indicate that an error occurred. The subsequent middleware (error handler or default handler) then deals with it appropriately.

# Templating Engines

Express.js, being a minimalist web framework for Node.js, does not come with built-in support for templating engines. However, it provides a way to integrate various templating engines into your application through the use of middleware.

Here are some popular templating engines that you can use with Express.js:

1. **Pug (formerly Jade)**:

   - Pug is a high-performance templating engine with a concise syntax that compiles to HTML.
   - Installation: `npm install pug`
   - Example usage:
     ```javascript
     app.set("view engine", "pug");
     //setting views file folder
     app.set("views", "./views");
     ```

2. **EJS (Embedded JavaScript)**:

   - EJS allows you to embed JavaScript directly within HTML templates.
   - Installation: `npm install ejs`
   - Example usage:
     ```javascript
     app.set("view engine", "ejs");
     ```

3. **Handlebars**:

   - Handlebars provides a simple syntax for creating templates with dynamic content.
   - Installation: `npm install express-handlebars`
   - Example usage:
     ```javascript
     const exphbs = require("express-handlebars");
     app.engine("handlebars", exphbs());
     app.set("view engine", "handlebars");
     ```

4. **Mustache**:

   - Mustache is a logic-less templating engine that can be used with various programming languages.
   - Installation: `npm install mustache-express`
   - Example usage:
     ```javascript
     const mustacheExpress = require("mustache-express");
     app.engine("mustache", mustacheExpress());
     app.set("view engine", "mustache");
     ```

5. **Nunjucks**:
   - Nunjucks is a powerful templating engine with features like inheritance, asynchronous control, and more.
   - Installation: `npm install nunjucks`
   - Example usage:
     ```javascript
     const nunjucks = require("nunjucks");
     nunjucks.configure("views", {
       autoescape: true,
       express: app,
     });
     app.set("view engine", "nunjucks");
     ```

Choose the templating engine that best fits your project's requirements and your familiarity with the syntax. Once you've installed the templating engine of your choice, configure Express.js to use it as the default view engine using `app.set('view engine', 'engine-name')`. Then, create your views (templates) with the appropriate file extension (e.g., `.pug`, `.ejs`, `.handlebars`) and place them in the `views` directory of your Express application.

# Routing

Create separate route files using `express.Router()` and use them in your `app.js` (or main application file). This approach helps organize your routes and keeps your codebase modular and maintainable.

1. **Create a Separate Route File**:
   - First, create a new JavaScript file (e.g., `routes.js`) where you'll define your additional routes.
   - In `routes.js`, you'll export a function that takes the `app` object as an argument and sets up your routes.

2. **Example: Creating a Separate Route File (`routes.js`)**:
   ```javascript
   // routes.js
   const express = require('express');
   const router = express.Router();

   // Define your routes here
   router.get('/login', (req, res) => {
     res.render('login', { title: 'Express Login' });
   });

   // Add more routes as needed

   module.exports = (app) => {
     app.use('/myroutes', router); // Mount the router at a specific path
   };
   ```

3. **Using the Route File in `app.js`**:
   - In your main application file (`app.js`), require the `routes.js` file and pass the `app` object to it.
   - This will set up the routes defined in `routes.js` under the `/myroutes` path.

4. **Example: Using the Route File in `app.js`**:
   ```javascript
   // app.js
   const express = require('express');
   const app = express();

   // Other app configurations (middlewares, views, etc.)

   // Include the routes from routes.js
   require('./routes')(app);

   // Start the server
   const PORT = process.env.PORT || 3000;
   app.listen(PORT, () => {
     console.log(`Server listening on PORT ${PORT}`);
   });
   ```

5. **Accessing the Routes**:
   - Now you can access the `/login` route by visiting `/myroutes/login` in your browser.
   - Add more routes to `routes.js` as needed, and they will be organized under the `/myroutes` path.

By following this pattern, you can keep your routes organized, maintainable, and easy to extend.

For more examples and details, refer to the [official Express documentation on route separation](https://expressjs.com/en/guide/routing.html#express-router).

# Database Integration

Integrating databases with **Express** is essential for modern web development. Let's explore how you can achieve this:

1. **Choose Your Database System**:
   - Express apps can work with various databases supported by Node.js. Some popular options include:
     - **PostgreSQL**
     - **MySQL**
     - **Redis**
     - **SQLite**
     - **MongoDB**

2. **Install the Relevant Node.js Driver**:
   - To connect your Express app to a database, load the appropriate Node.js driver for that database. Here are examples for a few popular databases:

   - **Cassandra**:
     - Install the `cassandra-driver`:
       ```javascript
       const cassandra = require('cassandra-driver');
       const client = new cassandra.Client({ contactPoints: ['localhost'] });

       client.execute('select key from system.local', (err, result) => {
         if (err) throw err;
         console.log(result.rows[0]);
       });
       ```

   - **Couchbase**:
     - Install `couchnode`:
       ```javascript
       const couchbase = require('couchbase');
       const bucket = (new couchbase.Cluster('localhost:8091')).openBucket('bucketName');

       // Add a document to the bucket
       bucket.insert('document-key', { name: 'Matt', shoeSize: 13 }, (err, result) => {
         if (err) console.log(err);
         else console.log(result);
       });

       // Get all documents with shoe size 13
       const n1ql = 'SELECT d.* FROM `bucketName` d WHERE shoeSize = $1';
       const query = N1qlQuery.fromString(n1ql);
       bucket.query(query, [13], (err, result) => {
         if (err) console.log(err);
         else console.log(result);
       });
       ```

   - **CouchDB**:
     - Install `nano`:
       ```javascript
       const nano = require('nano')('http://localhost:5984');
       nano.db.create('books');
       const books = nano.db.use('books');

       // Insert a book document in the books database
       books.insert({ name: 'The Art of War' }, null, (err, body) => {
         if (err) console.log(err);
         else console.log(body);
       });

       // Get a list of all books
       books.list((err, body) => {
         if (err) console.log(err);
         else console.log(body.rows);
       });
       ```

   - **MySQL**:
     - Install `mysql`:
       ```javascript
       const mysql = require('mysql');
       const connection = mysql.createConnection({
         host: 'localhost',
         user: 'dbuser',
         password: 's3kreee7',
         database: 'my_db'
       });

       connection.connect();
       connection.query('SELECT 1 + 1 AS solution', (err, rows, fields) => {
         if (err) throw err;
         console.log('The solution is:', rows[0].solution);
       });
       connection.end();
       ```

   - **MongoDB**:
     - Install `mongodb`:
       ```javascript
       const MongoClient = require('mongodb').MongoClient;

       // Example (v2.*)
       MongoClient.connect('mongodb://localhost:27017/animals', (err, db) => {
         if (err) throw err;
         db.collection('mammals').find().toArray((err, result) => {
           if (err) throw err;
           console.log(result);
         });
       });

       // Example (v3.*)
       MongoClient.connect('mongodb://localhost:27017/animals', (err, client) => {
         // ...
       });
       ```
3. **Implement CRUD Operations**:
   - Use the appropriate methods to create, read, update, and delete data in your database.
4. **Optimize for Performance and Security**:
   - Ensure proper error handling, query optimization, and security practices.