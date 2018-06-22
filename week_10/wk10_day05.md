## Week 10, Day 04

What we covered today:

  - Express
  - Singly Linked Lists

___


#### Warmup

- [Etch-a-Sketch](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week10/day_05_etch-a-sketch)

___

#### Codealongs

- [Singly Linked Lists](https://github.com/textchimp/wdi-27/tree/master/week10/singly-linked-lists)

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

### Singly Linked List

Singly Linked Lists are a type of data structure. In a singly linked list, each node in the list stores the contents and a pointer or reference to the next node in the list. It does not store any pointer or reference to the previous node. ... The last node in a single linked list points to nothing.

```bash
mkdir singly_linked_list
cd singly_linked_list/
touch singly_linked_list.rb
```

```ruby
class SinglyLinkedList

end

require 'pry'
binding.pry
```

```ruby
add a class into the SinglyLinkedList Class

class SinglyLinkedList

  class Node

  end

end
```

```ruby
class SinglyLinkedList

  class Node

  end

  attr_accessor :head

  def initialize
    @head = nil
  end

end
```

```ruby
class SinglyLinkedList

  class Node

  end

  attr_accessor :head

  def initialize
    @head = nil
  end

  def prepend(value)
    node = Node.new(value)
    node.next = @head
    @head = node
  end

end
```

```ruby
class Node
  attr_accessor :value, :next

  def initialize(value)
    @value = value
    @next = nil
  end
end
```

Now we can run the program to see how it works.

```bash
ruby singly_linked_list.rb
```

Our `binging.pry `will keep us in the program as well will have access to the class.

```bash
pry(main)> SinglyLinkedList
=> SinglyLinkedList
```

We are now able to instantiate a new instance of the `SinglyLinkedList`.

```bash
pry(main)> bros = SinglyLinkedList.new
=> #<SinglyLinkedList:0x00007fc048a83148 @head=nil>
```

We can then call the `prepend` method we just created to add a new node.

```bash
pry(main)> bros.prepend 'Groucho'
=> #<SinglyLinkedList::Node:0x00007fc0480c4d00 @next=nil, @value="Groucho">
```

This now have set the value of the head node to 'Groucho'.

```bash
pry(main)> bros
=> #<SinglyLinkedList:0x00007fc048a83148
 @head=#<SinglyLinkedList::Node:0x00007fc0480c4d00 @next=nil, @value="Groucho">>

pry(main)> bros.head
=> #<SinglyLinkedList::Node:0x00007fc0480c4d00 @next=nil, @value="Groucho">

pry(main)> bros.head.value
=> "Groucho"
```

Now if we were to add another Marx brother we will see that the head will change.

```bash
pry(main)> bros.prepend 'Harpo'
=> #<SinglyLinkedList::Node:0x00007fc0490d9880
 @next=#<SinglyLinkedList::Node:0x00007fc0480c4d00 @next=nil, @value="Groucho">,
 @value="Harpo">

pry(main)> bros
=> #<SinglyLinkedList:0x00007fc048a83148
 @head=
  #<SinglyLinkedList::Node:0x00007fc0490d9880
   @next=#<SinglyLinkedList::Node:0x00007fc0480c4d00 @next=nil, @value="Groucho">,
   @value="Harpo">>

pry(main)> bros.head.value
=> "Harpo"
```

We can see what the next head value or the previous node was.

```bash
pry(main)> bros.head.next.value
=> "Groucho"
```

As expected if you add another you can chain through to find the original node.

```bash
pry(main)> bros.prepend 'Zeppo'
=> #<SinglyLinkedList::Node:0x00007fc04806c218
 @next=
  #<SinglyLinkedList::Node:0x00007fc0490d9880
   @next=#<SinglyLinkedList::Node:0x00007fc0480c4d00 @next=nil, @value="Groucho">,
   @value="Harpo">,
 @value="Zeppo">

pry(main)> bros.head.value
=> "Zeppo"

pry(main)> bros.head.next.value
=> "Harpo"

pry(main)> bros.head.next.next.value
=> "Groucho"
```

```ruby
def initialize(value=nil)
  @head = Node.new(value) if value
end
```

Make a method that finds the last node.

```ruby
def last
  node = @head
  while node && node.next
    node = node.next # Traverse by one to the next node in the list.
  end
  node
end
```

We can now set a new instance of the class as the next node of the last.

```ruby
def append(value) # AKA .push
  last.next = Node.new(value)
end
```

Run the program again.

```bash
ruby singly_linked_list.rb


    36: end
    37:
    38:
    39:
    40: require 'pry'
 => 41: binding.pry
 ```

 Create a new instance of the class.

 ```bash
 pry(main)> bros = SinglyLinkedList.new 'Groucho'
=> #<SinglyLinkedList:0x00007ffd690683f8
 @head=#<SinglyLinkedList::Node:0x00007ffd690683d0 @next=nil, @value="Groucho">>

pry(main)> bros.append 'harpo'
=> #<SinglyLinkedList::Node:0x00007ffd68ad62f0 @next=nil, @value="harpo">

pry(main)> bros.append 'chico'
=> #<SinglyLinkedList::Node:0x00007ffd68a85940 @next=nil, @value="chico">

pry(main)> bros.head.value
=> "Groucho"

pry(main)> bros.head.next
=> #<SinglyLinkedList::Node:0x00007ffd68ad62f0
 @next=#<SinglyLinkedList::Node:0x00007ffd68a85940 @next=nil, @value="chico">,
 @value="harpo">
 ```

___


#### Homework
- 1) Complete the Singly Linked List methods
- 2) Try making a new CRUD backend in Express + MongoDB, using as a template the code from our Burning Airlines codealong
- 3) Play around with `express` (the Express Generator) to build the Express equivalent of a Rails site with separate routes and view templates)
- 4) Plan your YouTeach and final projects

#### Further Reading

- [Express](https://expressjs.com/)
- [Express Tutorial](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/skeleton_website)
- [What is a Linked List?](https://medium.com/basecs/whats-a-linked-list-anyway-part-1-d8b7e6508b9d)
