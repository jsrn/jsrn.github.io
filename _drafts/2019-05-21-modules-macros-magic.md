---
layout: post
title: "Modules, Macros, and Magic"
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

1) We have a table of comments.
2) We have a table of articles.
3) Each comment is related to an article by some ID.

Rails users will take for granted that if they have an instance of the Comment class, they will be able to execute `some_comment.article` to obtain the article that the comment is related to.

This post will give you an extremely simplified look at how something like Rails' ActiveRecord relations can be achieved. First, some groundwork.

# Modules

Modules in Ruby can be used to extend the behaviour of a class, and there are three ways in which they can do this: `include`, `prepend`, and `extend`. The difference between the three? Where they fall in the method lookup chain.

```ruby
class Car
  prepend Wheels
  include Engine
  extend Spoiler
end
```

In the above example:

* Methods from `Wheels` will be created as *instance methods* and *override* instance methods from `Car`.
* Methods from `Engine` will be created as *instance methods* but *not override* methods from `Car`.
* Methods from `Spoiler` will be added as *class methods* on `Car`.

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

  belongs_to :jim
end
```

In the above code, we're just defining a module and a class. No instance of the class is ever created. However, when we execute _just this code_ in an IRB session, you will see "I belong to jim!" as the output. Why? Because the code we write while defining a class is executed as that class definition is being interpreted.

What if we rew-rite our module to define a method using Ruby's [define_method](https://apidock.com/ruby/Module/define_method)?

```ruby
module Ownable
  def belongs_to(owner)
    define_method(owner.to_sym) do
      puts self.object_id
    end
  end
end

my_item = Item.new
my_item.jim
# some_jim
```

Excellent.

