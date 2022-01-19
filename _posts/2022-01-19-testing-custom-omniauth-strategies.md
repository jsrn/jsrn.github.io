---
layout: post
title: "Testing custom omniauth strategies"
description: Tripping up when writing unit tests for a custom omniauth strategy
category: programming
tags: [ruby, omniauth]
---

We wrote a custom [Omniauth](https://github.com/omniauth/omniauth) strategy to integrate with an an SSO provider. My problem arose when it came to making sure some changes to the strategy were sufficiently covered by unit tests. The Omniauth documentation seems to focus on integration testing using a simple Rails app as a test harness, but having an entire Rails application as a test dependency feels a little excessive.

So my starting point was to go through the [list of omniauth strategies](https://github.com/omniauth/omniauth/wiki/List-of-Strategies) to see if any of them have an approach I can use. There's a lot of good examples, but one problem. They all seem to be written using RSpec. Not a problem, but the test suite I've inherited is written in minitest. Not a big problem, but it is making me use my brain a bit.

Anyway, with a bit of digging, I managed to find enough examples to show me how to cobble together my extra test cases. I mainly wanted to write this as a sign post so if you stumble across this post after googling "custom omniauth strategy unit testing" you might wander over towards the list of existing strategies and take a look at how they did it.