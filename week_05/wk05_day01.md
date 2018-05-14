## Week 05, Day 01

What we covered today:
- Ruby on Rails - Introduction
- Ruby on Rails - Basic Project Setup (no database)
- Ruby on Rails - Intro to Project Setup (with database)

___

#### Warmup

- [Point Mutations - Ruby](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week05/day01_point_mutations)

___

### Slides

[Introduction to Rails](https://slack-files.com/files-pri-safe/T0351JZQ0-FAP0Z5P34/introduction-to-rails.pdf?c=1526278777-54d99b2fc669866a6f08ffb83ca34f360c6ab5a1)

___
#### Classwork


- [Rails-intro](https://github.com/textchimp/wdi-27/tree/master/week5/rails-intro)
- [Rails-stocks-and-movies](https://github.com/textchimp/wdi-27/tree/master/week5/stocks-movies)

___

### Ruby on Rails - Introduction

#### A Brief History of Rails

David Heinemeier Hansson ('DHH') extracted Ruby on Rails from his work on the project management tool Basecamp, at the web application company also called Basecamp (formerly 37Signals). It was a web application framework he developed while creating Basecamp. He released it as open source in July 2004 (but didn't give out commit rights until February 2005).

___
#### _Ruby on Rails - A Brief History of Rails - Further Reading_

- [Wikipedia - Ruby on Rails](http://en.wikipedia.org/wiki/Ruby_on_Rails)
___

#### What is Rails?

Rails is a web application framework, built with Ruby. It is designed to make programming all web applications much, much easier by making lots of assumptions about what you are going to need to get started.  It means you have to write a lot less code, while building a lot more.  Similar to Ruby, it makes development fun.

Rails is very opinionated (the same as its creator) - it makes an assumption there is a 'best' way to do something, and it encourages you to take that approach (while still allowing flexibility). If you follow the 'Rails Way', building a Rails app will be much easier.

The Rails philosophy includes three major guiding principles, which really just stem from Ruby itself (except the MVC pattern):

- **Don't Repeat Yourself ('DRY')** - DRY is a principle of software development, not exclusive to Ruby (though Ruby is very good at it, as is its community). It's main thing is that "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system". That makes it sound a lot more complex than it really is. All it means is that by not writing the same code over and over again, our code is more maintainable, extensible and less buggy.
- **Convention Over Configuration ('COC')** - Rails, as previously mentioned, is very opinionated. If you don't follow its guidelines (naming conventions etc.), it is difficult. But if you do, the set up is very efficient. It has the capacity to do most things by default as long as you follow the 'right' approach - the 'Rails way'.
- **Model, View and Controller ('MVC')** - This is probably the most important. Rails structures everything like this, so it is important to understand this. MVC is a software architecture pattern that breaks the code into small manageable chunks and stems from the problem when trying to modularize a user interface functionality so that it is maintainable and extensible. Those chunks are Models, Views and Controllers as you have probably guessed. But what are they?
    + **Models** - The model manages the behaviour and data of the application domain, also where the database classes are created.
    + **Views** - The view manages the display of information.
    + **Controllers** - The controller interprets the user behaviour ( it is like the switchboard )

  An easy way to understand MVC: the model is the data, the view is the window on the screen, and the controller is the glue between the two. -- [ConnellyBarnes](http://www.connellybarnes.com/work/)

   It is all about the interactions. A controller can send commands to the model to update the model's state (e.g., editing a document). It can also send commands to the associated view to change the presentation (and content).
   A model stores data that is retrieved by the controller and displayed in the view. Whenever there is a change to the data it is updated by the controller.
   A view requests information from the model that it uses to generate an output representation to the user.

#### How to install Rails

Simple! Rails is a gem. Just run `gem install rails`, make sure you don't add sudo at the start, even if prompted to do so!  Ruby is, obviously, a dependency, so if you don't have Ruby, get that first - see the Ruby installation guide in [Week 4, Day 1](../week_04/wk04_day01.md).

#### Basic Rails Project

##### _Step 1: Planning_

The first step for any Rails project is planning - it is very important to map out what your site will do, and how a user will use your site. Once you have figured that out, you can begin creating the project.

##### _Step 2. New Rails Project_

To create the Rails project, we use the `rails new project_name --skip-git --skip-turbolinks` command in terminal. This takes a lot of options, but for now we will just stick with the basic command without any options.

Let's create a new project called "basics".

```sh
  rails new basics --skip-git --skip-turbolinks
```

The terminal output should be about a hundred lines that look like `create app/assets/javascripts/application.js`, `bin/rails: spring inserted`, etc.

Then we need to then move into our project ( `cd project_name` ). This is where we'll run most commands relating to our project, like opening up a server, accessing the Rails console, bundling our Gems, etc.

```sh
  cd basics
```

From here, we can open all the files Rails generated for us in Atom.

```sh
  atom .
```

##### _Step 3. Customize Gemfile_

The Gemfile is where we specify what Ruby Gems we want to include in our application. Remember how with Sinatra we would first install a Gem from the command line and then `require` all the Gems our application depended on in the main.rb file? In a Rails app, we do this in our Gemfile.

The first changes we should make to the Gemfile are to make debugging easier.

After this, we alter our Gemfile to be more suited for debugging. The code we tend to use is this...

```ruby
group :development do
  gem 'pry-rails'

end
```
It's important to make sure those particular Gems go in a `group :development` block - we don't want those Gems being installed in the production environment!

Once we've updated our Gemfile, we need to 'bundle' our Gems - this will install the Gems in our Gemfile, along with their dependencies, which is quite important.

```sh
  bundle
```

The terminal output should be about a few dozen lines that look like `Using web-console 2.3.0`, etc. The last few lines should look something like this:

```sh
 => Bundle complete! 12 Gemfile dependencies, 57 gems now installed.
 => Use `bundle show [gemname]` to see where a bundled gem is installed.
```

If you see something else, there was probably an error bundling the Gems which you'll need to address before going any further.

##### _Step 4. Setup Routes_

Now we get into the Routes (found in config/routes.rb). This is like the 'gets' methods in our Sinatra app's main.rb file.

A route determines how an HTTP request to a particular URL is handled - for example, in the second route below, a `get` request to the `/home` path will be sent to the `home` action in the `pages` controller.

There are a million different ways of approaching this, but at its most basic, your Routes might look something like this:

```ruby
Rails.application.routes.draw do
  # Root routes look a little different to other routes. This basically just says that requests to the "root" path (eg "/") are sent to the "pages" controller's "home" action.
  root :to => 'pages#home'

  # STATIC ROUTES
  # request_type route => controller#action
  get '/home' => 'pages#home'

  # DYNAMIC ROUTES WITH VARIABLE BITS IN PARAMS (JUST LIKE SINATRA)
  get '/auto/:color' => 'auto#color'
end
```

Lets load up the server by running `rails server` or `rails s` from our Rails project's root directory.  If we go to `localhost:3000`, we'll see an error.  

We are up to an Error Driven Development point now - we let the errors guide where we go next.


##### _Step 5. Create a Controller_

The first error we encounter will be an `uninitialized Constant` error. This means that we haven't created the controller associated with this URL - in our routes (config/routes.rb), we have directed requests to the root path (ie "/", or "localhost:3000/" in development) to the **Pages controller**'s **Home action** (`root :to => "pages#home"`).

So, we need a Pages Controller, and we need a "home" method within it.

In our app/controllers/ folder, we need to create a file called **pages_controller.rb**. The spelling and syntax is important!

All our controllers inherit from the ApplicationController (remember classes in Ruby? This is an example of how Rails uses Ruby classes and inheritance) - this is what gives it all of its functionality.  Our Pages controller, at its most basic, is going to look something like this...

```ruby
# Create a controller called PagesController, which inherits from the  ApplicationController
class PagesController < ApplicationController

    # Define a method (or "action") called "home" in this controller.
    def home
    end
end
```

##### _Step 6. Create a View_
Once we have this setup, if we refresh the page we'll see the **Missing template** - this is telling us that, even though our routes.rb has a route matching the path requested, and even though we have a controller and action matching those specified in the router, we don't have a view for that action.

By default, Rails will look for a template in with the same name as the action within a folder that has the same name as the controller - this is why following naming conventions is so important in Rails!

In this case, Rails is looking for app/views/**pages**/**home**.html.erb, so let's make that file:

```sh
mkdir app/views/pages
touch app/views/pages/home.html.erb
```

This is the sort of setup you need to go through for each particular page in your application (there are many ways to speed up the process, though).

To summarize...

### Ruby on Rails - Basic Project Setup (no database)

This is a summary of the steps detailed above:


| Step | What              |  Where                                      | How                                      |
|----- |------------------ |-------------------------------------------- |----------------------------------------- |
| 1    | Planning          |                                             | Pen & paper                              |
| 2    | New Rails project | Terminal                                    | `rails new my_new_app_name --skip-git --skip-turbolinks`   |
| 3    | Edit Gemfile      | /Gemfile                                    | Add development Gems and bundle          |
| 4    | Setup routes      | /config/routes.rb                           | For each path, add a controller#action   |
| 5    | Create controller | /app/controllers/                           | Create a [controller]_controller.rb file |
| 6    | Create actions    | /app/controllers/[controller]_controller.rb | For each action, create a method         |         
| 7    | Create views      | /app/views/[controller]/[action].html.erb   | Create views for actions*                |
| 8    | Repeat 4-8        |                                             |                                          |

\* Remember - not all actions have a corresponding view!

___

### Ruby on Rails - Basic Project Setup (with database)

Treat this as a really rough guide, and definitely don't always follow it - you'll figure out your approach soon. This is a basic approach you might follow when setting up a new Rails project:



| Step | What                        |  Where                             | How                                                   |
|----- |---------------------------- |----------------------------------- |------------------------------------------------------ |
| 1    | Planning                    | -                                  | Pen & paper                                           |
| 2    | Create a new Rails app      | Terminal                           | `rails new kitten_party   `                           |
| 3    | Move into the new app's dir | Terminal                           | `cd kitten_party`                                     |
| 4    | Add development Gems        | /Gemfile                           | `gem "remove_turbolinks"`<br>`group :development do`<br> `gem "pry-rails"`<br> `gem "pry-stack_explorer"`<br> `gem "annotate"`<br> `gem "quiet_assets"`<br> `gem "better_errors"`<br> `gem "binding_of_caller"`<br> `gem "meta_request"`<br>`end` |
| 5    | Bundle Gems                 | Terminal                           | `bundle`                                              |
| 6    | Create database             | Terminal                           | `rake db:create`                                      |
| 7    | Generate first model        | Terminal                           | `rails generate model Kitten name:string age:integer` |
| 8    | Edit migration as required  | /db/migrate/[your_migration].rb    | Add timestamps, more columns, etc if required.        |
| 9*   | Generate first controller   | Terminal                           | `rails generate controller Kittens`                   |
| 10   | Add methods to the controller | /app/controllers/kittens_controller.rb | Add methods for each action |
| 11   | Add views for your actions  | /app/views/kittens/                | Add an [action].html.erb file for each action that has an associated view |
| 12   | Configure routes            | /config/routes.rb                  | `root :to => "kittens#index"`<br>`resources :kittens` |
| 13   | Repeat for other models     | -                                  | Repeat steps 7-12 for each model                      |

\* You can specify the actions for which you would like Rails to create views and methods when generating the controller by adding them to the end of the command - eg `rails generate controller Kittens index show edit new`. This will create those views, and the placeholder methods in your controller, but it also generates a bunch of routes that you will probably want to delete.

___
### Ruby on Rails - Migrations

Migrations are a way to modify your database schema using Ruby code. Using migrations, we can:

- Avoid writing SQL by hand;
- Change our database schema progressively over time;
- Roll back to earlier versions of our database schema;
- Rebuild our database schema from scratch at any time.

Note: Generally, we use migrations for changes to the database schema, not the data in a database; ie, to modify the tables in the database, not to modify the records in the database.

There are a few terminal commands that will generate a migration. These include:

```sh
rails generate model Kitten
rails generate migration add_species_to_kitten
```

Migrations are stored in the db/migrate directory and the filenames are prefixed with a timestamp representing the actual time the migration file was generated, eg: **20160706022128_create_kittens.rb**. Rails runs migrations according to the order of their timestamps, so it's important that you do NOT edit the filename of a migration.

We most commonly use migrations to:

1. Create a table in our schema;
2. Modify a table in our schema.

#### Migrations to create a table

Creating a model using `rails generate model` will create a migration for creating the table for that model:

```sh
rails generate model Kitten
# => create    db/migrate/20160706031227_create_kittens.rb
```

The migration file will look something like this:

```ruby
class CreateKittens < ActiveRecord::Migration
  def change
    create_table :kittens do |t|

      t.timestamps null: false
    end
  end
end
```

In its most basic form, we would then add columns to that table...

```ruby
class CreateKittens < ActiveRecord::Migration
  def change
    create_table :kittens do |t|
      t.string :name
      t.string :name
      t.integer :age
      t.timestamps null: false
    end
  end
end
```

Once we have filled out the migration, we run the migrations - this will run all migrations that have yet to be run, in order according to their timestamps:

```sh
rails db:migrate
```

It's important to check that the migration was successful - ideally, the terminal response should be something like this:

```sh
== 20160706031227 CreateKittens: migrating ====================================
-- create_table(:kittens)
```

If not, there was a problem with your migration! Read the terminal response and try again.


### Homework

- Read: [Rails Guides - Getting Started](http://guides.rubyonrails.org/v4.2/getting_started.html)
- Read: [SitePoint - A Quick Study of the Rails Directory Structure](https://www.sitepoint.com/a-quick-study-of-the-rails-directory-structure/)
- Make: [Magic 8 Ball | Secret Number | Rock Paper Scissors](https://gist.github.com/textchimp/378e0b7fff224fffe56402e7159f52c3)
