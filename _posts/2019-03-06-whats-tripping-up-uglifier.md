---
layout: post
title: "What's Tripping Up Uglifier?"
description: "Finding the source of syntax errors in Rails 4's JS Uglifier"
category: programming
tags: [ruby, programming, rails]
---
In production mode, Rails 4 uses the `uglifier` gem to compress your JavaScript code into something nice and small to send over the wire. However, when you've got a syntax error, it can fail rather... ungracefully.

You get a big stack trace that tells you that you've got a syntax error. It even helpfully tells you what the error was! Unexpected punctuation. There's a comma, somewhere. Where? Who knows. It doesn't tell you.

If you have a large project, or have multiple people making large changes, it can be a little time consuming checking the diffs of recent commits to figure out where the error was introduced. Here is how you can use the rails console to at least find the offending file:

Since the uglifier doesn't run in development, let's drop into the rails console in production mode:

```console
RAILS_ENV=production rails c
```

Now we can use the following snippet of code to find our errors:

```ruby
# Take each javascript file...
Dir["app/assets/javascripts/**/*.js"].each do |file_name|
  begin
    # ... run it through the uglifier on its own...
    print "."
    Uglifier.compile(File.read(file_name))
  rescue StandardError => e
    # ... and tell us when there's a problem!
    puts "\nError compiling #{file_name}: #{e}\n"
  end
end
```

This saved me a little time recently, so I hope it will be handy to someone else.