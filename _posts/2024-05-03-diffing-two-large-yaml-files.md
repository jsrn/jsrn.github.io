---
layout: post
title: Diffing two large YAML files
description: A fun new tool for the toolbelt.
---

I was recently assigned to see if breaking changes associated with an upgrade to [`grape-swagger`](https://github.com/ruby-grape/grape-swagger) were relevant to us.

The approach we took was to generate our doc using both versions of the gem and diff them to look for significant changes. We quickly encountered a problem: the order of items in the output didn't seem strictly deterministic, and the diff was too large to make sense of.

I ended up using the tool [`yq`](https://github.com/mikefarah/yq). This tool is like its cousin, `jq`, but with YAML support.

The final command, for posterity:

```shell
diff \
  <(yq -P 'sort_keys(..)' A.yaml) \
  <(yq -P 'sort_keys(..)' B.yaml)
```
