---
layout: tcd
id: "0008"
title: Ruby patterns
authored: "2026-04-10"
updated:  "XXXX-XX-XX"
---

"Homo sapiens is about pattern recognition, he says. Both a gift and a trap."  \
~ Pattern Recognition, William Gibson

- [General patterns](#general-patterns)
  - [Collecting parameter](#collecting-parameter)
  - [Null Object](#null-object)
  - [Pluggable Object](#pluggable-object)
  - [Pluggable Selector](#pluggable-selector)
- [Behavioural patterns](#behavioural-patterns)
  - [Command](#command)
  - [Double Dispatch](#double-dispatch)
  - [State](#state)
  - [Template Method](#template-method)
- [Structural patterns](#structural-patterns)
  - [Adapter](#adapter)
  - [Composite](#composite)
- [Testing patterns](#testing-patterns)
  - [Crash Test Dummy](#crash-test-dummy)
  - [Self Shunt](#self-shunt)

## General patterns

### Collecting parameter

#### What?

A mutable value that we pass from method to method, accumulating data as it goes.

#### Why?

Allows us to refactor a bulky method in which the results of multiple operations are collected in a single variable. This can be handy if we are sure that our method is violating the Single Responsibility Principle.

```ruby
class ScienceExperiment
  def collect_results
    results = []

    if test_validity_of_outliers?
      detected_outliers.each do |outlier|
        results << outlier if valid_outlier?(outlier)
      end
    end

    results << result_from_follow_up_check

    control_group.each do |control_group_result|
      results << reformat_control_group_result(control_group_result)
    end

    results
  end
end
```

#### How?

The above code is pretty messy. Let's apply the Collecting Parameter pattern.

```ruby
class ScienceExperimentWithCollectingParameter
  def collect_results
    results = []
    collect_valid_outliers(results)
    collect_results_from_follow_up_check(results)
    collect_formatted_control_group_results(results)
    results
  end

  private

  def collect_valid_outliers(results)
    # ... extracted code
  end

  def collect_results_from_follow_up_check(results)
    # ... extracted code
  end

  def collect_formatted_control_group_results(results)
    # ... extracted code
  end
end
```

### Null Object

#### What?

Replace conditional checks for `nil` with an object that returns a sensible default.

#### Why?

To tidy up conditional logic in our code.

Imagine you're building a message board. Messages are posted by registered users, but if a user should delete their account, the message is preserved and displayed without any identifying information.

Our view code may be scattered with helper methods like the following:

```ruby
def author_username(post)
  if post.author.present?
    post.author.username
  else
    'Deleted user'
  end
end
```

The more attributes of the user that we're displaying in the page, the more conditional statements we have checking for the existence of said user.

#### How?

How might we describe a user that no longer exists? We would like them to return some sensible data about what such a user might be, and if we ask them anything else that we might ask of an existing user, we at least want the system to _not crash_.

```ruby
class NullUser
  def username
    'Deleted user'
  end

  def method_missing(*)
    nil
  end
end
```

And what if our instances of `Post` had an author, even when they didn't?

```ruby
class Post
  def author
    self[:author] || NullUser.new
  end
end
```

Now the logic towards our view level becomes considerably more simple.

```ruby
def author_username(post)
  post.author.username
end
```

In fact, our helper method is now so simple that there is a strong argument for removing it entirely and calling our username method directly from the view.

```erb
<div class="post">
  <span class="author"><%= @post.author.username %></span>
</div>
```

### Pluggable Object

#### What?

Store a reference to an object which implements a known interface and make references to it throughout the rest of the object.

#### Why?

We would like to reduce the amount of conditional logic in the following code, which controls movement for a person who is either driving or walking. It's messy as it is, and if we add a third mode of transport, we're really going to be suffering.

```ruby
class Driver
  def start
    @driving = driving?
  
    if @driving
      press_accelerator
    else
      pedal
    end
  end

  def steer
    if @driving
      turn_wheel
    else
      turn_handlebars
    end
  end

  def stop
    if @driving
      release_accelerator
    else
      stop_pedaling
    end
  end
end
```

#### How?

```ruby
class Driver
  def start
    @mode = if driving?
              DrivingMode.new
            else
              CyclingMode.new
            end

    @mode.start
  end

  def steer
    @mode.steer
  end

  def stop
    @mode.stop
  end
end
```

### Pluggable Selector

#### What?

Replace subclasses with dynamically generated method calls.

#### Why?

If we have a large number of subclasses that each implement a single method, the overhead involved in maintaining the code can outweigh the benefit of separating them.

```ruby
class Person
  def greeting
    raise NotImplementedError
  end
end

class EnglishSpeaker < Person
  def greeting
    'Hello!'
  end
end

class FrenchSpeaker < Person
  def greeting
    'Bonjour!'
  end
end
```

#### How?

By dynamically constructing our method call based on the desired type, we limit the area in which our changes must take place and the system is easier to maintain and comprehend.

```ruby
class Person
  def greet(language)
    send "#{language}_greeting"
  end

  def english_greeting
    'Hello!'
  end

  def french_greeting
    'Bonjour!'
  end
end
```

## Behavioural patterns

### Command

#### What?

The Command Pattern allows us to group operations in an object which can be passed around and invoked on demand.

#### Why?

Useful for cases where we wish to pass an action around our system, store it, defer it, or queue it.

#### How?

We start with a **Receiver**. This is one of our business objects. We'll use a `User` in this example, and it is the object on which our command is going to operate.

```ruby
class User
  def send_welcome_email
    # ...
  end

  def apply_introductory_discount
    # ...
  end
end
```

The `Command` class is the lowest level building block of the pattern.

```ruby
class Command
  # This is to convey our intent that subclasses should implement this method.
  def execute
    raise NotImplementedError
  end
end
```

And now we can implement `GreetUserCommand`, which we can pass on or store.

```ruby
class GreetUserCommand < Command
  def initialize(user)
    @user = user
  end

  def execute
    @user.send_welcome_email
    @user.apply_introductory_discount
  end
end
```

### Double Dispatch

#### What?

To properly understand Double Dispatch, it'll be useful to understand what is meant by Single Dispatch.

You are probably used to looking at code such as the following.

```ruby
user.validate_password(password)
```

In the above code, the specific method that ends up being executed depends on the runtime type of a single object: `user`.

This is known as _single dispatch_.

In a _multiple dispatch_ system, the implementation of `validate_password` that ends up being invoked would depend on the runtime types of both `user` **and** `password`.

When working with languages that don't directly support double dispatch (like Ruby), we can implement the Double Dispatch Pattern to gain the benefits.

#### Why?

The Double Dispatch Pattern is useful for scenarios where you have a set of classes, any one of which may have to interact with another. Imagine we're building a document display system where a number of objects may be rendered inside a number of media.

```ruby
class LineGraph
  def render(medium)
    case medium
    when WordDoc
      # ...
    when Website
      # ...
    when Pdf
      # ...
    end
  end
end

class BarChart
  def render(medium)
    case medium
    when WordDoc
      # ...
    when Website
      # ...
    when Pdf
      # ...
    end
  end
end
```

The problem with the above is twofold:

1. Each object must know the rendering internals of multiple media.
2. If we add a new medium, all of our objects must be updated to account for it.

Applying the Double Dispatch Pattern allows us to bring the rendering logic out of our objects and in to a more sensible place, as well as reducing the number of classes that must be edited if we add a new medium in which our objects can be displayed.

#### How?

```ruby
# Our objects
class LineGraph
  def render(medium)
    medium.render_line_graph(self)
  end
end

class BarChart
  def render(medium)
    medium.render_bar_chart(self)
  end
end

# Our display media
class WordDoc
  def render_line_graph(line_graph)
    # ...
  end

  def render_bar_chart(bar_chart)
    # ...
  end
end

class Website
  def render_line_graph(line_graph)
    # ...
  end

  def render_bar_chart(bar_chart)
    # ...
  end
end
```

### State

If you have an object whose behaviour depends on one of several states, you may end up with several very similar conditionals sprinkled throughout the class.

The State Pattern provides a mechanism for removing this duplicate conditional code by maintaining a reference to an object referencing the current state and delegating state-specific behaviour to that object.

```ruby
class TCPConnection
  attr_accessor :state

  def initialize
    @state = TCPClosed.instance
  end

  def open
    @state.open(self)
  end

  def close
    @state.close(self)
  end
end

class TCPClosed
  def open(connection)
    connection.state = TCPOpen.instance
  end

  def close(connection); end
end

class TCPOpen
  def open(connection); end

  def close(connection)
    connection.state = TCPClosed.instance
  end
end
```

### Template Method

#### What?

A Template Method is a method defined purely in terms of the methods that it expects a subclass to implement.

#### Why?

The Template Method Pattern allows a developer to define the overall structure of an operation in a parent class while providing control over the specific implementation to subclasses.

#### How?

```ruby
class BaseTest
  # This is our template method. We don't necessarily expect set_up, run_test,
  # or tear_down to be implemented in the BaseTest class.
  def execute
    set_up
    run_test
    tear_down
  end
end

class OurTest < BaseTest
  def set_up
    # our implementation
  end

  def run_test
    # our implementation
  end

  def tear_down
    # our implementation
  end
end
```

## Structural patterns

### Adapter

You may be called upon to use an object in such a way that defies the expectations of either the object in question or the system you are developing. There are a number of reasons this might be the case:

- The object's interface differs greatly from that of other, similar objects in your system.
- The object's interface is way more complicated than you have any need for.
- I want to limit the scope for required changes if I replace this object with a different one in the future.

You can solve this by creating an _adapter_ that provides a more agreeable interface between the object in question and the rest of the system.

Let's take for example a system that concerns itself with the accurate measurement of animals.

```ruby
class Slug
  def length_in_millis
    37
  end
end

class Antelope
  def length_in_millis
    2400
  end
end
```

As luck would have it, some friendly American programmers/naturalists have written their own package for their own system that concerns itself with the accurate measurement of animals. They've kindly given us access to it so we can make use of it in our application.

```ruby
class GrizzlyBear
  def length_in_inches
    78.7
  end
end
```

_Oh._ They've given us access to their animal classes, but it's all in Imperial. Well, there are a couple of things we can do here. We can create an adapter class that allows us to decorate an animal with the desired interface:

```ruby
class AnimalAdapter
  MILLIMETRES_IN_AN_INCH = 25

  def initialize(animal)
    @animal = animal
  end

  def length_in_millis
    @animal.length_in_inches * MILLIMETRES_IN_AN_INCH
  end
end

bear = GrizzlyBear.new
adapted_bear = AnimalAdapter.new(bear)
adapted_bear.length_in_millis
#=> 1967.5
```

We can extend the class directly to layer our desired interface over existing functionality:

```ruby
class AdaptedBear < GrizzlyBear
  MILLIMETRES_IN_AN_INCH = 25

  def length_in_millis
    length_in_inches * MILLIMETRES_IN_AN_INCH
  end
end

bear = AdaptedBear.new
bear.length_in_millis
#=> 1967.5
```

### Composite

#### What?

Once applied, the Composite Pattern allows you to operate on objects of different types within a tree structure using a single interface.

```
         Folder
        /      \
   Folder      Folder
  /     \      /    \
File   File  File   File
```

#### Why?

By using a single interface to operate on objects of different types, you can simplify your client code by making it type agnostic.

In the above diagram, you might wish to ask any object its `size` without worrying about whether that size is directly returned from a File object or recursively calculated from the contents of a folder.

#### How?

Firstly, we need to define a common interface that will be implemented by all members of the tree structure, whether they are primitives (Files) or containers (folders). Ideally, this interface should implement as many of the common operations as possible, minimising the amount of behaviour that will need to be implemented by subclasses.

```ruby
class Component
  def size
    raise NotImplementedError.new, "#size is not implemented on #{self.class}"
  end

  def children
    []
  end

  def add(child); end

  def remove(child); end

  def get_child(index)
    nil
  end
end
```

We can then implement the behaviour in our composite Folder class. Specifically, we implement the behaviour responsible for operating on the list of the object's direct children, as well as the behaviour that delegates the calculation of the folder's size to that list of children.

```ruby
class Folder
  def initialize
    @children = []
  end

  def size
    children.collect(&:size).sum
  end

  def children
    @children
  end

  def add(child)
    @children << child
  end

  def remove(child)
    @children.delete(child)
  end

  def get_child(index)
    children[index]
  end
end
```

Finally, we implement the operation on our leaf File class.

```ruby
class File
  def size
    size_on_disk
  end
end
```

Our client might then use the code like:

```ruby
documents = Folder.new
documents.add(File.new)
documents.add(File.new)

baking_recipes = Folder.new
baking_recipes.add(File.new)

vegetable_recipes = Folder.new
vegetable_recipes.add(File.new)
vegetable_recipes.add(File.new)

all_recipes = Folder.new
all_recipes.add(baking_recipes)
all_recipes.add(vegetable_recipes)

file_system = Folder.new
file_system.add(documents)
file_system.add(all_recipes)

# This creates the following structure:
#
# file_system
# |- documents
#    |- file
#    |- file
# |- all_recipes
#    |- baking_recipes
#    |  |- file
#    |- vegetable_recipes
#       |- file
#       |- file
```

Our client can now call `size` on any object in the above structure without regard for the object's type.

## Testing patterns

### Crash Test Dummy

#### What?

A Crash Test Dummy is an object that we have created to fail in a specific — and spectacular — way.

#### Why?

We're our own error handling code around a third party API, file system, or similar with well defined but difficult to reproduce failure modes. How do you force someone elses' API to return a given error code, for example, or test that your application gracefully handles a full hard disk without filling the hard disk with trash data?

#### How?

We can make a crash test dummy by mocking or stubbing the object that does the actual system interaction and forcing it to raise a certain exception. This allows us to test the error handling in our code without performing extreme acts.

```ruby
require 'net/http'

# Our API client. It performs HTTP interactions, it (usually) works, but it
# doesn't do any error handling of its own.
class MyApiClient
  def read_from_endpoint
    :some_good_stuff_returned_from_the_api
  end
end

# The class under test. This class consumes data from the API using the
# API client, and we want to ensure that our rescue block behaves as
# expected.
class ApiConsumer
  def pull_data
    response = MyApiClient.new.read_from_endpoint
  rescue Net::ReadTimeout
    response = :read_timeout_failure
  ensure
    response
  end
end

RSpec.describe ApiConsumer do
  it 'handles read timeouts from the api' do
    # We're using a stubbed method to implement our crash test dummy.
    expect_any_instance_of(MyApiClient).to receive(:read_from_endpoint)
                                             .and_raise(Net::ReadTimeout)

    consumer = ApiConsumer.new
    expect(consumer.pull_data).to eq(:read_timeout_failure)
  end
end
```

### Self Shunt

The Self Shunt Pattern allows us to turn our test class itself into the mock object we pass to our object under test.

#### Why?

If we are testing object A and want to ensure that it performs the appropriate actions on object B, we might construct a mock B to pass to A.

We can reduce the complexity of our test and the number of objects required by ensuring that our test itself implements the interface we would expect of B, and then pass `self` to A.

#### How?

```ruby
# The class that we're trying to test. We want to ensure that when passed an
# object which adheres to the loggable interface, the `write_to_log` method
# adds a log line to the logger object.
class SomeClass
  def write_to_log(logger, log_line)
    logger.add(log_line) 
  end
end

# This defines the behaviour of the logger object, extracted to a module for
# convenience. In a real example, we might not be so lucky to have everything
# wrapped into a little bundle for us, but hey.
module Loggable
  attr_reader :lines

  def add(log_line)
    @lines ||= []
    @lines << log_line
  end
end

class Logger
  include Loggable  
end


RSpec.describe SomeClass do
  include Loggable

  it 'uses the self shunt pattern to make an object under test interact directly with the test class' do
    object_under_test = SomeClass.new
    object_under_test.write_to_log(self, 'this_is_a_test')
    expect(lines.include?('this_is_a_test')).to be true 
  end
end
```
