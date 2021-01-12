---
layout: post
title: "Assertions of Truth in Ruby Tests"
description: Comparing two styles of boolean assertion in Ruby tests.
category: programming
tags: [ruby, testing, programming]
---

I used to write my assertions like this:

```ruby
assert user.active?
refute user.inactive?
```

Then I joined a team where I was encouraged to write this:

```ruby
assert_equal true, user.active?
assert_equal false, user.inactive?
```

_What?_ That doesn't look very nice. That's doesn't feel very _Ruby_. It's less elegant!

Here's the thing, though: your unit tests aren't about being elegant. They're about guaranteeing correctness. You open the door to some insidious bugs when you test truthiness instead of truth.

* You might perform an assignment rather than comparing equality.

```ruby
def active?
  # Should be status == :active
  status = :active
end
```

* You might check the presence of an array, rather than its length.

```ruby
def has_users?
  # Should be user_list.any?
  user_list
end

def user_list
  []
end
```

Over time, I've gotten used to it. This style still chafes, but not as bugs caused by returning the wrong value from a predicate method.
