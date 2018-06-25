## Week 10, Day 04

What we covered today:

  - Node.js Testing
  - Regular expressions

____

#### Warmup

- [Wordy Calculator](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week11/day_01_wordy_calculator)

___

#### Codealongs

- [node-testing-jasmine](https://github.com/textchimp/wdi-27/tree/master/week11/node-testing-jasmine)
- [regular-expressions](https://github.com/textchimp/wdi-27/tree/master/week11/regular-expressions)

### Testing in Node

Testing is a key element to any application. For Node.js, a Framework available for Testing is called Jasmine. Jasmine was initially known as JsUnit before being upgraded and renamed.

Jasmine helps in automated Unit Testing, something which has become quite a key practice when developing and deploying modern day web applications.

#### _Overview of Jasmine for testing Node.js applications_

Jasmine is a **Behavior Driven Development(BDD)** testing framework for JavaScript. It does not rely on browsers, DOM, or any JavaScript framework. Thus, it's suited for websites, Node.js projects, or anywhere that JavaScript can run. To start using Jasmine, you need to first download and install the necessary Jasmine modules.

Next you would need to initialize your environment and inspect the jasmine configuration file. The below steps show how to setup Jasmine in your environment

**Step 1)** Installing the NPM Modules

To use the jasmine framework from within a Node application, the jasmine module needs to be installed first. To install the jasmine-node module, run the below command.

```bash
npm install jasmine-node
```

**Step 2)** Initializing the project – By doing this, jasmine creates a spec directory and configuration json for you. The spec directory is used to store all your test files. By doing this, jasmine will know where all your tests are, and then can execute them accordingly. The JSON file is used to store specific configuration information about jasmine.

To initialize the jasmine environment, run the below command

```bash
jasmine init
```

**Step 3)** Inspect your configuration file. The configuration file will be stored in the spec/support folder as jasmine.json. This file enumerates the source files and spec files you would like the Jasmine runner to include.

```javascript
{
  "spec_dir": "spec",
  // Note that the spec directory is specified here. When jasmine runs it searches for all tests in this directory.
  "spec_files": [
    // The next thing to note is the spec_files parameter – This denotes that whatever test files are created they should be appended with the 'spec' keyword.
    "**/*[sS]pec.js"
  ],
  "helpers": [
    "helpers/**/*.js"
  ],
  "stopSpecOnExpectationFailure": false,
  "random": "false"
}
```

#### _Using Jasmine to test Node.js applications_

In order to use Jasmine to test Node.js applications, a series of steps needs to be followed.

In the example below, we are going to define a module which add 2 numbers which need to be tested. We will then define a separate code file with the test code and then use jasmine to test the Add function accordingly.

**Step 1)** Define the code which needs to be tested. We are going to define a function which will add 2 numbers and return the result. This code is going to be written in a file called `Add.js.`

```javascript
// The "exports" keyword is used to ensure that the functionality defined in this file can actually be accessed by other files.
var exports = module.exports = {};

// We are then defining a function called 'AddNumber.' This function is defined to take 2 parameters, a and b. The function is added to the module "exports" to make the function as a public function that can be accessed by other application modules.
exports.AddNumber = function( a,b ){
  // We are finally making our function return the added value of the parameters.
  return a+b;
};
```

**Step 2)** Next we need to define our jasmine test code which will be used to test our "Add" function In the Add.js file. The below code needs to put in a file called add-spec.js.

_Note:_ - The word 'spec' needs to be added to the test file so that it can be detected by jasmine.

```javascript
// We need to first include our Add.js file so that we can test the 'AddNumber' function in this file.
var app = require("../Add.js");

// We are now creating our test module. The first part of the test module is to describe a method which basically gives a name for our test. In this case, the name of our test is "Addition".
describe("Addition",function(){
  // The next bit is to give a description for our test using the 'it' method.
  it("The function should add 2 numbers",function() {
    // We now invoke our Addnumber method and send in 2 parameters 5 and 6. This will be passed to our Addnumber method in the App.js file. The return value is then stored in a variable called value.
    var value = app.AddNumber(5,6);
    // The final step is to do the comparison or our actual test. Since we expect the value returned by the Addnumber function to be 11, we define this using the method expect(value).toBe(the expected value).
    expect(value).toBe(11);
  });
});
```

In order to run the test, run the command `jasmine`.

#### _Testing with Jasmine and Nodemon_

First we will create a Node project. Using the -y flag will skip the usual project setup questions.

```bash
mkdir node-testing
cd node-testing
npm init -y
```

