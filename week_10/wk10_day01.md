## Week 10, Day 02

What we covered today:

  - Burning Airlines in Vue

___

#### Warmup

- [Prime Factors](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week10/day_01_prime_factors)

___

### Burning Airlines - Vue

- [Codealong](https://github.com/textchimp/wdi-27/tree/master/week10/burning-airlines-rails-vue)

Create a directory where you want to store your application.

```bash
mkdir ba-vue
```

Make a new rails project and cd into it.

```bash
rails new ba-rails-backend --skip-spring --skip-turbolinks --skip-git -d postgresql -T
```

Create the airplane, flight, user and reservation models;

`rails g model airplane name:string rows:integer cols:integer`
`rails g model Flight flight_number:integer departure_date:datetime origin:string destination:string airplane_id:integer`
`rails g model User email:string name:string password_digest:string`
`rails g model Reservation user_id:integer flight_id:integer row:integer col:integer paid:datetime`

Add necessary gems to the Gemfile

```ruby

gem 'bcrypt'
gem 'pry-rails'
gem 'pry-byebug'
gem 'rack-cors'

```

Create, migrate and seed the db

`rails db:create`
`rails db:migrate`

```ruby
# in seeds.rb

User.destroy_all
Flight.destroy_all
Reservation.destroy_all
Airplane.destroy_all

u1 = User.create name: 'Test User 1', email: 'one@one.com', password: 'chicken'
u2 = User.create name: 'Test User 2', email: 'two@two.com', password: 'chicken'
u3 = User.create name: 'Test User 3', email: 'three@three.com', password: 'chicken'

a1 = Airplane.create name: '737', rows: 40 , cols: 8
a1 = Airplane.create name: '757', rows: 60, cols: 6

f1 = Flight.create flight_number: '256', departure_date: '2018-10-23 4:20',  origin: 'SYD', destination: 'SFO', airplane: a1
f2 = Flight.create flight_number: '512', departure_date: '2018-12-25 11:20', origin: 'SIN', destination: 'JFK', airplane: a2
f3 = Flight.create flight_number: '1024', departure_date: '2018-12-01 12:20', origin: 'SYD', destination: 'SFO', airplane: a1

r1 = Reservation.create flight: f1, user: u1, row: 10, col: 2
r2 = Reservation.create flight: f1, user: u2, row: 12, col: 4
r3 = Reservation.create flight: f1, user: u3, row: 20, col: 2
r4 = Reservation.create flight: f2, user: u3, row: 11, col: 2

puts "Created #{ Flight.length } flights, #{ Airplane.length } airplanes, #{ User.length } users, and #{ Reservation.length } reservations."
puts "Airplane 1 has #{ Airplane.first.flights.length } flights."
puts "Flight 1 has #{ Flight.first.reservations.length } reservations and #{ Flight.first.passengers.length } passengers."
puts "User 3 has #{ User.third.reservations.length } reservations."

```

Create a controller to handle the routes

`rails g controller Flights search show`

Remove the default routes from routes.rb and add one to handle a search from the flights page

```ruby
# routes.rb

Rails.application.routes.draw do
  get '/flights/search' => 'flights#search'
end

```

Add the methods to the Flights controller

```ruby

# flights_controller.rb

class FlightsController < ApplicationController
  def search
    puts "origin: #{params[:origin]}"
    puts "destination: #{params[:destination]}"

    results = Flight.where origin: params[:origin], destination: params[:destination]

    render({
      json: results,
      include: { airplane: { only: :name }},
      status: :ok
    })
      # include: [:airplane]
  end

  def show
  end

end

```

Don't forget to add the following code to the config/application.rb file

```ruby
config.middleware.insert_before 0, Rack::Cors do
 allow do
   origins '*'
   resource '*', :headers => :any, :methods => [:get, :post, :options, :delete]
 end
end

```

Without this you will recieve a CORS error/ blocked request - http://www.blog.bdauria.com/?p=427

___

Time to create the Frontend in Vue

Vue CLI: Vue provides an official CLI for quickly scaffolding ambitious Single Page Applications. It provides batteries-included build setups for a modern frontend workflow. It takes only a few minutes to get up and running with hot-reload, lint-on-save, and production-ready builds - https://vuejs.org/v2/guide/installation.html

Install the Vue CLI

```bash
npm install -g vue-cli
```

You will now have access to the vue command, which will be used to create the Vue project similar to create-react-app

```bash
vue init
vue-init <template-name> [project-name]

vue list

  ★  browserify - A full-featured Browserify + vueify setup with hot-reload, linting & unit testing.
  ★  browserify-simple - A simple Browserify + vueify setup for quick prototyping.
  ★  pwa - PWA template for vue-cli based on the webpack template
  ★  simple - The simplest possible Vue setup in a single HTML file
  ★  webpack - A full-featured Webpack + vue-loader setup with hot reload, linting, testing & css extraction.
  ★  webpack-simple - A simple Webpack + vue-loader setup for quick prototyping.

```

```bash

$ ba-vue/ba-rails-backend
cd ..
vue init webpack ba-vue-frontend

... you will then be prompted for the following;
? Project name ba-vue-frontend
? Project description A Vue.js project
? Author ...your amazing TA <your@email.com>
? Vue build standalone
? Install vue-router? Yes
? Use ESLint to lint your code? No
? Set up unit tests Yes
? Pick a test runner karma
? Setup e2e tests with Nightwatch? No
? Should we run `npm install` for you after the project has been created? (recomme
nded) (Use arrow keys)
❯ Yes, use NPM
  Yes, use Yarn
  No, I will handle that myself

```

Inside ba-vue-frontend run the Vue server

```bash

cd ba-vue-frontend
npm run dev

...

Your application is running here: http://localhost:8080

```
___

Further Reading:

- [Vue Intro](https://vuejs.org/v2/guide/index.html)
- [Vue Components](https://vuejs.org/v2/guide/components.html)
- [Template Syntax](https://vuejs.org/v2/guide/syntax.html)
- [Vue + Axios](https://vuejs.org/v2/cookbook/using-axios-to-consume-apis.html)
