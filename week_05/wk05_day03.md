## Week 05, Day 03

What we covered today:

- Ruby on Rails - Associations

____

#### Warmup

- [Atbash Cipher - Ruby](https://https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week05/day03_atbash_cipher)

____
#### Classwork

- [MoMA](https://github.com/textchimp/wdi-27/tree/master/week5/moma-app)

____

### Ruby on Rails - Associations

Associations allow you to create relationships between any two models (that inherit from ActiveRecord::Base). By declaratively telling Rails that two models have a certain association with each other, we can greatly streamline our code.

Rails supports six types of associations:


- `belongs_to`
- `has_many`
- `has_one`
- `has_many :through`
- `has_one :through`
- `has_and_belongs_to_many`

The two simplest associations are `belongs_to` and `has_many`.

#### `belongs_to`

A belongs_to association sets up a one-to-one connection with another model, such that each instance of the declaring model "belongs to" one instance of the other model. For example, if your application includes customers and orders, and each order can be assigned to exactly one customer, you'd declare the order model this way:

```ruby
  class Order < ActiveRecord::Base
    belongs_to :customer, optional :true
  end

```

A few things to note about `belongs_to` associations:
- The singularized form of the 'other' model (`customer`) _must_ be used for the association to work.
- The `order` table must also include the `customer_id` for the customer records to which it belongs.
- A `belongs_to` association often has a reciprocal `has_one` or `has_many` association on the other model.

#### `has_many`

A has_many association indicates a one-to-many connection with another model. You'll often find this association on the "other side" of a belongs_to association. This association indicates that each instance of the model has zero or more instances of another model. For example, in an application containing customers and orders, the customer model could be declared like this:

```ruby
class Customer < ActiveRecord::Base
  has_many :orders
end
```

A few things to note about `has_many` associations:
- The pluralized form of the 'other' model (`orders`) _must_ be used for the association to work.
- A `has_many` association often has a reciprocal `belongs_to` association on the other model.
- The model with the `has_many` association doesn't store the 'foreign key' of the related model. That goes on the model with the `belongs_to` association.


#### `has_one`

A `has_one` association also sets up a one-to-one connection with another model, but with somewhat different semantics (and consequences). This association indicates that each instance of a model contains or possesses one instance of another model.

```ruby
class User < ActiveRecord::Base
  has_one :account
end
```

#### `has_many :through`

A `has_many :through` association is often used to set up a many-to-many connection with another model. This association indicates that the declaring model can be matched with zero or more instances of another model by proceeding through a third model.

```ruby
class Doctor < ActiveRecord::Base
  has_many :appointments
  has_many :patients, through: :appointments
end

class Appointment < ActiveRecord::Base
  belongs_to :doctor
  belongs_to :patient
end

class Patient < ActiveRecord::Base
  has_many :appointments
  has_many :doctors, through: :appointments
end
```

#### `has_one :through`

A `has_one :through` association sets up a one-to-one connection with another model. This association indicates that the declaring model can be matched with one instance of another model by proceeding through a third model.

```ruby
class Supplier < ActiveRecord::Base
  has_one :account
  has_one :account_history, through: :account
end

class Account < ActiveRecord::Base
  belongs_to :supplier
  has_one :account_history
end

class AccountHistory < ActiveRecord::Base
  belongs_to :account
end
```

#### `has_and_belongs_to_many`

A has_and_belongs_to_many association creates a direct many-to-many connection with another model, with no intervening model. This association is often abbreviated to 'HABTM'.

```ruby
class Assembly < ActiveRecord::Base
  has_and_belongs_to_many :parts
end

class Part < ActiveRecord::Base
  has_and_belongs_to_many :assemblies
end
```
___

#### _Ruby on Rails - Associations_

- [Rails Guides - Association Basics](http://guides.rubyonrails.org/v4.2/association_basics.html)

___

#### Homework
- DIY Rails App - Build your own Rails app from scratch, including CRUD (with 2 models, and working associations)
- Read:
  + [Rails Guides - Active Record Migrations](http://guides.rubyonrails.org/v4.2/active_record_migrations.html)
  + [Rails Guides - Active Record Associations](http://guides.rubyonrails.org/v4.2/association_basics.html)
