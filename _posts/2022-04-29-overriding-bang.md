---
layout: post
title: "Overriding the unary bang"
description: When (not) to take advantage of some of Ruby's flexibility
category: programming
tags: [ruby]
---

Reading [The Well Grounded Rubyist](https://www.manning.com/books/the-well-grounded-rubyist-third-edition), I was reminded that among the operators you can override on an object is the unary bang.

This is the operator that you would normally use to find the logical negation of an object. Since Ruby gives us the concepts of _truthiness_ and _falsiness_, this means that the logical negation of all values other than `nil` or `false` is `false`.

The result is that people often (inadvisably) use `!` as a presence check, e.g.

```ruby
book = Book.find_by(title: 'The Well Grounded Rubyist')
handle_missing_book if !book
```

In this case, overriding the unary bang for an object seems like a potential [footgun](https://en.wiktionary.org/wiki/footgun). What if we override this method like so?

```ruby
class Thing
  def !
    return truthy_value
  end
end
```

We end up with the following scenario:

```ruby
my_thing = Thing.new

# Executes. Our object isn't bil or false, so we expected this.
do_thing if my_thing

# Doesn't execute. Still going to plan.
do_thing unless my_thing

# Both execute. What?!
do_thing if !my_thing
do_thing if not my_thing
```

This is confusing, and I immediately take two things from it:

1. Don't rely on truthiness to check presence, perform an explicit call to `#nil?` (or `#blank?` or `#present?` if you're using Rails.)
2. You shouldn't override the unary bang method.

I'll stand by the first one, but surely there must be exceptions to the second. It seems like such a bad idea that I really _want_ there to be exceptions.

Please let me know if you've got any.