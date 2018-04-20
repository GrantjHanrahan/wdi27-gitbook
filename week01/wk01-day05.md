## Week 01, Day 05

What we covered today:

- [Warmup and solution - Leapyear](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week01/day05_leapyear)
- Javascript - Review
- Javascript - Scope
- JavaScript - Advanced Objects
    - Methods
    - Factories and Constructors
- JavaScript - Advanced Functions
    - The `this` Keyword
    - The `arguments` Object

<!--
### Javascript - Review

- [Javascript - Review](https://www.teaching-materials.org/jsreview/#/1)
- Remember, we covered Arrays on day [three](wk01-day03.md) and Objects on day [four](wk01-day04.md)

___ -->
<!--
### Javascript - Scope

- [In class review of scope](https://github.com/wofockham/wdi-24/blob/master/01-javascript/scope/js/scope.js)

___ -->


### JavaScript - Advanced Objects

#### Methods

In JavaScript, functions are first-class. This means that the language supports:

- Constructing new functions during the execution of a program;
- Passing functions into other functions as arguments, and (relevantly);
- **Storing functions in data structures**;

Since we can store functions in data structures, we can store a function as the property of an object - a function stored as the property of an object is called a 'method'.

We've already seen a bunch of methods - for example, when we call `.toString()` on a number, we are actually calling the `.toString()` method of the global Number object (which is a property of that object).


#### Factories and Constructors

First off, both **constructors** and **factories** are "blue prints".  They bootstrap development.  Often they are more hassle than they are worth though, so be wary.  Think about whether all the code necessary to get a factory or constructor efficiently is actually worth it.  Object literals can get you through 95% of the time.

#### Factories


We can use 'factory' functions to create objects.

```js
const createCat = function(name, age, breed, food) {

  // Our factory can include a default value for the objects it produces
  if (food === undefined) {
    food = "Bacon"
  }
  return {
    name: name,
    age: age,
    breed: breed,
    // Building on our knowledge of functions in objects (ie, methods), lets add a few methods to objects produced by this factory
    meow: function() {
      console.log("Meooooow");
    },
    eats: function(food) {
      console.log("Lewis eats " + food)
    }
  };
};

const lewis = createCat("Lewis", 1, "Maine Coon", "Strawberries");

const cats = [
  createCat("Audrey", 1, "Domestic Shorthair", "Tuna"),
  createCat("Lewis", 1, "Maine Coon", "Strawberries"),
  createCat("Cooper", 1, "Domestic Shorthair")
]

```
<!--
##### _Inheritence_

We can create factories that inherit properties from other factories, thereby producing objects with properties from inherited from two factory functions.

In the example below, the generic `AnimalFactory` creates and object and, importantly, returns that object. Since the function returns an object, when we call the AnimalFactory from the CatFactory and DogFactory functions, a generic animal object is returned to the function that called it, and further properties (specific to CatFactory and DogFactory) are added to the object.

```js

const AnimalFactory = function() {
  let animal = {};
  animal.kingdom = "Animal";
  animal.alive = true;
  animal.eating = function() {
    console.log("Leave me alone, I'm eating");
  };
  return animal;
}

const CatFactory = function(name, breed) {
  let cat = AnimalFactory();
  cat.name = name;
  cat.breed = breed;
  cat.food = function() {
    console.log("Here's your dinner, " + cat.name + "!");
  };
  return cat;
}

const DogFactory = function(name, breed, toy) {
    let dog = AnimalFactory();
    dog.name = name;
    dog.breed = breed;
    dog.toy = toy;
    dog.play = function() {
        console.log("Here's your " + ", " + dog.name + "!");
    }
    return dog;
}

const lewis = CatFactory("Lewis", "Maine Coon");
const baxter = DogFactory("Baxter", "Wolfhound", "squeaky toy");

// The objects produced by the CatFactory and DogFactory functions will both inherit properties and methods from the AnimalFactory, as well as the properties and methods from their own factory.

lewis.food();
// => "Here's your dinner, Lewis!"
lewis.alive
// => true

dog.play();
// => "Here's your squeaky toy, Baxter!"
dog.eating();
// => "Leave me alone, I'm eating"

``` -->

#### Constructors

Constructors are essentially factories for objects that are invoked via the `new` operator. When a constructor is invoked using the `new` operator, a constructor will:

- Create a new object
- Set `this` within the function to that object
- Return the object.

Normally, JS objects are only maps from strings to values, but JS also supports prototypal inheritance - something that is truly object-oriented. They are quite similar to classes in other languages.

##### _What does it entail?_

The initial set up - this is where you set up the instance data...

```js
const Point = function ( x, y ) {
    this.x = x;
    this.y = y;
}
```

The application of methods or functions...  These will apply to any instance.

```js
Point.prototype.dist = function () {
    return Math.sqrt( this.x * this.x + this.y * this.y );
};
```

The invocation of a new instance.

```js
let p = new Point( 3, 4 );
console.log( "Point X: " + p.x );
```

We can also check to see if an object is an instance of a constructor:

```js
typeof p
// Returns "object"

p instanceof Point
// true
```
___

#### _JavaScript - Advanced Objects - Factories and Constructors - Further Reading_

- [Phrogz - OOP in JS - Inheritence](http://phrogz.net/JS/classes/OOPinJS2.html)

___

### Javascript - Advanced Functions

<!-- - [JavaScript - `this`, Factories, and Constructors](https://github.com/ga-wolf/wdi-16/blob/master/01-javascript/this_and_fancy_objects/this-factories-and-constructors.pdf) -->

#### The `this` keyword (AKA self)

The `this` keyword is one of the most powerful things in JavaScript, but also one of the hardest to understand. It gets insanely complicated, and there are a lot of exceptions to the simplistic generalizations below.

In JavaScript, `this` will always refer to the owner of the function we are executing. If the function is not within an object, or another function, `this` will refer to the global object (or window).  Window is an object that exists in every browser, applying keys and values to this object will make them globally accessible.  Don't do it regularly though.

```js
// GLOBAL THIS //
const doSomething = function () {
    console.log( this );
    // Will log the window object
}

// OBJECT THIS //
const objectFunction = {
    testThis: function () {
        console.log( this );
        // Would log the objectFunction object
    }
}
objectFunction.testThis();

// EVENT THIS //
const button = document.getElementById("myButton");
// This is a basic click handler - you aren't expected to understand this yet!
button.addEventListener( "click", function() {
    console.log( this );
    // Will log the HTML element that this event ran on (button with id myButton)
});
```

#### A simplistic generalization of `this`

- In a simple function (one that isn't in another function or object) - `this` stays as the default - window.
- In a function that is within an object, `this` is defined as the object - it's immediate parent.
- In an event handler (a function that is called based on browser interaction), `this` is defined as the element that was interacted with.

The `this` keyword is really useful when we have a function that accepts an object as an argument - we can use `this` to access properties of the object.

#### Global / default binding

```js
sayHello();
```

#### Implicit binding
When we call a function through an object, `this` becomes the object.

```js
dog.sayHello();
```

#### Explicit binding

```js
const sayHello = function() {
    console.log("Hello, " + this.name);
}

const person = {
    name: "Lewis"
}

// Using the .call method
sayHello.call(person);
// => "Hello, Lewis"

// Using the .apply method
sayHello.apply(person);
const hi = sayHello.bind(person);
// This creates a new function where the keyword "this" will always represent the person object passed into the .bind method.
hi();
// => "Hello, Lewis"
// Creates a new function where the keyword 'this' will always represent 'person', and can be called using hi();
```

#### `new` binding

```js
const Person = function(name) {
    this.name = name;
    this.sayHello = function() {
        console.log("Hello, " + this.name);
    }
}

const lewis = new Person("Lewis");

lewis.name
// => "Lewis"
lewis.sayHello();
// => "Hello, Lewis"
```

___

#### _JavaScript - `this` - Recommended Reading_
- [REPL - Lizzie the Cat](https://repl.it/lwW/1)
- [Kyle Simpson - You Don't Know JS - `this` and Object Prototypes](https://github.com/getify/You-Dont-Know-JS/tree/master/this%20%26%20object%20prototypes)
- [Todd Motto - Understanding the 'This' Keyword in JavaScript](http://toddmotto.com/understanding-the-this-keyword-in-javascript/)
- [MDN - This](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)
- [JavaScript is Sexy - Understand JavaScript's 'This' With Clarity ](http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it/)
- [Quirks Mode - This](http://www.quirksmode.org/js/this.html)

___

#### JavaScript - The `arguments` object

The `arguments` object is an array-like object that can be used to access the arguments passed into a function, regardless of whether they match named parameters.

##### _Using 'arguments' to create default arguments_
If we call a function without passing in arguments to match the number of named parameters, we run into `undefined` problems.

```js
const nameImprover = function (name, adj) {
	return 'Col ' + name + ' Mc' + adj + ' pants';
};

nameImprover("Badger");
// => "Badger Mcundefined pants"
```

We can check whether any of the arguments are undefined, and set a default value if so:

```js

const nameImprover = function (name, adj) {
  if (adj === undefined) {
    adj = "Fancy"
  }
	return 'Col ' + name + ' Mc' + adj + ' pants';
};

nameImprover("Badger");
// => "Badger McFancy pants"

```

##### _Using `arguments` to create variadic functions_

A **variadic function** is a function where the total number of parameters is unknown when the function is declared, and can vary when the function is called. We can use the arguments object to create variadic functions in JavaScript.

```js

// Create a function that will return the sum of any and all numbers passed in as arguments
const addNumbers() {
  let sum = 0;
  for (let i = 0; i < arguments.length; i++) {
    sum += arguments[i];
  }
  return sum;
}

addnumbers(3,5);
// => 8

addNumbers(3,5,7,9);
// => 24
```

___

### Homework

- [MTA](https://gist.github.com/textchimp/7c83f4458f6b45ba5d31b0d21607c87a)
- [JavaScript Readings](https://gist.github.com/wofockham/8a702a9bf0a1456df7d4)
