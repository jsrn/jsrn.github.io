---
layout: notes
---

# The Actor Model

An **actor** is a primitive unit of computation that receives a message and performs some computation based on it.

This is similar to the way in which we conceptualise objects, except:

- Actors are completely isolated from each other and will never share memory.
- Actors can maintain private state that can never be directly changed by another actor.

In the actor model, everything is an actor, and actors have mailboxes in which they can accept messages.

You can have multiple actors, but each actor will only process one message at a time.

When an actor receives a message, it can:

- Create more actors
- Send messages to other actors
- Designate what to do with the next message

## Links

* https://www.brianstorti.com/the-actor-model/
* https://github.com/ruby/ruby/blob/master/doc/ractor.md

## Bits I'm Not Sure About

* Why was my Ractor based bulk prime generator slower than the serial one?
* What does "Designate what to do with the next message" mean?