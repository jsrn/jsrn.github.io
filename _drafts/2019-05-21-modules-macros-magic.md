---
layout: post
title: "Modules, Macros, and Magic"
description: "A look at how relations are implemented."
category: programming
tags: [ruby, rails, modules, macros, relations]
---

If you've used the [Rails](https://rubyonrails.org/) framework, you will probably recognise this:

```ruby
class Comment < ApplicationRecord
  belongs_to :article
end
```

What we have here is a `Comment` class which extends `ApplicationRecord`. This represents an ActiveRecord model, and it is the backbone of many applications.

We declare that our Comment class belongs to an instance of the Article class, and we take for granted that within our application we will be able to execute

```ruby
comment = Comment.first
comment.article
```

and get the article that this comment belongs to.

In this article, we will look at how you can implement an **extremely simplified** version of a Rails relation. First, some groundwork.

# Modules

Modules in Ruby can be used to extend the behaviour of a class, and there are three ways in which they can do this: `include`, `prepend`, and `extend`. The difference between the three? Where they fall in the method lookup chain.

```ruby
module Audible
  def speak
    puts "Hi! I'm talking to you from within the module."
  end
end

class JamesIncludes
  include Audible

  def speak
    puts "Hi! I'm talking to you from within the class."
  end
end

JamesIncludes.new.speak
# Hi! I'm talking to you from within the class.

class JamesPrepends
  prepend Audible

  def speak
    puts "Hi! I'm talking to you from within the class."
  end
end

JamesPrepends.new.speak
# Hi! I'm talking to you from within the module.

class JamesExtends
  extend Audible

  def speak
    puts "Hi! I'm talking to you from within the class."
  end
end

JamesExtends.new.speak
# Hi! I'm talking to you from within the class.
```



