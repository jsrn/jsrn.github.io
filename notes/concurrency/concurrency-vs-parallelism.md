---
layout: notes
---

# Concurrency vs. Parallelism

When tasks are run in serial, one task must be completed before another can begin.

```
A ------
B       ----
```

Concurrency is when a multi-tasking processor can run a task without having finished another task.

```
A -- -  - --
B   - -- -
```

Parallelism is when multiple tasks can be performed _at the same instant_, as with multi-core processors or distributed computing.

```
A ------
B ----
```
