---
layout: adventure
title: "Ruby Adventure: Structuring an application"
---

Previous: [The Ruby programming lanuage](/ruby-adventure/the-ruby-programming-language)

## Structuring an application

One goal of this guide is to help new programmers bridge the gap between knowing how to write classes and methods in isolation to knowing how to structure a full application. We have all of these nouns, now where do we put them? We could put everything in a file called `game.rb` and it would work, but is that the kind of world you want to live in? A world where you have to scroll up and down through thousands of lines of code to change the number of hit points a potion heals? No thank you.

We're going to need to learn how to include code from one file to another.

```ruby
# lib/player.rb
class Player
  def preen
    puts "The Player flips their hair and looks " \
      "at their reflection in a pool of water. " \
      "Did they always look this good?"
    end
  end
end

# game.rb
require "lib/player"

Player.new.preen
```

## A few words on SOLID

Many object-oriented practitioners advocate for the SOLID principles, which I will summarise here.

### Single object responsibility

A given object should only be responsible for one thing.

### Open / closed

Classes should be open for extension, but closed for modification.

### Liskov substitution

Any object should be replacable by an instance of their subclasses without affecting the correctness of the program.

### Interface segregation

A client should not be forced to implement an interface that it doesn't use.

### Dependency inversion

Depend on abstractions, not on concretions. This is a simple affair in Ruby thanks to its dynamic typing, and can be achieved simply by applying a few techniques.

Look at the following example.

```ruby
class Hamster
  def feed
    do_something_with(HamsterFood.new)
  end
end
```

In this case, Hamster depends on HamsterFood. If HamsterFood changes, we may have to change Hamster. If want to feed Hamster something other than HamsterFood, we definitely have to change Hamster. We can avoid this conundrum by injecting our dependencies.

```ruby
class Hamster
  def feed(food)
    do_something_with(food)
  end
end
```

Hamster no longer depends on the concrete. It relies on the programmer to supply an object that can be treated like HamsterFood. Perhaps any subclass of Food!

```ruby
hamster = Hamster.new
[Hamburger, Tomato, Cheese, Spaghetti, Burrito].each do |foodstuff|
  hamster.feed(foodstuff.new)
end
```

That's one full hamster. More importantly, it's a hamster that doesn't have to change no matter what we want to feed it, so long as that food obeys some basic rules about what it entails to be Food.

Next: [Our first game](/ruby-adventure/our-first-game)