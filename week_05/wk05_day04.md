## Week 05, Day 04

What we covered today:

- PostgreSQL - Install
- Ruby on Rails - [Associations cont'd](week_05/wk05_day03.md)
- Ruby on Rails - User login:
  - Ruby on Rails - Authentication
  - Ruby on Rails - Authorisation
  - Ruby on Rails - Sessions
  - Ruby on Rails - Validations

____
#### Warmup

- [Roman Numerals](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week05/day04_roman-numerals)

____
____
#### Classwork

- [Tunr](https://github.com/textchimp/wdi-27/tree/master/week5/tunr-wdi27)

____


### PostgreSQL - Install

#### Installing PostgreSQL

  1. [Download Postgres](http://www.postgresapp.com).
  2. Drag **Postgres** from your Downloads folder to your Applications folder.
  2. Configure your $PATH to allow access to Postgres CLI tools
    1. Open your bash profile.
  ```sh
  atom ~/.bash_profile
  ```
    2. Add this line to your bash profile.
  ```sh
  export PATH=$PATH:/Applications/Postgres.app/Contents/Versions/latest/bin
  ```
  3. Open an new window in your terminal. This will allow the change to take effect.
  4. Install the Postgres Gem - the Ruby interface for PostgreSQL
  ```sh
  gem install pg
  ```
  5. Confirm that Postgres has been installed and your $PATH configured properly:
  ```sh
  postgres #This should return:
  # => postgres does not know where to find the server configuration file.
  # => You must specify the --config-file or -D invocation option or set the PGDATA environment variable.
  ```
  6. Open Postgres - type 'Postgres' into Spotlight Search and open it up. Close the dialog box that opens up - you should then see the silhouette of an elephant in your Mac's menu bar.

  #### Using Postgres

  Generally, you should use PostgreSQL for your Rails applications - it is far more scalable, pretty much just as easy to set up as any other database and works on Heroku (where we will be hosting our applications during the course).  To start a Rails application with a postgreSQL database, run:

  ```sh
  rails new app_name --database=postgresql
  ```
  or simply
  ```sh
  rails new app_name -d postgresql
  ```

___

### Associations cont'd
[See yesterday's notes](week_05/wk05_day03.md)
___


### Authentication

#### Authentication v Authorization

- **Authentication** is the process of determining who a user is - it is about identity.
- **Authorization** is the process of declaring what a user is allowed to do - it is about permissions.

The term 'authentication' is often loosely applied to the end-to-end process of signing-up, logging-in and signing-out a user.

#### Authentication in Rails

Authentication is rough, and rarely (if ever) will you be asked to build your own authentication system.  There are plenty of gems that will do that sort of thing, but if you want to do it for yourself - there are a bunch of things you need to do.

First things first - we don't store passwords in plain text, we store it in something called a `password_digest`. This is an encrypted password.

Let's create our User model!!

#### User model and users database table

```ruby
# Create our User model and the migration for creating the users table in the database.
rails generate model User name:string password_digest:string age:integer email:string

# Run the migration generated to create the users table in the database.
rails db:migrate
```

#### Password encryption

Now, we need to use our Gemfile to get something that does the password encryption. We normally use [bcrypt](https://github.com/codahale/bcrypt-ruby) for this. Let's add it!

```ruby
gem 'bcrypt'
```

In our User model, we need to add the following line:

```ruby
has_secure_password
```

Including this method in our user model adds methods to set and authenticate encrypted passwords. In order for the `has_secure_password` method to work, your database table _must_ have a `password_digest` column.

`has_secure_password` adds the following validations to your model:

- A password must be present when a User object is created;
- A password must be <= 72 characters long;
- The password and password_confirmation\* must match.

\* The `password == password_confirmation` validation check will only be triggered if a password_confirmation field is present. For forms that do not require this validation (eg, sign-in), just leave the password_confirmation field out altogether.

#### Forms

Even though the users table in our database has a `password_digest` column, any forms relating to sign-up or sign-in _still_ need to use `password` and `password_confirmation` parameters. A user doesn't enter their `password_digest` directly - Rails knows that the model "has secure password", and when bcrypt sees a `password` and `password_confirmation`, it knows what to do with it.

#### Interlude - HTTP 'state'

Now we can authenticate a user based on their credentials, including an encrypted password, but before we get into authorization (what a user can _do_, having been _authenticated_), we need a way to be able to remember who an authenticated user is.

HTTP is stateless, so either the browser or the application (or both) need to 'remember' who a user is. To do this, we're going to store their identity in something called the **session hash**\*. Without this, a user would need to actively prove their identity every single time they send a request to the server.

\*The session hash is not actually a hash, it just behaves like one.

### Authorisation

Now that we have set up the authentication system, let's work on authorisation - specifying what actions we will allow users to perform.

Let's say we want most actions in our application to be restricted to authenticated users - users who have signed in using valid credentials, and whose user_id is now stored in the sessions hash (thanks to our `session#create` action).

Since this is not going to be limited to a particular controller, we'll put this code in our application_controller, from which all other controllers inherit. So, in app/controllers/application_controller.rb:

```ruby
class ApplicationController < ActionController::Base

  # Before any action is performed, call the fetch_user method.
  before_action :fetch_user

  private

  def fetch_user
    # Search for a user by their user id if we can find one in the session hash.
    if session[:user_id].present?
      @current_user = User.find_by :id => session[:user_id]

      # Clear out the session user_id if no user is found.
      session[:user_id] = nil unless @current_user
    end
  end


  def authorize_user
    redirect_to root_path unless @current_user.present?
  end
end
```

Now we have a `@current_user` variable which will be available whenever a session includes a `user_id`. We can use the presence of this variable to perform simple authorisation tasks.

We also have an `authorise_user` method, which will redirect a user to the root_path if that `@current_user` variable is not present. We probably don't want to create a `before_action` for this method in our application controller, since there are going to be a number of actions we want unauthenticated users to be able to do, like access the homepage, sign-up, sign-in, etc.

Instead, we'll call that method on a controller-by-controller basis.

```ruby
class UsersController < ApplicationController
  before_action :authorise_user, :except => [:index]
end
```

The `:except` method allows us to limit the scope of this authorisation method to all actions within the Users controller _except_ a particular action or actions - in this case, the index action. This means that unauthenticated users will still be able to view the Users index.

We can extend this pattern to other types of authorisation. For example, if our users table has an `admin` column with the data type `boolean`, we could restrict particular actions to authenticated admin users. For example:

```ruby
class AdminController < ApplicationController
  before_action :authorise_admin

  def burn_all_bridges
    [User, Song, Mixtape, Artist, Genre, Album].each do |table|
      table.destroy_all
    end
  end

  private
  def authorise_admin
    redirect_to root_path unless @current_user.present? && @current_user.admin
  end
end

```


#### Sessions

When a user visits a Rails site (or rather, when a client sends a request to a Rails server), the application checks whether the client includes a cookie containing a valid session ID for the application. If none exists (ie, if this is a new user, or they are not logged in), the app will create a new session on the application side, and store that in an encrypted cookie on the client-side.

Two parts of the session that are pivotal to authentication and authorization are the **session hash** and **flash hash**:

##### _Session hash_

The session hash:

- Is a collection of key/value pairs;
- Includes a session_id. This ID is:
  + A 32 character string of random characters;
  + Included in every cookie sent from the application to the client's browser;
  + Sent to the server with every request from the client;
- Is unique to each user (and unique to a particular user's 'session');
- Is private between the user and the server.

We can store a (small) bunch of information in the session hash, including an authenticated user's `user_id` - this is the key to effective authorization in Rails.

##### _Flash hash_

The flash hash is a special part of the session that is cleared after each HTTP request - this means that values stored there will only be available in the next request, which is useful for passing error messages etc (eg, if a user is not authorized to access a particular action).

The flash hash:

- Is a collection of key/value pairs;
- Has arbitrary keys, but the convention is to use:
  - `flash[:notice]` or `flash[:success]` for storing success messages;
  - `flash[:error]` for storing error messages.
- Will be available for:
  1. the current request; and
  2. the subsequent request (unless `flash.now` is used)

  before it is destroyed (unless `flash.keep` is used).

A few helpful tips for dealing with the flash hash:
- If you want to set a flash that will only be available for the current action - and _not_ be available to the subsequent action - use the `flash.now` method.
- If you want to keep a flash for more than one controller action, use the `flash.keep` method in the action which should carry the flash on to the next action.

Example usage:

```rb
def create
  user = User.create(user_params)
  if @user.save
    flash[:notice] = 'User was successfully created.'
    redirect_to user_path(user)
  else
    flash.now[:error] = 'Could not create user.'
    render 'new'
  end
end
```

Below is a pattern for storing a user's `user_id` to their session hash when they sign-in, and to track this for authorization within the application.

##### _Create a session controller_

Our session controller has no associated model and only requires three actions - new, create and destroy.

```sh
rails generate controller Session new create destroy
```

Note: This will create two views that we don't need. Delete those.

```sh
rm app/views/session/create.html.erb
rm app/views/session/destroy.html.erb
```

Note: This will also have create several routes that we don't want. Delete the following routes from your config/routes.rb file:

```rb
get 'session/new'
get 'session/create'
get 'session/destroy'.
```

##### _Setup login routes_

Add the following routes to your config/routes.rb file:

```rb
root :to => 'pages#home'              # Replace this with whatever you want your root_path to be.
                                      # This path is where unauthorized users will be redirected_to.
get '/login' => 'session#new'         # This will be our sign-in page.
post '/login' => 'session#create'     # This will be the path to which the sign-in form is posted
delete '/login' => 'session#destroy'  # This will be the path users use to log-out.
```

##### _Setup session controller actions_

We want to use our session controller to handle login and logout. If a user logs in with a valid username & password combination, their user_id will be stored in the session hash. If a user logs out, the user_id will be removed from the session hash.

In your app/controllers/session_controller.rb file:

```rb
class SessionController < ApplicationController


  def new
  # This is the action for user login. The view will have the login form template.
  end

  def create
  # This is the action to which the login form post request is posted. It will add the user's id to the session hash, which will be used for authentication and authorization throughout the session.
    user = User.find_by :email => params[:email]
    if user.present? && user.authenticate(params[:password])
      # If a user record with the entered in the form is present AND the user is authenticated (using bcrypt's authenticate method and the password entered in the form), store their id in the session hash and redirect them to the root path.
      session[:user_id] = user.id
      flash[:notice] = "User created!"
      redirect_to root_path
    else
      # If the user cannot be authenticated, redirect them to the login_path.
      flash[:error] = "User could not be created!"
      redirect_to login_path
    end
  end

  # This is the action to which the user sign-out delete request is posted.
  def destroy
    session[:user_id] = nil
    redirect_to root_path
  end
end
```

##### _Add sign-up / log-in / sign-out links to our layout_

You'll probably won't to wrap this in some conditional logic once you have your `fetch_user` method set up (see Authorization, below), but these links in your app/views/layouts/application.html.erb layout will allow users to sign-up, log in and sign-out:

```html
<ul>
  <li><%= link_to("Sign Up", new_user_path %>) %></li>
  <li><%= link_to("Sign In", login_path %>) %></li>
  <li><%= link_to("Log Out", login_path, :method => :delete %>) %></li>
</ul>
```
___

#### _Authentication - Recommended Readings_

- [The Buckner Life - Simple Authentication with Bcrypt](https://gist.github.com/thebucknerlife/10090014) - this is a great, step-by-step Bcrypt tutorial.
- [The Odin Project - Sessions, Cookies and Authentication](http://www.theodinproject.com/ruby-on-rails/sessions-cookies-and-authentication)
- [Rails Guides - Security Guide](http://guides.rubyonrails.org/security.html)

### Validations

#### Interlude - front-end v back-end validations

Validations in a full-stack web app basically fall into two categories - front-end validations and back-end validations. This part of the GitBook only deals with the latter. Front-end validations are client-side validations which prevent users from performing certain actions in a form _before_ anything is posted back to the application - they submit a user from entering certain information in a form, or submitting a form with/out certain attributes, like putting alphabetic characters in a number field, or exceeding a character limit for a text field, or leaving a required field blank.

Front-end validations are great for user experience and performance, but they're vulnerable to circumvention - we usually also want back-end validations.

#### Active Record validations

As with virtually every problem in the universe, Active Record has a solution.  Instead of trying to create our own validations for data that is getting submitted to a database, lets see what solutions Active Record has.

These validations go within the model against which the validation needs to be performed (for example, validations relating to the User model should go in app/models/user.rb).

##### _Why should we use validations?_

Because the internet is a scary, scary world and there are scary, scary things that people will try and put in your database.

##### _When do validations take place?_

Active Record Validations occur when any of the following methods take place:

- create
- create!
- save
- save!
- update
- update!

If you want to skip validations, they won't take place when using the following methods.

- decrement!
- decrement_counter
- increment!
- increment_counter
- toggle!
- touch
- update_all
- update_attribute
- update_column
- update_columns
- update_counters

You can also pass ` :validate => false ` to the save method.

##### _How to test validity?_

There are methods!

- valid?
- invalid?

If a model is invalid, you can access the errors easily as well.  These will be in the ` .errors.messages `.

##### _Alright, enough of this.  Now for some Validation Helpers_

```rb
class User < ActiveRecord::Base
  has_many :books
  validates :column_name, :key => value
end
```

###### acceptance

This method validates that a checkbox on the user interface was checked when a form was submitted.  This is how terms of service are accepted for example.

```rb
validates :terms_of_service, acceptance: true

# It defaults to "1", if you want to accept something else

validates :terms_of_service, acceptance = { :accept => "yes" }
```

###### validates_associated

If you have associations, you can validate those associations by using validate associated.

```rb
validates_associated :column_name
```

Don't do this on both sides of the associations! It will cause an infinite loop.

###### confirmation

You should use this helper if you have two text fields that should receive exactly the same content.  This validation creates a virtual attribute whose name is the name of the field but with "\_confirmation" at the end.

```rb
validates :email, :confirmation => true

# This should always work with a nil check as well

validates :email_confirmation, :presence => true
```

###### exclusion

This helper validates that the attributes' values are not included in a given set.  It requires an :in option that receives the set of values that cannot be included, as well as an optional :message option to show how you can include the attribute's value.

```rb
validates :subdomain, :exclusion => {
  :in => [ "www", "us", "ca", "jp" ],
  :message => "%{ value } is reserved. "
}

# Notice the interpolation with value, that takes the excluded value
```

###### format

This helper validates the attributes' values by testing whether they match a given regular expression, which is specified using the :with option.

```rb
validates :legacy_code, format: {
  :with => /\A[a-zA-Z]+\z/,
  :message => "only allows letters"
}
```

Alternatively, you can require that the specified attribute does not match the regular expression by using the :without option.

###### length

This helper validates the length of the attributes' values. It provides a variety of options, so you can specify length constraints in different ways:

```rb
validates :name, length: { minimum: 2 }
validates :bio, length: { maximum: 500 }
validates :password, length: { in: 6..20 }
validates :registration_number, length: { is: 6 }
```

###### numericality

This helper works with numericality.

```rb
validates :count, numericality: { :only_integer => true }
```

Besides `:only_integer`, you can also work with the following:

- `:greater_than` - Specifies the value must be greater than the supplied value. The default error message for this option is "must be greater than %{count}".
- `:greater_than_or_equal_to` - Specifies the value must be greater than or equal to the supplied value. The default error message for this option is "must be greater than or equal to %{count}".
- `:equal_to` - Specifies the value must be equal to the supplied value. The default error message for this option is "must be equal to %{count}".
- `:less_than` - Specifies the value must be less than the supplied value. The default error message for this option is "must be less than %{count}".
- `:less_than_or_equal_to` - Specifies the value must be less than or equal the supplied value. The default error message for this option is "must be less than or equal to %{count}".
- `:odd` - Specifies the value must be an odd number if set to true. The default error message for this option is "must be odd".
- `:even` - Specifies the value must be an even number if set to true. The default error message for this option is "must be even".

###### presence

Checks whether the value of a particular set of attributes is nil or an empty string.

```rb
validates :email, :presence => true
```

###### absence

Does the opposite or `:presence`, makes sure it is not there.

```rb
validates :email, :absence => true
```

###### uniqueness

Makes sure there is only one of these columns with this particular value.

```rb
validates :email, uniqueness: true
```

##### _Working with Validation Errors_

Taking an invalid person as example...

```rb
person.valid?
# Returns false

person.errors
 # Returns an instance of the class ActiveModel::Errors containing all errors. Each key is the attribute name and the value is an array of strings with all errors.

person.errors.messages
# Returns all of the messages associated
```
___

#### _Ruby on Rails - Validations - Further Readings_

- [Rails Guides - Active Record Validations](http://guides.rubyonrails.org/v4.2/active_record_validations.html)

___
