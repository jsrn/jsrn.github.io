---
layout: notes
---

# Copy-On-Write

Copy-on-write is a technique used in computer programming where making a copy of a modifiable resource doesn't _actually_ make a copy until an edit is made.

This is a trade-off: duplicating an object with no further updates consumes less memory. In exchange, the initial write becomes more expensive, as the copy has to be performed.

## Links

* [Copy-on-write - Wikipedia](https://en.wikipedia.org/wiki/Copy-on-write)
