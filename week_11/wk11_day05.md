## Week 11, day 05

What we covered today:

  - Websockets, Socket.io
  - Advanced JS Async

___

#### Warmup

- [Sssnek](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week11/day_05_Sssnek)

___

#### Codealongs

- [Tweeteater](https://github.com/textchimp/wdi-27/tree/master/week11/node-websockets-tweetstreamer)
- [Advanced JS Async](https://github.com/textchimp/wdi-27/tree/master/week11/promises-async-await-fetch)

___


### Websockets - Socket.IO

The first goal is to setup a simple HTML webpage that serves out a form and a list of messages. We’re going to use the Node.JS web framework express to this end.

First let’s create a `package.json` manifest file that describes our project. I recommend you place it in a dedicated empty directory (I’ll call mine chat-example).

```javascript
{
  "name": "socket-chat-example",
  "version": "0.0.1",
  "description": "my first socket.io app",
  "dependencies": {}
}
```

Now, in order to easily populate the dependencies with the things we need, we’ll use `npm install --save`:

```sh
npm install --save express
```

Now that express is installed we can create an `index.js` file that will setup our application.

```javascript
var app = require('express')();
var http = require('http').Server(app);

app.get('/', function(req, res){
  res.send('<h1>Hello world</h1>');
});

http.listen(3000, function(){
  console.log('listening on *:3000');
});
```

This translates into the following:

- 1) Express initializes app to be a function handler that you can supply to an HTTP server (as seen in line 2).
- 2) We define a route handler / that gets called when we hit our website home.
- 3) We make the http server listen on port 3000.

If you run `node index.js` you should see the following in your terminal:

```sh
listening on *:3000
```

And if you point your browser to http://localhost:3000 you should see `Hello world`

#### _Serving HTML_

So far in `index.js` we’re calling res.send and pass it a HTML string. Our code would look very confusing if we just placed our entire application’s HTML there. Instead, we’re going to create a `index.html` file and serve it.

Let’s refactor our route handler to use `sendFile` instead:

```javascript
app.get('/', function(req, res){
res.sendFile(__dirname + '/index.html');
});
```

And populate `index.html` with the following:

```html
<!doctype html>
<html>
  <head>
    <title>Socket.IO chat</title>
    <style>
      * { margin: 0; padding: 0; box-sizing: border-box; }
      body { font: 13px Helvetica, Arial; }
      form { background: #000; padding: 3px; position: fixed; bottom: 0; width: 100%; }
      form input { border: 0; padding: 10px; width: 90%; margin-right: .5%; }
      form button { width: 9%; background: rgb(130, 224, 255); border: none; padding: 10px; }
      #messages { list-style-type: none; margin: 0; padding: 0; }
      #messages li { padding: 5px 10px; }
      #messages li:nth-child(odd) { background: #eee; }
    </style>
  </head>
  <body>
    <ul id="messages"></ul>
    <form action="">
      <input id="m" autocomplete="off" /><button>Send</button>
    </form>
  </body>
</html>
```

If you restart the process (by hitting `Control+C` and running `node index` again) and refresh the page, you should now see a chat box at the bottom of the page.

#### _Integrating Socket.IO_

`Socket.IO` is composed of two parts:

- A server that integrates with (or mounts on) the `Node.JS HTTP Server`: socket.io
- A client library that loads on the browser side: `socket.io-client`

During development, `socket.io` serves the client automatically for us, as we’ll see, so for now we only have to install one module:

```sh
npm install --save socket.io
```

That will install the module and add the dependency to `package.json`. Now let’s edit `index.js` to add it:

```javascript
var app = require('express')();
var http = require('http').Server(app);
var io = require('socket.io')(http);

app.get('/', function(req, res){
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', function(socket){
  console.log('a user connected');
});

http.listen(3000, function(){
  console.log('listening on *:3000');
});
```

Notice that I initialize a new instance of `socket.io` by passing the http (the HTTP server) object. Then I listen on the connection event for incoming sockets, and I log it to the console.

Now in index.html I add the following snippet before the `</body>`:

```javascript
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io();
</script>
```

That’s all it takes to load the `socket.io-client`, which exposes a io global, and then connect.

Notice that I’m not specifying any URL when I call `io()`, since it defaults to trying to connect to the host that serves the page.

If you now reload the server and the website you should see the console print “a user connected”.

