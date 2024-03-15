# npm

1. **What is npm?**

   - **npm** stands for **Node Package Manager**. It is the **default package manager** for **Node.js**, a popular runtime for executing JavaScript code on the server side.
   - npm allows JavaScript developers to **share and manage packages** easily. These packages can include libraries, tools, and other code that enhance your projects.
   - The npm registry hosts over **1.3 million packages**, with a staggering weekly download rate of **16 billion**!

2. **Components of npm:**

   - **CLI (Command-Line Interface)**:
     - The npm CLI tool is used for **publishing** and **downloading** packages.
     - It's like an army of **hardworking wombats** that assist developers by delivering dependencies to their projects.
   - **Online Repository (npmjs.com)**:
     - Think of npmjs.com as a **fulfillment center**.
     - Sellers (npm package authors) send their goods (packages) to this center, and it distributes them to buyers (npm package users).

3. **package.json**:

   - Every JavaScript project (whether Node.js or browser-based) can be scoped as an npm package.
   - The **package.json** file describes the project and its metadata.
   - It's like stamped labels on the npm good boxes delivered by our wombats.
   - Key properties in package.json:
     - `name`: Name of your project.
     - `version`: Project version (useful for deployment).
     - `description`: Project description.
     - `license`: Project license.
     - **npm scripts**: Custom commands defined in package.json to run tools installed locally.

4. **Using npm**:
   - To initialize a new project, run:
     ```bash
     npm init
     ```
   - Install packages using:
     ```bash
     npm install package-name
     ```
   - Manage dependencies in your project by adding them to your **package.json** file.
   - Explore the vast npm registry to find packages that suit your needs.

Remember, npm is the heart of the JavaScript ecosystem, allowing developers to collaborate, share, and build amazing software.

