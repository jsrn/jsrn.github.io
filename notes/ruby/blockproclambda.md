---
layout: notes
---

# Procs, Blocks and Lambdas

This gets asked a lot, and I always always _always_ struggle to remember it.

## Blocks

A block is an anonymous function that can be passed into a method.

```ruby
def use_block
  yield "Now this is pod racing!"
end

use_block do |text|
  puts text
end
```

## Procs

A proc is a stored block.

```ruby
my_proc = Proc.new { |name| puts "Hi, #{name}!" }
```

You now have a proc object that you can treat as any other, storing it or passing it from object to object until such time as you want to call it.

```ruby
my_proc.call("Steve")
```

## Lambdas

A lambda also lets you define a block and its parameters for later use.

```ruby
my_lambda = ->(name) { puts "Hi, #{name}!" }
```

A lambda is a lot like a proc, except:

**It is picky about the number of arguments.**

```ruby
my_proc = Proc.new { |name| puts "Hi, #{name}!" }
my_lambda = ->(name) { puts "Hi, #{name}!" }

my_proc.call
# Hi, !
my_lambda.call
# ArgumentError (wrong number of arguments (given 0, expected 1))
```


**They return differently.**

A lambda has its own return value.

```ruby
my_lambda = ->(number) { return number * 2 }
puts my_lambda.call(5)
# 10
```

A proc will attempt to return from the current context.

```ruby
class ProcDemo
  def five_or_ten?
    my_proc = Proc.new { return 5 }
    answer = my_proc.call
    answer * 2
  end
end

ProcDemo.new.five_or_ten?
# 5
```
