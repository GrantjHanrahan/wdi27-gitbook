## Week 04, Day 01

What we covered today:
- Ruby - Installation and RVM
- Ruby - A Brief History of Ruby
- Ruby - Introduction
    + Data Types
      - Strings and Numbers
    + Operators
    + Variables
    + Methods
    + Ruby - Fundamentals - Pt I
    + Conditionals
    + Control Structures
- Ruby - Methods

- [Warmup - TextSoup2 -  JS](https://github.com/GrantjHanrahan/wdi27-homework/tree/master/warmups/week04/day01_textsoup2)

___

#### Slides

- [Ruby - Introduction](https://github.com/textchimp/wdi-27/raw/master/week4/introduction-to-ruby.pdf)

___


### Ruby - Installation and RVM

- [RVM and Ruby Installation Guide](https://gist.github.com/ga-wolf/98718aa5c63d8323ae46)

#### How to get Ruby

Install Developer Tools from Xcode (this should have happened for most of you):

` xcode-select --install `

Install HomeBrew:

` /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" `

Access a specific URL using a secured line and run the downloaded program:

` curl -sSL https://get.rvm.io | bash -s stable `

Restart the terminal, and try running the command:

` rvm `.

If it doesn't work...

- Open the bash_profile up in Atom

  ` atom ~/.bash_profile `

- Add these lines into the bottom of the .bash_profile and save it

  ` [[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" `

  ` export PATH="$PATH:$HOME/.rvm/bin" `

Restart the terminal and run the following commands to make sure it's all cool:

` rvm `

` rvm list known `

` rvm get stable --auto `

Go here and find the most recent version - https://www.ruby-lang.org/en/downloads/

Latest version at the time of writing was 2.4.0, swap those in for the latest stable version that shows on that website

` rvm install ruby-2.4.0 `

` rvm --default use 2.4.0 `

Let's test that it has all worked.

- ` ruby -v `
- ` rvm -v `
- ` which ruby ` - should not return anything in the /usr/local/bin
- ` which rvm `

If all of this has worked, run...

` gem install pry `

#### Common Commands

`ruby -v` - Will return the current version of Ruby

`which ruby` - What is the path to the version of Ruby you are using

`ruby hello_world.rb` - Runs the hello_world.rb file

`irb` - Runs a ruby console

`pry` - Runs a better console

`<CTRL> + D` - Ends a file running in irb or ruby

### Ruby - A Brief History of Ruby

Created in 1993 by Yukihiro Matsumoto (Matz).  He knew Perl and he also knew Python.  He thought that Perl smelt like a toy language apparently, and he disliked Python because it wasn't a true object-oriented programming language.  Ruby was primarily influenced by Perl and SmallTalk though.

Matz wanted a language that:

- Syntactically Simple
- Truly Object-Oriented
- Had Iterators and Closures
- Exception Handling
- Garbage Collection
- Portable

#### Programming Mottos

- Perl - There's More Than One Way To Do It (T.M.T.O.W.T.D.I)

- Python - There Should Be One And Only One Way To Do It

- Ruby - Designed For Programmer Happiness

___

#### _Further Reading - A Brief History of Ruby_

- [SitePoint - History of Ruby](http://www.sitepoint.com/history-ruby/)

___

### Ruby - Introduction

#### Data Types - Strings and Numbers

##### _Strings_

Again, delimited by quotes.

`"string"`
`'string'`

##### _Numbers_

There are multiple types:

- FixNum
- Float
- BigNum

#### Operators

##### _Arithmetic Operators_

Lots of them, but the basic ones are:

```ruby
+     # Addition
-     # Subtraction
*     # Multiplication
/     # Division
**    # Exponent (to the power of)
%     # Modulus
```

##### _Assignment Operators_


```ruby
=     # Assignment
+=    # Add then assign
-=    # Subtract then assign
*=    # Multiply then assign
/=    # Divide then assign
%=    # Modulus then assign - assigns the remainder of the modules to the left operand
```

##### _Logical Operators_

```ruby
&&    # And
and   # And (alternative to &&)
||    # Or
or    # Or (alternative to ||)
!     # Not
not   # Not (alternative to !)

```

##### _Comparison Operators_

All the usual suspects, plus a few new ones:

```ruby
>       # Greater than
>=      # Greater than or equal to
<       # Less than
<=      # Less than or equal to
==      # Equality (generally just use the double equals in Ruby)
!=      # Inequality
<=>     # Combined comparison - returns -1 if less than, 0 if equal, and 1 if greater than)
.eql?   # True if the receiver and argument have the same type and equal values
.equal? # True if the receiver and the argument have the same object ID

```

___

#### _Ruby - Operators - Recommended Readings_
- [Tutorials Point - Ruby Operators](https://www.tutorialspoint.com/ruby/ruby_operators.htm)

___

#### Variables

Unlike JavaScript, we don't need to use the `let` or `const` (or any other) keyword when declaring a variable in Ruby.

`ruby = "is nice"`

It's much harder to accidentally make global variables in Ruby.

`$ruby = "is nice"`

To make a `const` variable you must define the variable name ALL in caps.

`RUBY = "is nice"`

Ruby will allow you to change this constant variable but will notify you if you change it.

#### Methods (Functions in JS)

`puts "this is like console.log`
`print "this is also like console.log`
`p "this is a bit more complex`

Parentheses in method calls are mostly optional in Ruby, but they are occasionally necessary (eg, in method or function chaining)

```ruby
puts("this")
puts "is the same as this"

```

Methods in Ruby have an **implicit return**, meaning that you don't need to use the `return` keyword in your methods - Ruby returns the last statement in a method on its own.

### Ruby Fundamentals - Pt I

##### _Basic naming conventions_

snake_case_everywhere - it's very rare to see camelCase!

##### _Variable interpolation in strings_

String interpolation is where we put Ruby code to be evaluated inside a string - the code to be evaluated is inserted within `#{}`. This is most often used when interpolating variables, but we can put any expression within the curly brackets (eg `#{5 + 4}` will interpolate `9` into the string).

```ruby
name  = "gilberto"
drink = "scotch"

"My name is #{ name } and I drink #{ drink }!"
# A lot nicer than "my name is " + name + " and I drink " + drink
# Which is the way you would do it in JS
```

IMPORTANT: Interpolation only works with double quotes!!  Single quotes mean 'leave this string alone, this is mine'.

##### _Comments in Ruby_

```ruby
# This is is a single line comment

# This is
# a multiline
# comment

# OR (don't do this)

=begin
This is also a multi line comment
You can't have any an empty line between the =begin and the start of the comment
=end
```

##### _Getting user input_

In JavaScript, we have `alert` and `prompt`, in Ruby we have `puts` and `gets`.

```ruby
# Initial greeting
puts "What is your first name?"

# first_name = gets
# This will wait for user input, and include the new line in the variable

first_name = gets.chomp
# This will wait for user input, and strip the new line from the variable
# For more documentation on chomp - http://ruby-doc.org/core-2.2.0/String.html#method-i-chomp

puts "Your first name is #{ first_name }."

p "What is your surname? " # using `p` will allow you to put the input on the same line.
surname = gets.chomp # Chaining
puts "Your surname is #{ surname }."

puts "Your full name is #{ first_name } #{ surname }"
# fullname = "#{ first_name } #{ surname }"
# Sames as ... puts "Your full name is #{ fullname }"

puts "What is your address?"
address = gets.chomp
puts "Your name is #{ fullname } and you live at #{ address }"

# INTERPOLATION ONLY WORKS IN DOUBLE QUOTES!
```

#### Conditionals

##### _`If` statements_

```ruby
if 13 > 10
    p "Yep, it is a bigger number"
end

grade = "A"

if grade == 'A'
    puts "You are killing it"
elsif grade == 'B'
    puts "You are coasting fine"
elsif grade == 'C'
    puts "Acceptable"
else
    puts "Please see me after class"
end

p "Yep, it is a bigger number" if 13 > 10 # This only works in single line statements

# It's called a modifier (if modifier)
```

##### _`Unless` statements_

```ruby
x = 1
unless x > 2
    puts "x is less than 2"
else
    puts "x is greater than 2"
end

# Modifier or backwards form
code_to_perform unless conditional
```

##### _`case` statements_

Think of these as shorter if statements, but don't overuse them (particularly in JS)

```ruby
grade = 'B'
case grade
when 'A'
    p 'You are killing it'
when 'B'
    p 'You are coasting fine'
when 'C'
    p 'Acceptable'
else
    p 'Please see me after class'
end

case expression_one
when expression_two, expression_three
    statement_one
when expression_four, expression_five
    statement_two
else
    statement_three
end

# Very similar to the switch statement in JavaScript!
```

The case statement will compare the **case expression** with whatever a particular **when expression** evaluates to. This is fine when testing direct equality between the case expression and the when expressions. Example:

```rb
score = 1
case score
when 1
  print "You got one"
when 2
  print "You got two"
end
# => "You got one"
```

The first `when` expression -  `1` - evaluates to `1`, and since `1 == 1` evaluates to `true`, the first `when` expression is satisfied, and "You got one" is printed

But this is a problem when our `when` expression contain is more complex. Consider the following:

```rb
score = 1
case score
when score <= 1
  print "You got one or less"
when score > 1
  print "You got two or more"
end
# => nil
```

The first `when` expression -  `score <= 1` - evaluates to `true`, and since `1 != true`, so none of the `when` expressions will be satisfied.

For complex expressions, the case expression should be left blank.
```rb
score = 1
case
when score <= 1
  print "You got one or less"
when score > 1
  print "You got two or more"
end
# => "You got two or more"
```

___

#### _Ruby - Conditionals - Exercises_

- [Drinking Age, Air Conditioning, Sydney Suburbs](https://gist.github.com/textchimp/0065f9596b90cd977b003594b9b7834b)

- [Drinking Age - Solution](https://github.com/textchimp/wdi-27/blob/master/week4/ruby-intro/drinking-age.rb)
- [Air Conditioning - Solution](https://github.com/textchimp/wdi-27/blob/master/week4/ruby-intro/air-conditioning.rb)
- [Sydney Suburbs - Solution](https://github.com/textchimp/wdi-27/blob/master/week4/ruby-intro/suburbs.rb)

___

#### Control Structures

##### _While Loops_

```ruby
while conditonal
    statement
end

while true
    p "OMG"
end # BAD IDEA

i = 0
while i < 5
    puts "i: #{ i }"
    i += 1
end
```

##### _Until Loops_

```ruby
until conditional
    statement
end

i = 0
until i == 5
    puts "i: #{ i }"
    i += 1
end
```

##### _Iterators_

So, so common in Ruby.

```ruby
5.times do
    puts "OMG"
end
# => "OMG
# => "OMG
# => "OMG
# => "OMG
# => "OMG

5.times do |i|
    puts "i: #{ i }"
end
# => I: 0
# => I: 1
# => I: 2
# => I: 3
# => I: 4
# => I: 5


5.downto(0) do |i|
    puts "i: #{ i }"
end
# => I: 5
# => I: 4
# => I: 3
# => I: 2
# => I: 1
# => I: 0
```


##### _`for` loops_

**For loops** are very rarely used in Ruby.

```ruby
# Don't ever use them
for i in 0..5
   puts "i: #{ i }"
end
# => I: 0
# => I: 1
# => I: 2
# => I: 3
# => I: 4
# => I: 5
```

##### _Generating random numbers_

```ruby
Random.rand # Generates a number between 0 and 1
Random.rand(10) # Generates a random number up to 10 (including zero and 10)
Random.rand(5..10) # Generates a number between 5 and 10 (also includes them)
Random.rand(5...10) # Does not include 5 and 10
```
___

#### _Ruby - Control Structures - Exercise_

- [Guess The Number](https://gist.github.com/textchimp/391d14a59080a8b35c4262ec113d2f7e)

- [Solution](https://github.com/textchimp/wdi-27/blob/master/week4/ruby-intro/guesser.rb)

___

### Methods

Methods are declared using the `def` keyword and called using the method's name.

#### Methods with no arguments

```ruby
# Method definition
def hello
    puts "Hello"
end

# Method call
hello
# => "Hello"
```

#### Methods with arguments

For methods with defined parameters, arguments can be passed in with or without parentheses.

```ruby
def hello( name )
    puts "Hello, #{name}"
end

hello("Grant")
# => "Hello, Grant"

hello "Grant"
# => "Hello, Grant"
```

If a method is defined with one or more parameters but is called without passing in the right number of arguments, an error will be thrown (`Argument Error: wrong number of arguments`).

#### Methods with default arguments

To avoid `Argument Error`s thrown by calling an argument without the requisite number of arguments, we can set default parameters in the method definition.

```ruby
def hello( name = "World" )
  puts "Hello, #{name}"
end

hello
# => "Hello, World"

hello("Grant")
# => "Hello, Josh"

3.times { hello("Grant") }
# => "Hello, Grant"
# => "Hello, Grant"
# => "Hello, Grant"

def add(a, b)
  return a + b
end

add 4, 11
# => 15

new_total = add 4, 11
new_total
# => 15

```
___


#### _Ruby (General) - Recommended Readings_

- [Why's (Poignant) Guide To Ruby](http://poignant.guide/)
- [The Bastard's Book of Ruby](http://ruby.bastardsbook.com/)

___

### Homework

- [Calculator](https://gist.github.com/wofockham/2752aa06121df7f3024c)
- Also:
  + Watch [Pry - Introductory Screencast](http://pryrepl.org/)
  + Browse [Ruby - Documentation](https://www.ruby-lang.org/en/documentation/)
  + Work through some of [Ruby Monk](https://rubymonk.com/)
