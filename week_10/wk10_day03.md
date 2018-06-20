## Week 10, Day 03

What we covered today:

  - 2D Graphics in the browser: P5.js
  - Rspec
  - Ruby on Rails - Testing
___

#### Codealong

- [Fruit Store](https://github.com/textchimp/wdi-27/tree/master/week10/fruitstore-tdd)

___
#### P5

p5.js is a JavaScript library that starts with the original goal of <a href = "http://processing.org">Processing</a>, to make coding accessible for artists, designers, educators, and beginners, and reinterprets this for today's web.

Using the original metaphor of a software sketchbook, p5.js has a full set of drawing functionality.

Reference docs: [here](https://p5js.org/reference/)

___


### Ruby on Rails - Testing - Rspec

- [Rspec-rails documentation](https://github.com/rspec/rspec-rails)
- [Class Demo](https://github.com/textchimp/)

We have done a little bit of Rspec now, but it has all been with pure Ruby, no Rails. We want to get it working with Rails, and luckily, that isn't difficult.

#### Set up a Rails project

Start by creating a new Rails project, using the `-T` option to tell Rails _not_ to generate tests for us.

```sh
rails new fruitstore -T --skip-git
cd fruitstore
atom .
```

Then, include the Rspec gem in the test and development group in our **GEMFILE**:

```rb
gem 'rspec-rails'
```

Then bundle the gems.

```sh
bundle
```

#### Install Rspec in the project

This still hasn't installed Rspec in our project. To do that, run the following command from Terminal:

```sh
rails generate rspec:install
```
This does a bunch of things, it generates us a spec folder (where all of our tests will live), but it also changes the behaviour for other rails generators. For example, when you generate a model, the generator will now add a test for that model. Same for controllers, etc, generated using a generator.

#### Create a model

```sh
rails generate model Fruit
rails db:migrate
```

Go back to the terminal and run `rspec` and you will see it is now connected and working.

We're going to create a single string column for the name attribute of a Fruit object.

```sh
rails generate migration add_name_to_fruits name:string
rails db:migrate
```

##### _Testing Single Table Inheritance_

The whole purpose of this project is to make it easy to upload Fruits, whether they're apples or pears. We could have multiple tables for these things, but we don't have to. We can use a thing called Single Table Inheritance (S.T.I). This is where a model (that doesn't have a table) inherits from another model (that has a database table associated with it). It looks something like the following:

```rb
# In app/models/fruit.rb
class Fruit < ActiveRecord::Base
end

# In app/models/apple.rb
class Apple < Fruit
end

# In app/models/pear.rb
class Pear < Fruit
end
```

Because Pear and Apple inherit from a Fruit, and Fruit inherits from ActiveRecord::Base, Pear and Apple also inherit from ActiveRecord::Base.

This structure of inheritance means that we can create Fruits, Apples or Pears and it still just makes a record in the Fruits table. Example:

```rb
pear = Pear.new
apple = Apple.new
fruit = Fruit.new
```

First, thought, we need to a `type` column to our fruits table.

```sh
rails generate migration add_type_to_fruits type:string
rails db:migrate
```

To test this out we could have a block in our Rspec tests.

```rb
Rspec.describe Shelf, :type => :model do
  describe "A pear" do
    before do
      @pear = Pear.create :name => "Nashi"
    end

    it "should remember the class via Single Table Inheritance (S.T.I)" do
      pear = Fruit.find( @pear.id )
      expect( pear ).to_not be_nil
      expect( pear.class ).to eq Pear
      expect( pear ).to eq @pear
      expect( pear.is_a?( Fruit ) ).to be true
      expect( pear.class.ancestors ).to include Fruit
    end
  end
end
```

And that should be all sorted! Single Table Inheritance doesn't regularly come up so if it is seeming a bit weird, don't worry about it. Neither Joel nor I have ever used it professionally. But it is cool to show how to test that sort of stuff, and it shows why type is a reserved column name in a database.

##### _Testing Associations_

One thing that you might be interested in, is testing associations in Rails. Unfortunately Rspec doesn't have something that automates this. To test associations, use a gem called [Shoulda Matchers](https://github.com/thoughtbot/shoulda-matchers).

Add the following to the test environment block in your **GEMFILE**:

```rb
gem 'shoulda-matchers'
```

Then bundle your gems:

```sh
bundle
```

Now you need to tell the gem a couple of things:

- Which test framework you're using
- Which portion of the matchers you want to use

You can supply this information by using a configuration block. Place the following in `rails_helper.rb`:

```rb
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec
    with.library :rails
  end
end
```

Now let's create a shelf table for our Fruit, and set up the associations

```sh
rails generate model Shelf
rails db:migrate
```

Now, in **app/models/shelf.rb**:

```rb
class Shelf < ActiveRecord::Base
  has_many :fruits
end
```

And, in **app/models/fruit.rb**:

```rb
class Fruit < ActiveRecord::Base
  belongs_to :shelf, :optional => true
end
```

To test these associations...

```rb
# Put this at the top of the thing describing fruits
it { should belong_to :shelf }

# Put this at the top of the thing describing shelves
it { should have_many :fruits }
```

#### Tracking test coverage

Now obviously we have started to get lots and lots of tests. We need to start keeping track of our test coverage! Let's add a gem called [Simplecov](https://github.com/colszowka/simplecov). Add this into your Gemfile (again, in the test environment) - `gem 'simplecov', :require => false, :group => :test`.

Then add, at the very top of your `rails_helper.rb` file, the following lines:

```rb
require 'simplecov'
SimpleCov.start
```

```sh
bundle install
```

This will generate everything we need! Open up the **coverage/index.html** file in the browser and you will see everything that it tests.

```sh
open coverage/index.html
```

```sh
rake stats
```

#### Testing controllers

We have only tested models so far, so lets figure out how to test controllers.
We need to add another gem for controller testing:
- [rails-controller-testing](https://github.com/rails/rails-controller-testing)

```rb
gem 'rails-controller-testing'
```

```sh
bundle
```

Let's generate a fruits controller and set up our routes:

```sh
rails generate controller Fruits
```

Now, in our **config/routes.rb** file, add restful resources for the fruits controller:

```rb
resources :fruits, only => [:index]
```

The next error that pops up will be that the action doesn't exist, so lets add an index method in the **app/controllers/fruits_controllerrb**:  

```rb
class FruitsController < ApplicationController
  def index
  end
end
```

After that, we will have a missing template (or view), so let's make that file as well.  

```sh
touch app/assets/views/fruits/index.html.erb
```
Now lets add some tests to our **fruits_controller_spec.rb** file.

```rb
RSpec.describe FruitsController, type: :controller do
  describe "GET to index" do
    before do
      3.times { |i| Fruit.create :name => "Fruit number #{ i }" }
    end

    describe "As HTML" do
      before do
        get :index # Fake browser
      end

      it "should respond with a status of 200" do
        expect( response ).to be_success
        expect( response.status ).to eq( 200 )
      end

      it "should respond with @fruits that contains all fruits in reverse order" do
        # We need to do a bit of work in the controllers for this.  Add @fruits = Fruit.all.reverse into the index method.

        # Remember that this all runs of the before block.

        # Checks existence
        expect( assigns( :fruits ) ).to be

        # Checks how many things are in @fruits
        expect( assigns( :fruits ).length ).to eq 3

        # Make sure the class is correct
        expect( assigns( :fruits ).first.class ).to eq( Fruit )

        # Validates whether it is in reverse order.
        expect( assigns( :fruits ).first.name ).to eq( "Fruit number 2" )
      end

      it "should render the index view" do
        expect( response ).to render_template( :index )
      end
    end
  end
end
```

##### _Testing JSON responses_

That will work for normal HTML, but what if we wanted to test JSON responses? Lets add some tests.

```rb
describe "As JSON" do
  before do
    get :index, :format => :json
  end

  it "should response with a status 200" do
    expect( response ).to be_success
    expect( response.status ).to eq(200)
  end

  it "should have the right content type" do
    expect( response.content_type ).to eq( "application/json" )
  end

  it "should include the fruit data in JSON format" do
    # response.body returns a string so we need to turn it into an actual object
    fruit = JSON.parse( response.body )
    expect( fruits.length ).to eq( 3 )
    expect( fruits.first['name'] ).to eq( "Fruit number 2" )
  end
end
```

In the index method in **app/controllers/fruits_controller.rb**, lets add this

```rb
def index
  @fruits = Fruit.all.reverse
  # @fruits = Fruit.order('id DESC') will actually use less memory as it doesn't need to create a duplicate version of the results and reverse it.
  respond_to do |format|
    format.html {}
    format.json { render :json => @fruits }
  end
end
```

That should work out the errors!

_A Bit more on Running Rspec_

- ` rspec ` - Run all tests
- ` rspec spec/models ` - Run all model tests
- ` rspec spec/controllers ` - Run all controller tests
- ` rspec -e "The description of the test you want to run" ` - Tell it an individual test to run.

##### Rspec and POST requests

So we are getting through all of this Rspec stuff but so far we have only done things like GET requests and checked existence of variables. Now we need to sort out validations such as POST requests and seeing how the page is actually rendered. It isn't overly difficult! This is a really basic version of it.

```rb
describe 'POST to create' do
  describe "A fruit with valid information" do
    before do
      post :create, :params => { :fruit => { :name => "Strawberry" } }
    end

    it "should redirect to the show action" do
      # pending # Don't actually run this test!
      expect( response ).to redirect_to( assigns(:fruit) ) # Show page
    end

    it "should increase the number of fruits in the database"
      # pending
      expect( Fruit.count ).to eq( 1 )
    end
  end

  describe "A fruit with invalid information" do
    before do
      post :create, :params => { :fruit => { :name => "" } }
    end

    it "should give us a 200 success status" do
      # pending
      expect( response.status ).to eq( 200 )
    end

    it "should render the new template" do
      # pending
      expect( response ).to render_template( :new )
    end

    it "should not increase the number of fruits" do
      # pending
      expect( Fruit.count ).to eq( 0 )
    end
  end
end
```

The rails scaffold generates some pretty great tests, so make sure you check that out.

```sh
rails new scaffy -T --skip-git
cd scaffy
atom .
```

Add the 'rspec-rails' gem in the gem file.

```rb
gem 'rspec-rails'
```

```sh
bundle
rails generate rspec:install
```

rails generate scaffold Post title:string content:text author:string

Scaffolding has created a bunch of test automatically. You can check the below files to see how they have been set up.

```sh
posts_routing_spec.rb
models/post_spec.rb
controllers/posts_controller_spec.rb
views/index_html_erb_spec.rb
```

Worth checking out [betterspecs](http://betterspecs.org/) as well. It's crowdsourced advice on how to write good tests.

#### Rspec terminal commands

Here are a few of the common ones:

```sh
rspec # Run all tests
rspec spec/models  # Run all model tests
rspec spec/controllers  # Run all controller tests
rspec -e "The description of the test you want to run"  # Tell Rspec to run a particular test
```

#### _Ruby on Rails - Testing - Rspec - Further Reading_

- [Rspec](http://rspec.info/)
- [Rspec Rails - Documentation](http://rspec.info/documentation/3.5/rspec-rails/)
- [Better Specs](http://betterspecs.org/)
- [Team Treehouse - An Introduction to Rspec](http://blog.teamtreehouse.com/an-introduction-to-rspec)

___

### simpleCov

##### What is it?

SimpleCov is a gem which gives you detailed feedback on test coverage. It's a good way of understanding where and why you are lacking test coverage on your project.

##### Lets get started:

In our gemfile, in the test environment:

```rb
gem 'simplecov', :require => 'false'
```
We add the `:require => false` because we want to invoke simpleCov manually.

If it runs immediately when the program starts, it will not give us valid feedback on our program's test coverage.

Next, run `bundle install` to actually get our gem installed in our project.

From here, navigate to your `spec_helper.rb` and near the top, add these lines:
```rb
  require 'simplecov'
  SimpleCov.start
```

All this says, is when we run our spec helper, we're going to need simpleCov so we can interact with it, and we immediately want to make simpleCov to begin monitoring our project.

That's it! From here all we need to do is call `rspec` (or whatever test unit you are using) in the terminal.

When we run our tests, simpleCov will create a report for us. The report is stored in a new folder within our rails project called coverage.

Running `open open coverage/index.html` should open the report for us, giving us our feedback.

If you need to know more about what has been analysed, clicking the magnifying glass icons will give you feedback on exactly what lines are not tested.

Want more? [read the docs](https://github.com/colszowka/simplecov).

____

#### Homework

- Continue with `learnyounode` and bring in questions tomorrow, to go through together
- Experiment with P5 graphics, try mapping between different values, add different physics effects, adding extra control panel sliders etc (loading images?)
- Plan your YouTeach and start writing down Final Project ideas
- If you're going to the Autopilot meetup tonight, be ready to tell us about it tomorrow!

##### Further Reading:

- [RSpec](http://rspec.info/)
- [An Introduction to RSpec](http://blog.teamtreehouse.com/an-introduction-to-rspec)
- [RSpec Best Practices](http://www.betterspecs.org/)
