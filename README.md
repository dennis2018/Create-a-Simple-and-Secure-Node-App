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
