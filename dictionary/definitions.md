### Dictionary

____

#### _API (Application Program Interface)_

  - A set of functions, procedures, methods or classes used by computer programs to request services from the operating system, software libraries or any other service providers running on the computer. A computer programmer uses the API to make application programs. Types of API include web services API like the Twitter API, which allows programs to use the API to receive updates on tweets.

____

#### _Argument_

  - The actual value passed in to a function or method. See also: Params.

____

#### _Async / Asynchronous_

    - Means that a script will send a request to the server, and continue it's execution without waiting for the reply. As soon as reply is received a browser event is fired, which in turn allows the script to execute associated actions. [Further analogies.](https://stackoverflow.com/questions/4559032/easy-to-understand-definition-of-asynchronous-event)

____

#### _Callbacks (Higher order functions)_

  - A callback function, also known as a higher-order function, is a function that is passed to another function (let's call this other function “otherFunction”) as a parameter, and the callback function is called (or executed) inside the otherFunction. [More.](http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/)

____

#### _CLI (command line interface)_

      - A user interface to a computer's operating system or an application in which the user responds to a visual prompt by typing in a command on a specified line, receives a response back from the system. Examples: Terminal, iTerm2, MS-DOS.

____

#### _Closure_

  - A really confusing concept, A closure is an inner function that has access to the outer (enclosing) function's variables—scope chain. The closure has three scope chains: it has access to its own scope (variables defined between its curly brackets), it has access to the outer function's variables, and it has access to the global variables. More info.

____

#### _Cross-origin/CORS/Xorigin_

    - Cross-origin resource sharing (CORS) is a mechanism that allows resources (e.g. fonts) on a web page to be requested from another domain outside the site on which that resource exists. If an AJAX request fails, there is a significant chance that CORS has been restricted for that site (to not allow outside data requests).

____

#### _DNS_

   - DNS (Domain Name Service) is effectively the phonebook of the internet. It allows humans to search for sensible human domain name, instead of things like IP addresses. When you visit a site, the DNS finds the IP address assosciated with that URL and connects your computer.

____

#### _Hoisting_

  - Hoisting is Javascript's default behaviour, moving all declared variables to the top of the current scope before they've been assigned values. [Examples here.](https://www.w3schools.com/js/js_hoisting.asp)

____

#### _Immutable_

    - In object-oriented and functional programming, an immutable object (unchangeable object) is an object whose state cannot be modified after it is created.

____

#### _IP address_

  - An Internet Protocol Address is a unique address assigned to any decive on the web. It is the means by which machines locate one another. In much the same way we would use a web address (like www.example.com), machines use IP addresses.

____

#### _Operators_

  - **Comparison operators** Comparison operators are used in logical statements to determine equality or difference between variables or values.

  **Examples;**

  |Operator|Meaning                  |True Statements      |
  |:-------|:------------------------|:--------------------|
  |==      |Equality                 |28 == '28', 28 == 28 |
  |===     |Strict Equality          |28 === 28            |
  |!=      |Inequality               |28 != 27             |
  |!==     |Strict Inequality        |28 !== 23            |
  |>       |Greater than             |28 > 23              |
  |>=      |Greater than or Equal to |28 >= 28             |
  |<       |Less than                |24 < 28              |
  |<=      |Less than or Equal to    |24 <= 24             |

  - **Logical operators** Logical operators are typically used with Boolean (logical) values. When they are, they return a Boolean value.

  |Operator|Meaning|True Expressions|
  |:-------|:------|:---------------|
  |&&      |AND    |4 > 0 && 4 < 10 |
  |&#124;&#124; |OR     |4 > 0 &#124;&#124;  4 < 3  |
  |!       |NOT    |!(4 < 0)        |

____

#### _Object-oriented programming (OOP)_

   - Refers to a type of computer programming (software design) in which programmers define not only the data type of a data structure, but also the types of operations (functions) that can be applied to the data structure.

#### _Params / Parameters_

  - The variable in a method/function definition `(var myFunction = function( iAmAParameter ){};)`. See also: Arguments.

____

#### _Primitive data types_

  - A primitive (primitive value, primitive data type) is data that is not an object and has no methods. Most of the time, a primitive value is represented directly at the lowest level of the language implementation. Javascript for instance, has 6 types:
    - Strings - things in quotes. `"I'm a string."`
    - Numbers - Raw number values. `1 2.3` (not "1").
    - Booleans - `True False`
    - Null - A value that has been explicitly told it has no value.
    - Undefined - A value that has not been defined.
    - Symbols (new in ECMAScript 2015)

____

#### _Promises_

  - Code that executes depending on the response of the code it's attached to. For instance `.done()` is a promise that will run once a task has been completed. [Further reading.](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261)

____

#### _Scope_

  - JS
    - JS
    - Global: A variable available to any code following the definition of that variable.
    - Local: A variable that is only available within that object.
    - Block: A variable only available between curly brackets.

  - Ruby
    - Class variable (@@a_variable): Available from the class definition and any sub-classes. Not available from anywhere outside.
    - Instance variable (@a_variable): Available only within a specific object, across all methods in a class instance. Not available directly from class definitions.
    - Global variable ($a_variable): Available everywhere within your Ruby script.
    - Local variable (a_variable): As above, available only within your method/loop etc.
  [More info.](https://www.natashatherobot.com/ruby-variable-scope/)

____

#### _Server_

  - A server is a computer program that provides services to other computer programs (and their users) in the same or other computers. The computer that a server program runs in is also frequently referred to as a server. That machine may be a dedicated server or used for other purposes as well.

____

#### _Strongly & Weakly typed languages_

  - The difference between a strongly typed language and a weakly typed language is basically that a strongly typed language checks the type of a variable before performing an operation on it, whereas a weakly typed language does not. In addition, strongly typed languages require explicit casts, while weakly typed languages perform implicit casts.

____

#### _undefined_

    - A value that has no definition. You'll most often see this if you are trying to call or return a variable thai is either out of scope, or spelled incorrectly.

____

#### _XHR/XMLHttpRequest_

  - Abbreviated as XHR, XMLHttpRequest is a set of APIs that can be used by Web browser scripting languages, such as JavaScript to transfer XML and other text data to and from a Web server using HTTP.
