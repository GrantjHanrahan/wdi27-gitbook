## Week 07, Day 05

What we covered today:

- ReactJS
  - Components
  - Props
  - Events

___

#### Warmup
- [Arrays - Flatten and Reverse](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week07/day05_arrays)

___

### React

#### Getting started with React

- [create-react-app](https://github.com/facebookincubator/create-react-app)

Creating web applications is hard work and like all good programmers we're going to be lazy. `create-react-app` does exactly that! It creates a React application without needing to install or configure tools like `Webpack` or `Babel`. They are hidden away, all set up with preconfigurations so we can focus on the code.

The first thing we need to do is install it globally.

`npm install -g create-react-app`

Once installed you can run the below code with the argument of your applications name. This will create the foundations of the application and store it in a folder with the same name as your application.

`create-react-app your_app_name_here`

`cd` into your applications directory and run `cat package.json` to show all the dependencies it has created for you.

```bash
{
  "name": "intro",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "react": "^16.2.0",
    "react-dom": "^16.2.0",
    "react-scripts": "1.0.17"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}
```

#### ReactJS - Components

Components are the building blocks of React. In a way they're pretty similar to Rails partials.

Components are basically functions that return HTML necessary to render a piece of the page. [Tyler McGiniss](https://twitter.com/tylermcginnis33) thinks of them as "[Kolaches](https://en.wikipedia.org/wiki/Kolach)", because 'they have everything you need, wrapped in a delicious composable bundle."

Components:
- Can be written in either pure JavaScript or JSX
- Can only return a single element (but that element can have multiple children)
- Can either receive data from its parent component, or from the component itself
- Should pass the **FIRST** test, and be:
  - **Focused**
  - **Independent**
  - **Reusable**
  - **Small**
  - **Testable**

### Anatomy of a component

Here is an example of a simple component:

```js
// Import any modules required - at a minimum, we will need React, and (usually) ReactDOM.
import React from 'react';
import ReactDOM from 'react-dom'

// Create a new class that extends the React.Component
class HelloWorld extends React.Component {
  // The only thing your component absolutely needs is a render function that returns HTML.
  render() {
    return (
      <div>Hello World</div>
    );
  }
}

// The ReactDOM.render function takes two arguments - what we want to render, and where we want to render it
ReactDOM.render(
  <HelloWorld />,                 // Invoke the component we want to render.
  document.getElementById("app")  // Where we want to render the component.
);

```

#### Properties (`props`)

Component `props` are a great way to pass variables between components.

Let's make this a bit more dynamic by passing `props` from the `HelloWorld` component to a new `HelloUser` component, and using JSX to interpolate the variables (encapsulated in curly brackets, eg `{this.props.name}`).

In this context, `HelloWorld` is the **parent component**, which is **invoking** `HelloUser` - a **child component**.

```js
// We can also require 'Component' so we don't have to refer to it through `React.Component`. The cool kids are using PureComponent so we will require that instead. This will stop you from building bad habits later on.
import React, { PureComponent } from 'react';
import ReactDOM from 'react-dom';

class HelloUser extends PureComponent {
  render() {
    return (
      // The HelloWorld component that invokes HelloUser passed a name attribute, which can be accessed via this.props.name
      <p> Hello {this.props.name} </p>
    );
  }
}

class HelloWorld extends PureComponent {
  render() {
    return (
      <div>
        <h1>Hello World</h1>
        <hr/>
        // Invoke the component we want to render - HelloUser - and pass in a 'name' attribute that can be accessed in the HelloUser component via this.props.name
        <HelloUser name="Groucho" />
        <HelloUser name="Chico" />
        <HelloUser name="Gummo" />
      </div>
    );
  }
}

ReactDOM.render(
  <HelloWorld />,
  document.getElementById("app")
);
```

##### _`this.props.children`_

One useful component prop is `this.props.children` - this will give you the content of the component. If we have nested components, `this.props.children` will return an array of components nested within the parent component.

##### _`prop` types and validation_

To ensure our components are being used correctly, we can add validators to our component.

```js
class HelloUser extends PureComponent {
  render() {
    return (
      <p> Hello, {this.props.name} </p>
      <p> You are visitor number {this.props.visitor} to this site </p>
    );
  }
}

HelloUser.propTypes = {
  // Guarantee that our name prop is a string and is always present.
  name: React.PropTypes.string.isRequired,
  // Guarantee that our visitor prop is a number.
  visitor: React.PropTypes.number
};

```

##### _Default props_

We can also define default values for a component's props:

```js
class HelloUser extends PureComponent {
  render() {
    return (
      <p> Hello, {this.props.name} </p>
      <p> You are visitor number {this.props.visitor} to this site </p>
    );
  }
}
// Guarantee that our component's props will include defined values for name and number
HelloUser.defaultProps = {
  name: "User",
  number: 1
}
```

#### State

Props are fine for configuring components with initial values, but if we want to update a property of the object over time, or in response to something, we need to use the component's **state**.

To create a **stateful** component (as opposed to a **stateless** component), we need three things:

- A `constructor` method
- A `super` method
- A `this.state` property

##### _Constructor method_

The `constructor` method is called automatically whenever a component is created - it's similar to Ruby classes' `initialize` method.

##### _Super method_

The `super` method should sit inside the `constructor` method. This gives the component all the functionality of the `constructor` method of the class it inherited from - in this case, the `React.Component` class.

##### _this.state_

Be default, a component's `state` property is undefined. Using the constructor method, we can set `this.state` to be a value - for example, an object with a `text` key, the value of which might change in response to user input.

```js
class MyComponent extends PureComponent {
  constructor() {
    super();
    this.state = {
      text: ""
    }
  }
  //etc etc
}
```

#### Lifecycle

A stateful React component has three main parts to its lifecycle:

- **Mounting** - The component is being inserted into the DOM
- **Updating** - The component is being re-rendered in the virtual DOM to determine whether the DOM needs updating
- **Unmounting** - The component is being removed from the DOM

React gives us a number of **lifecycle methods** that allow us to hook into the various parts of the lifecycle.

##### _Mounting_

- `getInitialState()`: object is invoked before a component is mounted. Stateful components should implement this and return the initial state data.
- `componentWillMount()` is invoked immediately before mounting occurs. This is a good point to do things that might take a bit of time, such as AJAX requests.
- `componentDidMount()` is invoked immediately after mounting occurs - ie, the element is on the page. Initialization that requires DOM nodes should go here.

##### _Updating_
- `componentWillReceiveProps(object nextProps)` is invoked when a mounted component receives new props - the properties have been updated, and the component is about to receive them. This method should be used to compare `this.props` and `nextProps` to perform state transitions using this.setState().
- `shouldComponentUpdate(object nextProps, object nextState): boolean` is invoked when a component decides whether any changes warrant an update to the DOM. Implement this as an optimization to compare `this.props` with `nextProps` and `this.state` with `nextState` and `return false` if React should skip updating.
- `componentWillUpdate(object nextProps, object nextState)` is invoked immediately before updating occurs - React is about to start updating the page with new component markup. You cannot call `this.setState()` here.
- `componentDidUpdate(object prevProps, object prevState)` is invoked immediately after updating occurs - React has made changes to the DOM.

##### _Unmounting_

- `componentWillUnmount()` is invoked immediately before a component is unmounted and destroyed. This is where you should do your cleanup - turn off timers, stop animations, etc.

#### Exemplar component

Without filling out the methods themselves, this is what a stateful component with propType validation, prop defaults and lifecycle hooks might look like:

```js
class HelloUser extends PureComponent {

// CONSTRUCTOR METHOD
  constructor() {
    super();
    this.state = {
      text: ""
    }
  }

// LIFECYCLE HOOKS
  // - Mounting
  getInitialState() { }
  componentWillMount() { }
  componentDidMount() { }
  // - Updating
  componentWillReceiveProps(object nextProps) { }
  shouldComponentUpdate(object nextProps, object nextState): boolean { }
  componentWillUpdate(object nextProps, object nextState) { }
  componentDidUpdate(object prevProps, object prevState) { }
  // - Unmounting
  componentWillUnmount() { }

// RENDER METHOD
  render() {
    return (
      <p> Hello, {this.props.name} </p>
      <p> You are visitor number {this.props.visitor} to this site </p>
    );
  }
}

// PROPTYPE VALIDATION
HelloUser.propTypes = {
  name: React.PropTypes.string.isRequired,
  visitor: React.PropTypes.number
};

// PROP DEFAULTS
HelloUser.defaultProps = {
  name: "User",
  number: 1
}

// INVOCATION
ReactDOM.render(
  <HelloUser name="Badger" visitor=1138 />,
  document.getElementById("app")
);
```
___

#### _ReactJS - Components - Further Reading_

- [ReactJS - Documentation - Reusable Components](https://facebook.github.io/react/docs/reusable-components.html#prop-validation)
- [Ricosta Cruz - ReactJS Cheatsheet](http://ricostacruz.com/cheatsheets/react.html)

___

### Class Demos

- [React Intro](https://)

Navigate to you directory where you want to store your application and use Create React App to generate a new application for you.

`create-react-app intro`

Navigate into the new folder

`cd intro`

Start the server `npm run start` or just `npm start` this will automatically open your browser and run localhost:3000

Open the application in ATOM

`atom .`

Create a new file in `src` folder called `Helloworld.js` and add the below code.

```js
import React from 'react';

// extend is like inheritance
class HelloWorld extends React.Component {
  // must always have the render function
  // using ES6 same as const render = function(){}
  render(){
    // using JSX so we don't need this to be in a string.
    return (<h1>Hello World!</h1>);
  }
}
```
___

Now we need to display this new component
Go to `App.js` and change the render to look like this

```js
class App extends Component {
  render() {
    return (
      <div className="App">
        <HelloWorld />
      </div>
    );
  }
}
```
___

We still haven't imported the `<HelloWorld />` component so we can't see it yet.
Add the import to the top of the `App.js` page.

```js
import HelloWorld from './HelloWorld';
```
___

We wont be able to see it yet because we haven't exported from the bottom of the `Helloworld.js` file.

```js
import React from 'react';

class HelloWorld extends React.Component {
  render(){
    return (<h1>Hello World!!!</h1>);
  }
}
// Export HelloWorld so it can be seen
export default HelloWorld;
```
___

We can refactor our code to include `PureComponent`. This will stop you from having bad habits.

```js
import React, { PureComponent } from 'react';

class HelloWorld extends PureComponent {
  render(){
    return (<h1>Hello World!!!</h1>);
  };
}
export default HelloWorld;
```
___

Create file in the `src` directory called `HelloUser`.
Open the file and add the below code.

```js
Import React with PureComponent

import React, { PureComponent } from 'react'; // destructuring

class HelloUser extends PureComponent {
  render(){
    //you can interpolate with {}
    return (<h2>Hello { this.props.name }</h2>)
  }
}

export default HelloUser;
```
___

Open `App.js` and import `HelloUser`
You can pass information through to components via `props` by giving the component an argument. Add a `name` with a value to the `HelloUser` component.

```js
import React, { Component } from 'react';
import HelloWorld from './HelloWorld';
import HelloUser from './HelloUser';

class App extends Component {
  render() {
    return (
      <div className="App">
        <HelloWorld />
        <HelloUser name="Greg"/>
      </div>
    )
  }
}
export default App;
```
___

Open console in your browser and go to the React tab. To install the `React Developer Tools` in chrome follow this link. [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)
You will now see that there is a `Props` window.
Navigate to the new component, you will see that we now have a value in the props window of `name: "Greg"`
___

### More Class Demos
- [React Intro](https://github.com/textchimp/wdi-27/tree/master/week7/react-intro)
- [React Calculator](https://github.com/textchimp/wdi-27/tree/master/week7/react-calculator)
- [React Clickr](https://github.com/textchimp/wdi-27/tree/master/week7/react-clickr)
- [React Social Media](https://github.com/textchimp/wdi-27/tree/master/week7/react-social-network)

___

#### Homework
[React tic tac toe](https://facebook.github.io/react/tutorial/tutorial.html)
