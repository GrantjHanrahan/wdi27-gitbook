## Week 08, Day 01

What we covered today:

- React Data Flow
- React Flickr Search
- React Routing
___

#### Warmup
- [AtBash - JS](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week08/day01_atBash)

___

### React

#### React Flickr Search

- [Class Demo](https://github.com/textchimp/wdi-27/tree/master/week8/react-flickr-search)

This module will recreate the Flickr Search we created on [Day 03](wk07_day03.md) using React.

##### Create a new React application.

```bash
create-react-app flickr-search
cd flickr-search/
cd src
mkdir components
mv App.js components/
rm App*
cd ..
atom.
```

Change the impost line on the `index.js` for App as we have moved it into the components folder.
```js
import App from './components/App';
```

open the `App.js` file and remove the unwanted information, the end result should look like this.
We also want to a new component `<FlickrSearch />` to the render.

```js
import React, { Component } from 'react';

class App extends Component {
  render() {
    return (
      <div className="App">
        <FlickrSearch />
      </div>
    );
  }
}

export default App;
```

Open the application in your browser
```bash
npm start
```

Create new file `FlickrSearch.js` in the `src` folder.
Import React, define the class, then export the component.

```js
import React, { PureComponent as Component} 'react';

class FlickrSearch extends Component {
  render(){
    return(
      <div>
        <h1>FlickrSearch coming soon</h1>
      </div>
    )
  }
}
export default FlickrSearch
```

Go to the `App.js` file and import into `FlickrSearch`.

```js
import FlickrSearch from './FlickrSearch';
```

Add two new components to `FlickrSearch` in the render function.

```js
import React, { PureComponent as Component} 'react';

class FlickrSearch extends Component {
  render(){
    return(
      <div>
        <h1>Image Search</h1>
        <SearchForm />
        <Gallery />
      </div>
    )
  }
}
export default FlickrSearch
```

Add the two classes to the top of the `FlickrSearch` page.

```js
class SearchForm extends Component {
  render(){
    return(
      <h2>Search coming soon</h2>
    )
  };
}

class Gallery extends Component {
  render(){
    return(
      <h2>Gallery coming soon</h2>
    )
  };
}
```

Inside the `SearchForm` class add an input with function, and a button.
```js
class SearchForm extends Component {
  render(){
    return(
      <form>
        <input type="search" placeholder="Butterflies" onInput={ this._handleInput } />
        <input type="submit" value="Search" />
      </form>
    )
  };
}
```

Create constructor and event handler.

```js
class SearchForm extends Component {

  constructor(){
    super();
    this.state = { query: ''};
  }

  _handleInput(e){
    this.setState( {query: e.target.value} )
  }

  render(){
    return(
      <form>
        <input type="search" placeholder="Butterflies" onInput={ this._handleInput } />
        <input type="submit" value="Search" />
      </form>
    )
  };
}
```

We now need to bind `this` in the constructor.

```js
constructor(){
  super();
  this.state = { query: ''};
  this._handleInput = this._handleInput.bind(this);
}
```

Pass the event handler to the form through `onSubmit`.

```js
render(){
  return(
    <form onSubmit={ this._handleSubmit }>
      <input type="search" placeholder="Butterflies" onInput={ this._handleInput } />
      <input type="submit" value="Search" />
    </form>
  )
};
```

Define `_handleSubmit` and prevent the form from submitting to the server.

```js
_handleSubmit(e){
  e.preventDefault();
}
```

We want to parent (HistoryEraser) to do the AJAX request as we will need to pass the information to the siblings.
We will take the input from the child (SearchForm) and pass it back up to the parent (HistoryEraser) so we can do the search and pass it to back to the sibling (Gallery).

We need to define another function `fetchImages`.

```js
class FlickrSearch extends Component {
  fetchImages(q){
    console.log(`searching for ${q}`);
  }

  render(){
    return(
      <div>
        <h1>Image Search</h1>
        <SearchForm onSubmit={ this.fetchImages }/>
        <Gallery />
      </div>
    )
  };
}
```

Then we pass the function through the `SearchForm` component in the `FlickrSearch` render.

```js
class FlickrSearch extends Component {
  render(){
    return(
      <div>
        <h1>Image Search</h1>
        <SearchForm onSubmit={ this.fetchImages }/>
        <Gallery />
      </div>
    )
  };
}
```

Pass the state of `query` into the `_handleSubmit` function.

```js
_handleSubmit(e){
  e.preventDefault();
  this.props.onSubmit( this.state.query );
}
```

then bind submit in constructor.

```js
constructor(){
  super();
  this.state = { query: ''};
  this._handleInput = this._handleInput.bind(this);
  this._handleSubmit = this._handleSubmit.bind(this);
}
```

Make a request to Flickr, in particular we want to make a `jsonp` request.

```js
fetchImages(q){
  const flickrURL = 'https://api.flickr.com/services/rest/?jsoncallback=?';
  const flickrParams = {
    method: 'flickr.photos.search',
    api_key: '2f5ac274ecfac5a455f38745704ad084',
    text: q,
    format: 'json',
    per_page: 500
  }
  console.log( flickrParams );
}
```

We now need to install [jsonp-es6](https://www.npmjs.com/package/jsonp-es6).

```bash
  npm install --save jsonp-es6
```

Import at the top of the `FlickrSearch` file.

```js
import jsonp from 'jsonp-es6';
```

Add the jsonp request to the bottom of the function.

```js
fetchImages(q){
  const flickrURL = 'https://api.flickr.com/services/rest/?jsoncallback=?';
  const flickrParams = {
    method: 'flickr.photos.search',
    api_key: '2f5ac274ecfac5a455f38745704ad084',
    text: q,
    format: 'json',
    per_page: 500
  }
  jsonp(flickrURL, flickrParams, {callback: 'jsoncallback'}).then(function(results){
    console.log( results );
  });
}
```

You will need to restart the server with `npm start` now that we have installed `jsonp-es6`.

Now we need to generate the `url`.

```js
jsonp(flickrURL, flickrParams, {callback: 'jsoncallback'}).then(function(results){
  const images = results.photos.photo.map(generateURL);
  console.log(images);
});

const generateURL = function (photo) {
     return [
       'http://farm',
       photo.farm,
       '.static.flickr.com/',
       photo.server,
       '/',
       photo.id,
       '_',
       photo.secret,
       '_q.jpg' // Change "q" for different sizes
     ].join('');
   }
```

Add constructor to the `FlickrSearch` at the top.

```js
constructor(){
  super();
  this.state = { images: [] };
}
```

Set the state of the images.

```js
jsonp(flickrURL, flickrParams, {callback: 'jsoncallback'}).then(function(results){
  const images = results.photos.photo.map(generateURL);
  this.setState({images: images}); //ES6: you can just use {images}
});
```

Bind `this` to `fetchImages` in constructor.
```js
constructor(){
  super();
  this.state = { images: [] };
  this.fetchImages = this.fetchImages.bind(this);
}
```

`this` has no value within the `jsonp` request. We could bind `this` to the end of the function like so:

```js
jsonp(flickrURL, flickrParams, {callback: 'jsoncallback'}).then(function(results){
  const images = results.photos.photo.map(generateURL);
  this.setState({images: images}); //ES6: you can just use {images}
}.bind(this));
```

But we will use a 'fat arrow' function to retain the value of `this`.
This will create a second copy of `this` to use later.

```js
jsonp(flickrURL, flickrParams, {callback: 'jsoncallback'}).then((results) => {
  const images = results.photos.photo.map(generateURL);
  this.setState({images: images}); //ES6: you can just use {images}
});
```

##### Setting up the Gallery

Pass the images state to the Gallery component

```js
render(){
  return(
    <div>
      <h1>Image Search</h1>
      <SearchForm onSubmit={ this.fetchImages }/>
      <Gallery images={ this.state.images }/>
    </div>
  )
};
```

Update the Gallery class to loop through each URL and `map` it to the `src` of an image tag.

```js
class Gallery extends Component {
  render(){
    return(
      <div>
        { this.props.images.map( (imageURL) => <img src={imageURL} /> ) }
      </div>
    )
  };
}
```

We will need to give each image a unique key to make sure there aren't multiple images.

```js
class Gallery extends Component {
  render(){
    return(
      <div>
        { this.props.images.map( (imageURL) => <img src={imageURL} key={ imageURL } /> ) }
      </div>
    )
  };
}
```
___

#### functional components

Functional components are used when your component doesn't need state or callbacks.

```js
function Image (props){
  return(
    <img src={props.url} width="150" height="150" alt={props.url} />
  )
}
```

Refer to the `Image` component in the render of `Gallery`.

```js
class Gallery extends Component {
  render(){
    return(
      <div>
        { this.props.images.map( (imageURL) => <Image url={imageURL} key={imageURL} /> ) }
      </div>
    )
  };
}
```
___

#### React Routing

- [React Router](https://github.com/ReactTraining/react-router)
- [Class Demo](https://github.com/textchimp/wdi-27/tree/master/week8/react-routing)

Create a new React application.

```bash
create-react-app routing
$ cd src
mkdir components
mv App.js components/
rm App*
cd ..
npm install --save react-router react-router-dom
atom .
```

Open `App.js` and remove the information you don't need.
The end result should look like this:

```js
import React, { Component } from 'react';

class App extends Component {
  render() {
    return (
      <div className="App">

      </div>
    );
  }
}

export default App;
```

As we have moved the `App.js` file we need to change the referencing in the `index.js` file.

```js
import App from './components/App';
```

Create file in the `src` folder called `Routes.js`.

Create the routes and export them then import `Routers` and `React`.

```js
import React from 'react';
import { HashRouter as Router, Route } from 'react-router-dom';

export const Routes = { // You don't need 'default' if you are exporting on this line
  <Router>
    <div>
      <Route exact path="/" component={ Home } />
      <Route exact path="/faq" component={ FAQ } />
    </div>
  </Router>
}
```

In the `index.js` remove `import App from './components/App';` and replace it with `import Routes from './Routes';`
You will also need to remove `<App />` from the ReactDOM.render and replace with `Routes`. Note that we are referring to the imported `Routes` and not the component. This is why we don't need the opening and closing brackets `< />`.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
//import App from './components/App';
import Routes from './Routes';
import registerServiceWorker from './registerServiceWorker';

ReactDOM.render(Routes, document.getElementById('root'));
registerServiceWorker();
```

Run the application from your terminal
```bash
npm start
```

You will see that there a few errors that we need to take care of before moving on.

We need to create `Home` component.

Create a file new file in the `src` folder called `Home.js` and add the below code.

```js
import React, {PureComponent as Component} from 'react';

class Home extends Component {
  render(){
    return(
      <h2>This is the home page</h2>
    )
  };
}

export default Home;
```

Create another file in the `src` folder called `FAQ` and add this code.

```js
import React, {PureComponent as Component} from 'react';

class FAQ extends Component {
  render(){
    return(
      <h2>This is the FAQ page</h2>
    )
  };
}

export default FAQ;
```

Import into both `Home` and `FAQ` at the top of the `Routes.js` file and export `Routes` at the bottom.

```js
import React from 'react';
import { HashRouter as Router, Route } from 'react-router-dom';

import Home from './components/Home';
import FAQ from './components/FAQ';

export const Routes = {
  <Router>
    <div>
      <Route exact path="/" component={ Home } />
      <Route exact path="/faq" component={ FAQ } />
    </div>
  </Router>
}

export default Routes;
```

##### Adding links to the page

Open the `Home.js` file and import `Link` from `react-router-dom`.

```js
import React, {PureComponent as Component} from 'react';
import { Link } from 'react-router-dom';

class Home extends Component {
  render(){
    return(
      <div>
        <h2>This is the home page</h2>
        <p>
          Please check out our
          <Link to="/faq">
            Frequently asked questions
          </Link>
        </p>
      </div>
    )
  };
}

export default Home;
```

Now we need to link back from the FAQ page to the Home page.

```js
import React, {PureComponent as Component} from 'react';
import { Link } from 'react-router-dom';

class FAQ extends component {
  render(){
    return(
      <div>
        <h2>This is the FAQ page</h2>
        <Link to="/">
          Back to home
        </Link>
      </div>
    )
  };
}

export default FAQ;
```

Go back onto your browser and navigate through the links. You will see that you're now able to use the back arrow to move around the pages.
___

### Homework

Combine the react router into the flickr-react-app.
- [Flickr Search Router](https://github.com/textchimp/wdi-27/tree/master/week8/react-flickr-search-router)

Some topics below for you to brush up on:

- [React Router Documentation](https://reacttraining.com/react-router/)
- [Thinking In React](https://reactjs.org/docs/thinking-in-react.html)
- [Learning ES6](http://learnharmony.org/)
- [AXIOS - npm](https://www.npmjs.com/package/axios)
