---
layout: post
title: "Collagist: Extracting rails app functionality into a gem"
description: ""
category: programming
tags: []
---

A while back, I wrote an rails application that allows you to upload a .zip file of images, (somewhat) smartly tiles them in one big image, and returns it as a downloadable file. It's available at http://collage.jsrn.net, and it's a pretty great way to tile all your pictures of cute animals.

It deserves better - you and I should be able to tile our pictures of cats in any sort of ruby application!

I've extracted the functionality to the collagist gem to make everything nice and portable. I've removed the ability to extract the images from a zip file I want to stick to the philosophy of doing one thing, and doing it well.

It now only does one thing.

<a href="https://github.com/jsrn/collagist">Code</a>.

<a href="https://rubygems.org/gems/collagist">Rubygems page</a>.


It may or may not do it well.