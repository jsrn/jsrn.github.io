---
layout: post
title: "Getting Our Awkward Dependency To Run At Scale"
description: ""
category: programming
tags: [work, ruby, windows, web development, concurrency]
---

We have a fairly important dependency for our SaaS product, and getting it running smoothly and at reasonable scale has been difficult. It’s a bit of third party software that takes some XML and turns it into a PDF. It takes about 1 second to process, and it’s a good bit of kit.

## The Problems

With multiple users trying to get their PDFs through our API, a 1 second processing time per PDF means that a queue can build up quickly. This can be disastrous for our overall performance. We need concurrent processing, but:

1. The dependency is packaged as an Adobe AIR based .exe. By default, Adobe AIR applies a mutex lock to all programs so only one instance can be run at any time.
2. The application runs poorly through Wine, and Adobe AIR is not well supported on Linux systems. It is more reliable to run the dependency on a Windows EC2 instance.
3. It was difficult to get a multi-threaded/multi-process ruby server running on Windows because the Windows API lacks some POSIX fork calls. In particular, we were using Puma, which flat out refused to start in clustered mode.

At this point, it felt like the dream of concurrent PDF rendering was a way off, and we’d potentially have to ditch Ruby and re-write the admittedly small API in another language. Fortunately, we managed to hack together an ugly but functional solution before taking the drastic step of a re-write.

## The Solutions

Adobe AIR applies a mutex lock to the applications, but it doesn’t try very hard to enforce it. By copying the application and changing the unique ID in the application manifest, you can run multiple copies of most AIR applications without issue. (Source: [stackoverflow](http://stackoverflow.com/questions/2217307/starting-an-adobe-air-application-multiple-times)).

Despite the difficulty in running a multi-process ruby web-server on Windows, there’s nothing stopping us from running multiple instances of a single-process web-server on different ports. We would just have to do our own basic load balancing from the client.

```ruby
URLS = ["https://a.b.c.d"]
PORTS = [80, 81, 82, 83, 84]

def random_endpoint
  "#{URLS.sample}:#{PORTS.sample}"
end
```

Each running instance of our API can then be assigned its own instance of the dependency. We did this by mapping ports to folders.

![Chart](/assets/images/awkward-dependency.png)

This approach is bad for us because it means more single endpoints that need to be monitored for uptime, and the the deployment process is more fiddly because we can’t easily rely on trusted tools. It’s good for the users, though, because they don’t have to wait while the processing queue grows.