---
layout: adventure
title: "Ruby Adventure: The Ruby programming language"
---

Previous: [Introduction](/ruby-adventure)

At this point, I'm assuming you've completed the [Try Ruby](https://try.ruby-lang.org/) guide.

## Meet Ruby

> MINASWAN  \
> "Matz is nice and so we are nice."

[Ruby](https://ruby-lang.org) is a *dynamic*, *object-oriented* programming language. It was invented in the mid 1990s by Yukihiro Matsumoto, known more commonly in the community as Matz. When you're programming in Ruby, you are programming in a language that was designed with your happiness in mind. You're programming in a language with a beautiful, almost prose-like syntax and a wide and expressive standard library.

At this point you'll be passingly familiar with our various heroes. [Try Ruby](https://try.ruby-lang.org/) will have shown you the fundamentals of combining our basic types with our control flow structures, creating wonderful combinations of strings, loops, integers and if-statements.

We won't dwell on them.


## Objects and messages

I mentioned that Ruby is an *object-oriented language*. This means that rather than being built around the concepts of *data* and *functions that operate on data*, we have *objects* and *messages that we send to objects*.

The twist: in Ruby, everything is an object.

```ruby
1.is_a?(Object) # true
"am i an object?".is_a?(Object) # true
false.is_a?(Object) # true
```

Even nothing is an object.

```ruby
nil.is_a?(Object)
```

We send messages like `is_a?` to our objects, and then our objects decide what to do about them. You can think of objects as your nouns and messages as instructions or questions. You're probably wishing I'd stop yapping about theory and start yapping about making a game, so I'll ask you three questions:

1. What nouns might you expect to be in your game? `Player`, `Item` and `Monster` seem like pretty good candidates.
2. What questions might you ask your nouns? `.current_hp`? `.position`?
3. What instructions might you give them? `.die`


## Trusworthy objects

Objects in Ruby are like people. They have a the happy face that they show the world and a murky interior. The public face of an object is called its *interface*.

Like people, Ruby objects have multiple levels of secrecy: things they'll tell anyone, things they'll tell their friends, and things they'll take to the grave.

```ruby
class Wanderer
  # This method is part of the Wanderer's public interface.
  def greet; end

  # This method is protected. It's only accessible from
  # within instances of the Wanderer class or any
  # subclasses of Wanderer.
  protected
  def tell_story; end

  # This method is private. It's only accessible from *this*
  # instance.
  private
  def tell_secret; end
end
```

To create a robust and maintainable system, it is best to ensure that the public interfaces of your objects are carefully curated. A good rule of thumb is that your public interface should be stable, but the private interface on which it depends can be more fluid. If your public interface changes as infrequently as possible, and you only ever depend on the public interface, change is unlikely to cause widespread breakage.

> The road to maintenance nirvana is paved with classes that depend on things that change less often than they do.
> ~ Sandi Metz, Practical Object-Oriented Design In Ruby

Next: [Structuring an application](/ruby-adventure/structuring-an-application)