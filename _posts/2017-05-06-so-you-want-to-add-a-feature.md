---
layout: post
title: "So You Want To Add A Feature?"
description: "Questions to ask before development begins."
category: programming
tags: [planning, software development]
---

Everyone loves features. People wouldn’t use your software if it couldn’t do something for them. A lot of the time, the killer feature is the reason the software was created. You have a problem you want to solve, and you have some idea how to solve it. Other times, you’ve already got a product, but you want to make it more powerful or more flexible. It can be difficult not to get carried away when you’ve got an idea for a new feature, but it’s important not to rush into development without proper consideration. I’ve recently been reading [Code Complete](https://cc2e.com) by Steve McConnell and it contains an extensive list for checking whether your project requirements are missing any obvious holes.

Inspired by this, I put together a heavily modified version of the list to use at work. It’s smaller and more tuned for the work that I do, which is web development. It’s more focused on adding a new feature to an existing project than it is on building a requirements list for a new project.

## What Does It Need To Do?

* How does the user interact with it? What sort of interface might it have?
* What’s the output? What’s displayed to the user, and in what format?
* Why do they need this output? Is it in the best format for their needs?
* What data is needed from the user? What data is needed from other parts of the system?
* What sort of input validation will be needed?
* Does the new feature clash with any existing features?
* Is this similar to an existing feature? Is a new feature more suitable than a modification to an existing one?
* Does it meet all of the user’s needs? Are all use-cases accounted for? Is it _intuitive_?

## How Good Does It Need To Be?

* What’s the expected response time? How fast do you expect the feature to be?
* What’s the acceptable response time? How slow can you get away with it being?
* What authentication is required? Will the feature behave differently for different access levels?
* What are the consequences if the feature breaks? Is there any data that must be preserved?
* How are you going to detect and recover from errors?

## Are The Requirements Good?

* Are the requirements written from the [user’s perspective](https://twitter.com/goatuserstories)?
* Do any requirements conflict with each other?
* Do the requirements avoid dictating the design?
* Are the requirements sufficiently detailed?
* Can each requirement be understood by a developer who wasn’t involved in its creation?
* Is each requirement testable? Can its completeness be confirmed or denied?

## Are The Requirements Complete?

* Is there a record of any unanswered questions?
* If all of the requirements are fulfilled, will the feature be acceptable to the user?
* Are any requirements present just to appease someone? If excessive bikeshedding occurred, requirements that aren’t strictly needed could have made it into the list.
* If all of the requirements are fulfilled, will you be proud of the result?

By making sure I’ve at least attempted to answer each of these questions before I begin typing, the route to a high quality finished product will almost certainly be quicker and more pleasant than if I’d just rushed in headlong.