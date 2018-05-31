## Week 07, Day 04

What we covered today:
  - Flickr Solution
  - Transpilation Intro
  - Node Installfest
  - Frontend Frameworks:
    - Vue.js intro

___

#### Warmup
- [Space Age](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week07/day04_space_age)

___

#### Flickr Solution

- [Class Demo - Flickr](https://github.com/textchimp/wdi-27/tree/master/week7/flickr-search)

### Codealong

- [Vue intro](https://github.com/textchimp/wdi-27/tree/master/week7/vue-intro)

___

### Transpilation Intro

- [Babeljs.io](https://babeljs.io/)
- [Babel language pluggin - ATOM](https://atom.io/packages/language-babel)

##### Set up npm in your project

```bash
npm init -y
```

##### Install babel-cli and babel-env

```bash
npm install --save-dev babel-cli babel-env
```
Note the .babelrc file is created for you.

##### Create a .gitignore so you don't commit your 34 MB node_modules folder

```bash
echo "node_modules/" >> .gitignore
```

##### Add a script to build your project (in package.json)

```json
"scripts": {
  "build": "babel src -d js",
  "watch": "babel --watch src -d js",
},
```

##### Ensure your es6 code is in a `src` directory

##### Build!

```bash
npm run build
```

or

```bash
npm run watch
```

___

### Node Installfest

- [nodejs.org](https://nodejs.org/en/)

##### Open the node repl
`node`

##### check the version of node
```plain
node -v
=> v9.4.1
```
##### The below line will either install or update node depending on if you have it installed
`brew install nodejs`

##### Navigate back to the root directory
`cd ~`

##### Create a new folder
`mkdir .npm-packages`

##### Create a new file
`touch .npmrc`

##### Open that file in ATOM
`atom .npmrc`

##### Paste the below line into file
`prefix=${HOME}/.npm-packages`

##### Go back to your terminal and open your `.bash_profile` file
`atom ~/.bash_profile`

##### Add the below code to your `.bashrc `
`# Via https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md
NPM_PACKAGES="${HOME}/.npm-packages"
PATH="$NPM_PACKAGES/bin:$PATH"`

##### Run the below code to check if anything is inside the folder (you may not have anything in there yet)
`ls -l ~/.npm-packages`

##### Install learnyounode - globally
`npm install -g learnyounode`
Learnyounode is an application that runs in the terminal to teach you how to use `node`

##### To open the application run
`learnyounode`

___

### Vue

##### What is it?

Vue (pronounced /vjuː/, like view) is a progressive framework for building user interfaces. Unlike other monolithic frameworks, Vue is designed from the ground up to be incrementally adoptable.

The core library is focused on the view layer only, and is easy to pick up and integrate with other libraries or existing projects. On the other hand, Vue is also perfectly capable of powering sophisticated Single-Page Applications when used in combination with modern tooling and supporting libraries.

##### Lets get started

Add vue to your js folder:

```
  project$ wget unpkg.com/vue -O js/vue.js
```

##### Docs:
The Vue documentation is really easy to follow. I'm not going to waste everyone's time regurgitating it, so go to the
 [source](https://vuejs.org/v2/guide/#Getting-Started)!

___

#### Homework

-  [Continue the Vue tutorial (components)](https://vuejs.org/v2/guide/#Getting-Started)
