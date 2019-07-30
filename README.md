# Create-a-Simple-and-Secure-Node-App
Learn how to secure a simple Node.js and Express app by adding user authentication with Passport.js and Auth0.

In this tutorial series, you'll learn how to create a secure and simple Node.js application built with the Express web framework. You'll see how Passport.js with Auth0 is used to manage user authentication and protect routes. On the frontend, you'll use Pug templates for rendering views along with CSS for maintaining style sheets.

Additionally, you'll learn to streamline your Node.js development workflow by using nodemon to restart the server and browser-sync to reload the browser whenever relevant source files change.

## What You Will Build

To make this exercise fun and appealing, you'll build a login portal for a fictional restaurant named toodyREST that specializes in making delicious food for developerHub.
Using server-side rendering (SSR), the web app will consist of two views: a public login screen and a protected account information screen.

## Prerequisites
- Basic understanding of Node.js and JavaScript.

- A terminal app for MacOS and Linux or PowerShell for Windows.

- Node.js v8+ and a Node.js package manager installed locally.

### To install Node.js and NPM, use any of the official Node.js installers provided for your operating system.

## Bootstrapping a Node.js Project
- Create a project directory named toody-portal to hold your application, and make that your working directory:

```
mkdir  toody-portal
cd toody-portal
```

- Use the npm init -y command to create a package.json file for your application:
```
npm init -y
```

- By passing it the -y flag as an argument, the file is created with sensible defaults:

```
{
  "name": "toody-portal",
  "version": "1.0.0",
  "description": "",
  "main": "./index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

The main property represents the primary entry point to your application, a file named index.js. Create that file:

```
# For macOS/Linux use:
touch index.js
# For Windows PowerShell use:
ni index.js
```
## Creating an NPM script to run the application
Instead of using the node command to run the application, you'll use nodemon, a tool that monitors your application and automatically restarts your server when source code changes. With node, you'd have to restart the server manually when changes are made.

Install nodemon as a dependency of your Node.js application:
```
npm install --save-dev nodemon

```
Next, replace the test script under the scripts property of package.json with the following dev script:

```
{
  "name": "toody-portal",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "nodemon ./index.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "nodemon": "^1.19.1"
  }
}
```

The dev NPM script maps to the nodemon index.js command. nodemon takes as argument the entry point of an application and executes it. To run dev or any command from a package's scripts object use npm run:

```
npm run dev
```

## You can delete the test NPM script as testing Node.js applications is out of the scope of this article.

# Integrating Express application framework with Node.js
To use the Express framework in your application, install it as a project dependency:

```
npm install express
```
### Where is the --save flag? By default with npm v5.0+, npm install adds the module to the dependencies list in the package.json file. Earlier versions of npm require the --save flag to be specified explicitly.

With express installed, open index.js and populate it with the following content to create a simple Express app:

```
// index.js

const path = require("path");
const express = require("express");

const app = express();
const port = process.env.PORT || "8000";

app.get("/", (req, res) => {
  res.status(200).send("toody: Food For Devs");
});

app.listen(port, () => {
  console.log(`Listening to requests on http://localhost:${port}`);
});
```
The above index.js script imports the express module which exports as default a function that creates an Express application. This function is executed and the returned Express application is stored in a variable named app. The script defines a port variable that assumes the value of a defined environmental variable, process.env.PORT, or 8000. Next, it creates a simple route that handles GET HTTP requests made to the root path, /, and replies with a string. Finally, the Express app is set up to listen for HTTP requests in the port previously defined.
If the app is not running, run the app to test this script:
```
npm run dev
```
nodemon should output something similar to the following in the terminal:

```
[nodemon] restarting due to changes...
[nodemon] starting `node index.js`
Listening to requests on http://localhost:8000
```
Open any browser and visit http://localhost:8000/ to see the app in action. If everything is running successfully, the browser should display the string toody: Food For Devs on a plain page.

If you want to learn more details about Express routing, please refer to the Express "Basic routing" document from the official docs.

With a basic web app in place, it's time to create views using Pug.

# Using Pug template engine to Create Express Views
To create rich web apps, you need dynamic templates that can render UI elements conditionally and can be populated with values from the server. This is something that cannot be achieved with static HTML files. Instead, a template engine like Pug is needed. Pug templates offer conditional rendering, template hydration, template inheritance to compose pages, and a terse syntax that resembles writing Python code.

Express needs to be configured to use Pug as a template engine and with a location to look for templates. To install pug as a project dependency without stopping the running server, open a new tab or window and execute the following command:

```
npm install pug
```
Next, create a directory named views under the project directory:

```
mkdir views
```

To optimize template creation, you'll create a layout to encapsulate the top-level HTML structure of a page. This layout will be extended by other templates to render a complete and valid HTML page. In Pug, this is known as template inheritance and it's done by using the block and extends artifacts.

Under the views directory, create a layout.pug file:
```
# For macOS/Linux use:
touch views/layout.pug
# For Windows PowerShell use:
ni views/layout.pug
```
Add the following content to it:

```
block variables
doctype html
html
  head
    meta(charset="utf-8")
    link(rel="shortcut icon", href="/favicon.ico")
    meta(name="viewport", content="width=device-width, initial-scale=1, shrink-to-fit=no")
    meta(name="theme-color", content="#000000")
    title #{title} | WHATABYTE
  body
    div#root
      block layout-content
```