For more in-depth information, check out the [npm documentation](https://docs.npmjs.com/about-npm/) and [this beginner's guide](https://nodesource.com/blog/an-absolute-beginners-guide-to-using-npm/).

# npm commands

Here's a comprehensive list of commonly used npm commands:

## Project Initialization and Dependency Management:

1. **npm init**: Initializes a new Node.js project and creates a `package.json` file interactively.
   - Example: `npm init`

2. **npm install**: Installs dependencies listed in `package.json`.
   - Example: `npm install`

3. **npm install \<package\>**: Installs a specific package locally.
   - Example: `npm install lodash`

4. **npm install \<package\> --save-dev**: Installs a package as a development dependency.
   - Example: `npm install jest --save-dev`

5. **npm install \<package\> -g**: Installs a package globally.
   - Example: `npm install nodemon -g`

6. **npm update**: Updates packages to their latest versions based on version range specifiers in `package.json`.
   - Example: `npm update`

7. **npm uninstall \<package\>**: Uninstalls a package.
   - Example: `npm uninstall lodash`

## Running Scripts:

8. **npm start**: Runs the script specified in the `"start"` field of `package.json`.
   - Example: `npm start`

9. **npm test**: Runs the test script specified in the `"scripts"` section of `package.json`.
   - Example: `npm test`

10. **npm run \<script\>**: Runs a custom script defined in the `"scripts"` section of `package.json`.
    - Example: `npm run build`

## Managing Dependencies:

11. **npm ls**: Lists installed packages and their dependencies.
    - Example: `npm ls`

12. **npm outdated**: Checks for outdated packages and displays available updates.
    - Example: `npm outdated`

13. **npm audit**: Performs a security audit of dependencies and provides recommendations for fixing vulnerabilities.
    - Example: `npm audit`

## Publishing Packages:

14. **npm publish**: Publishes the package to the npm registry.
    - Example: `npm publish`

15. **npm version \<update_type\>**: Increments the package version according to the specified update type (`patch`, `minor`, `major`).
    - Example: `npm version patch`

## Miscellaneous:

16. **npm cache clean**: Clears the npm cache.
    - Example: `npm cache clean`

17. **npm login**: Logs in to npm registry.
    - Example: `npm login`

18. **npm logout**: Logs out from npm registry.
    - Example: `npm logout`

19. **npm link**: Links a package locally for development.
    - Example: `npm link`

20. **npm help**: Displays npm help documentation.
    - Example: `npm help`

These are some of the most commonly used npm commands. They provide essential functionality for managing Node.js projects, dependencies, scripts, and publishing packages.

# Package.json

The **`package.json`** file is a crucial part of the **Node.js ecosystem**. 

1. **What is `package.json`?**
   - The **`package.json`** file serves as the **manifest** for your Node.js project.
   - It contains **metadata** about your project, including its name, version, dependencies, scripts, and more.
   - Think of it as the **heart** of your Node.js system.

2. **Key Properties in `package.json`:**
   - **`name`**:
     - The name of your package/module.
     - If you plan to **publish** your package, this field is **required**.
     - The name, along with the version, forms a unique identifier.
     - Some rules:
       - Must be less than or equal to **214 characters**.
       - Avoid uppercase letters.
       - Avoid non-URL-safe characters.
       - Don't use the same name as a core Node module.
   - **`version`**:
     - Also required if you plan to **publish** your package.
     - Follows **semantic versioning** (e.g., `1.2.3`).
   - **`description`**:
     - A string describing your package.
     - Helps people discover your package when searching on npm.
   - **`keywords`**:
     - An array of strings representing keywords related to your package.
     - Improves discoverability.
   - **`homepage`**:
     - The URL to your project's homepage (e.g., GitHub repository).
   - **`bugs`**:
     - The URL to your project's issue tracker or an email address for reporting issues.

3. **Other Useful Fields:**
   - **`dependencies`**:
     - Lists the packages your project depends on.
   - **`scripts`**:
     - Custom commands you can run using `npm run`.
   - **`license`**:
     - Specifies the license for your package.

4. **Creating `package.json`:**
   - Initialize it using:
     ```bash
     npm init
     ```
   - Follow the prompts to provide necessary information.

5. **Why Is It Important?**
   - **Dependency Management**:
     - npm uses `package.json` to install dependencies.
   - **Project Configuration**:
     - Set up build scripts, start commands, and other project-specific configurations.
   - **Version Control**:
     - Commit `package.json` to version control (e.g., Git).

Remember, understanding and configuring `package.json` is essential for working with Node.js, npm, and modern JavaScript projects. 

For more details, check out the [official npm documentation](https://docs.npmjs.com/cli/v6/configuring-npm/package-json/) and other helpful resources..

# Semantic versioning

## Semantic Versioning Format:

- Semantic versioning follows the format `MAJOR.MINOR.PATCH`.
  - `MAJOR` version is incremented for incompatible API changes.
  - `MINOR` version is incremented for backward-compatible feature additions.
  - `PATCH` version is incremented for backward-compatible bug fixes.

## Version Range Specifiers:
- Use version range specifiers to define acceptable versions of dependencies in `package.json`.

### Wildcard:
- `*`: Matches any version.
  - Example: `"lodash": "*"` (Accepts any version of lodash)

### Exact Version:
- `<version>`: Specifies an exact version.
  - Example: `"lodash": "4.17.21"` (Accepts only version 4.17.21 of lodash)

### Caret (^):
- `^MAJOR.MINOR.PATCH`: Allows updates that do not modify the left-most non-zero digit.
  - Example: `"lodash": "^4.17.21"` (Accepts any version from 4.17.21 to <5.0.0)

### Tilde (~):
- `~MAJOR.MINOR.PATCH`: Allows updates that do not modify the left-most non-zero digit or the next non-zero digit.
  - Example: `"lodash": "~4.17.21"` (Accepts any version from 4.17.21 to <4.18.0)

### Range:
- `>=`, `>`, `<`, `<=`: Greater than or equal to, greater than, less than, less than or equal to a specified version.
  - Example: `"lodash": ">=4.17.21 <5.0.0"` (Accepts any version from 4.17.21 to <5.0.0)

## Version Selection:
- Use `npm install` or `npm update` commands to install or update dependencies based on the specified version range in `package.json`.

### Install:
- `npm install`: Installs dependencies defined in `package.json`.

### Update:
- `npm update`: Updates dependencies to the latest versions that satisfy the version range specifiers in `package.json`.

## Checking Installed Versions:
- Use `npm list` command to view installed dependencies and their versions.
- Use `npm outdated` command to check for outdated dependencies and their available updates.

## Conclusion:
- Semantic versioning helps maintain compatibility and manage dependencies effectively.
- Choose version range specifiers wisely to ensure compatibility and stability in your project.

By following this cheat sheet, you can effectively manage dependencies and ensure compatibility with Semantic Versioning in npm.

# creating and publishing your own package

1. **Choose a Unique Package Name**:
   - Your package needs a **unique name**. Make sure it hasn't been used before.
   - Visit the [NPM registry](https://www.npmjs.com/) and search for your desired package name to ensure no exact match exists.
   - If the name is taken, consider choosing a different one or using a **scoped package** (more on that later).

2. **Install Node.js**:
   - If you haven't already, **install Node.js** from the official website. npm comes bundled with Node.

3. **Initialize a Git Repository**:
   - Create a new project folder for your package.
   - Navigate into the folder and run:
     ```bash
     git init
     ```
   - This sets up version control for your package.

4. **Initialize npm in Your Project**:
   - Navigate to the root directory of your project and run:
     ```bash
     npm init
     ```
   - Follow the prompts to provide information about your package:
     - `package-name`: Choose a unique lowercase name (you can use hyphens).
     - `version`: Start with 1.0.0 and update it following semantic versioning.
     - `description`: Describe what your package does.
     - `entry point`: Default is `index.js`.
     - `test command`: Specify the test command (e.g., `npm run test`).
     - `git repository`: Link to your remote repository on GitHub.
     - `keywords`: Add relevant keywords for discoverability.
     - `author`: Add your name.
     - `license`: Choose a license (default is ISC License).


5. **Write Your Code**:
   - Create the functionality you want to package.
   - Export relevant functions or classes using `module.exports`.

6. **Test Locally**:
   - Test your package locally to ensure it works as expected.

7. **Publish Your Package**:
   - First, create an account on the [NPM registry](https://www.npmjs.com/signup).
   - Log in using `npm login`.
   - Finally, publish your package:
     ```bash
     npm publish
     ```

8. **Updating Your Package**:
   - When you make changes, update the version in `package.json`.
   - Publish the new version using `npm publish`.
