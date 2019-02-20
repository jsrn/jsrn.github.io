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

## Nuke Those TODOs

> Why do this? Because code is a lousy place to track todos. When a todo lives in your code, it can’t be prioritized or scheduled, and tends to get forgotten.

This definitely matches my experience. Across the project, there were 11 TODO comments. I was slightly embarrassed to find that they were _all_ by me, and I don’t remember ever personally resolving a single one. Uh oh. I guess TODO comments really do tend to get forgotten.

The oldest offender has been haunting the code base since the 20th May, 2015. Only two of them were from this year. After cleaning them up, we ended up with:

* Three immediate changes to the code. These issues had been hidden in a comment for over a year, and resolving them on the spot was actually less effort than finding a way to describe them in a ticket.
* Two comments that were no longer needed at all.
* Six new issues properly logged for the whole team to see.

If you’ve got an old code base, I encourage you to do something similar: go through it and figure out what the oldest TODO is. Is it something you’re aware of? Does it even apply any more?

## Get Rid Of A Warning

![Deprecation warnings in a console](/assets/images/code-quality/warnings.png)

Deprecation warnings are funny. I read them, and nod along and say “_Gee_, I hope it’s not going to be too painful for Future James to deal with that.”

After a while, they get tuned out. It’s just a warning, right? It’s not a full blown issue just yet. Ignoring the big block of spewed out warnings becomes routine. They’re not causing any harm, and they become invisible.

But it could turn into an issue. It could conceal an issue. I was ignoring them because, hey, the tests still passed, but it’s easy to see how another, more serious message could get lost in that huge scrawl of text.

On top of that, you have to contend with the [broken window theory](https://blog.codinghorror.com/the-broken-window-theory/). If your code is kicking out a lot of warnings, then people are more likely to let new ones slide. You may inadvertently cultivate a collective attitude of carelessness, and that is when the more serious issues are going to creep in.

## Remove Unused Code

As a project grows, it’s natural that bits of code fall out of use. There are different types of unused code, each causing their own problems for developers.

### Commented Code

Large blocks of commented code are usually there because a developer concluded that the code was not needed, but did not want to delete it out of concern that the code would be useful in the future.

If you’re using version control, you will always be able to leaf through old versions of the code if you want to see how it changed over time. The unlikely benefit of having that old code on hand is outweighed by the extra cognitive load of having it there.

### Unused Classes

If you’re familiar with a code base, having unused classes floating about seems like it’s not really a problem. Since you’re familiar with the classes that you do need to use, the ones you don’t might be out of sight, out of mind.

The problem arises when someone is trying to understand how your code works. Mental energy is spent understanding a class that isn’t used or needed. Energy is spent trying to understand how that class interacts with the rest of the system when it doesn’t.

### Unused Local Variables

Variables that are created but never used are quite harmful but extremely easy to clean up. Especially if your method is long and complex (which is a problem for another day), it might not be obvious that a method isn’t being used. Not only does it cause extra cognitive strain, but it can affect your application’s performance, too.

I recommend you configure your editor to lint your code and show you your unused variables in real time.

## Trim Unused Branches

Delete local tracking branches that no longer exist on origin:

```
git remote prune origin
```

Delete old or unused branches on origin with:

```
git ls-remote --heads origin
git push origin --delete <some branch>
```

This is quick and easy to do, but the effect is worthwhile. By curating your branches, you can paint a picture of everything that’s going on with the code base. To stick to the tree analogy, you get to see the directions in which your application is growing.
