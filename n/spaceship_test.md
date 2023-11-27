---
layout: page
title: The Spaceship Test
description: The Spaceship Test is an informal measure of code quality based around the very scientific metric "does the outline of my code look kinda like a spaceship if I squint at it?"
---

The Spaceship Test is an informal measure of code quality based around the very scientific metric "does the outline of my code look kinda like a spaceship if I squint at it?"

Here's a slightly contrived example with some deliberately messy code.

```ruby
require 'some_file'

class MyClass
  def initialize
    result = a + b
    result2 = some_method(result)

    if result2 > 100
      do_thing
      if random_criteria?
        return
      end
    else
      result2 = result2 ** 2
      result3 = some_method(result2)
      if random_criteria?
        raise StandardError
      end
    end
  end
end
```

Let's apply the Spaceship Test. Sit back and squint so you can't really make out the code itself. You're just focusing on the shape of it--specifically the whitespace on each line BEFORE the code starts.

```ruby
require 'some_file'

class MyClass
▒▒def initialize
▒▒▒▒result = a + b
▒▒▒▒result2 = some_method(result)

▒▒▒▒if result2 > 100
▒▒▒▒▒▒do_thing
▒▒▒▒▒▒if random_criteria?
▒▒▒▒▒▒▒▒return
▒▒▒▒▒▒end
▒▒▒▒else
▒▒▒▒▒▒result2 = result2 ** 2
▒▒▒▒▒▒result3 = some_method(result2)
▒▒▒▒▒▒if random_criteria?
▒▒▒▒▒▒▒▒raise StandardError
▒▒▒▒▒▒end
▒▒▒▒end
▒▒end
end
```

The general idea is that the _less_ your outline looks like the side profile of a spaceship, the better. I bet it has **short methods** and I bet those methods have **minimal conditional logic**. I bet it's really easy to understand.

```
▒▒
▒▒▒▒
▒▒▒▒

▒▒▒▒
▒▒▒▒▒▒
▒▒▒▒▒▒
▒▒▒▒▒▒▒▒
▒▒▒▒▒▒
▒▒▒▒
▒▒▒▒▒▒
▒▒▒▒▒▒
▒▒▒▒▒▒
▒▒▒▒▒▒▒▒
▒▒▒▒▒▒
▒▒▒▒
▒▒

pchooooooooo *spaceship noises*
```
