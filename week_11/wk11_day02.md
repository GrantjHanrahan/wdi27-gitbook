## Week 11, day 02

What we covered today:

  - Regex contd
  - Recursion
  - Javascript - Github Search
    - Webpack

___

#### Warmup

- [Markov Text](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week11/day_02_markov_text)

___

#### Codealongs

- [Regex](https://github.com/textchimp/wdi-27/tree/master/week11/regular-expressions)
- [Recursion](https://github.com/textchimp/wdi-27/tree/master/week11/recursion)
- [Webpack- React](https://github.com/textchimp/wdi-27/tree/master/week11/webpack-react-github-app)

___

### Recursion


Create a new folder and add a new ruby file into it.

```sh
mkdir recursion
cd recursion
touch countdown.rb
atom .
```

Iterative method:

```ruby
def countdown_i (n=10)
  while n >= 0
    puts n
    n -= 1
    sleep 1
  end

  puts "Blast off!"

end

require 'pry'
binding.pry
```

Call the method

```ruby
ruby countdown.rb
countdown_i
```

Recursive method:

```ruby
def countdown_r (n=10)
  if n < 0 #Base case
    puts "Blast off"
  else
    puts n
    sleep 1
    countdown_r n-1 # Recursive case which moves us closer to the case
  end
end

require 'pry'
binding.pry
```

Call the method

```ruby
ruby countdown.rb
countdown_r
```

Some things are better solved with recursion like factorials.

```ruby
5! = 5 x 4 x 3 x 2 x 1
```

Create a new file.

```sh
touch factorial.rb
```

Iterative method:

```ruby
def factorial_i(n)
  result = 1
  while n > 1
    result = result * n # Mutation
    n -= 1 # Move closer to the base case
  end
  result
end

require 'pry'
binding.pry
```

Call the method

```ruby
ruby factorial.rb
factorial_i 5
```

Recursive method:

```ruby
def factorial_r(n)
  if n > 1 # Recursive case
    n * factorial_r(n -1)
  else
    1 # Base case
  end
end

require 'pry'
binding.pry
```

Call the method

```ruby
factorial.rb
factorial_r 5
```

Create another file. Fibonacci

```sh
touch fibonacci.rb
```

Iterative method:

```ruby
def fibonacci_i(n)
  a = 1
  b = 1

  while n > 1
    a, b = b, a + b  # Parallel assignment: a = b; b = a + b
    n -= 1
  end
  a # The final result will be in a
end

require 'pry'
binding.pry
```

Recursive method:

```ruby
# Very slow
def fibonacci_r(n)

  if n <= 2
    1 # Base case
  else
    fibonacci_r(n - 1) + fibonacci_r(n - 2)
  end
end


def factorial_r(n)
  if n > 1 # Recursive case
    n * factorial_r(n -1)
  else
    1 # Base case
  end
end
```

This example is extremely slow because we call the method twice with each iteration. This will exponential increase, doubling the
If you call `fibonacci_r(100)` it looks for `fibonacci_r(98) + fibonacci_r(99)`. The problem is that it will then call `fibonacci_r()` twice for each of those `fibonacci_r(97) + fibonacci_r(98)`. As you can see you will be calling `fibonacci_r(98)` twice and create two copies of the branch even though you have already figured that out.

___

### Github Search

Adapted/corrected from: https://github.com/developit/zero-to-preact

```sh
mkdir project-name
cd project-name
npm init -y
```

```sh
npm install --save-dev webpack webpack-dev-server babel-core babel-loader babel-preset-es2015 babel-preset-env babel-plugin-transform-react-jsx babel-plugin-transform-object-rest-spread
```

```js
// package.json
"scripts": {
    "build": "webpack",
    "start": "webpack-dev-server --progress --hot --inline"
},
```

```js
// webpack.config.js
let path = require('path');

module.exports = {
  entry: './src',
  output: {
    path: path.join(__dirname, 'build'),
    filename: 'bundle.js'
  },
  resolve: {
    alias: {
      'react': 'preact-compat',
      'react-dom': 'preact-compat'
    }
  },
  module: { // Que?
    rules: [
      {
        test: /\.jsx?/i,
        loader: 'babel-loader',
        options: {
          presets: ['es2015'],
          plugins: [
            ['transform-react-jsx']
          ]
        }
      }
    ]
  },
  // devtool: 'source-map',
  devServer: {
    contentBase: path.join(__dirname, 'src'),
    compress: true,
    historyApiFallback: true
  }
}
```

```html
src/index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Hello World</title>
</head>
<body>
  <script src="/bundle.js" async></script>
</body>
</html>
```

```sh
npm install --save preact preact-compat
```

```js
// src/index.js
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import 'preact/devtools'

import App from './components/App';

ReactDOM.render(<App />, document.body);
```

```js
// src/components/App.js
import React, { Component } from 'react';
import Hello from './Hello';

export default class App extends Component {
  render() {
    return (
      <div class="app">
        <h1>Hello World</h1>
        <Hello />
      </div>
    )
  }
}
```

```js
// src/components/Hello.js
import React, { Component } from 'react';

export default () => {
  return (
    <p class="hello">
      Hello Cruel World
    </p>
  );
}
```

```js
// src/index.js
import React, { Component } from 'react';
import ReactDOM from 'react-dom';

import App from './components/App';

let root;

let init = function () {
  let App = require('./components/App').default;
  root = ReactDOM.render(<App/>, document.body, root);
}

init();

if (module.hot) {
  module.hot.accept('./components/App', init);
}
```

## Alternatively
```sh
create-react-app <appname>
cd appname
npm run eject # Don't forget to fill out the survey!
# ... Configure Preact as above
```
___

#### Homework

- 1) Recursive DOM tree traversal function
- 2) Github React: render user stats with a `<Stats />` component (save the Ajax response into state first), and list of user repos with a `<Repos />` component (you will need to add a method in `src/githubUtils.js` to retrieve this list); implement the random user profile button functionality in `Home.js` (define an array of valid GitHub usernames to choose from)
- 3) Final project brainstorming!

#### Further Reading

- [Recursion](https://www.cs.utah.edu/~germain/PPS/Topics/recursion.html)
- [Recursion, Recursion, Recursion](https://medium.freecodecamp.org/recursion-recursion-recursion-4db8890a674d)
- [Recursion | The Bastards Book of Ruby](http://ruby.bastardsbook.com/chapters/recursion/)
- [Webpack](https://webpack.js.org/)
- [Webpack Concepts](https://webpack.js.org/concepts/)
