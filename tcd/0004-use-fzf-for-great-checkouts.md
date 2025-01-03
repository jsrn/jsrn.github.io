---
layout: tcd
id: "0004"
title: Use fzf for great checkouts
authored: "2025-01-03"
updated:  "XXXX-XX-XX"
---

The output of my `git branch` is a terrible mess. I should be more proactive about cleaning up old branches. I won't be.

I was recently pointed to [`fzf`](https://github.com/junegunn/fzf), "a command-line fuzzy finder." The following alias in my shell config means I'll never have to face the consequences of my untidiness.

```bash
fco='git branch | fzf | xargs git checkout'
```

![fco demo](0004-fco.gif)
