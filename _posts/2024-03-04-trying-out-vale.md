---
layout: post
title: Trying out Vale to tighten up my writing
description: I do a lot of writing at work. I'm okay, but not exceptional at it. I hedge too much, I'm too verbose, and I favour an enjoyable turn of phrase over absolute clarity.
---

I do a lot of writing at work. I'm okay, but not exceptional at it. I hedge too much, I'm too verbose, and I favour an enjoyable turn of phrase over absolute clarity.

My other sin is that I love a new toy. I've been enjoying the free trial of [iaWriter](https://ia.net/writer). The editing experience is top tier, and the price tag for the MacOS version is a modest one-off purchase of $49.99. What I really want is a tool that tells me when my writing is bad, and largely, it's delivered.

iaWriter offers the following annotations:

- Fillers
- Cliches
- Redundancies
- Custom regular expressions

Still, something wasn't quite sitting well with me. I already use Obsidian for my day to day notes, and VSCode for a lot of other editing. Do I really want to throw another editor into the mix?

I wondered if I could achieve similar results with VSCode. I'd heard that the technical writing team at work make a lot of use of something called [Vale](https://vale.sh/). It lints prose in the same way that I'm used to my code editors annotating my code with observed flaws and suggested improvements. On top of that, it's highly customisable. I don't know if it's necessarily more customisable than iaWriter, but it does seem to be customisable in a way that doesn't need me to be as good at regular expressions.

Here's what I've done:

1. I created a new VSCode profile called "Writing." It has a larger font, and I've turned off as many of the visual distractions as I can.
2. I installed the "Vale VSCode" extension by Chris Chinchilla.
3. I set up my Vale configuration based around some existing style guides and a few of my own bug-bears. You can find my current configuration in my [dotfiles repository on GitHub](https://github.com/jsrn/dotfiles/tree/main/vale).
4. I set the following shell alias: `alias write='code --profile Writing'`

My hope is that using VSCode in "writing mode" will guide me towards sharper prose without me having to invest time and money in a whole new editor.

It's not a perfect solution. Ultimately, this is still a crutch, and it still relies on third party software, but at least this way I can configure my linting rules how I like, and keep them under version control indefinitely. I just hope I spend more time writing and less time endlessly tinkering with Vale configurations.
