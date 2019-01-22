---
layout: post
title: "Setting Up Dominoes"
description: "If you want to progress without friction, you need to be kind to your future self."
image: assets/images/dominos.png
category: misc
tags: [personal]
---

If I don't set myself up to win, I find it massively impacts how I perform in my day to day. You've probably noticed the same. It's easier to eat a healthy lunch if you prepare and pack it the day before. Same with wearing a nice outfit to work. It's easier to exercise if you do it before you sit down and get comfortable. I place dozens of these little dominoes throughout my day so when the critical moment comes...

<img src="/assets/images/dominos.png">

There's no friction. You just go. I've been doing this for myself for years, but I've recently been trying to focus on thinking of ways I can set up those dominoes for others.

Here's a scenario: A small company has decided that they want to integrate automated tests into their development process. However, that thing has a few attributes that make it harder:

1. It's not mission critical. The sky won't fall if you skip it.
2. It's inconvenient. If you're not used to it, it's not a natural part of your work flow.

If something is inconvenient to do and there are no immediate dire consequences (most of the time) to not doing it, it will likely be difficult to build a habit. It's why a lot of companies are lax with testing or security practices, and it's why you probably don't floss enough. They're different manifestations of the same problem. You build a [desire path](https://99percentinvisible.org/article/least-resistance-desire-paths-can-lead-better-design/) around it.

If you want to integrate new behaviours, you have to minimise resistance. If it's a new step in the process, but you just plonk it in as optional, it might as well not be there.

I'll go ahead and provide some more concrete examples.

1. If you want people to run the unit tests, linters, etc. before pushing, distribute a [pre-push hook](https://git-scm.com/book/gr/v2/Customizing-Git-Git-Hooks).
2. If you really want the tests to run, set up a continuous integration system. Don't allow any code into the production environment that hasn't been through CI and passed.
3. If you want people to provide enough detail when they're submitting a bug report, provide a template to be filled out.
4. If you need something from someone you're about to private message, ask them the question straight away rather than just saying “hi, are you there?”

It's difficult to encourage people to act a certain way if the environment encourages them to behave counter to that ideal. Design the environment, and hopefully the behaviour will follow.
