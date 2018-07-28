---
layout: post
title: "Starting Rails Apps with Foreman"
description: ""
category: programming
tags: [ruby, rails, foreman]
---

A recent requirement of an application I work on has been sending emails as a background task. I chose to use sidekiq for this, which introduced redis to the application as a dependency.

We were already using memcached as our data store for sessions, so running the application in development now became a matter of

<pre>
$ memcached -d
$ redis-server
$ bundle exec sidekiq
$ rails s
</pre>

That's four commands to run and four terminal windows/tabs to keep open with output logs. I asked on IRC how I could manage this better, and a couple people recommended [Foreman](https://github.com/ddollar/foreman). Foreman is a utility for "managing Procfile based applications".

After you've installed Foreman, you can describe one or more processes, like so:

<pre>
web:       rails s
memcached: memcached
redis:     redis-server
sidekiq:   bundle exec sidekiq
</pre>

You simply put the above (or similar) into a file named `Procfile` in the root of your rails application and bingo. Instead of running all of these commands, we can just run `foreman start`. It will run each command and merge the stdout/stdin into one glorious, colour coded log.

<img src="/assets/images/foreman.png">

Alternatively, you can run `foreman web`, `foreman memcached` etc. to run a single task.

If you're finding the process of starting your rails app's various dependencies has become tiresome and haven't tried something like Foreman, I recommend it.