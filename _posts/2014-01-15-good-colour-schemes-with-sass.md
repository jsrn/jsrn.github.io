---
layout: post
title: "Good Colour Schemes with SASS"
description: ""
category: programming
tags: [sass, scss, css, color schemes, colour schemes]
---

As someone who does a fair amount of front end work, but doesn't have a great eye for colour, one of the biggest problems I face is finding colours for a website that match.

I was watching talks on YouTube while working, and ended up watching [CSS preprocessors with Jonathan Verrecchia](http://www.youtube.com/watch?feature=player_embedded&v=FlW2vvl0yvo), (a must watch, if you're still on the fence regarding whether or not to use a pre-processor for your projects), and in it, he demonstrates a technique for generating colour palettes for your sites using SASS.

{% highlight scss %}
$base: #633;
$complement1: adjust-hue($base, 180);
$complement2: darken($complement1, 5%);
$lighten1: lighten($base, 15%);
$lighten2: lighten($base, 30%);
{% endhighlight %}

With a snippet like this in your project, all you need to do is define your primary hue in `$base`, and you will have two complementary colours, and two lighter shades at your disposal.

<figure>
	<img src="/images/swatches/swatches.png">
</figure>
