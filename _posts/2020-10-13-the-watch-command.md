---
layout: post
title: "The Watch Command"
description: Monitoring command output with the linux 'watch' command.
category: programming
tags: [linux, bash]
---

I was shoulder-surfing my coworker the other day when he did something that I imagine is common knowledge to everyone except me.

When I'm trying to do something like monitor how quickly a file is growing, it's not uncommon to see a terminal window on my screen that looks like this:

```
➜ du -hs index.html
4.0K	index.html
➜ du -hs index.html
4.0K	index.html
➜ du -hs index.html
5.0K	index.html
➜ du -hs index.html
6.0K	index.html
➜ du -hs index.html
8.0K	index.html
➜ du -hs index.html
12.0K	index.html
```

Not only is this untidy, you hardly look impressive, sitting there jabbingly wildly at your up and return keys.

This is why I found it somewhat revelatory when my coworker entered the command `watch du -hs index.html` and I saw something like the following:

```
Every 2.0s: du -hs index.html

4.0K    index.html
```

From the man pages:

> NAME  \
>     watch - execute a program periodically, showing output fullscreen
>
> SYNOPSIS  \
>     watch [options] command
>
> DESCRIPTION  \
>     watch runs command repeatedly, displaying its output and errors (the first screenfull). This allows you to watch the program output change over time. By default, command is run every 2 seconds and watch will run until interrupted.

If you're a macOS user like myself, this command is available via the Homebrew package [watch](https://formulae.brew.sh/formula/watch).
