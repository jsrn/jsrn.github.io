---
layout: post
title: "Fuzzy Searching: Part 3"
description: ""
category: programming
tags:  [search, algorithms, string searching, fuzzy searching]
---

Previous posts: 
[Part 1]({% post_url 2013-08-09-fuzzy-searching %})  /
[Part 2]({% post_url 2013-08-12-fuzzy-searching-part-2 %})

Code: 
<a href="http://github.com/jsrn/Search.py">Search.py on GitHub</a>

In my last posts, I showed how the Damarau-Levenshtein distance is a great metric to use for fuzzy string searching. However, the previous examples have been primarily aimed at searching for single words within a list of single words. In practice, it is not realistic to think that the search term will always be a single word, nor should we restrict ourselves to only searching a database of single words.

For example, we may want conduct the following search:

	S: quick fox jumps

Should the list of searchable text contain it, we could reasonably expect the following result:

	T: The quick brown fox jumps over the lazy dog.

The current algorithm has reasonable performance in this area:

	Searching for term 'excavate topsoil' in a list of 39 words.
	Excavate by hand / topsoil [1]
	Excavate by machine / topsoil [1]
	Load surplus excavated [...] temporary spoil, average distance [1]

However, performance completely falls down if order of the words in S differs to the order of words in T. We can see that the results barely have anything to do with the searched terms:

	Searching for term 'topsoil excavate' in a list of 39 words.
	Load and deposit in temporary spoil heaps, average distance [3]
	Break up debris and arisings, [...] aside for re-use [4]

To ensure that our results are reliable regardless of word order, it is important that we treat a sentence as a collection of words, rather than a very long word that happens to contain spaces.

*To be continued.*