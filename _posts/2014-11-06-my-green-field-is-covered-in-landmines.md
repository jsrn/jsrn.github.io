---
layout: post
title: "My Green Field is Covered in Landmines"
description: ""
category: programming
tags: [rails, activerecord, database, legacy issues, legacy database]
---

The best thing about starting a project, apart from (hopefully) getting to make something awesome, is the lack of constraint. All the new tips and tricks you've picked up, all of the skills honed during your last project, can now be applied to a beautifully clean blank canvas, and the result, surely, will be a masterpiece.

That's how I felt when I started my current project. It's a Ruby on Rails application that we're building to supplement and eventually replace the company's existing desktop software. We're moving into the future!

_Nope._

As it happens, the data from the web application has to be compatible with the desktop application. The desktop application is large and powerful, but it been designed, modified, grown and evolved over some 15 to 20 years. You can imagine what the database looks like to someone who wasn't there to experience and understand the history of it all.

And you can't make sweeping changes or refactor easily without breaking compatibility with the desktop version.

_Yeah._

In the transition period, until the web application is strong enough to stand on its own, this is the database that we need to work with. As a result, I've picked up a thing or two about strapping a rails application onto a database that was never designed with the wonders of arel and Active Record in mind.

### Override Your Table Names

Rails does an amazing thing where it will infer the name of your table from the name of your model. This is great when you get to pick the name of your table, but when you _can't_, there's no reason to be stuck with a silly model name to make everything work. You can manually set the table name for a given model like so:

{% highlight ruby %}
class Widget < ActiveRecord::Base
  self.table_name = "box_o_widgets"
end
{% endhighlight %}

### Aliases Are Your Friend

It's possible _(or even likely)_ that over such a long time, columns are bound to be created with... less than ideal names. Maybe the current usage of the column no longer matches its original purpose, or maybe it's just plain badly named. I get good mileage out of using the `alias_attribute` method in my models to map a new name to the old and ugly column names.

{% highlight ruby %}
class Widget < ActiveRecord::Base
  alias_attribute :nice_name, :whatDoesTHISeven_do
end
{% endhighlight %}

This has some pros and cons, and I'm still weighing up which side I come down on. The main advantage, clearly, is that it makes almost all of the code that uses that attribute of that model cleaner. The column name is easier to remember, and it's easier to keep your mental model of what attributes an object has when their names are short and descriptive.

The second advantage is that it can help you plan out your new schema for when that glorious day comes that you can move the data from the old database into the new one. And since, for the most part, you've been using the new column name all along, you should **hopefully** just be able to delete the alias, and everything will work as it used to.

Here's the downside. I said **hopefully** because **not every ActiveRecord method respects aliased attributes**. There are some unfortunate cases where when you use the alias, rails will throw its hands up at you, shrug, and say the column doesn't exist in the database. However, it's easy enough to check your model for the true column name in these cases, and it still cuts down on the amount of variable replacement you'll be doing come Switchover Day.

### Handle Your Defaults

Okay, great! I have my sensibly named model, and my sensibly named columns. Life is going well, and it's time to start adding data to the database.

Oh no, everything has died.

It turns out that the original database actually has very little in the way of default values for columns, and very lots in the way of falling over when certain columns are `null`. Since there is little we can currently do to modify the old database, we have to find an alternative solution. `before_create` to the rescue.

`before_create` is called on an ActiveRecord object before `.save`, if the object doesn't yet correspond to a row on the database. I think this is the optimal time to take care of the unfortunate business of default values, as **a)** it's only called once and **b)** it can be tucked away in a callback out of the way of your logic. When it's no longer needed, the callback can just be deleted.

This is what we're looking at in full:

{% highlight ruby %}
class Widget < ActiveRecord::Base
  self.table_name = "box_o_widgets"
  alias_attribute :nice_name, :whatDoesTHISeven_do
  before_create :set_defaults

  def set_defaults
    self.nice_name ||= ""
    # We have to be careful with booleans, since ||= will trigger
    # even if the value has already explicitly been set to false
    self.some_bool ||= true unless self.some_bool == false
  end
end
{% endhighlight %}

It's far from ideal working with an old, change resistant database, but these little things have eased the pain for me immeasurably.
