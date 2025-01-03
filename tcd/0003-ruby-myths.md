---
layout: tcd
id: "0003"
title: Ruby myths
authored: "2022-11-19"
updated:  "2022-12-14"
---

## Ruby is slow

### "Slow" isn't useful

First off, a concession: Yes, Ruby is slower than many languages on many benchmarks. **However.** "Slow" is a useless descriptor if you don't have a "fast enough."

Allow me to strain an analogy. If you're a driver, I'd bet that whatever car you drive isn't as fast as what they take on the F1 track. There's a good chance that you still consider your car _fast enough_. If you do have a super fast car, you might brag to your friends about how it does zero to sixty in 0.036 seconds, but 99% of the time you're never going above the posted speed limit.

At some point you start asking more relevant questions like how many cupholders it has.

### Did you know…

... that contributors to the Ruby language have made [excellent progress](https://codefol.io/posts/is-ruby-3-actually-three-times-faster/) on Ruby 3x3, the goal that Ruby 3 should be _three times_ faster than Ruby 2?

... that there are Ruby implementations like [JRuby](https://www.jruby.org/) and [TruffleRuby](https://www.graalvm.org/ruby/) built for concurrency and speed?

... that Ruby's got a snazzy new [JIT compiler](https://github.com/ruby/ruby/blob/master/doc/yjit/yjit.md) that's leading to some [pretty nice gains](https://shopify.engineering/yjit-just-in-time-compiler-cruby)?

... that Ruby 3 introduced [Ractors](https://ruby-doc.org/core-3.0.0/Ractor.html), a handy implementation of the Actor Model for concurrency?

## Ruby is dead

<https://isrubydead.com/>

## Ruby doesn't scale

[GitHub](https://github.com) is doing fine. [GitLab](https://gitlab.com) is doing fine. [Shopify](https://shopify.com) is doing fine.

[Top Ruby Companies](https://toprubycompanies.info/) indicates that, at the time of writing, the last 12 months have seen an overall revenue of $61.5 billion dollars for companies that make Ruby a key part of their stack. That seems like scale to me.

## Ruby is just for web development

You can do a heck of a lot with Ruby other than web development. Ruby is a great choice for plenty of software, like...

## Development tools

Package managers like [Homebrew](http://brew.sh). Local deployment configurations like [Vagrant](https://www.vagrantup.com) and infrastructure management tools like [Puppet](https://puppet.com/open-source/#osp) or [Chef](https://github.com/chef/chef). Hey, why don't you write something like [Fastlane](https://fastlane.tools/) to automate huge swathes of your mobile app deployment pipeline?

### Desktop applications

[Shoes](http://shoesrb.com) helps you build GUI applications for the desktop.

### Games

You can make games in Ruby using tools like [Gosu](https://www.libgosu.org) or [DragonRuby](https://dragonruby.org), and Ruby is used as the plugin language for some versions of the popular [RPG Maker](https://www.rpgmakerweb.com) engine.

### Security testing

[Metasploit](https://www.metasploit.com) modules are written in Ruby.

### Music

Drop beats with [Sonic Pi](http://sonic-pi.net).
