---
layout: post
title: "The Surprising Case of `gets` in Ruby"
description: gets is often used in introductory Ruby tutorials, but they rarely tell the whole story.
category: programming
tags: [ruby, gets, programming]
---

`gets` is seen is basically every introductory Ruby tutorial, but they rarely tell the whole story.

You'll be told to write something like this:

```ruby
#!/usr/bin/env ruby

puts "What is your name?"
your_name = gets.chomp
puts "Hi, #{your_name}!"
```

Confusingly, if this is in a script that takes additional command line arguments, you may not see "Hi, Janet!"

If you execute `./gets.rb 123` you will pretty quickly be greeted by the following error:

```
./gets.rb:4:in `gets': No such file or directory @ rb_sysopen - 123 (Errno::ENOENT)
```

The tutorial didn't warn you about _this_. The tutorial is giving you a reduced view of things that may, if you're like me, leave you scratching your head several years later.

`gets` doesn't just read user input from $stdin. `gets` refers to [Kernel#gets](https://ruby-doc.org/core-2.7.0/Kernel.html#method-i-gets), and it behaves like so:

> Returns (and assigns to $_) the next line from the list of files in ARGV (or $*), or from standard input if no files are present on the command line.

If you really, truly want to prompt the user for their input, you can call `gets` on $stdin directly. And who wouldn't, with a user like you?

```ruby
your_name = $stdin.gets.chomp
```
