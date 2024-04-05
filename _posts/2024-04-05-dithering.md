---
layout: post
title: "Dithering"
description: I'm going back and forth on dithering. Will I regret it in ten years?
---

<figure>
    <img
        src="/assets/images/vintagecomputers-small.jpg"
        alt="Some vintage computers dithered using Ditherista and the ZhouFang algorithm">
    <figcaption>Some vintage computers dithered using Ditherista and the ZhouFang algorithm.</figcaption>
</figure>

Pictures are great. Allegedly worth a thousand words. Despite knowing and believing this, I always find myself going back and forth when it comes to adding images to my website or journal and the reason is this:

- I intend for both this website and my personal notes to function as portable capsules five, ten, twenty years from now.
- Both are self-contained directories which I store in git repositories.
- Images take up a lot more space than text.
- 20 years worth of images take up a LOT more space than text.
- I'm scared that GitHub will eventually yell at me and I'll have to host the repository and site elsewhere.

I've been looking at ways of reducing the footprint of images that I might add, and I've spent the morning playing with two tools:

- [Optipng](https://optipng.sourceforge.net), which squeezes a PNG into a smaller space without losing data.
- [Ditherista](https://github.com/robertkist/ditherista), which does almost the opposite. Dithering reduces the colour palette of an image file and applies noise to prevent banding or other weird visual artifacts.

I'm dithering about dithering. Ha ha.

Pros:

1. It's cool and "vintage" looking and I'm incredibly pretentious.
1. It usually saves a lot of space.
1. The output is good enough in most cases.

Cons:

1. It's incredibly lossy by design. If I keep the dithered version in the git repository and the originals elsewhere, I'm much more likely to eventually lose the originals. I might regret that.
1. I get attached to consistency, but dithering doesn't always make the image smaller. Making an exception in those cases would be the correct thing to do, but it would _feel_ wrong.

Oh well. I'm sure I'm overthinking it.
