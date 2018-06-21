## Week 10, Day 04

What we covered today:

  - Rails TDD
  - NodeJS

___


#### Warmup

- [Collatz](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week10/day_04_collatz_js)

___

#### Codealongs

- [Fruitstore TDD](https://github.com/textchimp/wdi-27/tree/master/week10/fruitstore-tdd)
- [Node Servers](https://github.com/textchimp/wdi-27/tree/master/week10/node-servers)
- [Burning Airlines - Node & Vue](https://github.com/textchimp/wdi-27/tree/master/week10/burning-airlines-rails-vue)

___

### Node.js

[Node](https://nodejs.org/en/)Node is an open-source, cross-platform JavaScript engine that's mostly used to create web servers using JavaScript. It was written in 2009 by Ryan Dahl while he was working at Joyent.

#### JavaScript engines​

A JavaScript engine is a compiler that turns JavaScript into machine code for microprocessors - typically, it will turn it into things like:

- **IA32** - Intel Architecture 32 bit
- **x86-64** - A 64 bit version of the Intel Architecture
- **ARM** - Acorn Reduced Instruction Set Computing (RISC) Machine
- **MISP ISAs** - Microprocessor without Interlocked Pipeline Stages Instruction Set Architecture

​Here are a few JavaScript engines: ​
- **SpiderMonkey** - Firefox, Netscape Navigator - the first JavaScript Engine
- **V8** - Chrome, Opera
- **Chakra** - Internet Explorer
- **Nitro** - Safari

**Node** is the V8 JavaScript engine, taken out of a browser, with a little bit extra. It lets us write server-side code in JavaScript, allowing us to do the same sorts of things we do with Ruby.

___

### _NodeJS - JavaScript Engines - Further Reading_

- [Wikipedia - JavaScript Engines](https://en.wikipedia.org/wiki/JavaScript_engine#JavaScript_engines)
___

### Why NodeJS?

#### _What is Node good at?_

- Streaming data
- "Soft" real-time
- Every developer knows a bit of JS
- "Tooling"
- Unopinionated and flexible
- It can be isomorphic
- Corporate backing
- Performant
- Good community - easy to find developers


#### _What is Node not so good at?​_

- "Tooling" - too much out there
- Being opinionated
- Memory leaks
- Dependency injection
- Serving static files (like HTML)
- Providing structure
- Generic JavaScript woes (callbacks, etc.)
- Allowing easy debugging
- Producing light-weight codebases
- Providing functionality out of the box
- Getting away from fancy buzzwords

___

### Installation and Usage

#### _Installation_

```bash
# Initial installation using Homebrew
brew install node
# Upgrading installed version using Homebrew
brew upgrade node
```

#### _Usage_

```bash
# Open a Node REPL
node
# Run a Node application
node someFileName.js
```

___

### _NPM - Introduction_

#### Usage

We haven't gotten into Webpack yet, but this is an example of how we would create a new NodeJS application.

#### _Initialize an NPM project_

Running this command will create a package.json file for your NodeJS application. This describes your application, and allows NPM to manage your application's dependencies.

```bash
npm init
```

We can also skip the 'wizard'-style Q&A process with the -y flag.

```bash
npm init -y
```

#### _Install packages_

This will install Webpack and all its dependencies. The --save flag will also add webpack to your package.json.

```bash
npm install webpack --save
```

#### _Update packages_

Periodically, you should update the packages on which your application depends. This will update all local packages in your package.json file (and all their dependencies).

```bash
npm update
```

#### _Further Reading_

- [NPM](https://www.npmjs.com/)
- [NPM - Introduction](https://docs.npmjs.com/getting-started/what-is-npm)
- [NodeSource - Understanding NPM](https://nodesource.com/)
- [NPM Dependency Tree - Example: ExpressJS](http://npm.anvaka.com/#/view/2d/express)

___

### Creating a simple Node server

Create a new directory and cd into it then open in Atom.

```bash

mkdir simple-node-server
cd simple-node-server/
touch index.js
atom .

```
___

Add console log into the `index.js` file you've just created.

```js
console.log(`hello world`);
```

Run the program by using the below code in the terminal. Make sure you're in the `simple-node-server/` directory.

```bash
node index.js
```

You should be able to see the console log in the terminal `hello world.`

___

Go back to the `index.js` file and remove the console log as it was only used to make sure we have a connection. Define a variable and require 'http'

```js
const http = require('http');
```

___

Create a connection to the server with a 'fat arrow' function that way we can retain the value of `this`.

```js
http.createServer( (request, response) => {

});
```

This is a direct comparison of the old way of writing the function.

```js
http.createServer( function(request, response){

});
```

___


Because node is unopinionated we need to specify our requests and an end to the response.

```js
http.createServer( (request, response) => {
  response.writeHead(200, { 'Content-Type': 'test/plain' }); //telling browser this is not HTML its just plain text.
  response.end('Hello World');
});
```
___

We also have to tell the server to start with a 'listen' promise chained to the end of the function. This will watch port 8000.

```js
http.createServer( (request, response) => {
  response.writeHead(200, { 'Content-Type': 'test/plain' }); //telling browser this is not HTML its just plain text.
  response.end('Hello World');
}).listen(8000);
```
___

Unfortunately, Node doesn't log anything to the console so we have no way of knowing what's happening. We can get around this by placing console logs in our code.

```js
const http = require('http');

http.createServer( (request, response) => {
  response.writeHead(200, { 'Content-Type': 'test/plain' }); //telling browser this is not HTML its just plain text.
  response.end('Hello World');
}).listen(8000);

console.log('Server now running at http://localhost:8000');
```
___

Now go back to the terminal an run the server

```bash
node index.js
```
___

Add some additional logs to let us know when someone is trying to request a page.

```js
http.createServer( (request, response) => {
  console.log( 'Page requested');
  response.writeHead(200, { 'Content-Type': 'test/plain' }); //telling browser this is not HTML its just plain text.
  response.end('Hello World');
}).listen(8000);
```

___

### Modules

Node's module system allows us to:
- Encapsulate related code
- Organise code in a modular way
- Include code from external libraries
- Reuse code across different files.

The `require` keyword returns an object, which references the value of the `module.exports` object for any file.

#### _Exporting Modules_

Say we had a file which had a single function - a function stored in a variable called `sayHello` which we wanted to make available to other files in our application. We can export the function so it is included in the object returned by `require()` call to this file.

```js
// Approach one - using object literal syntax to create the exports object.
var sayHello = function() {
  console.log('Hello');
};
module.exports = { sayHello : sayHello };

// Approach two - adding the name and value of the variable/method to the exports object.
exports.sayHello = function() {
  console.log('Hello');
};

// Approach three - assigning an object as the value of the exports object.
var config = {
  sayHello: function() {
    console.log('Hello')
  }
};
module.exports = config;
```

#### _Importing Modules_

The `require` method takes a single argument - the name of the library, or the file system path to, the module you want to include. It returns an object - the `module.exports` object for the file passed in as an argument to the method.
Using the example above (assuming the file below is in the same path as the file above (which we'll call greeting.js)):

```js
var speak = require('./greeting.js');
speak.sayHello();
// => "Hello"
```

#### _Working with files (synchronous)_

```js
var fs = require('fs');
​
var someText = fs.readFileSync(
    __dirname + "/someFile.txt",
    'utf8' );
​
console.log( someText );
```

#### _Working with files (asynchronous)​_

```js
var fs = require('fs');
​
fs.readFile(__dirname + "/someFile.txt", "utf8", function (err, data) {
    // ERROR FIRST!
    if (err) {
    }
});
```

#### _Streams and chunks​_

- **Stream**: a sequence of data broken into pieces.
- **Chunk**: a piece of a stream. ​ Streams can be broken into readable and writable chunks. ​

```js
var fs = require('fs');

// Readable streams
var readable = fs.createReadStream(__dirname + "/original.txt");

// Writable streams
var writable = fs.createWriteStream(__dirname + "/copy.txt");
​
readable.on("data", function (chunk) {
    console.log( chunk );
    writable.write( chunk );
});

// Connecting two streams - .pipe
readable.pipe( writable );
```
___


### Express

Express is a light-weight web application framework to help organise your web application into an MVC architecture on the server side. You can use a variety of choices for your templating language (like EJS, Jade, and Dust.js). EJS would be a similar syntax to what we're use to with Rails erb.html files.

You can then use a database like MongoDB with Mongoose (for modeling) to provide a backend for your Node.js application. Express.js basically helps you manage everything, from routes, to handling requests and views.

___

#### _Installing Express_

Navigate to the root directory of your project and install express.

```bash
npm install -g express-generator
```
___

We now have access to the express commands. Create your app using express.

```bash
express hello-world
cd hello-world/
npm install
```
___

Set an environment var of debug

```bash
DEBUG=hello-world:* npm start
```

Open your browser and navigate to `http://localhost:3000/`.

___

Open your project in Atom ... or VS Code

```bash
atom .
```
___

If you check the `app.js` file you will see all the dependancies that are being required. In the views folder there is an `index.jade` file that is the `erb.html` equivalent for express. We don't actually want to be using .jade as we're more accustom to erb.


___

Using `express -e` will generate a new project with `.ejs` file in the view folder.

```bash
cd ..
express -e hello-world
npm install
```

___

Now check the view and we will have a new type of file that look more closely to what we're use to `index.ejs`

___

Shutdown the previous server with `ctrl + c` and start a new one.

```bash
DEBUG=hello-world:* npm start
```

___


#### _Enable server restart on file changes_

Any changes you make to your Express website are currently not visible until you restart the server. It quickly becomes very irritating to have to stop and restart your server every time you make a change, so it is worth taking the time to automate restarting the server when needed.
One of the easiest such tools for this purpose is [nodemon](https://github.com/remy/nodemon). This is usually installed globally (as it is a "tool"), but here we'll install and use it locally as a developer dependency, so that any developers working with the project get it automatically when they install the application. Use the following command in the root directory for the skeleton project:

```bash
npm install --save-dev nodemon
```

If you open your project's package.json file you'll now see a new section with this dependency:

```js
"devDependencies": {
    "nodemon": "^1.12.1"
  }
```

Because the tool isn't installed globally we can't launch it from the command line (unless we add it to the path) but we can call it from an NPM script because NPM knows all about the installed packages. Find the the `scripts` section of your package.json. Initially it will contain one line, which begins with `"start"`. Update it by putting a comma at the end of that line, and adding the `"devstart"` line seen below:

```js
"scripts": {
    "start": "node ./bin/www",
    "devstart": "nodemon ./bin/www"
  },
```

We can now start the server in almost exactly the same way as previously, but with the devstart command specified:

```bash
DEBUG=express-locallibrary-tutorial:* npm run devstart
```

- Note: Now if you edit any file in the project the server will restart (or you can restart it by typing rs on the command prompt at any time). You will still need to reload the browser to refresh the page.

We now have to call "npm run <scriptname>" rather than just npm start, because "start" is actually an NPM command that is mapped to the named script. We could have replaced the command in the start script but we only want to use nodemon during development, so it makes sense to create a new script command.  

#### _The generated project_

Let's now take a look at the project we just created.

_Directory structure_

The generated project, now that you have installed dependencies, has the following file structure (files are the items not prefixed with "/"). The package.json file defines the application dependencies and other information. It also defines a startup script that will call the application entry point, the JavaScript file /bin/www. This sets up some of the application error handling and then loads app.js to do the rest of the work. The app routes are stored in separate modules under the routes/ directory. The templates are stored under the /views directory.

```bash
/express-locallibrary-tutorial
    app.js
    /bin
        www
    package.json
    /node_modules
        [about 4,500 subdirectories and files]
    /public
        /images
        /javascripts
        /stylesheets
            style.css
    /routes
        index.js
        users.js
    /views
        error.pug
        index.pug
        layout.pug
```

The following sections describe the files in a little more detail.

#### _package.json_

The package.json file defines the application dependencies and other information:

```js
{
  "name": "express-locallibrary-tutorial",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "node ./bin/www",
    "devstart": "nodemon ./bin/www"
  },
  "dependencies": {
    "body-parser": "~1.18.2",
    "cookie-parser": "~1.4.3",
    "debug": "~2.6.9",
    "express": "~4.15.5",
    "morgan": "~1.9.0",
    "pug": "2.0.0-beta11",
    "serve-favicon": "~2.4.5"
  },
  "devDependencies": {
    "nodemon": "^1.12.1"
  }
}
```
The dependencies include the express package and the package for our selected view engine (pug). In addition, we have the following packages that are useful in many web applications:

- [body-parser](https://www.npmjs.com/package/body-parser): This parses the body portion of an incoming HTTP request and makes it easier to extract different parts of the contained information. For example, you can use this to read POST parameters.

- [cookie-parser](https://www.npmjs.com/package/cookie-parser): Used to parse the cookie header and populate req.cookies (essentially provides a convenient method for accessing cookie information).

- [debug](https://www.npmjs.com/package/debug): A tiny node debugging utility modelled after node core's debugging technique.

- [morgan](https://www.npmjs.com/package/morgan): An HTTP request logger middleware for node.

- [serve-favicon](https://www.npmjs.com/package/serve-favicon): Node middleware for serving a favicon (this is the icon used to represent the site inside the browser tab, bookmarks, etc.). The scripts section defines a "start" script, which is what we are invoking when we call npm start to start the server. From the script definition you can see that this actually starts the JavaScript file ./bin/www with node. It also defines a "devstart" script, which we invoke when calling npm run devstart instead. This starts the same ./bin/www file, but with nodemon rather than node.

```js
"scripts": {
    "start": "node ./bin/www",
    "devstart": "nodemon ./bin/www"
  },
```

#### _www file_

The file /bin/www is the application entry point! The very first thing this does is require() the "real" application entry point (app.js, in the project root) that sets up and returns the express() application object.

```js
#!/usr/bin/env node

/**
 * Module dependencies.
 */

var app = require('../app');
```

- Note: require() is a global node function that is used to import modules into the current file. Here we specify app.js module using a relative path and omitting the optional (.js) file extension.

The remainder of the code in this file sets up a node HTTP server with app set to a specific port (defined in an environment variable or 3000 if the variable isn't defined), and starts listening and reporting server errors and connections. For now you don't really need to know anything else about the code (everything in this file is "boilerplate"), but feel free to review it if you're interested.

#### _app.js_

This file creates an express application object (named app, by convention), sets up the application with various settings and middleware, and then exports the app from the module. The code below shows just the parts of the file that create and export the app object:

```js
var express = require('express');
var app = express();
...
module.exports = app;
```

Back in the www entry point file above, it is this module.exports object that is supplied to the caller when this file is imported.

Lets work through the app.js file in detail. First we import some useful node libraries into the file using require(), including express, serve-favicon, morgan, cookie-parser and body-parser that we previously downloaded for our application using NPM; and path, which is a core Node library for parsing file and directory paths.

```js
var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');
```

Then we `require()` modules from our routes directory. These modules/files contain code for handling particular sets of related "routes" (URL paths). When we extend the skeleton application, for example to list all books in the library, we will add a new file for dealing with book-related routes.

```js
var index = require('./routes/index');
var users = require('./routes/users');
```

- Note: At this point we have just imported the module; we haven't actually used its routes yet (this happens just a little bit further down the file).

Next we create the app object using our imported express module, and then use it to set up the view (template) engine. There are two parts to setting up the engine. First we set the 'views' value to specify the folder where the templates will be stored (in this case the sub folder /views). Then we set the 'view engine' value to specify the template library (in this case "pug").


```js
var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'pug');
```

The next set of functions call `app.use()` to add the middleware libraries into the request handling chain. In addition to the 3rd party libraries we imported previously, we use the `Express.static` middleware to get Express to serve all the static files in the directory `/public` in the project root.

```js
// uncomment after placing your favicon in /public
//app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));
```

Now that all the other middleware is set up, we add our (previously imported) route-handling code to the request handling chain. The imported code will define particular routes for the different parts of the site:


```js
app.use('/', index);
app.use('/users', users);
```

- Note: The paths specified above (`'/'` and `'/users'`) are treated as a prefix to routes defined in the imported files. So for example if the imported users module defines a route for `/profile`, you would access that route at `/users/profile`.

The last middleware in the file adds handler methods for errors and HTTP 404 responses.

```js
// catch 404 and forward to error handler
app.use(function(req, res, next) {
  var err = new Error('Not Found');
  err.status = 404;
  next(err);
});

// error handler
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});
```

The Express application object (app) is now fully configured. The last step is to add it to the module exports (this is what allows it to be imported by /bin/www).

```js
module.exports = app;
```

The route file /routes/users.js is shown below (route files share a similar structure, so we don't need to also show index.js). First it loads the express module, and uses it to get an `express.Router` object. Then it specifies a route on that object, and lastly exports the router from the module (this is what allows the file to be imported into `app.js`).

```js
var express = require('express');
var router = express.Router();

/* GET users listing. */
router.get('/', function(req, res, next) {
  res.send('respond with a resource');
});

module.exports = router;
```

The route defines a callback that will be invoked whenever an HTTP `GET` request with the correct pattern is detected. The matching pattern is the route specified when the module is imported (`'/users'`) plus whatever is defined in this file (`'/'`). In other words, this route will be used when an URL of `/users/` is received.

- Tip: Try this out by running the server with node and visiting the URL in your browser: `http://localhost:3000/users/`. You should see a message: 'respond with a resource'.

One thing of interest above is that the callback function has the third argument `'next'`, and is hence a middleware function rather than a simple route callback. While the code doesn't currently use the `next` argument, it may be useful in the future if you want to add multiple route handlers to the `'/'` route path.

#### Views

The views (templates) are stored in the /views directory (as specified in app.js) and are given the file extension .pug. The method Response.render() is used to render a specified template along with the values of named variables passed in an object, and then send the result as a response. In the code below from /routes/index.js you can see how that route renders a response using the template "index" passing the template variable "title".

```js
/* GET home page. */
router.get('/', function(req, res) {
  res.render('index', { title: 'Express' });
});
```

The corresponding template for the above route is given below (index.pug). We'll talk more about the syntax later. All you need to know for now is that the title variable (with value 'Express') is inserted where specified in the template.

```js
extends layout

block content
  h1= title
  p Welcome to #{title}
```
___

#### Homework
- 1) See if you can connect our Node+Mongo BA backend to the Vue.js frontend (you should copy the frontend folder to a new location before you start fiddling with it, i.e. `cp -r ba-vue-frontend NEW-FOLDER-PATH`; note you will need to `rm -rf node_modules` and run `npm install` again in the new folder, or all the packages will break)
- 2) `learnyounode`
- 3) YouTeach preparation, final project brainstorming
- 4) *General review*: You know better than us what you need most practice with - revisit old warmups you didn't get to finish (you have the solutions now!), revisit old homework, etc

#### Further Reading

- [NodeJS](https://nodejs.org/docs/latest-v9.x/api/synopsis.html)
- [Express](https://expressjs.com/)
- [MongoDB](https://docs.mongodb.com/)
- [NoSQL vs Relational databases](https://www.youtube.com/watch?v=qI_g07C_Q5I)
