---
layout: post
title: Diagnosing "early termination of worker" errors
description: What to try when you're not getting the error message you need.
category: programming
permalink: /diagnosing-early-termination-of-worker
---

You might be trying to start your Rails server and getting something like the following, with no accompanying stack trace to tell you exactly what it is that's gone wrong.

```
[10] Early termination of worker
[8] Early termination of worker
```

This is a note-to-self after encountering this when trying to upgrade a Rails app from Ruby 2.7 to 3.2. What helped me was a comment from [this Stack Overflow post](https://stackoverflow.com/q/59861277).

If `rails s` or `bundle exec puma` is telling you the workers are being terminated but not telling you why, try:

```bash
rackup config.ru
```