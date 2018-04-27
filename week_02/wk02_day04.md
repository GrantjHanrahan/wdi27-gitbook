
## Week 02, Day 04

What we covered today:
- JavaScript - Libraries
- JavaScript - jQuery - Introduction
- JavaScript - jQuery - Events
- JavaScript - jQuery - Animation and Effects
- JavaScript - jQuery - Events
- JavaScript - jQuery - Animation and Effects
- JavaScript - jQuery - Patterns and Anti-patterns
- JavaScript - jQuery - Plugins
___


#### Warmup
- [Warmup and solution - Scrabble - JS](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week02/day05_scrabble)
___

#### Slides

- [jQuery - Introduction](https://slack-files.com/files-pri-safe/T0351JZQ0-FAEAGU8JH/intro-to-jquery.pdf?c=1524814388-d77a897fa1a9cde5306eee8337f18a8bba7a1992)

-[Advanced jQuery](https://slack-files.com/files-pri-safe/T0351JZQ0-FAEDAV8G5/advanced-jquery.pdf?c=1524814458-eaeec763655b5f02be62818571b36ac3f87cf45a)

___

#### Exercises
1. [Making a Video Player](https://gist.github.com/textchimp/4871fa2c333cd19151db3ab8e0083513)
2. [DOM Detective](https://gist.github.com/textchimp/00baa95feb73c773ac62c0547c3b3702)

___

#### Links
[Programmer Ryan Gosling](http://programmerryangosling.tumblr.com/)

___

### JavaScript - Libraries

#### What is a library?

A collection of reusable methods designed for a particular purpose.  You just reference a javascript file with a particular library in it - and away you go!

jQuery is the most common JavaScript library on the web today. As of 2016, it was used in:

- Over 43 million websites, including:
  + Over 12% of all websites;
  + Over 78% of the top million websites;

___

### JavaScript - jQuery - Introduction


##### _What is jQuery_

An open source JavaScript library that simplifies the interaction between HTML and JavaScript.  A JavaScript library is collection of reusable methods for a particular purpose.

It was created by John Resig in 2005, and released in January of 2006.

Built in an attempt to simplify the existing DOM APIs and abstract away cross-browser issues.

##### _Why jQuery?_

- Well documented
- Lots of plugins
- Small-ish size (23kb)
- Everything works in IE 6+, Firefox 2+, Safari 3+, Chrome, and Opera 9+ (if you are using 1.11.3 or previous, greater than that scraps up until IE8 support)
- Millions and millions of sites using it.

##### _What does it do for us?_

- Data Manipulation
- DOM Manipulation
- Events
- AJAX
- Effects and Animation
- HTML Templating
- Widgets / Theming
- Graphics / Chart
- App Architecture

##### _Why use it?_

```js
// No library:
const elems = document.getElementsByTagName("img");
for (let i = 0; i< elems.length; i++) {
  elems[i].style.display = "none";
}

// jQuery:
$('img').hide();
```

> "The code that has the least amount of bugs is the code that doesn't exist." - _Joel Turnbull_

#### The basics

**Select -> Manipulate -> Admire**

```js
const $paragraphs = $("p")
$paragraphs.addClass('special');

// OR
$("p").addClass("special");
```

##### _Selecting elements_

All CSS selectors are valid.

| HTML                           | CSS Selector               | jQuery                |
|--------------------------------|----------------------------|-----------------------|
|`<p>`                           |`p`                         |`$('p')`               |
|`<p class="intro"`              |`.intro`                    |`$('.intro')`          |
| _or_                           |`p.intro`                   |`$('p.intro')`         |
|`<p id="badger"`                |`#badger`                   |`$('#badger')`         |
|`<div> <p></p> </div>`          |`div p`                     |`$('div p')`           |
|`<ul> <li></li> <li></li> </ul>`|`ul li:last-child`          |`$('ul li:last-child')`|      


jQuery also offers a bunch of other selectors. Some common ones are:

| Selector    | Example               | Selects                                                             |
|-------------|-----------------------|---------------------------------------------------------------------|
| `:first`    | `$("p:first")`        | The first paragraph element.                                        |
| `:last`     | `$("div > p:last")`   | The last paragraph element that is a direct child of a div element  |
| `:has()`    | `$("div:has(img)")`   | Any divs that have one or more descendent img elements              |
| `:visible`  | `$(".kitten:visible")`| Any elements with the class "kitten" that have height || width > 0  |
| `:hidden`   | `$(".kitten:hidden")` | The opposite of :visible. Includes `display:none`.                  |

See [jQuery API - Selectors](http://api.jquery.com/category/selectors/).

##### _Reading elements_

If we had this element in the HTML...

`<a id="yahoo" href="http://www.yahoo.com" style="font-size:20px">Yahoo!</a>`

We can select it using `$("a#yahoo")`

We can store it using `let $myLink = $("#yahoo");`

We can get the content within it using `$("#yahoo").html()`

We can get the text within it using `$("#yahoo").text()`

We can get the HREF attribute using `$("#yahoo").attr("href")`

We can get the CSS attribute using `$("#yahoo").css('font-size')`

##### _Changing elements_

`$("#yahoo").attr("href", "http://generalassemb.ly")`

`$("#yahoo").css("font-size", "25px")`

`$("#yahoo").text("General Assembly")`

`$("#yahoo").attr("href", "http://generalassemb.ly").css("font-size", "25px").text("General Assembly")`

##### _Create, manipulate and inject_

```js
// Step 1: Create element and store a reference
    const $para = $('<p></p>'); // You can create any element with this!

// Step 2: Use a method to manipulate (optional)
    $para.addClass('special'); // So many functions you could use

// Step 3: Inject into your HTML
    $('body').append($para); // Also could use prepend, prependTo or appendTo as well
```

#### HTML elements v DOM nodes v jQuery objects

A DOM node is not actually an HTML element - it is a representation of an HTML element, which we can use to create, modify, and read properties of HTML elements.

Similarly, it's important to note that a jQuery object is not actually a DOM node - it is a wrapper around a DOM element/collection of DOM elements that allows us to use the jQuery library's methods.

Consider this HTML:

```html
<div>
  <p> This is a paragraph of text.</p>
</div>
```

We can't access and interact with an HTML element directly from JavaScript.
```js
<p>
// => Uncaught SyntaxError: Unexpected token <
```

We need to use the DOM API to select the DOM node that represents that HTML element.
```js
document.querySelector("p");
// => <p> This is a paragraph of text.</p>
```

This is a DOM node, not an HTML element.
```js
typeof(document.querySelector("p"));
// => "object"
```

If we create a jQuery object but want to access the DOM elements within that object, we need to pull out the native DOM element from the jQuery object.

```js
let paragraph = document.querySelector("p");  
// This is a DOM node.
let $paragraph = $("p:first");                
// This is a jQuery element.

$paragraph === paragraph
// => false
// The first object is a jQuery object. The second object is a DOM node.

$paragraph;
// => [ <p> This is a paragraph of text </p> ]
// Note the square brackets - this is a jQuery object, not a DOM node.

let p = paragraph[0]
// We have taken the jQuery object and stored the first DOM node in that element to a variable called p

p === paragraph
// => true

```

While we can mix regular JavaScript and jQuery together in our code, we cannot call jQuery methods on regular DOM objects (and vice-versa).

```js

// CREATING A JQUERY OBJECT

let $paragraphs = $('p');
// This creates an array-like jQuery object comprised of all the DOM elements in the Document Object Model that represent paragraph tags.

// GETTING A DOM NODE FROM A JQUERY OBJECT - METHOD 1

let firstParagraph = $paragraphs[0];
// This is a DOM node. We have selected the first DOM node in the $paragraphs jQuery object

// GETTING A DOM NODE FROM A JQUERY OBJECT - METHOD 2

let firstParagraph = $paragraphs.get(0)
// This is a DOM node. We have selected the first DOM node in the $paragraphs jQuery object using jQuery's .get() method.

let allParagraphs = $paragraphs.get()
// This is an array DOM nodes. We have created an array of all the native DOM nodes in the $paragraphs jQuery object using jQuery's .get() method without passing any arguments.

// CREATING A JQUERY OBJECT FROM ANOTHER JQUERY OBJECT

let $myParagraph = $( paragraphs[0] );
// This is a jQuery object.

let $myParagraph = $paragraphs.el(0);
// This is a jQuery object - this is the preferred method.

// We can also loop through our array...
for( let i = 0; i < paragraphs.length; i++ ) {
   let element = paragraphs[i];
   let paragraph = $(element);
   paragraph.html(paragraph.html() + ' wowee!!!!');
};

// Or use jQuery to do it - this is preferred
$paragraphs.each(function () {
  let $this = $( this );
  $this.html( $this.html() + " wowee!!!" );
});
```

___


### JavaScript - jQuery - Events

#### Listeners and handlers

We can use the .on() method to add an event listener to the selected jQuery objects.

The first parameter accepted by the .on method is the event to listen for - the 'listener'.

The last parameter accepted by the .on() method is the 'handler' - the function to execute  when the event is triggered. The handler can be either an anonymous function (ie, the handler is the function itself) or can be a call to another function.

```js
const onButtonClick = function() {
  console.log('clicked!');
};

$('button').on('click', onButtonClick); // Attaching an event handler with a defined function to the button

$('button').on('click', function () { // Attaching an event handler with an anonymous function to the button
  console.log('clicked!');
});

$('button').click(onButtonClick);
```

##### _Other event types_

- Keyboard Events - 'keydown', 'keypress', 'keyup'
- Mouse Events - 'click', 'mousedown', 'mouseup', 'mousemove'
- Form Events - 'change', 'focus', 'blur'

Arguments get passed into callbacks by default.

```js
const myCallback = function (event) {
    console.log( event );
    // The event parameter is a big object filled with ridiculous amounts of details about when the event occurred etc.
};

$('p').on('mouseenter', myCallback);
```

##### _Preventing default events_

```js
$('a').on('click', function (event) {
    event.preventDefault();
    console.log('Not going there!');
});

$('form').on('submit', function (event) {
    event.preventDefault();
    console.log('Not submitting, time to validate!');
});
```

### JavaScript - jQuery - Animation and Effects

jQuery give us access to a whole bunch of really useful methods for creating and controlling animations and effects. These include simple methods for frequently used effects like .fadeIn and .fadeOut, as well as more complex and customizable methods like .animate. For a full list, check out [jQuery.com - Effects](http://api.jquery.com/category/effects/).

A few common ones are:

- toggle
- fadeIn
- fadeOut
- show
- hide
- animate

```js
// TOGGLE - changes the selected element's display property to show or hide (display: none) the element depending on the value of the element's display property when the method is called. This effectively calls the .show or .hide methods, depending on whether the element is display: none or display: block/inline-block/inline/table when the method is called.
$('#error').toggle();

// The .toggle method takes two optional arguments - the duration (of the transition to/from display:none) and a function to call once the animation is complete. This can be a named function or an anonymous function (the example below calls an anonymous function):

$("#error").toggle(1000, function() {
  alert("ERROR");
});

// FADEIN - displays an element by changing its opacity to 1 (ie, opaque). If no duration is passed in as an argument, the default duration of 400ms will apply
$('#error').fadeIn();


// As with most of jQuery's effects, the .fadeIn method takes two optional arguments - the duration (of the transition to opacity: 1) and a function to call once the animation is complete. This can be a named function or an anonymous function (the example below calls a named function):

$("#error").fadeIn(1000, errorFunction);

//  SHOW - Show the element by changing its display property from display:none.
$('#error').show(1000, function(){
    $(this).addClass('redText')
});

//  HIDE - Hide the element by changing its display property to display: none.
$('#error').hide(1000, function(){
    $(this).addClass('removed')
});

// ANIMATE - Allows us to animate a custom set of CSS properties. Remember that a dash-cased CSS property name (eg font-size) must be camelCased for JavaScript's benefit (eg fontSize). We can only change properties that can be reduced to numeric values, so the core jQuery library doesn't allow animation of, for example, background-color (some plugins and extensions, like jQuery UI, can achieve this).
$("#cat").animate({
  width: "500px",
  fontSize: "24px",
  letterSpacing: "2px",
  marginLeft: "20px"
}, 200)

```
___

### JavaScript - jQuery - Patterns and anti-patterns

##### _The `ready` event_

In order to do cool jQuery stuff, we need to make sure that all of the content in the HTML document has been parsed by the browser before our script is run, so put all your DOM-related jQuery code in a function that is called when the `document` is `ready`.

```js
$(document).ready(function(){

});
```
The below function works in a similar way the the `ready` function but will wait until ALL elements (eg. images) are loaded before it will run the code block.

```js

$(document).load(function(){

});
```

##### _Variable names_
Pattern: Prefix variable names that represent jQuery objects with `$`. That will remind us that this object has all of jQuery's fancy methods available to it.

Remember: in variable names, $ is just another character we can use. It's not magic!

```js
const $myVar = $('#myNode');
```

##### _Event delegation_

When elements are being created after the document is loaded, bind the event to an element that will _definitely_ exist on document.ready (a failsafe element would be `$(document)`).

```js
$("body").on('click', 'a', myCallback);
```

##### _Named functions for event handlers_
Event handlers can either call anonymous functions or named functions. Anonymous functions are useful, but named functions are re-usable.  

```js
// Anonymous function:
$p.on("click", function() {
$(this).css("backgroundColor", "gainsboro");
});

// Named function:  
const myCallback = function() {
  $(this).css("backgroundColor", "gainsboro");
};

$("p").on('click', myCallback);

```

##### _Chaining methods_

jQuery lets us chain methods together, allowing for pretty succinct code.

```js
banner.css('color', 'red');
banner.html('Welcome!');
banner.show();

// Is the same as:
banner.css('color', 'red').html('Welcome!').show();

// Is the same as:
banner.css('color', 'red')
      .html('Welcome!')
      .show();
```
___

### JavaScript - jQuery - Plugins

#### Finding plugins

Go through
- [jQuery plugin](http://plugins.jquery.com/)
- [here](https://www.javascripting.com/)

#### Selecting a good plugin

Look for plugins that:
- Have good documentation
- Are flexible
- Have some community around them
    + Check the number of forks and issues on GitHub.
- Are actively maintained
    + When was the last commit in the GitHub repo?
- Have a small  file size
- Have good browser compatibility
- Have responsive design

For more details, go [here.](http://blog.pamelafox.org/2013/07/which-javascript-library-should-i-pick.html)

#### Using a plugin

- Download the plugin and associated files from the site or Github repository
- Copy the files into your project's folder
- In the HTML, reference any associated CSS
- In your HTML, after you've referenced jQuery, and before you reference your own code, add the script tag(s) that reference the plugins JS file(s).
___

### Homework

- [GA Bank](https://gist.github.com/textchimp/c71a71a95ab2ff17539f48decf42c367)
- [Try jQuery](http://try.jquery.com/)
- [Learn jQuery](https://learn.jquery.com/)
- [jQuery basics](https://learn.jquery.com/)
- [jQuery API](https://api.jquery.com/)
