---
layout: post
title: "Tidying Code For Beginners"
description: "In 2017 I took Ben Orenstein's Code Quality Challenge. Here are my notes."
category: programming
tags: [programming, code quality]
---

![Readme](/assets/images/code-quality/readme.png)

## Improve Your README

For a lot of code bases, the README is the first thing that people are likely to see. The quality of your README is going to set an important first impression. How do I use this? How do I install it? _What does it even do?_

If I’m looking for new tool, those are some pretty important questions. At the minimum, I think a README ought to clearly answer:

* What is it? What’s it for?
* How do I install it in order to use it?
* How do I install it in order to develop it?
* How do I use it? Provide an example or two or ten of the most common usage scenarios.
* How do I ask questions about it? Issue tracker? Twitter?
* How can I contribute to it?

I’m guilty of missing most or all of these on pretty much any tool I’ve ever published. Most of them are useless, so I’m not too worried, but my work project has users. It has other team members. We’re accountable to each other for any bad documentation.

Our on-boarding process has done a lot to improve our README. We don’t hire often, but when we do, our documentation is the primary reference for getting new people up to speed. A lot of the time they’ll ask a question or struggle with something, and we’ll go _“wow, we really should have written that down.”_ There’s a lot of institutional knowledge floating about and it’s difficult to know what should be written down until it’s approached by a totally fresh pair of eyes. Developer on-boarding is a great way to test your documentation.

A common theme in the responses to the challenge thread was the difficulty of keeping the README up to date. Since there’s no way I know of to have a bad README fail a build, I can’t think of any good way to mitigate this other than encouraging diligence and maybe making it someone’s (or many persons’, or everybody’s) responsibility to keep it up to date.

### The Takeaway

* Whether you intend it to be or not, your README is your first impression. There’s a relatively small set of things that people expect to find in a README, and including them can improve everybody’s experience.
* Documentation will be bad if nobody uses it. Use your documentation as a first point of reference, and you will have an incentive to maintain it. Run your project by oral tradition, and your written documentation will get out of date _fast_.
* “Work not documented is work not done.” If you’ve done or changed something, but you haven’t written anything about it in the docs, will it take someone by surprise or cause unnecessary confusion? If so, you haven’t finished working.