---
layout: post
title: "Finding A Method Declaration With Source Location"
description: "I recently had one of those moments where some code failed, but not in the way I expected it to."
category: programming
tags: [programming, ruby, debugging]
---

I recently had one of those moments where some code failed, but not in the way I expected it to. To create a contrived example, the scenario was basically this:

```ruby
class LocationsController < ApplicationController
  def update
    my_location = Location.find(params[:id])
    do_something_with(location)
  end
end
```

This code won't do what I want it to because I have defined the variable `my_location` but then passed `location` to another method. What I initially expected upon finding the bug is that I would have seen a `NameError` complaining that the local variable `location` didn't exist. But I didn't. As far as I could tell, `location` existed and it was `nil`.

It wasn't referenced anywhere else in `LocationsController`, nor in `ApplicationController`. So what is it? Fortunately, Ruby provides a great way to find out.

```ruby
class LocationsController < ApplicationController
  def update
    puts method(:location).source_location
    # "/usr/local/bundle/gems/actionpack-5.0.7.2/lib/action_controller/metal.rb"
    # 149
  end
end
```

That's that mystery solved. There is a method called `location` implemented a layer or two up the inheritance hierarchy. We knew it had to come from _somewhere_, but `source_location` lets us determine the exact location.
