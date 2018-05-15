## Week 05, Day 02

What we covered today:
- Ruby on Rails - Conventions
- Ruby on Rails - Helpers
- Ruby on Rails - Rails in the Terminal
- Ruby on Rails - Basic Project Setup (with database)
- Ruby on Rails - Migrations
___

#### Warmup

- [Robot Name - Ruby](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week05/day02_robot_factory)

___
#### Classwork

- [Codealong](https://github.com/textchimp/wdi-27/tree/master/week5)

___

### Ruby on Rails - Conventions

#### Naming conventions

##### _Models_

Models are singular, with a capitalized class name and inherit from ActiveRecord::Base.

The Model files are located in `app/models`, have a singular, snake_cased file name and an extension of .rb

##### _Controllers_

Controllers are plural, with a PascalCase class name and inherit from ApplicationController.

The Controller files are located in `app/controllers`, have a plural, snake_cased file name ending in `controller` and an extension of `.rb`.

##### _Views_

Views are a bit more difficult. They reside in a folder in `app/views`, the name of that folder comes from the associated controller and is plural and snake_cased.  

The name of the actual file is the name of the associated action (or method), and have an extension of `.html.erb`.
They don't inherit from anything.


| Type        | File Name              | Location          | Class Name        | Inherits From         |
| ----------- | ---------------------- | ----------------- | ----------------- | --------------------- |
| Model       | planet.rb              | app/models        | Planet            | ActiveRecord::Base    |
| Controllers | planets_controller.rb  | app/controllers   | PlanetsController | ApplicationController |
| Views       | index.html.erb         | app/views/planets | N/A               | N/A                   |

___

### Ruby on Rails - Helpers

View helpers are collections of methods for patterns often expressed in Rails views. They can only be used in our views, and obviously need to be wrapped in ERB tags.

#### Number Helpers

Number helpers are methods for converting numbers into formatted strings. Methods are provided for phone numbers, currency, percentage, precision, positional notation and file size.

```ruby
number_to_currency( value )
number_to_human( value )
number_to_phone( value, options )
number_to_phone( value, :area_code => true )
```

[Here are all of the number helpers.](http://api.rubyonrails.org/v4.2/classes/ActionView/Helpers/NumberHelper.html)

#### Text helpers

These are a set of methods for filtering, formatting and transforming strings.

```ruby
# Pluralize - format
pluralize( value, 'singular_case' )
# Pluralize - example: @person_count = 4
pluralize( @person_count, 'person' )
# => 4 people

# Truncate - format
truncate( value, options )
# Trunace - example: @story = "The quick brown fox jumped over the lazy dog."
truncate( @story, :length => 15 )
# => "The quick brown"

# Cycle - format
cycle( list_of_values )
# Cycle - example:
cycle( 'red', 'green', 'orange', 'purple' )
```

[Here are all of the text helpers.](http://api.rubyonrails.org/v4.2/classes/ActionView/Helpers/TextHelper.html)

#### Assets Helpers

These are methods for generating HTML links (or simple paths) to assets such as images, JavaScript files, stylesheets, and feeds.

```ruby
# image_tag - format
image_tag( 'path', options )
# image_tag - examples
image_tag( 'funny.jpg' )
# => <img src="http://badgerville.com/assets/funny.jpg">
image_tag( 'http://fillmurray.com/500/500', :class => "oh-bill" )
# => <img src="http://fillmurray.com/500/500" class="oh-bill">
image_path( 'edit.png' )
# => /assets/edit.png
```

[Here are all of the asset helpers.](http://api.rubyonrails.org/v4.2/classes/ActionView/Helpers/AssetUrlHelper.html)

#### URL helpers

These are a set of methods for making links and getting URLs.

```ruby
link_to( 'Home', root_path )
link_to( 'Work Path', work_path( work.id ) )
link_to( 'Work Path', work_path( work ) )
link_to( 'Work Path', work )
button_to( 'Test Path', root_path, :method => 'GET' )
```

[Here are all of the url helpers.](http://api.rubyonrails.org/v4.2/classes/ActionView/Helpers/UrlHelper.html)

___

#### _Rails - Helpers - Further Readings_

- [Rails Guides - Overview of Helpers Provided by Action View](http://edgeguides.rubyonrails.org/action_view_overview.html#overview-of-helpers-provided-by-action-view)
- [Ruby on Rails API - ActionView::Helpers](http://api.rubyonrails.org/classes/ActionView/Helpers.html) - a list of all Action View helpers.

___

### Ruby on Rails - Rails in the Terminal

There are a few commands that are absolutely essential for Rails development. The more you know them, the better it is!

At its most basic:
- `rails new` - Generate a new application
- `rails server` - Runs the server
- `rails generate` - Generate a whole heap of things within an application
- `rails console` - Opens up a console
- `rails dbconsole` - Opens up a direct connection to SQL
- `bundle` - Install gems and their dependencies

Running any command with `-h` or `--help` at the end will show you the documentation for that particular command.  But the real power comes from...

#### Customization and Automation!

##### `rails new`

```ruby
rails new app_name --skip-git --skip-turbolinks
rails new app_name -T # Skips the Test Suite
rails new app_name --database=postgresql # Specifies the Database (changes it from sqlite3)
rails new app_name -d postgresql
```

##### `rails server`

```ruby
rails server
rails s # Shorthand for rails server
rails server -p 3001 # Specifies another port (have multiple servers at once!)
rails server -e production # Changes the state of the application (different gem sets etc. - don't worry about this one)
```

##### `rails generate`

```ruby
rails generate controller ControllerName list of actions
rails generate controller Greetings index create
rails g controller Greetings index create
# THIS WILL CREATE VIEWS, JS, CSS AND ACTIONS IN CONTROLLERS (PLUS TESTS)

rails g model ModelName field:type
rails g model Painting name:string year:date
# THIS WILL CREATE MODELS, MIGRATIONS, AND TESTS

rails g scaffold ModelName field:type field:type
rails generate scaffold Painting name:string year:date
# THIS WILL CREATE EVERYTHING
```

##### `rails destroy`

If you have run a `rails generate` command and want to reverse all the changes it made, including destroying all the files it created, you can use the `rails destroy` command.

```sh
rails destroy controller Kittens
rails destroy model Kitten
rails destroy scaffold Kittens
```

You can also include the `-p` flag ("pretend") to see what files _would be deleted_ by the command without actually deleting them!

```sh
rails destroy controller Kittens -p
# remove  app/controllers/kittens_controller.rb
# invoke  erb
# remove    app/views/kittens
# invoke  test_unit
# remove    test/controllers/kittens_controller_test.rb
# invoke  helper
# remove    app/helpers/kittens_helper.rb
# invoke    test_unit
# invoke  assets
# invoke    coffee
# remove      app/assets/javascripts/kittens.coffee
# invoke    scss
# remove      app/assets/stylesheets/kittens.scss
```

##### `rails console`

```ruby
rails console # OPENS UP YOUR RAILS APP IN PRY OR IRB
rails c # SHORTHAND

rails console staging # OPENS UP A SPECIFIC ENVIRONMENT

rails console --sandbox # CAN'T MAKE ANY ACTUAL CHANGES
```

##### `rails dbconsole`


```ruby
rails dbconsole # OPENS UP A DIRECT CONNECTION TO YOUR DATABASE
rails db # SHORTHAND
```

##### `bundle`

```ruby
bundle install # INSTALLS GEM AND DEPENDENCIES
bundle # SHORTHAND
```

##### `rails`

This is crazy powerful and does a million things, but here are some of the more important ones that you might need to know.  We will go into this in a lot more detail though!

```ruby
rails --tasks # Lists everything it can do

rails about # Lists everything about your Rails app

rails db:drop # DROPS THE DATABASE
rails db:create # CREATES THE DATABASE
rails db:migrate # MIGRATES TABLES INTO THE DATABASE (FROM db/migrations)
rails db:rollback # GOES BACK ONE STEP IN THE DATABASE (BACK ONE MIGRATION)

rails routes # LIST ALL OF YOUR ROUTES

rails stats # LINES OF CODE ETC.

rails notes # SEE HERE - http://guides.rubyonrails.org/v4.2/command_line.html#notes
```

___

#### _Ruby on Rails - Rails in the Terminal - Further Readings_

- [Rails Guides - The Rails Command Line](http://guides.rubyonrails.org/v4.2/command_line.html)

___


### Ruby on Rails - Basic Project Setup (with database)

Treat this as a really rough guide, and definitely don't always follow it - you'll figure out your approach soon. This is a basic approach you might follow when setting up a new Rails project:



| Step | What                        |  Where                             | How                                                   |
|----- |---------------------------- |----------------------------------- |------------------------------------------------------ |
| 1    | Planning                    | -                                  | Pen & paper                                           |
| 2    | Create a new Rails app      | Terminal                           | `rails new kitten_party  --skip-git --skip-turbolinks`                           |
| 3    | Move into the new app's dir | Terminal                           | `cd kitten_party`                                     |
| 4    | Add development Gems        | /Gemfile                           | `gem "pry-rails"`|
| 5    | Bundle Gems                 | Terminal                           | `bundle`                                              |
| 6    | Create database             | Terminal                           | `rails db:create`                                      |
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

#### Migrations to modify a table

You can also generate migrations to modify an existing table. If you follow a very specific format in your command, Rails will often be able to generate the necessary code in the migration for you. For example, if the migration name is of the form "AddXXXToYYY" or "RemoveXXXFromYYY" and is followed by a list of column names and types then a migration containing the appropriate add_column and remove_column statements will be created.

```sh
rails generate migration AddBreedToKittens breed:string

```

This will generate a migration which can be run without editing the migration itself at all:

```ruby
class AddBreedToKittens < ActiveRecord::Migration
  def change
    add_column :kittens, :breed, :string
  end
end

```

Otherwise, you can generate a migration (with any name you like) that you need to complete yourself:

```sh
rails generate migration ChangesToKittens
```

This will generate a migration which needs to be edited before running the migration:

```ruby
class ChangesToKittens < ActiveRecord::Migration
  def change
  end
end
```

#### Bad migrations

As a general rule, you should not edit or delete a migration once it has been run. This is likely to affect the integrity of your migration history, and cause you a _lot_ of headaches down the line. If you've run a migration and want to change it, the best approach is to write another migration that corrects the issue in your schema.

If you need to delete a migration, you _can_ roll back the migration (by running `rails db:rollback` until you see that the problematic migration has been reversed), then edit/edit the migration before running `rails db:create` again. You still need to be careful that none of the subsequent migrations depended on the migration you have edited/deleted.

___
### CRUD summary

| CRUD      | DESCRIPTION                    | HTTP VERBS     | URLS                     | ActiveRecord query for controller    | NAME      | TEMPLATE            |
|---------- | ------------------------------ |-------------- |---------                  |----------                            | ----------|-------              |
| CREATE    |  Show blank form for new item  | GET           | /butterflies/new          |  -                                   | #new      | new.html.erb        |
|           |  Form submits to here          | POST          | /butterflies              | Butterfly.create                     | #create   |(redirect to index)  |
| READ      |  Show index of all items       | GET           | /butterflies              | Butterfly.all                        | #index    | index.html.erb      |
|           |  Show details for one item     | GET           |/butterflies/:id           | Butterfly.find(id)                   | #show     | show.html.erb       |
| UPDATE    |  Show pre-populated edit form  | GET           |/butterflies/:id/edit      |    Butterfly.find(id)                | #edit     | edit.html.erb       |
|           |  Form submits to here          | POST          | /butterflies/:id          | Butterfly.update                     | #update   |   (redirect to show)|
| DELETE    |  Remove item from database     | (Delete)      | /butterflies/:id          | Butterfly.destroy                    | #destroy  |  (redirect to index)|



___

#### _Ruby on Rails - Migrations - Further Readings_

- [Rails Guides - ActiveRecord Migrations](http://guides.rubyonrails.org/v4.2/active_record_migrations.html)

___

#### Homework

- DIY Rails App - Build your own Rails app from scratch, including CRUD
