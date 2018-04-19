## Week 01, Day 04

What we covered today:
  - [Warmup and solution - Serge - JS](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week01/day04_serge)
  - Review [Shakespeare insult generator](https://github.com/textchimp/wdi-27/tree/master/week1/insult-generator)
  - JavaScript - Objects
___

#### Slides

- [Javascript Objects](https://textchimp.github.io/wdi-27/week1/javascript-collections.pdf)
___
#### Classwork

- [Shakespeare insult generator](https://github.com/textchimp/wdi-27/tree/master/week1/insult-generator)
- [Exercises](https://github.com/textchimp/wdi-27/tree/master/week1)
___


### JavaScript - Collections - Objects

In JavaScript, an object is a standalone entity - filled with properties and types (or keys and values).  It is very similar in structure to a dictionary.

So, most javascript objects will have keys and values attached to them - this could be considered as a variable that is attached to the object (also allows us to iterate through them).

They are sometimes called "associative arrays".  Remember that they are not stored in any particular order (they can change order whenever).


#### How to create an object

```js
// With object literal
const newObject = {};

// Using Object
const newObject = new Object();
```

#### How to add properties to an object

```js
// Remember to seperate by commas!
const newObject = {
  objectKey: "Object Value",
  anotherObjectKey: "Another Object Value",
  objectFunction: function () {

  }
};

const newObject = {};
newObject.objectKey = "Object Value";
newObject.objectFunction();
newObject["anotherObjectKey"] = "Another Object Value";

// Can also use Constructors and Factories - see Week 1 Day 5 notes.
```

#### How to access properties of an object

There are two ways to access the properties of an object:

- **Dot notation** - syntax: `object.propertyName`
- **Square bracket notation** - `object["propertyName"]`

Remember: like all JS variables - both the object name and property names are case sensitive.

```js
const favouriteCar = {
  manufacturer: "Jaguar",
  year: 1963,
  model: "E-Type"
}

// Dot notation access to object properties
favouriteCar.year

// Square bracket notation access to object properties
favouriteCar["year"]
```

- [Objects: Exercises](https://gist.github.com/textchimp/23db045edf474762828a9e912912c873)


#### How to iterate through an object

```js
Object.keys(newObject); // Returns an array of all the keys in the specified object.
Object.getOwnPropertyNames(newObject); // So does this

const obj = {
  a: 1,
  b: 2,
  c: 3
};

for (let prop in obj) {
  console.log( "o." + prop + " = " + obj[prop] );
}

```

#### Deleting properties of an object

```js
const favouriteCar = {
  manufacturer: "Jaguar",
  year: 1963,
  model: "E-Type"
}

delete favouriteCar.year;
```

#### Comparing objects

In JavaScript objects are a reference type. Two distinct objects are never equal, even if they have the same properties. Only comparing the same object reference with itself yields true.

```js
// Two variables, two distict objects with the same properties
const fruit = { name: "apple" };
const fruitbear = { name: "apple" };

fruit == fruitbear;
// => false
fruit === fruitbear;
// => false

// Two variables, a single object
const fruit = { name: "apple" };
const fruitbear = fruit;  // Assigns fruit object reference to fruitbear

// Here fruit and fruitbear are pointing to same object
fruit == fruitbear;
// => true
fruit === fruitbear;
// => true
```

There's no simple way to compare objects using 'vanilla' JavaScript, but there are some JavaScript libraries that make object comparison easier - UnderscoreJS, for example, has an method - "\_.isEqual", which tests equality based on object properties. There are lots of alternatives - for example, check out  [this](http://stackoverflow.com/questions/1068834/object-comparison-in-javascript) Stack Overflow thread - but I would stick to Underscore's method. [Underscore](http://underscorejs.org/) is a great JavaScript library which we'll be talking about it in great detail later on.

___

#### _JavaScript - Collections - Objects - Exercises_

- [Objects](https://gist.github.com/textchimp/23db045edf474762828a9e912912c873)

___

### Homework

- [Javascript Bank](https://gist.github.com/textchimp/be5cff64c320d0e0aa3008db0f3bfe85)
- Bonus - Read:
    + [MDN - Arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) - MDN is an amazing resource for JavaScript, HTML and CSS.
    + [JavaScript Tutorials - Arrays](http://javascript.info/tutorial/array)
    + [Speaking JavaScript - Arrays](http://speakingjs.com/es5/ch01.html#basic_arrays)
