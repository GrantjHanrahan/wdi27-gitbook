## Week 07, Day 01

What we covered today:
- Ruby - Test Driven Development (TDD)
- JavaScript - Callbacks Interlude
- JavaScript - AJAX and XHR

___

#### Warmup
- [Simple Sums](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week07/day01_simple_sums)

___

#### Codealong
- [TDD Bank](https://github.com/textchimp/wdi-27/tree/master/week7/intro-to-tdd)
- [AJAX requests](https://github.com/textchimp/wdi-27/tree/master/week7/ajax-intro)

___

### Test-Driven Development

Testing is huge in the Ruby and Rails communities.  It came out with Rails version 1 and has been a big part ever since.  Following the principles of T.D.D (Test Driven Development) is completely vital for large apps for tiny changes can change everything (as you all now know).  It is also vital because your final project needs to have 100% test coverage.

You normally follow the "Red-Green-Refactor" Cycle.  Write the tests, let them fail first, make them pass, then refactor if necessary.

The most common testing suite for Ruby is [Rspec](http://rspec.info/).

#### What is Rspec?

Rspec, like Sinatra, is a Ruby DSL - a language designed to solve problems in a particular 'domain' (domain as in 'field of stuff'). Whereas Sinatra is a DSL for creating web servers, Rspec is a DSL for testing applications. Like Sinatra, Rspec is a 'gem' (a Ruby library) that we include to give us methods we can use for a specific purpose - to write and run tests.

Rspec can be used in Rails or pure-Ruby environments.

#### Getting started with Rspec

##### _Install the Rspec Gem_

```sh
gem ` gem install rspec `.
```

##### _Create a new project_

```sh
mkdir tdd-bank
cd tdd-bank
```

##### _Create a program to test_

We're going to create a class called 'Bank', which will have all the code we need to run a bank. This is the file we'll be running our tests against.

```sh
touch bank.rb
```

And, in **bank.rb**...

```rb
class Bank

end
```

##### _Initialize Rspec_

In a non-Rails environment - i.e. pure Ruby - we need to do a few things to get it the testing suite working.  Run `rspec --init`.  This will make a spec folder and a .rspec file for us.  

```sh
rspect --init
# => create   .rspec
# => create   spec/spec_helper.rb
```

**.rspec** has all our configurations, and **spec_helper.rb** kicks everything off for us.


##### _Create a spec file_

The spec file will be home to our tests - it's where we describe how we want our application to behave. Create a file called 'bank_spec.rb' in your spec/ folder.

```sh
touch bank_spec.rb
```

Our project tree should now look like this:

```
| spec
|    | bank_spec.rb
|    | spec_helper.rb
| .rspec
| bank.rb
```

##### _Describe what we want our bank to do_

This is the essence of test driven development - before we even start adding methods to our `Bank` class, we write the tests describing how we expect our bank to behave. Four methods are the cornerstone of Rspec testing:

- `let`   
- `describe`
- `it`
- `expect`

```rb
describe Bank do
  # This sets up a description of a class (i.e. it will search for a class with the name Bank)
end
```

But, there isn't a `Bank` anywhere here - that class is defined in our **bank.rb** file,  so we need to require that file in our spec. You do this by adding ` require_relative "../bank" ` at the very top of the **bank_spec.rb** file.

We need to actually test individual pieces of everything in here (make sure they are tiny bits at a time!).

```rb
require_relative "../bank"

describe Bank do
  let (:bank) { Bank.new( 'TDD Bank' ) }
  # This 'let' method is like a rails 'before_action' - it will run every time a 'describe' method gets run.
  # In this case, for every block below (describe .new, describe "#create_account", etc, create a new Bank object and let that be called "bank".

  describe ".new" do
    # Class methods (like (Bank).new) are prefixed by a dot when describing them.
    it "Creates a new Bank object" do
      # We "expect" a bank object to not return 'nil' (to_not eq nil) once we have instantiated it.
      expect( bank ).to_not eq nil
    end
  end

  describe "#create_account" do
    it "creates a new account for a user with a given starting balance" do
      bank.create_acount 'Bob', 200
      expect(bank.accounts['Bob']).to eq 200
    end
  end
end
```

Basically, testing is broken up into lots of little bits.  A `describe` block for the class as a whole, then `describe` blocks for each class method and instance method, and then `it` blocks for each bit of functionality in the application.

Remember:

- **Class methods** are:
  - methods that we call on the class itself (eg `.new`)
  - prefixed with `.`, followed by the method name (eg, `.new`)
- **Instance methods** are:
  - methods that we call on instances of the class, rather than the class itself (eg `#create_account`)
  - prefixed with `#`, followed by the method name (eg, `#create_account`).

##### _Run your tests_

Run your tests crazily regularly.

```sh
rspec
```

Right now, we're going to see a whole bunch of failing tests:

```
1) Bank.new Creates a new Bank object
   Failure/Error: let (:bank) { Bank.new( 'TDD Bank' ) }

   ArgumentError:
     wrong number of arguments (1 for 0)
   # ./spec/bank_spec.rb:4:in 'initialize'
   # ./spec/bank_spec.rb:4:in 'new'
   # ./spec/bank_spec.rb:4:in 'block (2 levels) in <top (required)>'
   # ./spec/bank_spec.rb:12:in 'block (3 levels) in <top (required)>'

2) Bank#create_account creates a new account for a user with a given starting balance
   Failure/Error: let (:bank) { Bank.new( 'TDD Bank' ) }

   ArgumentError:
     wrong number of arguments (1 for 0)
   # ./spec/bank_spec.rb:4:in 'initialize'
   # ./spec/bank_spec.rb:4:in 'new'
   # ./spec/bank_spec.rb:4:in 'block (2 levels) in <top (required)>'
   # ./spec/bank_spec.rb:18:in 'block (3 levels) in <top (required)>'

Finished in 0.00109 seconds (files took 0.10468 seconds to load)
2 examples, 2 failures

Failed examples:

rspec ./spec/bank_spec.rb:10 # Bank.new Creates a new Bank object
rspec ./spec/bank_spec.rb:17 # Bank#create_account creates a new account for a user with a given starting balance

```

This helpfully tells us:

- What tests failed;
- Why those tests failed.

Let's go and fix up our Bank class in **bank.rb** to fix the errors one at a time. Let's start with Bank.new, which failed because our spec passed an argument (the name of the bank - 'TDD Bank') to the initialize method, but our Bank class doesn't have an initialize method (or a named parameter for that method).

```rb
class Bank
  def initialize(name)
    @name = name
  end
end

```

If we run `rspec` now, we see we're slowly moving from RED to GREEN - only one test has failed now:

```
Failures:

  1) Bank#create_account creates a new account for a user with a given starting balance
     Failure/Error: bank.create_account 'Bob', 200

     NoMethodError:
       undefined method `create_account' for #<Bank:0x007fdb9ba74cd8 @name="TDD Bank", @accounts={}>
     # ./spec/bank_spec.rb:18:in `block (3 levels) in <top (required)>'

Finished in 0.00276 seconds (files took 0.27577 seconds to load)
2 examples, 1 failure

Failed examples:

rspec ./spec/bank_spec.rb:17 # Bank#create_account creates a new account for a user with a given starting balance
```

Rspec has told us that there's an undefined method `create_account`, so let's add that method to our Bank class. We just keep continuing our work on the Bank class in **bank.eb** in this fashion until all our tests pass.

```rb
class Bank

  def initialize(name)
      @name = name
      @accounts = {}
  end

  def create_account(name, balance)
      @accounts[name] = balance
  end
end
```

If we run `rpsec` now, we get the following failure for our `#create_account` test:

```
NoMethodError:
  undefined method `accounts' for #<Bank:0x007f80e93749b8 @name="TDD Bank", @accounts={"Bob"=>200}>
# ./spec/bank_spec.rb:19:in `block (3 levels) in <top (required)>'
```

Okay, at the moment, we get a no method error for "accounts". Our spec sits outside the Bank class, so it can't _read_ that attribute of the class. We need to add an `attr_reader` for `:accounts`.

```rb
class Bank

    attr_reader :accounts
    def initialize(name)
        @name = name
        @accounts = {}
    end

    def create_account(name, balance)
        @accounts[name] = balance
    end
end
```

We're all green now, so would keep going through this process until our application was built:

- Pick a feature of the application;
- Create a test describing the behaviour you expect it to have;
- Run the test and see why it fails;
- Write the code necessary to make the test pass;
- Move on to the next feature.

___


### JavaScript - Callbacks Interlude

Callbacks are ridiculously important in JavaScript. They are absolutely everywhere and it's important to get a solid understanding of them.

JavaScript can be used asynchronously - we can write JavaScript in such a way that statements are removed from the main program flow so as not to block the execution of subsequent statements.

There are a couple of keys to understanding callbacks.

#### The problem with asynchronous JavaScript

```js
const do_first = function () {
    console.log("The do_first function was called");
}

const do_second = function () {
    console.log("The do_second function was called");
}

do_first();
do_second();
// => "The do_first function was called"
// => "The do_second function was called"
```

So that works!  The order has been kept.  But what happens if the `do_first()` function takes a little while longer than the `do_second()`?

```js
const do_first = function () {
    setTimeout( function(){
        console.log("The do_first function was called");
    }, 1000 );
}

const do_second = function () {
    console.log("The do_second function was called");
}

do_first();
do_second();
// => "The do_second function was called"
// => "The do_first function was called"

```

Well, that isn't ideal.  Obviously the `do_first()` is the function that we want to have completed before the `do_second()`. There are a bunch of different ways around this, but one of the best ways is using **callbacks**.

#### Functions are objects

Well, they are!  When push comes to shove, they are Function objects created using a Function constructor.  Interestingly enough, behind the scenes, a Function object contains a random string which contains JavaScript code.  Due to this fact, we can store functions as strings - and therefore pass functions in as arguments to other functions.

#### Passing in a function as an argument

A callback is just a function passed into another function. They allow for functions to be executed asynchronously.

```js
const do_first_and_second = function ( callback ) {
    console.log( "Do First." );
    callback();
}

const do_second = function () {
    console.log( "Do Second." );
}

do_first_and_second( do_second );
// => "Do first"
// => "Do second"
```

To work with the timing of the things...

```js
const do_first_and_second = function ( callback ) {
    setTimeout(function () {
        console.log( "Do First." );
        callback();
    }, 1000);
}

const do_second = function () {
    console.log( "Do Second." );
}

do_first_and_second( do_second );

// => "Do First."
// => "Do Second."

```

This is how things like `$(document).ready()`, `setTimeout()`, etc, work.

```js
console.log( "One" );

$(document).ready(function() {
  console.log( "Two" );
});

setTimeout(function() {
  console.log("Three")
}, 1000);

console.log( "Four" );

// => "One"
// => "Four"
// => "Two"
// => "Three"

```

___


#### _JavaScript - Callbacks Interlude - Further Reading_

- [JavaScript is Sexy - Understand JavaScript Callback Functions and How To Use Them](http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/)
- [SitePoint - Demistifying JavaScript Closures, Callbacks and IIFEs](https://www.sitepoint.com/demystifying-javascript-closures-callbacks-iifes/)
- [Amy Simmons - Understanding Closures, Callbacks and Promises](https://gist.github.com/amysimmons/3d228a9a57e30ec13ab1)

___

### JavaScript - AJAX and XHR

AJAX stands for 'Asynchronous JavaScript and XML'. It is a set of client-side techniques used to create asynchronous web applications. AJAX lets applications send and retrieve data from a server asynchronously, meaning that a page can be loaded/updated without interfering with the display and behaviour of the 'current' page, and without requiring the page to be reloaded.

#### XMLHttpRequests (XHR - Extensible Markup Language HTTP Requests)

XHR was something that was designed by Microsoft, and something that has since been adopted by Apple, Mozilla and Google. It is also now standardized with the [W3C.](http://www.w3.org/TR/XMLHttpRequest/)

It provides a way for us to get information from a given URL without refreshing the page, it can update just a part of a page. It's the cornerstone of AJAX.

Despite the name, XMLHttpRequests can be used to retrieve any type of data - including files.

#### The basic approach

- Create an instance of the XMLHttpRequest object.
- Open a HTTP request (on the instance).
- Send the Request (on the instance).

```js
// 1. Create an instance of the XMLHttpRequest object
const xhr = new XMLHttpRequest();
// 2. Open an HTTP request, in the format below:
//    xhr.open(HTTP VERB, PATH/URL)
xhr.open( "GET", "http://www.some-website.com/some-path" );
// 3. Send the request.
xhr.send();
```

Due to the asynchronous nature of JavaScript, we need to figure out a way to retrieve the data.  Once it has been been created, the XHR instance object includes a `readystate` key.  This key will have a value in the range of 0-4:

- **0**: request not initialized (not sent).
- **1**: server connection established.
- **2**: request received.
- **3**: processing request in the browser.
- **4**: request finished and response ready to be used.

We are provided with a handy `onreadystatechange()` function, which can listen for changes to the `readyState` of a request.

```js
const xhr = new XMLHttpRequest();
xhr.open( "GET", "http://www.some-website.com/some-path" );

xhr.onreadystatechange = function () {
    console.log( "Ready State: " + xhr.readyState );
    // This will print out a number between 0 and 4 depending on the state of the request.
}
xhr.send();
```

What we really want to do is make sure that everything has been loaded first.

```js

// Instantiate a new XHR request
const xhr = new XMLHttpRequest();
xhr.open("GET", "http://www.some-website.com/some-path");
// When the readystate of the request changes...
xhr.onreadystatechange = function () {
    // ... if the readystate is 4 (that is, the request has been returned and is ready to be used)...
    if ( xhr.readyState === 4 ) {
        // ... store the response in a variable called 'received_data', and...
        let received_data = xhr.responseText;
        // ... append that to the body element of the page.
        $( "body" ).append( received_data );
    }
}
xhr.send();

```

There is a better way to check whether `xhr.readystate === 4`, though - we can do it in a way that uses callbacks.

```js
xhr.onload = function () {
    // Success handler goes in here!
}
```

It's worth noting that a request's `readyState` will be 4 even if the response from the server wasn't what we were hoping for (eg, a 404 response, which indicates that the client was able to communicate with the server, but that the resource requested could not be found). Don't worry about this too much, but we might also want to check for the response status (eg `((xhr.readyState === 4) && (xhr.status === 200))`).

#### Parsing xhr.responseText as JSON

We're usually going to want to manipulate the data returned in the responseText. One thing we will commonly need to do is parse the responseText (a string) as JSON.

```js

const xhr = new XMLHttpRequest();
xhr.open("GET", "http://www.some-website.com/some-path");

xhr.onreadystatechange = function () {
    if ( xhr.readyState === 4 ) {
        let received_data = JSON.parse(xhr.responseText);
        $( "body" ).append( received_data["name"] );
    }
}
xhr.send();

```


#### _AJAX - Further Reading_

- [MDN - XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest).
- [MDN - Synchronous and Asynchronous Requests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Synchronous_and_Asynchronous_Requests).
- [MDN - Using XMLHttpRequest](https://developer.mozilla.org/en/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest).
- [Callback Hell](http://callbackhell.com/)
___

#### Homework

- Redo _Google Books_ using AJAX requests to make a single page app.
