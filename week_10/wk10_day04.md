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
