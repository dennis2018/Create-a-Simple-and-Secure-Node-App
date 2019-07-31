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
This template uses the title variable to render the document title of a page. This variable value will be passed from the server to the template.

Next, create an index.pug file under the same directory:

```
# For macOS/Linux use:
touch views/index.pug
# For Windows PowerShell use:
ni views/index.pug
```   
Open index.pug and add the following content to it:

```
extends layout

block layout-content
  div.View
    h1.Banner toody
    div.Message
      div.Title
        h3 Making the Best
        h1 Food For Devs
      span.Details Access the toody Team Portal
    div.NavButtons
      if isAuthenticated
        a(href="/user")
          div.NavButton Just dive in!
      else
        a(href="/login")
          div.NavButton Log in
```
Naming conventions for CSS classes vary by preference. It's common to see kebab-case used to name CSS classes. However, a few other designers and developers use PascalCase. As long as you are consistent on applying your conventions, use the casing of your preference. Personally, I find PascalCase to be more readable and easier to copy by double-clicking. When using self-contained components, I also find it easier to map the component name to the class name that modifies it.

Pug templates don't use HTML tags, instead, they simply use the names of HTML elements and whitespace to define the structure of a template. Attributes are placed next to an element name inside the parenthesis, closely resembling the structure of a function call. The content of an element can go next to the element name in the same line or indented below. Classes are defined using a .classname syntax while IDs are defined using a #idname syntax.

Visit the Getting Started page of the Pug documents for a complete overview of how Pug works.

The content of the div.NavButtons container is rendered based on the value of the isAuthenticated variable. This value will also be provided to the template by route controllers, allowing it to render the correct button based on the authentication status of a user.

To connect the templates with the controllers, configure Express to use Pug as the view template engine of the application, use the views subdirectory as the views source folder, and render a view template as the response of a route controller.

Open index.js and update it like so:

```
// index.js

const path = require("path");
const express = require("express");

const app = express();
const port = process.env.PORT || "8000";

app.set("views", path.join(__dirname, "views"));
app.set("view engine", "pug");

app.get("/", (req, res) => {
  res.render("index", { title: "Home" });
});

app.listen(port, () => {
  console.log(`Listening to requests on http://localhost:${port}`);
});
```

The script uses the app.set(name, value) method to assign a setting name to a value. You may store any value but some names are reserved to configure the behavior of the server. Two of those names are views and views engine.

The script configures Express to use the views directory as a source for view templates. The path.join() method joins the given path segments together using the platform-specific separator as a delimiter and then normalizes the resulting path. Just below, you also configure Express to use Pug as the view engine.

The GET / route controller is updated to render the index.pug template as the client response. Refresh the browser to see the new page rendered on the screen.
The res.render(view) method takes a string view argument that represents the file path of the view file to render. It renders the file and sends the rendered HTML as a string to the client. The template file path is relative to the view directory configured earlier. The file extension for the template file defaults to .pug as Pug is the default view engine.

The res.render() method takes a second, optional locals parameter, an object that lets you pass data from the controller to the template. Its object properties define local variables that can be accessed by the template. Therefore, you pass the { title: "Home" } object as a second argument of the res.render() method to define a local title variable within the index.pug template.

The index.pug template doesn't use the title local variable directly but it makes it available to the template that does, layout.pug, by extending it.

Refer to Using template engines with Express for further implementation details.

With the page running, if you make any changes in index.pug template, you'd need to refresh the browser to see the change. Manually refreshing the browser to see changes can slow down your development process. You are going to learn how to overcome this inefficiency using BrowserSync in the next section.

## Adding Live Reload to Express Using Browsersync
Front-end frameworks, such as React and Angular, use tools like Webpack and ParcelJS to provide you with an efficient development environment. For example, if you change a CSS rule on a stylesheet or the return value of a JavaScript function, the browser gets automatically refreshed to reflect the changes made when you save them. To emulate that live reload behavior easily with Express templates, you can use Browsersync.

Start by installing Browsersync as follows:

```
npm install --save-dev browser-sync
```
Next, you'll create an NPM script that configures and runs Browsersync to serve your web application. Open and update package.json with the following ui NPM script:

```
"ui": "browser-sync start --proxy=localhost:8000 --files='**/*.css, **/*.pug, **/*.js' --ignore=node_modules --reload-delay 10 --no-ui --no-notify"
```

```
{
  "name": "toody-portal",
  "version": "1.0.0",
  "description": "",
  "main": "./index.js",
  "scripts": {
    "dev": "nodemon ./index.js",
    "ui": "browser-sync start --proxy=localhost:8000 --files='**/*.css, **/*.pug, **/*.js' --ignore=node_modules --reload-delay 10 --no-ui --no-notify"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {...},
  "dependencies": {...}
}
```
The ui script uses a couple of Browsersync command line options to configure the behavior of Browsersync:

browser-sync start --proxy=localhost:8000: Proxy the Node/Express app served on localhost:8000 as Browsersync only creates a static server for basic HTML/JS/CSS websites. You still need to run the server with nodemon separately.

--files='**/*.css, **/*.pug, **/*.js': Using glob patterns, specify the file paths to watch. You'll watch CSS, Pug, and JavaScript files.

--ignore=node_modules: Specify the patterns that file watchers need to ignore. You want to ignore node_module to let Browsersync run fast.

--reload-delay 10: Time in milliseconds to delay the reload event following file changes. Ten milliseconds is enough to prevent Nodemon and Browsersync from overlapping which can cause erratic behavior.

--no-ui: Don't start the Browsersync user interface, a page where you can control the behavior of Browsersync.

--no-notify: Disable the notify element in browsers. This prevents a distracting message notifying you that Browsersync is connected to the browser from showing up.

To serve the Express app user interface, run the following command in a separate terminal tab or window:

```
npm run ui
```
The terminal will output information on the local and external locations where the static pages are being served:

```
[Browsersync] Proxying: http://localhost:8000
[Browsersync] Access URLs:
 -------------------------------
    Local: http://localhost:3000
 External: http://<YOUR IP ADDRESS>:3000
 -------------------------------
[Browsersync] Watching files...
```
Browsersync opens a new window or tab on your operating system default browser automatically to present the web app interface — if it didn't, open http://localhost:3000/.

To test this setup, you can make any change on the index.pug file, save it, and watch the site served on port 3000 update itself.

# Serving Static Assets with Express
To enhance the web application views with styling and images, Express needs to be configured to access static files, such as stylesheets and images, from a project directory. For your application, use a directory named public that you can create as follows:

```
mkdir public
```
The express.static() built-in middleware function lets you specify the path of the directory from which to serve static assets. To mount this middleware function, you use the app.use() method. Update the content of index.js as follows to perform both tasks in one line of code:

```
// index.js

// Imports and app instance

app.set("views", path.join(__dirname, "views"));
app.set("view engine", "pug");
app.use(express.static(path.join(__dirname, "public")));

// Route controllers

// App listening
```

You are now using the public directory as the source of static assets. You should put inside this directory any CSS or image files that your application needs to use.

## Styling Express Templates using CSS
