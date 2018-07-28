---
layout: post
title: "Reflections: What I learned from my first large web application"
description: ""
category: programming
tags: [programming, web development]
---

<em>This is a long and slightly embarrassing post about how I realised I was awful at web development, and the lessons learned from my first real web application.</em>

<hr>

After university, I got my first programming job. I was to be a junior web developer. My formal education in web development didn't extend far, and while I was a decent enough programmer at the time, many of the nuances of coding for the web were lost on me.

After spending a few months maintaining the project of a colleague, I had the opportunity to spearhead my own project. Filled with undeserved confidence and power, I set off to create the greatest application in the world. Unfortunately it flopped, but I learned a thing or two along the way:

1) <a href="#specs-are-important">Specifications are important</a>

2) <a href="#tools-are-useful">Tools are useful</a>

3) <a href="#have-a-backup-plan">Have a backup plan</a>

4) <a href="#user-feedback-is-important">User feedback is important</a>


<h2 id="specs-are-important">Specifications are important</h2>

It's unlikely that any significantly sized codebase will have had every detail and design decision mapped out in advance. Constraints and requirements are always subject to change, and it's important to remain adaptable to user feedback.

However, I'm sure it's not uncommon for a developer to hear the words <em>"just do it, and we'll see how it looks."</em> What I should have done then, and hope to do more in the future, is to help the decision makers help me. Before making a prototype in code, why not whip up some visual wireframes or point to a few examples from other sites that use the same techniques? If they're struggling to decide which approach to take, this can be a good way to show them how it looks in the wild, without spending hours on mockups.

More recently I've been trying to bring details from actual UX studies and research to the table in order to justify decisions one way or another. Projects can be harmed by too much influence by the personal preferences of the bosses, the developers, or anyone involved in the entire process. However, people put in <em>a lot</em> of effort researching the best design and UX patterns to make things work well for the actual <em>user</em>, and it's a waste not to put that data to use.


<h2 id="tools-are-useful">Tools are useful</h2>

At the time, I approached development from a very ground up manner. I was only just grappling with the complexities of PHP and Javascript, let alone the wonders of web frameworks, CSS pre-processors, task runners, uglifying, minifying, and so on and so forth into a limitless pit of tools that are incomprehensible in their number and range, but can make life so much better once you just take the plunge.

<h3>You should probably use a framework</h3>

I didn't use a framework. At the time, it seemed to make sense. I already more or less knew PHP. The bosses are interested in results, right? Why should I spend so much time learning an entire new technology when I can just implement scraps of framework-esque behaviour where it suits the application? 

Well there goes a chunk of time crappily re-implementing routing, MVC, a billion CRUD functions, and so on.

Recently I've been doing my development in Ruby on Rails and <em>WOW</em>. I can't speak much for PHP frameworks, as the beginning of my quest to really become a better web developer coincides roughly with the beginning of my love affair with RoR, but I can only assume that PHP frameworks like Laravel, CodeIgniter, CakePHP, etc. provide many of the same advantages. I never really understood how much time I was wasting reinventing the wheel until someone gave me a ferarri with four of them.

<h3>You should probably use pre-processors</h3>

My first foray into this was checking out grunt and sass. I have never looked back. Grunt, along with grunt-contrib-concat and grunt-contrib-uglify, give the developer the power to structure their javascript in a way that makes sense for them. Modules broken into different files, but seamlessly turned into a single, minified .js file to be transmitted to the user. Sass allows you to structure your stylesheets in much the same way, and provides other benefits such as mixins, variables and nesting.


<h2 id="have-a-backup-plan">Have a backup plan</h2>

... and not just for your data. The project itself was your standard SaaS web application. Its gimmick was that along with the software itself, we would be a source for the data that went with it. However, when there were problems with OUR data source, it revealed that our contingency plan was lacking. This wasn't the (only) reason the project wasn't a success, but it certainly didn't help.


<h2 id="user-feedback-is-important">User feedback is important</h2>

Despite the flaws in how I developed and managed this project, it was generally well received by the few people who used it. It filled a need for them, and somewhere above the layers of crappy code, jumbled CSS and bizzare configurations was a good product. One that I'm proud of. If there's one thing I can say we did <em>right</em>, it was engaging our users and finding out how well the product served their needs. When you can really address issues that a customer has and have them say <em>"wow, this software is great, it really solves my problem with such and such!"</em>, it's a good feeling.

<hr>

<em>The short version of this post is that I learned a lot of things that should have been obvious to anyone with half a brain, but sometimes painful experience can be the difference between knowing and understanding.</em>