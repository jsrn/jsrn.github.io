---
layout: post
title: "How I use iTerm2"
description: I use a pretty standard terminal configuration, but there are a couple things I like about it.
category: programming
tags: []
updated: Dec 8, 2021
---

I use [iTerm2](https://iterm2.com/) instead of the default MacOS terminal, and here's how I have it set up:

* Twee terminal background with MOTD from [this project](https://github.com/jsrn/wisdom/) because if I'm going to spend all day looking at a prompt, it should be pretty.

![Twee terminal](/assets/images/twee-term.png)

* [Zsh](https://www.zsh.org/) and [Oh My Zsh](https://ohmyz.sh/) for **really good tab-complete** and **git status on your prompt**.
* **Split panes.** I find it really useful to a webserver process log on the left and be able to run tests and other commands in the right.
* **Unlimited scroll buffer.** Sometimes I run a _really_ long command and want to paste the output into a file for safekeeping. Burning a little extra RAM is better than realising some vital bit of information has crept off the top of the screen.
* ⌥ + click anywhere in a command to move your cursor straight there.
* Bind keys to move between word boundaries, allowing you to skip back and forth within a command at a much higher speed with ⌥ + ←/→. Esc + b and Esc + f will move your cursor left and right, respectively.

![Terminal bindings for skipping to word boundaries](/assets/images/option-left-binding.png)

* Silence terminal bell because **I hate it**.
* Enable tab icons for running apps.

![iTerm2 with tab icons enabled](/assets/images/iterm-tab-icons.png)
