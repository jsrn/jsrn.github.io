---
layout: post
title: "Saving time with custom Rails console commands"
description: Save time and mental strain by defining custom Rails console methods for common tasks
category: programming
tags: [ruby, rails]
---

If you're like me, you probably spend a lot of time in the Rails console interrogating objects and fiddling with data to debug your application. As I work on a project, I end up with a growing list of useful snippets to lightly modify and paste into the console to complete common tasks.

A [tweet](https://twitter.com/websebdev/status/1451897969276604424?s=20) by [Sebastian Auriault](https://twitter.com/websebdev) showed me that you can define methods that will be directly accessible in your Rails console without necessarily affecting your production code.

# How?

Depending on where you want these methods to be available, you can add the following code to `config/initializers/console_methods.rb` or to `lib/console_methods.rb` and require the file in the appropriate environment(s).

```ruby
module Rails::ConsoleMethods
  # Grab your favourite test user without having
  # having to remember or type much about them
  def test_user
    User.find_by(email: 'my_test_user@example.com')
  end
  
  # Reset your test user back to some baseline
  def reset_test_user
    test_user.update(
      status: :pending,
      registration_confirmed_at: nil,
      current_balance: 0
    )
  end
  
  # Quickly benchmark some code
  def bm(&block)
    puts "Running a quick benchmark:"
    puts Benchmark.measure(&block)
  end
  
  # Clear all of your queued Sidekiq jobs
  def nuke_sidekiq
    Sidekiq::Queue.new.clear
  end
end
```

# Why?

None of these examples are breathtaking. Why bother at all?

1. **Sharing knowledge.** A shared, well-documented set of custom commands allows the entire team to take advantage of these little time-savers.
2. **Safety.** If you mistype `reset_test_user` you're a lot less likely to encounter unexpected behaviour than if you attempt to manually update records by hand.