Each socket also fires a special disconnect event:

```javascript
io.on('connection', function(socket){
  console.log('a user connected');
  socket.on('disconnect', function(){
    console.log('user disconnected');
  });
});
```

Then if you refresh a tab several times you can see it in action in your terminal.

#### _Emitting events_

The main idea behind `Socket.IO` is that you can send and receive any events you want, with any data you want. Any objects that can be encoded as `JSON` will do, and binary data is supported too.

Let’s make it so that when the user types in a message, the server gets it as a chat message event. The scripts section in `index.html` should now look as follows:

```javascript
<script src="/socket.io/socket.io.js"></script>
<script src="https://code.jquery.com/jquery-1.11.1.js"></script>
<script>
  $(function () {
    var socket = io();
    $('form').submit(function(){
      socket.emit('chat message', $('#m').val());
      $('#m').val('');
      return false;
    });
  });
</script>
```

And in `index.js` we print out the chat message event:

```javascript
io.on('connection', function(socket){
  socket.on('chat message', function(msg){
    console.log('message: ' + msg);
  });
});
```

#### _Broadcasting_

The next goal is for us to emit the event from the server to the rest of the users.

In order to send an event to everyone, Socket.IO gives us the `io.emit`:

```javascript
io.emit('some event', { for: 'everyone' });
```

If you want to send a message to everyone except for a certain socket, we have the `broadcast` flag:

```javascript
io.on('connection', function(socket){
  socket.broadcast.emit('hi');
});
```

In this case, for the sake of simplicity we’ll send the message to everyone, including the sender.

```javascript
io.on('connection', function(socket){
  socket.on('chat message', function(msg){
    io.emit('chat message', msg);
  });
});
```

And on the client side when we capture a chat message event we’ll include it in the page. The total client-side JavaScript code now amounts to:

```javascript
<script>
  $(function () {
    var socket = io();
    $('form').submit(function(){
      socket.emit('chat message', $('#m').val());
      $('#m').val('');
      return false;
    });
    socket.on('chat message', function(msg){
      $('#messages').append($('<li>').text(msg));
    });
  });
</script>
```

And that's it!

___


### Advanced JS Async

#### _The Fetch API_

The Fetch API provides a `fetch()` method defined on the window object, which you can use to perform requests. This method returns a Promise that you can use to retrieve the response of the request.

The fetch method only has one mandatory argument, which is the URL of the resource you wish to fetch. A very basic example would look something like the following. This fetches the top five posts from `r/javascript on Reddit`:

```javascript
fetch('https://www.reddit.com/r/javascript/top/.json?limit=5')
.then(res => console.log(res));
```

If you inspect the response in your browser’s console, you should see a `Response` object with several properties:

```javascript
{
  body: ReadableStream
  bodyUsed: false
  headers: Headers {}
  ok: true
  redirected: false
  status: 200
  statusText: ""
  type: "cors"
  url: "https://www.reddit.com/top/.json?count=5"
}
```

It seems that the request was successful, but where are our top five posts? Let’s find out.

#### _Loading JSON_

We can’t block the user interface waiting until the request finishes. That’s why `fetch()` returns a Promise, an object which represents a future result. In the above example, we’re using the then method to wait for the server’s response and log it to the console.

Now let’s see how we can extract the JSON payload from that response once the request completes:

```javascript
fetch('https://www.reddit.com/r/javascript/top/.json?limit=5')
.then(res => res.json())
.then(json => console.log(json));
```

We start the request by calling `fetch()`. When the promise is fulfilled, it returns a Response object, which exposes a json method. Within the first `then()` we can call this json method to return the response body as JSON.

However, the json method also returns a promise, which means we need to chain on another `then()`, before the JSON response is logged to the console.

And why does `json()` return a promise? Because HTTP allows you to stream content to the client chunk by chunk, so even if the browser receives a response from the server, the content body might not all be there yet!


#### _Async … Await_

The `.then()` syntax is nice, but a more concise way to process promises in 2018 is using async … `await` — a new syntax introduced by ES2017. Using async `await` means that we can mark a function as async, then wait for the promise to complete with the await keyword, and access the result as a normal object. Async functions are supported in all modern browsers

Here’s what the above example would look like (slightly expanded) using async … await:

