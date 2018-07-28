---
layout: post
title: "Solving scaling issues with more metal"
description: ""
category: programming
tags: [rails, scaling, unicorn]
---

Although the application is pretty modestly sized compared to many, we don't get good performance under load. I believe that most of this trouble is caused by the large overhead of connecting back to individual user databases on various servers.

Since it's not viable to tell all of the users to host their database in a datacentre with a solid connection, I think the core of the problem is the low unicorn worker count. When you only have a few worker processes, waiting behind somebody elses' slow query is excrutiating.

The plan to mitigate this badness is:

 1. Increase the amount of RAM on the server by as much as budget allows. The CPU usage is low, but unicorn workers get pretty fat from eating all that RAM.
 2. Increase the number of unicorn workers by as much as the RAM will allow.
 3. Include the unicorn killer gem to cull long running workers.

Basically, throw metal at the problem until it goes away.

### Useful reading:

 * https://www.digitalocean.com/community/tutorials/how-to-optimize-unicorn-workers-in-a-ruby-on-rails-app