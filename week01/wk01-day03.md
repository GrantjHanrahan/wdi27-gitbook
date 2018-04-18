## Week 01, Day 03

What we covered today.
  - [Our very first warmup and solution - Raindrops](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week01/day03_raindrops)
  - Git related homework stuffs
  - Javascript
    - Loops
    - Arrays
___

#### Slides

- [Loops](http://www.teaching-materials.org/javascript/slides/controlflow.html) (slide 11-14)
- [Arrays](http://www.teaching-materials.org/javascript/slides/controlflow.html) (slide 15 onwards)

___
### Our very first warmup!

![gras](https://media.giphy.com/media/LgwoVr7YgUkrC/giphy.gif)

  - [Warmup and solution - Raindrops - JS](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week01/day03_raindrops)
___
### Git and Github

#### Introduction

A Version Control System designed and developed by a Finnish guy, Linus Torvalds, for the Linux kernel in 2005.  Apparently, he named it Git because of the British-English slang meaning "unpleasant person".  Torvalds said: "I'm an egotistical bastard, and I name all my projects after myself".

##### _What does it do?_

- A way to snapshot - you get a time machine
- A way to collaborate - working in parallel
- Saves us from moving code around on Floppy Disks
- Automates merging of code
- Lets you distribute a project

N.B. It is quite hard to understand, and you won't understand most of the problems that it solves yet! Just trust us.

**NOTE:** `GitHub !== Git`. Git is on your local computer. GitHub is one of several web-based Git repository hosting services.

Some basic Git commands:

- `git init` - This initializes the git repository (behind the scenes it creates a .git file in the folder - it has called .git because any file prefixed by a . is hidden in finder and ls).  Make sure you don't do this in another git repository!
- `git status` - This will tell you what is happening the current project. Commit status, untracked files etc.
- `git add .` - This will add all files in the current directory into the "staging area".  Git is now paying attention to all files. You can alternatively specify a particular file - `git add octocat.txt`.  We could also add all files of a particular type - `git add '*.txt'` (this needs quotes!) or multiple files at a time - `git add octocat.txt blue_octocat.txt`.
- `git commit -m "You're commit message` - This is the thing that takes the snapshot. You must give it a message!
- `git log` - Will show you all of the snapshots in the current project
- This has all been happening locally! So we need to connect it to Github...
- `git remote add origin https://github.com/try-git/try_git.git` - It takes a *remote name*, the word origin, and a *remote repository*, the URL.
- `git push -u origin master` - The first time you run git push (which moves it up to github), make sure you specify `-u origin master`.  This tells your local machine to always use the remote link you specified in the step before. Once you have done this - you can run `git push` from now on
- `git pull` - this brings the code down from Github
- `git diff HEAD` - this will show the differences between the current commit and the previous commit
- `git diff --staged` - You can show the differences between the currently staged files by using this command
- `git reset FILENAME` - Will remove the specified files or folders from the staging area (so they won't be pushed!)
- `git checkout -- octocat.txt` - This will go back to the last commit involving the octocat.txt file using this.
- `git branch clean_up` - This allows us to work in an alternate reality - changes you make here won't effect anything in other branches
- `git checkout clean_up` - Will move to the clean_up branch.
- `git rm FILENAME FILENAME` - Will not only delete the file on your computer, but also remove it from the staging area with Git.
- `git add .` then run `git commit -m "COMMIT NAME"`.  This will take a screenshot of the current branch position.
- `git checkout master` - This will move to the master branch.
- `git merge clean_up` - Will merge the clean_up branch into the current branch.
- Now that everything has been merged, we can delete the clean_up branch - `git branch -d clean_up`.
- We can run `git push` now! Everything is up there.

**A really basic process:** (Follow these steps every time!)

- `git add .`
- `git commit -m "Commit message`
- `git pull`
- `git push`

#### Homework repository

For notes on submitting your homework, refer to the README file in my repository

- [WDI-27 Homework Repository](https://github.com/GrantjHanrahan/wdi27-homework)

___

#### _Git and GitHub - Introduction - Tutorials_

- [CodeSchool - TryGit](https://try.github.io/levels/1/challenges/1) - a great interactive, introductory GitHub tutorial.
- [A tutorial by the folks at Atlassian](https://www.atlassian.com/git/tutorials/setting-up-a-repository/)
- [A tutorial by Roger Dudler](https://rogerdudler.github.io/git-guide/)
- [A tutorial by Git Tower](https://www.git-tower.com/learn/)

___

### Javascript - Control Flow: Loops

#### The 'While' Loop

The **while loop** will tell Javascript to repeat the statements within the curly brackets until a condition is true.  **WARNING:** it is _ridiculously_ easy to make infinite loops with these - they'll crash your browsers and crush your spirits.

You need a condition in the parentheses, and you need something within the body (between the curly brackets) that will eventually change the condition to be false.

```js
let x = 0; //init

while (x < 5) { //conditional
  console.log(x);
  x += 1; // same as: x = x + 1; //update
}
// => 0
// => 1
// => 2
// => 3
// => 4
```

What would have happened if we didn't increment `x` each iteration? Or if we _decremented_ `x` each iteration? The condition (`x < 5`) would never be false - the condition would be true until the heat death of the universe.  That is, you'd be stuck in an infinite loop.

#### The 'For' Loop

A **for loop** is another, more specialized way of repeating statements.

It looks like the following:

```js
// 'For loop' syntax:
for (/*initialize*/; /*condition*/; /*update*/) {
  /* code to execute*/
}

// Example of a 'for loop':
for (let i = 0; i < 5; i = i + 1) {
    console.log( i );
}
// => 0 1 2 3 4

// The equivalent 'while loop' would be:
let i = 0; // This is the 'initialize' value
while (i < 5) { // This is the 'condition' (in the parentheses)
    console.log(i);
    i = i + 1; // This is the 'update'
}
// => 0 1 2 3 4
```

#### The 'Break' Statement

To prematurely exit any loop (`for` or `while`), use can use the **break** statement:

```js
for (let current = 100; current < 200; current++) {
    // 'current++'' is the same 'current += 1' and 'current = current + 1'
    // 'current--'' also exists ('current = current - 1')
    // This is called 'syntactic sugar'

    console.log('Testing ' + current);
    if (current % 7 === 0) {
        // The % stands for the modulus operator, it finds the remainder
        console.log('Found it! ' + current);
        break;
    }
}
```

___

#### _Control Flow in Javascript: Loops - Exercises_

- [Loops Exercises](https://gist.github.com/textchimp/db943c21196cc295305922a5e7b0a34f)
  - [Solution](https://github.com/wofockham/wdi-15/tree/master/01-javascript/variables/)

___

#### Recursion (Basics)

We will get into this a lot more, but basically - we can call functions within functions. This allows us to recreate loops using **if statements**!

See [the Countdown Example](http://repl.it/l50).

___

### Javascript - Collections

In addition to the primitive data types (strings, numbers, booleans, undefined, null), we have **arrays**, **objects** and **regular expressions**. We're going to leave regular expressions for the time being and take a look at the **collections**: arrays and objects. Collections are types of data that allow us to store and access collections of values.

___

### Javascript - Collections - Arrays

An array is a type of data that holds an ordered list of values. The values can be of any data type, and an array can include multiple data types. Arrays are sequences of elements that can be accessed via integer indices starting at zero.  More or less, an array is a special variable that can hold more than one value at a time.

**NOTE:** Don't forget - arrays are zero-indexed!

#### How to create an array

- `let testArray = [ 1, 2, 3 ];` - This is an array literal - definitely the way you should do it.
- `let testArray = new Array( 1, 2, 3 );` - Uses the Array constructor and the keyword new. Does the same thing, but stick to the other way.

#### How to access elements in an array

```js
let amazingFrenchAuthors = [ "Alexandre Dumas", "Gustave Flaubert", "Voltaire", "Marcel Proust", "Jean-Paul Sartre", "Stendhal", "Anäis Nin", "Simone de Beauvoir", "Rene Descartes", "Montesquieu" ];

console.log( amazingFrenchAuthors[0] ); // => "Alexandre Dumas"
console.log( amazingFrenchAuthors[3] ); // => "Marcel Proust"

// You can use variables and expressions to access elements in arrays as well!
let theBestOfTheBest = 4;
console.log( amazingFrenchAuthors[ theBestOfTheBest ] ); // Logs "Jean-Paul Sartre"
console.log( amazingFrenchAuthors[ amazingFrenchAuthors.length - 1 ] ); // Logs "Montesquieu" (the last element)

// This will turn a string into an array, with each element defined by the space
let bros = "Groucho Harpo Chico Zeppo".split(" ");
console.log(bros); // => ["Groucho", "Harpo", "Chico", "Zeppo"]
```

#### How to iterate through elements in an array

Stick to the **for loop** in most cases, but there are always thousands ways of doing something in Javascript.

```js
let greatPeople = [ "Louis Pasteur", "Jacques Cousteau", "Imhotep", "Sigmund Freud", "Wolfgang Amadeus Mozart" ];

for ( let i = 0; i < greatPeople.length; i++ ) {
    console.log( greatPeople[ i ] ); // Logs out the "i-th" element each iteration
}


[ 'a', 'b', 'c' ].forEach( function (elem, index) {
    console.log(index + '. ' + elem);
});
// => 0. a
// => 1. b
// => 2. c

```
___

#### _Javascript Collections - Arrays - Exercise_

- [Your Top Choices](https://gist.github.com/ga-wolf/4f200a907382aa53dc9e794d1ee18b3c)

___

#### How to set elements in an array

```js
let amazingFrenchAuthors = [ "Alexandre Dumas", "Gustave Flaubert", "Voltaire", "Marcel Proust"];

amazingFrenchAuthors[0] = "Stendhal"; // Just access them and reassign!

console.log( amazingFrenchAuthors );
// Logs [ "Stendhal", "Gustave Flaubert", "Voltaire", "Marcel Proust"]
```

#### Common methods and properties for arrays

```js
let amazingFrenchAuthors = [ "Alexandre Dumas", "Gustave Flaubert", "Voltaire", "Marcel Proust"];

// In the examples below, assume we're starting from the original array each time.

console.log( amazingFrenchAuthors.length ); // Returns 4 - the length property doesn't use a zero index

// POP //
amazingFrenchAuthors.pop(); // Removes the last element from an array and returns that element.
// => "Marcel Proust"

// PUSH //
amazingFrenchAuthors.push("Jean Paul Sartre"); // Adds one or more elements to the end of an array and returns the new length of the array.
// => 5

// REVERSE //
amazingFrenchAuthors.reverse(); // Reverses the order of the elements of an array — the first becomes the last, and the last becomes the first.
// END REVERSE //

// SHIFT //
amazingFrenchAuthors.shift(); // Removes the first element from an array and returns that element.
// END SHIFT //

// UNSHIFT //
amazingFrenchAuthors.unshift("Gustave Flaubert"); // Adds one or more elements to the front of an array and returns the new length of the array.
// END UNSHIFT //

// JOIN //
amazingFrenchAuthors.join(); // Joins all elements of an array into a string.
// END JOIN //

// SPLICE //
amazingFrenchAuthors.splice(); // Adds and/or removes elements from an array.
amazingFrenchAuthors = ["Alexandre Dumas", "Gustave Flaubert", "Voltaire", "Marcel Proust"];
amazingFrenchAuthors.splice(1,1);
// => ["Gustave Flaubert"]
// amazingFrenchAuthors is now ["Alexandre Dumas", "Voltaire", "Marcel Proust"]

amazingFrenchAuthors.splice(1,1, "Gustave Flaubert");
// Returns the deleted items, and adds in the next parameters ["Voltaire"]
// amazingFrenchAuthors is now ["Alexandre Dumas", "Gustave Flaubert", "Marcel Proust"]
// END SPLICE //

amazingFrenchAuthors = ["Alexandre Dumas", "Gustave Flaubert", "Marcel Proust"];

// INCLUDE //
amazingFrenchAuthors.includes( "Alexandre Dumas" ); // Returns true.
amazingFrenchAuthors.includes( "Montesquieu" ); // Returns false.
// END INCLUDE //

// INDEX OF //
amazingFrenchAuthors.indexOf("Alexandre Dumas"); // Returns 0 (ie, the first element in the array)
amazingFrenchAuthors.indexOf("Anäis Nin"); // indexOf returns -1 if it cannot find a match.
// END INDEX OF //
```
___

#### _Javascript - Collections - Arrays - Recommended Readings_

- [SitePoint - Manipulate Arrays in Javascript](http://www.sitepoint.com/quick-tip-create-manipulate-arrays-in-javascript/)


### Homework

- [Word Guesser](https://gist.github.com/textchimp/3a8f1e9445f79944f46e9d329b117597)
- [Arrays and Functions: Additional Exercises](https://gist.github.com/textchimp/3a8f1e9445f79944f46e9d329b117597)
- Bonus - Read:
    + [MDN - Arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) - MDN is an amazing resource for Javascript, HTML and CSS.
    + [Javascript Tutorials - Arrays](http://javascript.info/tutorial/array)
    + [Speaking Javascript - Arrays](http://speakingjs.com/es5/ch01.html#basic_arrays)
- [Cranky old man yelling about Javascript](https://www.youtube.com/watch?v=hQVTIJBZook)
- [Variables and Assignment](http://speakingjs.com/es5/ch01.html#_variables_and_assignment)
- [Eloquent Javascript - Values, Types, and Operators](http://eloquentjavascript.net/01_values.html)
-----

### Miscellaneous Links

- [Idiomatic Javascript style rules](https://github.com/rwaldron/idiomatic.js/)
- [How to add colours and styles to your console.logging](https://developers.google.com/web/tools/chrome-devtools/debug/console/console-write#string-substitution-and-formatting)