```javascript
async function fetchTopFive(sub) {
  const URL = `https://www.reddit.com/r/${sub}/top/.json?limit=5`;
  const fetchResult = fetch(URL)
  const response = await fetchResult;
  const jsonData = await response.json();
  console.log(jsonData);
}

fetchTopFive('javascript');
```

Not much has changed. Apart from the fact that we’ve created an async function, to which we’re passing the name of the subreddit, we’re now awaiting the result of calling `fetch()`, then using await again to retrieve the `JSON` from the response.

That’s the basic workflow, but things involving remote services doesn’t always go smoothly.

#### _Handling Errors_

Imagine we ask the server for a non-existing resource or a resource that requires authorization. With `fetch()`, you must handle application-level errors, like 404 responses, inside the normal flow. As we saw previously, fetch() returns a Response object with an ok property. If response.ok is true, the response status code lies within the 200 range:

```javascript
async function fetchTopFive(sub) {
  const URL = `http://httpstat.us/404`; // Will return a 404
  const fetchResult = fetch(URL)
  const response = await fetchResult;
  if (response.ok) {
    const jsonData = await response.json();
    console.log(jsonData);
  } else {
    throw Error(response.statusText);
  }
}

fetchTopFive('javascript');
```

The meaning of a response code from the server varies from API to API, and oftentimes checking response.ok might not be enough. For example, some APIs will return a 200 response even if your API key is invalid. Always read the API documentation!

To handle a network failure, use a try … catch block:

```javascript
async function fetchTopFive(sub) {
  const URL = `https://www.reddit.com/r/${sub}/top/.json?limit=5`;
  try {
    const fetchResult = fetch(URL)
    const response = await fetchResult;
    const jsonData = await response.json();
    console.log(jsonData);
  } catch(e){
    throw Error(e);
  }
}

fetchTopFive('javvascript'); // Notice the incorrect spelling
```

The code inside the catch block will run only when a network error occurs.

You’ve learned the basics of making requests and reading responses. Now let’s customize the request further.


#### _Change The Request Method And Headers_

Looking at the example above, you might be wondering why you couldn’t just use one of the existing `XMLHttpRequest` wrappers. The reason is that there’s more the fetch API offers you than just the `fetch()` method.

While you must use the same instance of `XMLHttpRequest` to perform the request and retrieve the response, the fetch API lets you configure request objects explicitly.

For example, if you need to change how `fetch()` makes a request (e.g. to configure the request method) you can pass a Request object to the `fetch()` function. The first argument to the Request constructor is the request URL, and the second argument is an option object that configures the request:

```javascript
async function fetchTopFive(sub) {
  const URL = `https://www.reddit.com/r/${sub}/top/.json?limit=5`;
  try {
    const fetchResult = fetch(
      new Request(URL, { method: 'GET', cache: 'reload' })
    );
    const response = await fetchResult;
    const jsonData = await response.json();
    console.log(jsonData);
  } catch(e){
    throw Error(e);
  }
}

fetchTopFive('javascript');
```

Here, we specified the request method and asked it never to cache the response.

You can change the request headers by assigning a `Headers` object to the request `headers` field. Here’s how to ask for `JSON` content only with the `'Accept'` header:

```javascript
const headers = new Headers();
headers.append('Accept', 'application/json');
const request = new Request(URL, { method: 'GET', cache: 'reload', headers: headers });
```

You can create a new request from an old one to tweak it for a different use case. For example, you can create a `POST` request from a `GET` request to the same resource. Here’s an example:

```javascript
const postReq = new Request(request, { method: 'POST' });
```

You also can access the response headers, but keep in mind that they’re read-only values.

```javascript
fetch(request).then(response => console.log(response.headers));
```

`Request` and `Response` follow the HTTP specification closely; you should recognize them if you’ve ever used a server-side language. If you’re interested in finding out more, you can read all about them on the [Fetch API page on MDN.](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

___

#### Further Reading

- [Socket.IO](https://socket.io/)
- [Socket.IO demos](https://socket.io/demos/chat/)
- [Socket.IO, Node.js and Express](https://www.programwitherik.com/getting-started-with-socket-io-node-js-and-express/)
- [Fetch API MDN](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [Introduction to Fetch API](https://www.sitepoint.com/introduction-to-the-fetch-api/)
- [Promises MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Promises Introduction](https://developers.google.com/web/fundamentals/primers/promises)