We will then add the Jasmine and Nodemon Node packages

```bash
npm install --save-dev jasmine nodemon
```

Our package.json will now look like the below

```javascript
{
  "name": "node-t2",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "jasmine": "^3.1.0",
    "nodemon": "^1.17.5"
  }
}
```

Now, we will add a `lib` folder to the project and within that an  `adder.js` file that will contain the code that will be tested

```bash
# => node-testing
mkdir lib
cd lib
lib/ touch adder.js
```

#### _Debugging Node_

To add Node debugging to our project, we can add the Google Chrome extension [Node.js V8 --inspector Manager (NiM)](https://chrome.google.com/webstore/detail/nodejs-v8-inspector-manag/gnhhdgbaldcilmgcpfddgdbkhjohddkj?hl=en). NiM manages the Chrome DevTools window/tab life-cycle leaving you with more ability to debug.

To add the NiM functionality into our project, edit the package.json to inclde a `watch` script

```javascript
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "debug": "node --inspect-brk lib/adder.js"
},
```
___


Inside `adder.js` add the following module;

```javascript
module.exports = {

  add(a, b){
    const sum = a + b;
    return sum;
  }
}
```

Next add a spec file to your project with the following command `./node_modules/.bin/jasmine init`. This will generate the following;

```bash
# in node-testing
spec
  - support
    - jasmine.json
```

From the spec folder create a file for writing the Jasmine tests;

```bash
# in node/ spec
touch adder.spec.js
```

In `adder.spec.js` require the `adder.js` file to gain access to it. This line works the same way as the `import` line in React

```javascript
// in adder.spec.js
const adder = require('../lib/adder');
```

Add the first test to the `adder.spec.js` file;

```javascript
describe('Adder module', () => {

  it('should export the add function', () => {
    expect( typeof adder ).toBe( 'object' )
  });

});
```

To run the test using NPM, adjust the `"test"` script in package.json

```javascript
"scripts": {
  "test": "./node_modules/.bin/jasmine || true",
  "debug": "node --inspect-brk lib/adder.js"
},
```

You can now run the test from your project with `npm run test`

```bash
# in node-testing
npm run test

# test output
> node-t2@1.0.0 test /Users/granthanrahan/wdi-27/revision/w11/node-t2
> jasmine || true

Randomized with seed 63925
Started
.

1 spec, 0 failures
Finished in 0.006 seconds
Randomized with seed 63925 (jasmine --random=true --seed=63925)
```

#### _Automated testing with Nodemon_

Nodemon can be used to automate the tests so that you do not need to run `npm run test` each time.

In package.json add the following `"watch"` script

```javascript
"scripts": {
  "test": "./node_modules/.bin/jasmine || true",
  "debug": "node --inspect-brk lib/adder.js",
  "watch": "nodemon --exec ./node_modules/.bin/jasmine"
},
```

Now the tests can be run with the command `npm run watch`

___


#### _Spies_

Jasmine has test double functions called spies. A spy can stub any function and tracks calls to it and all arguments. A spy only exists in the describe or it block in which it is defined, and will be removed after each spec. There are special matchers for interacting with spies.

In `adder.spec.js` include the following code to add a spy to the add() function

```javascript
const adder = require('../lib/adder');

beforeEach(() => {
  spyOn(adder, 'add').and.callThrough();
});
```

___

### Regular Expressions I

Regular Expressions are pattern matchers for text. Sounds relatively dry (or very, very dry) but they are incredibly powerful.  They are often considered a write-only language.  You can write it, but it is virtually impossible to read.  They occur in lots and lots of programming languages (they are even in javascript), and if you are interested in the history of them - see [here](https://en.wikipedia.org/?title=Regular_expression#History) - but the way that Ruby works with them stems from the Perl programming language.

#### So, how do we work with them in Ruby?

##### _Literal Characters_

We can match literal characters in virtually the same way as we would with two equals signs.  Instead of using the two equals signs though, we use ` =~ ` , think of this as a fuzzy or dynamic search.  To specify a regular expression in Ruby (and most other languages), we use ` // ` (these are considered Regexp delimiters).  Let's have a muck around with them.

```rb
"bob" == "bob" # This obviously returns true.

"bob" =~ /bob/
# This is true, they do match. But instead of returning true, it returns the index of where the pattern started to match. In this case, it returns 0.  It returns nil if it doesn't match anything.

"Hello, my name is bob" =~ /bob/
# Again, this is true, but returns 18
```

Obviously this isn't the powerful part though, considering this does virtually the same thing as the multiple equals signs.  So let's get into some of the cooler stuff.

##### _Metacharacters_

In regular expressions, there are characters that mean far more than literal characters.  This can prove problematic (but there are ways to escape metacharacters), but also the source of some of the most powerful parts.

```rb
"o!o" =~ /o.o/ # Returns 0
```

##### _Character Classes_

These are often used to check for capitalized and uncapitalized versions, but can also be used to check for multiple letters.  The metacharacters for these are ` [] `.

```rb
"bob" =~ /[Bb]ob/  # Returns 0
"Bob" =~ /[Bb]ob/  # Returns 0
"cob" =~ /[Bbc]ob/ # Returns 0
"dog" =~ /[Bb]ob/  # Returns nil
"any vowels" =~ /[aeiou]/ # Returns 0
```

For this sort of stuff, we can also use ranges. These are quite useful.

```rb
"A" =~ /[A-Z]/      # Returns 0
"a" =~ /[A-Z]/      # Returns nil
"a" =~ /[A-z]/      # Returns 0
```

There are lots of inbuilt ranges, and these are really useful to know.  Some of them are...

- ` \s ` - Will match any space characters (spaces, new lines etc.)
- ` \S ` - Anything other than space characters
- ` \w ` - Will match any word character (i.e. actual letters or numbers)
- ` \W ` - Will match any non-word character

```rb
"Wolf" =~ /[\w]/    # Returns 0
"Wolf" =~ /[\W]/    # Returns nil
"Wolf " =~ /[\W]/   # Returns 4

"Wolf " =~ /[\s]/   # Returns 4
"Wolf " =~ /[\S]/   # Returns 0
```

We can obviously add lots and lots of things between those square brackets.  But there are other ways we can do this as well.  We can use ` () ` and ` | ` to check for multiple things.

```rb
"Jane" =~ /(Jane|Serge)/    # Returns 0
"Serge" =~ /(Jane|Serge)/   # Returns 0
```

The round brackets are metacharacters in Regexp as well!  They are a way to say this or this (or this or this or this).  The pipe is the delimiter for saying "or".

There are a lot more metacharacters. The ` . `, for example, is a wildcard, it will match anything.

```rb
"jane" =~ /.ane/            # Returns 0
"zane" =~ /.ane/            # Returns 0
"Serge and Jane" =~ /.ane/  # Returns 10
```

This is still relatively hardcoded though, we need to specify where the characters are and what they are.  To help solve this problem, there are quantifiers.

##### _Quantifiers_

Quantifiers are a way to check in Regexp whether things exist, or exist more than once etc.

The ones that you will actually use:
- ` + ` means one or more
- ` ? ` means one or zero
- ` * ` means zero or more

It can look like the following:

```rb
"Hi there" =~ /i+/      # Returns 1
"Hi there" =~ /the?/    # Returns 3
"Hi theeere" =~ /the*/  # Returns 3
```

These are regularly used for existence checks.

##### _Capturers_

These are difficult to understand.  Basically, you match something in parentheses and can refer back to them.  You capture something by using round brackets, and refer back to them using a ` \ ` and an integer (that mimics the order of capture).

```rb
"WolfWolf" =~ /(....)\1/
# Matches any four characters, but then needs to have the same four characters straight after.  This returns zero.

"ArcticWolf ArcticWolf" =~ /(......)(....) \1\2/
# Matches "Arctic" in the first brackets, then "Wolf" in the second brackets.  "Arctic" is saved as \1 and "Wolf" is saved as \2
```

##### _Regex Cheatsheet_

- [Cheatsheet](https://www.ralfebert.de/snippets/ruby-rails/regex_cheat_sheet/)
___

#### Homework

- [Regex](https://gist.github.com/ga-wolf/0d1e53de6fdd1e402c303e59deb59d2a)
- [Interview Questions](https://gist.github.com/textchimp/ce8b6ec8a24f9bb99a6f2ab9b6752c69)

#### _Review_

- [Using Jasmine to test Node.js Applications](https://blog.codeship.com/jasmine-node-js-application-testing-tutorial/)
- [Jasmine](https://jasmine.github.io/)

- [THE Regular Expression book](http://shop.oreilly.com/product/9780596528126.do)
  - [Good Regexp chapters:](http://shop.oreilly.com/product/0636920018452.do)
- [Regexp in Ruby](http://ruby-doc.org/core-2.1.1/Regexp.html)
- [RegExp in JS](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
- [Test regular expressions interactively:](http://rubular.com/)
- [Regex Crossword](https://regexcrossword.com/)

___
