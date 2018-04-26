## Week 02, Day 03

What we covered today:
- JavaScript - The Document Object Model (DOM)
  + Accessing Elements (Selection)
  + Manipulating Elements
  + Creating Elements
  + Events
- JavaScript - Animation
  + Timers
  + Callbacks
- Emmet

___
#### Slides
- [The DOM, events, selectors,
timers and callbacks](https://slack-files.com/files-pri-safe/T0351JZQ0-FADHW6EUV/javascript-and-html.pdf?c=1524725103-2103b142e47284b35dd956c1ef015bfcccddd2bc)

<!--

- [Javascript Making webpages interactive](https://www.teaching-materials.org/jsweb/slides/welcome#/)
- [JavaScript and the DOM](http://www.teaching-materials.org/jsweb/slides/dom.html)
- [Selecting from the DOM](http://www.teaching-materials.org/jsweb/exercises/dom_detective.html)
- [Events and Animation](http://www.teaching-materials.org/jsweb/slides/animevents.html)
- [Chrome Developer Tools](https://developers.google.com/web/tools/chrome-devtools/) -->
<!-- ___ -->

### JavaScript - The Document Object Model (DOM)

#### What is the DOM?

The **Document Object Model**, or 'DOM':

1. Is a structural representation of HTML documents - the document is represented as a tree with nodes; and
2. Defines methods for accessing, manipulating and creating those nodes.

JavaScript can't access or manipulate what's in the browser directly - that's just a bunch of pixels. JavaScript can, however, interact with the DOM.

The DOM is HTML after it has been parsed by the browser. It can be different to the HTML you wrote, if the browser fixes issues in your code, if JavaScript changes it etc.


#### How do we use it?

The document object gives us ways of accessing and changing the DOM of the current webpage.

General strategy:
- Find the DOM node using an access method
- Store that DOM node in a variable
- Manipulate the DOM node by changing its attributes, styles, inner HTML, or appending new nodes to it.

#### Accessing elements (Selection)

##### _Get element by ID_

```js
// The method signature:
// document.getElementById(id);

// If the HTML had:
// <img id="mainpicture" src="http://fillmurray.com/200/300">
// We'd access it this way:
let img = document.getElementById('mainpicture');

// DON'T USE THE HASH!
```

##### _Get elements by tag name_

```js
// The method signature:
// document.getElementsByTagName(tagName);

// If the HTML had:
<li class="catname">Lizzie</li>
<li class="catname">Daemon</li>

// We'd access it this way:
let listItems = document.getElementsByTagName('li');
for (let i = 0; i < listItems.length; i++) {
  let listItem = listItems[i];
}
```

##### _Query selector and query selector all_

```js
// The HTML5 spec includes a few even more convenient methods.
// Available in IE9+, FF3.6+, Chrome 17+, Safari 5+:

document.getElementsByClassName(className);
let catNames = document.getElementsByClassName('catname');
for (let i = 0; i < catNames.length; i++) {
  let catName = catNames[i];
}

// Available in IE8+, FF3.6+, Chrome 17+, Safari 5+:
document.querySelector(cssQuery);
document.querySelectorAll(cssQuery);
let catNames = document.querySelectorAll('ul li.catname');
```

Remember, some of these methods return arrays and some return single things!

##### _Methods that return single elements_

```js
getElementById();
querySelector(); // returns only the first of the matching elements
  let firstCatName = document.querySelector('ul li.catname');
```

##### _Methods that will return an array of elements_

Others return a collection of elements in an array: (NOTE: not exactly)

```js
getElementByClassName();
getElementByTagName();
querySelectorAll();

let catNames = document.querySelectorAll('ul li.catname');
let firstCatName = catNames[0];
```
___

#### Manipulating elements

##### _Changing an element's attributes_

You can access and change attributes of DOM nodes using JavaScript object dot notation.

If we had this HTML:

<img id="mainpicture" src="http://fillmurray.com/200/300">

We can change the src attribute this way:

```js
let oldSrc = img.src;
img.src = 'http://fillmurray.com/100/500';

// To set class, use the property className:
img.className = "picture";
```

##### _Changing an element's style_

You can change styles on DOM nodes via the style property.

If we had this CSS:

```css
body {
  color: red;
}
```

We'd run this JS:

```js
let pageNode = document.getElementsByTagName('body')[0];
pageNode.style.color = 'red';
```

**IMPORTANT:** CSS property names with a "-" must be camelCased and number properties must have a unit:

For example, to replicate this CSS:

```css
body {
  background-color: pink;
  padding-top: 10px;
  font-style: oblique;
}
```

We would need to do this:

```js
let pageNode = document.querySelector('body');
pageNode.style.backgroundColor = 'pink';
pageNode.style.paddingTop = '10px';
padgeNode.style.fontStyle = 'oblique';
```

##### _Changing an element's HTML_

Each DOM node has an innerHTML property with the HTML of all its children:

`let pageNode = document.getElementsByTagName('body')[0];`

You can read out the HTML like this:

`console.log(pageNode.innerHTML);`

```js
// You can set innerHTML yourself to change the contents of the node:
pageNode.innerHTML = "<h1>Oh Noes!</h1> <p>I just changed the whole page!</p>"

// You can also just add to the innerHTML instead of replace:
pageNode.innerHTML += "...just adding this bit at the end of the page.";
```
___

#### Creating elements

The document object also provides ways to create nodes from scratch:

`document.createElement(tagName);`

`document.createTextNode(text);`

`document.appendChild();`

```js
let pageNode = document.getElementsByTagName('body')[0];

let newImg = document.createElement('img');
newImg.src = 'http://fillmurray.com/400/300';
newImg.style.border = '1px solid black';
pageNode.appendChild(newImg);

let newParagraph = document.createElement('p');
let paragraphText = document.createTextNode('Squee!');
newParagraph.appendChild(paragraphText);
pageNode.appendChild(newParagraph);
```
___

##### _Document Object Model - Manipulating elements - Exercises_

- [Exercises: DOM Access](https://gist.github.com/textchimp/f395efd8f5a8c3c2750b4af455078ac1)
- [Exercises: Logo Hijack](https://gist.github.com/textchimp/2609786b842837f5d6f276e0dee6665d)
- [Exercises: About Us](https://gist.github.com/textchimp/7aba85195a551a8dc1c71209a2c6ac8a)
___

#### Events

##### _Adding event listeners_

In IE 9+ (and all other browsers):

`domNode.addEventListener(eventType, eventListener, useCapture);`

```js
// HTML = <button id="counter">0</button>

const counterButton = document.getElementById('counter');
const button = document.querySelector('button')
button.addEventListener('click', makeMadLib);

const onButtonClick = function() {
    counterButton.innerHTML = parseInt(counterButton.innerHTML) + 1;
};
counterButton.addEventListener('click', onButtonClick, false);
```

##### _Some event types_

The browser triggers many events. A short list:

- **mouse events** (MouseEvent): mousedown, mouseup , click, dblclick, mousemove, mouseover, mousewheel , mouseout, contextmenu
- **touch events** (TouchEvent): touchstart, touchmove, touchend, touchcancel
- **keyboard events** (KeyboardEvent): keydown, keypress, keyup
- **form events**: focus, blur, change, submit
- **window events**: scroll, resize, hashchange, load, unload

##### _Getting details from a form_

```js
// HTML
// <input id="myname" type="text">
// <button id="button">Say My Name</button>

const button = document.getElementById('button');
const onClick = function(event) {
    let myName = document.getElementById("myname").value;
    alert("Hi, " + myName);
};
button.addEventListener('click', onClick);
```

- [JSBin - Event Listener - button click](http://jsbin.com/goguxomiwo/edit?js,output)

___

#### _Document Object Model - Events - Exercises_

- [Madlibs and Calculator](https://gist.github.com/wofockham/e440100cee2a3eb7e606)

___

### JavaScript - The Window Object

When you run JS in the browser, it gives you the window object with many useful properties and methods:

```js
window.location.href; // Will return the URL
window.navigator.userAgent; // Tell you about the browser
window.scrollTo(10, 50);
```

Lots of things that we have been using are actually a part of the window object.  As the window object is the assumed global object on a page.

```js
window.alert("Hello world!"); // Same things
alert("Hello World!");

window.console.log("Hi"); // Same things
console.log("Hi");
```


### Animation in JavaScript

#### Starting an animation

The standard way to animate in JS is to use these 2 window methods.

To call a function once after a delay:

`window.setTimeout(callbackFunction, delayMilliseconds);`

To call a function repeatedly, with specified interval between each call:

`window.setInterval(callbackFunction, delayMilliseconds);`

Commonly used to animate DOM node attributes:

```js
const makeImageBigger = function() {
  const img = document.getElementsByTagName('img')[0];
  img.setAttribute('width', img.width+10);
};
window.setInterval(makeImageBigger, 1000);
```

#### Animating styles

It's also common to animate CSS styles for size, transparency, position, and color:

```js
let img = document.getElementsByTagName('img')[0];
img.style.opacity = 1.0;
window.setInterval(fadeAway, 1000);
const fadeAway = function() {
  img.style.opacity = img.style.opacity - .1;
};

// Note: opacity is 1E9+ only (plus all other browsers).
//
let img = document.getElementsByTagName('img')[0];
img.style.position = 'absolute';
img.style.top = '0px';
const watchKittyFall = function() {
  let oldTop = parseInt(img.style.top);
  let newTop = oldTop + 10;
  img.style.top = newTop + 'px';
};

window.setInterval(watchKittyFall, 1000);
// Note: you must specify (and strip) units.
```

#### Stopping an animation

To stop at an animation at a certain point, store the timer into a variable and clear with one of these methods:

```js
window.clearTimeout(timer);
window.clearInterval(timer);
```

```js
let img = document.getElementsByTagName('img')[0];
img.style.opacity = 1.0;

const fadeAway = function() {
  img.style.opacity = img.style.opacity - .1;
  if (img.style.opacity < .5) {
    window.clearInterval(fadeTimer);
  }
};
let fadeTimer = window.setInterval(fadeAway, 100);
```

#### Emmet

Emmet is an Atom Package for speeding up the process of coding HTML and CSS using **abbreviations** and **action shortcuts**.  It's particularly helpful when writing HTML.  Check out:

- [Emmet Cheat Sheet](http://docs.emmet.io/cheat-sheet/)
- [Emmet Syntax](http://docs.emmet.io/abbreviations/syntax/)

#### Abbreviations

All of Emmet's abbreviations work by writing the abbreviation and pressing tab at the end of the abbreviation.

##### _Tag Name_

Whether it is a ` p `, a ` div `, or anything else. If you type the tag name, and then hit tab, it will create the element.

##### _Classes and IDs ( # or . )_

```
div.className
<!-- Makes: -->
< div class="className"></div>

div#tagName
<!-- Makes: -->
<div id="tagName"></div>

div.firstClassName.secondClassName
<!-- Makes: -->
<div class="firstClassName secondClassName"></div>

div.className#secondClassName
<!-- Makes: -->
<div class="className" id="secondClassName"></div>
```

##### _Children ( > )_

This is for nesting elements!

```
div>p
<!-- Makes: -->
<div>
    <p></p>
</div>

header>nav>p
<!-- Makes: -->
<header>
    <nav>
        <p></p>
    </nav>
</header>
```

##### _Sibling ( + )_

This is for creating elements next to each other.

```
header+div.container
<!-- Makes: -->
<header>
</header>
<div class="container">
</div>
```

##### _Multiplication ( * )_

This is for making multiple elements at once.

```html
div>ul>li*3
<!-- Makes: -->
<div>
    <ul>
        <li></li>
        <li></li>
        <li></li>
    </ul>
</div>
```

##### _Climb Up ( ^ )_

This is to climb out of a nesting.

```html
header>p^div
<!-- Makes: -->
<header>
    <p></p>
</header>
<div></div>
```

##### _Grouping ( () )_

This is to group chunks of elements so you don't need to worry about climbing.

```html
(header>h1)+(nav>a)
<!-- Makes: -->
<header>
    <h1></h1>
</header>
<nav><a href=""></a></nav>
```

##### _Attributes ( [] )_

This is to give custom attributes.

```html
img[src="" title="" alt=""]
<!-- Makes: -->
<img src="" alt="" title="">
```

##### _Text ( {} )_

This is to add text to things.

```html
a{This is a link to something}
<!-- Makes: -->
<a href="">This is a link to something</a>
```

These things can all be used together!

```html
div.container>(header>h1)+(ul>li*5)
<!--  Makes: -->
<div class="container">
    <header>
        <h1></h1>
    </header>
    <ul>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
    </ul>
</div>
```

#### Action Shortcuts

There are a bunch of these, but some particularly handy ones are:

- `cmd` + `up` - remove tag
- `ctrl` + `alt` + j - find matching tag
- `shift` + `cmd` + `m` - merge lines
- `cmd` + `shift` + `y` - evaluate maths expression
- `ctrl` + `d` - balance tags (ie, select the tag boundaries from the current caret position)

___

#### _Animation in JavaScript - Examples_<div></div>

- [Bill fader](http://jsbin.com/refocisune/edit?html,js,output)
- [Bill bigger](http://jsbin.com/tavefaquve/edit?html,js,output)
- [Bill faller](http://jsbin.com/yahifogoni/edit?html,js,output)
- [Bill fade (elegant/fancy/pretentious)](http://jsbin.com/sonoludazu/1/edit?js,output)

___

#### Homework

- [The Cat Walk](https://gist.github.com/textchimp/20df0be7bcd892797619bdad203af94e)
- [Inspo](https://gist.github.com/ga-wolf/ae7d0e1df214e45213c5)
