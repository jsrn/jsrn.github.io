---
layout: notes
---

# Concurrency in Ruby

## Fibers

[Fibers](https://www.rubyguides.com/2019/11/what-are-fibers-in-ruby/) are like threads, except you have greater control over when they execute.

## Ractors

Ractors are an experimental [Actor Model](https://en.wikipedia.org/wiki/Actor_model)-like concurrent abstraction designed to provide parallel execution without (as many) thread safety concerns.

They were introduced in [Ruby 3.0.0-preview1](https://www.ruby-lang.org/en/news/2020/09/25/ruby-3-0-0-preview1-released/).
