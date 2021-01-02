---
layout: post
title: "When Is a Class Not Pulling Its Weight?"
description: While the Single Responsibility Principle is touted as a universal good, how small is too small?
category: programming
tags: [OOP, single responsibility principle, refactoring]
---

> I use Inline Class if a class is no longer pulling its weight and shouldn’t be around any more. Often this is the result of refactoring that moves other responsibilities out of the class so there are little left.

I recently finished reading [Refactoring: Ruby Edition](https://martinfowler.com/books/refactoringRubyEd.html), and while there was a lot of talk about extracting logic into single purpose classes, there was also mention of removing classes that weren't deemed to be "pulling their weight."

This left me with a question. How little is too little? I'm assuming that when the author says there is little left in the class, they don't mean there's _nothing_ left, so what sort of classes might exist that don't deserve the mental space they occupy?

The example given in the book is that of a `TelephoneNumber` class which is quite close to simply being a [value object](https://martinfowler.com/bliki/ValueObject.html), but without the immutability or comparison logic. Its only role is to put an area code and a phone number into a nicely formatted string. This logic is pulled into the `Person` class, to whom the phone number belongs.

So say you've got class `B` that you're considering merging into class `A`. Some good reasons to make this merge might be:

* `B` only operates on instances of `A` or its fields.
* `B` is only referenced by `A`.
* You struggle to differentiate between the domain concepts modeled by `A` and `B`.

I think it's less a question of whether a class has enough responsibility, and more a question of whether a class has enough responsibility _that rightfully belongs to it_.
