---
layout: post
title: "One line Ruby webserver for testing timeout handling"
description: What it says on the tin
category: programming
tags: [ruby]
---

I was just changing error handling around net timeouts, and naturally I wanted to test it. There are plenty of good tools floating around for throttling your network connection to test under different speeds and conditions, but frankly I couldn't be bothered with all that.

The Ruby:

```ruby
require 'socket'

t = TCPServer.new(9999)

while s = t.accept
  puts 'accepted connection, snoozin :)'
  sleep 60
  s.close
end
```

The bash:

```bash
ruby -rsocket -e "t = TCPServer.new(9999); while s = t.accept; puts 'accepted connection, snoozin :)'; sleep 60; s.close; end"
```