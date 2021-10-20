---
title: Guard clauses
layout: page
description: Guard clauses are a valuable tool for keeping your methods easy to understand.
---

# Guard Clauses

Guard clauses are a valuable tool for keeping your methods easy to understand. The idea is simple: return early from a method in order to

* reduce cognitive load by eliminating nested conditional logic
* get your error handling out of the way early, leaving the main body of the method to its main purpose.


```ruby
# Before
def update_published_date
  if published_date.present?
    published_date = Time.zone.now
  end
end

# After
def update_published_date
  return if published_date.nil?

  published_date = Time.zone.now
end
```