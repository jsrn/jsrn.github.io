---
layout: post
title: Questionable Ruby
description: Just because you can, doesn't mean you should. Misadventures in Ruby programming.
category: programming
permalink: /questionable-ruby
updated: Jul 29, 2022
---

There's a lot you can do with Ruby's concepts of object individuation. A lot you probably shouldn't do.

## Taxation

Some objects just hoard too many methods. By applying a modest tax rate, you can reclaim memory from your most bloated objects back to the heap.

```ruby
class Object
  def tax(tax_factor = 0.2)
    meths = self.class.instance_methods(false)
    tax_liability = (meths.length * tax_factor).ceil

    tax_liability.times do
      tax_payment = meths.shuffle.shift
      instance_eval("undef #{tax_payment}")
    end
  end
end

class Citizen
  def car; end
  def house; end
  def dog; end
  def spouse; end
  def cash; end
end

c = Citizen.new
c.tax
c.house
# undefined method `house' for #<Citizen:0x00007fc342a866c0>
```

## Excitability

Write your code like you write your emails when you're trying a little too hard to come across as friendly.

```ruby
klasses = ObjectSpace.each_object(Class)
klasses.each do |klass|
  next if klass.frozen?

  klass.instance_methods.each do |meth|
    next if meth.to_s.end_with?('!')

    klass.class_eval do
      alias_method "#{meth}!".to_sym, meth
    end
  end
end

[1, 3, 2].max!
# 3
```


## Where's Wally?

A fun game to play with your friends. Hides Wally in a randomly selected method, and you get to look for him!

```ruby
klasses = ObjectSpace.each_object(Class)
klass = klasses.to_a.sample
method = klass.instance_methods.sample

klass.instance_eval do
   define_method(method) do |*args|
     puts "You found Wally!"
   end
end
```

## Finite resources

Budgeting is important.

```ruby
$availableCalls = 1000

trace = TracePoint.new(:call) do |tp|
  if tp.defined_class.ancestors.include?(Unsustainable)
    raise StandardError.new, 'No more method calls. Go outside and play.' if $availableCalls.zero?

    $availableCalls -= 1
  end
end
trace.enable

module Unsustainable; end

class EndlessGrowth
  include Unsustainable

  def grow; end
end

1001.times { EndlessGrowth.new.grow }
# No more method calls. Go outside and play. (StandardError)
```

## Ruby for anarchists

```ruby
class Object
  def self.inherited(subclass)
    return if %w[Object].include?(self.name)
    raise StandardError.new, '🏴'
  end
end

class RulingClass; end
class UnderClass < RulingClass; end # raises StandardError
```
