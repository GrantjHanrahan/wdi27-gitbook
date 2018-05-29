## Week 07, Day 02

What we covered today:

- JavaScript - AJAX with jQuery
- JavaScript - Same Origin Policy

___

#### Warmup
- [Allergies](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week07/day02_allergies)

___
#### Codealongs
- [Google Books - API (ajax-jquery)](https://github.com/textchimp/wdi-27/tree/master/week7/ajax-jquery)
- [AJAX on Rails](https://github.com/textchimp/wdi-27/tree/master/week7/ajax-dashboard)
___

### JavaScript - AJAX with jQuery

jQuery gives us a bunch of methods that make AJAX a lot easier.

#### How to do it

There are so many ways of using AJAX with jQuery. The most flexible is using the `$.ajax` method, which has been around since jQuery 1.0.  All of the other AJAX methods that you can use with jQuery use this behind the scenes.

```js
// Basic syntax:
$.ajax( url, { OPTIONS } );
// Example:
$.ajax( "/stats", { method: "GET" } );

// Preferred syntax:
$.ajax({ OPTIONS });
// Preferred syntax example:
$.ajax({
  url: '/post',
  type: 'GET',
  // etc
})
```

Here is a more complex example using the preferred syntax.

```js
// Using the core $.ajax() method
$.ajax({

    // The URL for the request
    url: "/post",

    // The data to send (will be converted to a query string)
    data: {
        id: 123
    },

    // The type of request
    type: "GET",

    // The type of data we expect back
    dataType : "json",

    // Code to run if the request succeeds.
    // The responseText is passed to the 'success' function as the 'data' argument
    success: function( data ) {
        $( "<h1>" ).text( data.title ).appendTo( "body" );
        $( "<div class=\"content\">").html( data.html ).appendTo( "body" );
    },

    // Code to run if the request fails.
    // The request, the status code of the request and the error thrown are passed to the 'error' function as arguments
    error: function( xhr, status, errorThrown ) {
        alert( "Sorry, there was a problem!" );
        console.log( "Error: " + errorThrown );
        console.log( "Status: " + status );
        console.dir( xhr );
    },

    // Code to run when the request is completed, regardless of success or failure
    complete: function( xhr, status ) {
        console.log( "The request is complete." );
    }

});
```

We could also use promises for this, instead of encapsulating the handlers (success, error etc.).  We can chain jQuery AJAX methods together, and jQuery 'promises' that these will get run.

```js
$.ajax({...}).always(function () {
    // This will always run
}).done( function (data) {
    // This will run on success.
}).fail( function (data) {
    // This will run if there is an error
});
```

#### jQuery AJAX convenience methods

jQuery also offers a number of 'convenience methods' that use `$.ajax` behind the scenes. These are shorthand for more complicated `$.ajax` functions, meaning they're simpler to write, but less customizable.

| Convenience Method                                     | Long form $.ajax function                                    |
| -------------------------------------------| -------------------------------------------------|
| [$.get](http://api.jquery.com/jQuery.get/) | `$.ajax({` <br>&nbsp; ` dataType: dataType,` <br>&nbsp;` url: url,` <br>&nbsp;` data: data,` <br>&nbsp;` success: success` <br> `});` |
| [$.getJSON](http://api.jquery.com/jQuery.getJSON/) | `$.ajax({` <br>&nbsp;` dataType: "json",` <br>&nbsp;` url: url,` <br>&nbsp;` data: data,` <br>&nbsp;` success: success` <br> `});` |
| [$.getScript](http://api.jquery.com/jQuery.getScript/) | `$.ajax({` <br> ` dataType: "script",` <br> ` url: url,` <br> ` success: success` <br> `});` |
| [$.post](http://api.jquery.com/jQuery.post/) | `$.ajax({` <br>&nbsp;` type: "POST"` <br>&nbsp;` dataType: dataType,` <br>&nbsp;` url: url,` <br>&nbsp;` data: data,` <br>&nbsp;` success: success` <br> `});`|


#### jQuery `.load`
___

#### _JavaScript - AJAX with jQuery - Further Reading_

- [jQuery Fundamentals - AJAX & Deferreds](http://jqfundamentals.com/chapter/ajax-deferreds)
- [Learn jQuery - jQuery AJAX methods](https://learn.jquery.com/ajax/jquery-ajax-methods/)
- [jQuery API Documentation - $.ajax](http://api.jquery.com/jquery.ajax/)

___

### JavaScript - Same Origin Policy

The 'Same Origin Policy' is a browser security mechanism that restricts how a document or script loaded from one 'origin' can interact with a resource from another 'origin'.

An 'origin', in this context, is the combination of the protocol, port and host - you can generally just think of this as the 'domain'. The Same Origin Policy states that JavaScript that originated from one domain cannot request a resource from another domain. A request that breaches this protocol will generally respond with a 'No Cross-Origin Resource Sharing' error (a **CORS** error) in the developer tools console.

CORS errors are not easy to deal with.

#### Cross Origin Resource Sharing

A resource's Cross Origin Sharing policy can be checked by:

1. Sending an AJAX request to the URL (eg `$.ajax('http://www.smh.com.au')`):
  - If Cross Origin Resource Sharing is allowed, this will return an object with no errors;
  - If Cross Origin Resource Sharing is **not** allowed, this will return a CORS error along the following lines:

    ```sh
    XMLHttpRequest cannot load http://www.some-website.com/. No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin '[your_URL]' is therefore not allowed access.
    ```
2. Checking the Cross Origin Resource Sharing policy in the HTML's response headers - open up your browser's developer tools, click 'Network', select the document and look under the 'Response Headers':
  - If Cross Origin Resource Sharing is allowed, you will see something like: `Access-Control-Allow-Origin: *`

Websites can allow different levels of Cross Origin access - see the MDN Article on HTTP Access Control (CORS), below.

There are a few ways to deal with CORS errors, but they generally rely on the host of the resource you're trying to access accommodating your needs. The origin needs to:

- Allow CORS; or
- Support JSONP*; or
- Host your code.

\* JSONP ('JavaScript Browser Notation With Padding') is a super basic form of authentication. JSONP sends the server some information about the client (eg, the client's IP address) in the request headers, and allows the server to track use of their API, and prevent abuse of their API (eg, by denying repeated requests from the same IP address).


##### _Dealing with CORS in development_

Cross-Origin Resource Sharing is (for our purposes) only supported by http and https. Often when we're developing front-end components, we won't have a server running, meaning the protocol being used will be `file://` (not `http://` or `https://`), which will result in a CORS error (visible in the console). The easiest way to get around this is to use Python's SimpleHTTPServer module, instead of just opening the html directly in the browser using `file://` (eg, via Atom's Open In Browser package).

The following terminal command will create a web server using the contents of the current working directory:

```sh
python -m SimpleHTTPServer
# => Serving HTTP on 0.0.0.0 port 8000 ...

```

Then navigate to **localhost:8000** in your browser.

___

#### _JavaScript - Same Origin Policy - Further Reading_

- [MDN - Same Origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
- [MDN - HTTP Access Control (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

___

#### _JavaScript - APIs - Useful Links_

- [GitHub - Todd Motto - Public APIs](https://github.com/toddmotto/public-apis)

___

#### Homework

Build a cool single page thing using an API.
Some resources:
- [Nasa API](https://api.nasa.gov/)
- [International Space Station API](http://open-notify.org/Open-Notify-API/ISS-Location-Now/)
- [Wikipedia API](https://www.mediawiki.org/wiki/API:Main_page)
- [Wolfram alpha API](http://products.wolframalpha.com/api/)
- [Air Quality](https://docs.openaq.org/)
- [Beer Brewery](https://docs.openaq.org/)
- [Movie DB](https://developers.themoviedb.org/3)
- [Star Wars](https://swapi.co/)
- [Pokemon API](http://pokeapi.co/)
- [Donald Trump Quotes](https://docs.tronalddump.io/)
- [Marvel API](https://developer.marvel.com/)
- [Aus census API](https://www.data.gov.au/)
- [Other open APIs](https://github.com/toddmotto/public-apis)
- [Wiki list of open APIs](https://en.wikipedia.org/wiki/List_of_open_APIs)
- [The open web list of APIs](https://www.programmableweb.com/category/all/apis)


Further reading:
- [AJAX resources](https://gist.github.com/wofockham/3d4faf65dc9a5674ce11)
