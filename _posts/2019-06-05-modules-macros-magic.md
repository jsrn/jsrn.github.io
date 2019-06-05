---
layout: post
title: "Modules, Macros, Metaprogramming and Magic"
description: "Implementing simplified ActiveRecord relations using macros."
category: programming
tags: [ruby, rails, modules, macros, relations]
---

If you've used the [Rails](https://rubyonrails.org/) framework, you will probably recognise this:

```ruby
class Comment < ApplicationRecord
  belongs_to :article
end
```

This snippet of code implies three things:

1. We have a table of comments.
2. We have a table of articles.
3. Each comment is related to an article by some ID.

Rails users will take for granted that if they have an instance of the Comment class, they will be able to execute `some_comment.article` to obtain the article that the comment is related to.

This post will give you an extremely simplified look at how something like Rails' ActiveRecord relations can be achieved. First, some groundwork.

# Modules

Modules in Ruby can be used to extend the behaviour of a class, and there are three ways in which they can do this: `include`, `prepend`, and `extend`. The difference between the three? Where they fall in the method lookup chain.

```ruby
class MyClass
  prepend PrependingModule
  include IncludingModule
  extend ExtendingModule
end
```

In the above example:

* Methods from `PrependingModule` will be created as **instance methods** and **override** instance methods from `MyClass`.
* Methods from `IncludingModule` will be created as **instance methods** but **not override** methods from `MyClass`.
* Methods from `ExtendingModule` will be added as **class methods** on `MyClass`.

We can do fun things with `extend`.

# Executing Code During Interpretation Time

```ruby
module Ownable
  def belongs_to(owner)
    puts "I belong to #{owner}!"
  end
end

class Item
  extend Ownable
  belongs_to :overlord
end
```

In the above code, we're just defining a module and a class. No instance of the class is ever created. However, when we execute _just this code_ in an IRB session, you will see "I belong to overlord!" as the output. Why? Because the code we write while defining a class is executed as that class definition is being interpreted.

What if we re-write our module to define a method using Ruby's [define_method](https://apidock.com/ruby/Module/define_method)?

```ruby
module Ownable
  def belongs_to(owner)
    define_method(owner.to_sym) do
      puts self.object_id
    end
  end
end
```

Whatever we passed as the argument to `belongs_to` will become a method on instances of our `Item` class.

```ruby
our_item = Item.first
our_item.overlord
#  => 70368441222580
```

Excellent. You may have heard this term before, but this is "metaprogramming". Writing code that writes code. You just metaprogrammed.

# Tying It Together

You might also notice that we're getting closer to the behaviour that we would expect from Rails.

So let's say we have our `Item` class, and we're making a videogame, so we're going to say that our item belongs to a player.

```ruby
class Item
  extend Ownable
  belongs_to :player
end
```

Our Rails-like system could make some assumptions about this.

1. There is a table in the database called `players`.
2. There is a column in our `items` table called `player_id`.
3. The player model is represented by the class `Player`.

Let's return to our module and tweak it based on these assumptions.

```ruby
module Ownable
  def belongs_to(owner)
    define_method(owner.to_sym) do
      # We need to get `Player` out of `:player`
      klass = owner.to_s.capitalize.constantize
      # We need to turn `:player` into `:player_id`
      foreign_key = "#{owner}_id".to_sym
      # We need to execute the actual query
      klass.find_by(id: self.send(foreign_key))
      # SELECT * FROM players WHERE id = :player_id LIMIT 1
    end
  end
end

class Item
  extend Ownable
  belongs_to :player
end

my_item = Item.first
my_item.player
# SELECT * FROM players WHERE id = 1 LIMIT 1
# => #<Player id: 12>
```

Neat.
