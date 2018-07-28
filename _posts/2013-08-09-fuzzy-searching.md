---
layout: post
title: "Fuzzy Searching"
description: ""
category: programming
tags: [search, algorithms, string searching, fuzzy searching, levenshtein]
---

So my boss says to me, *"I'd like to be able to type into this box and have a list of all matching items come up."*

*"Sure,"* I say. *"Can do."*

But it occurs to me that I don't really know a whole lot about how string searching works. I haven't really done a lot of it beyond the basic MySQL `MATCH (...) AGAINST (...)` or various languages' `str.contains()` methods. This certainly works okay, but I don't want to be tied to a specific SQL function for my entire life, so it's time to take a look under the hood.

There are two things to worry about when it comes to searching for a string in a larger set of strings:

 1. Does the string match?
 2. How WELL does it match?

You see examples of this every day when you're using any sort of search function on the internet. It is not good enough to simply say *"Yes, this item matches our search term."* Unless your search algorithm has some way to order the results in terms of how well the results match the search, it's not much use to those using it.

For this reason, many of the simpler string matching techniques cannot help us. The needle/haystack and regular expression approaches both leave us with a strict yes/no, so we need to use a fuzzier technique.

### Approximate (Fuzzy) String Matching

Now that we've given up trying to figure out whether two strings match or not, we need a way to quantify how far away two strings are from matching. This is called the **edit distance** and is described as the number of primitive operations needed to convert the string into an exact match.

The main primitive operations are:

 * insertion: cot -> co**a**t
 * deletion: co**a**t -> cot
 * substitution: co**a**t -> co**s**t
 * transposition: co**st** -> co**ts**

Different matchers may apply different weights or limits to the different operations.

#### Goals

 1. For a given search term T, and a list of strings S = {S1, S2, ..., Sn}, find the subset R of S such that items in R are approximate matches to search term T.
 2. Sort R such that the closest approximate matches come first.

#### Computing the Levenshtein Distance

The simplest form of this algorithm is the recursive method, which takes two strings and returns the Levenshtein distance between them. However, it is not efficient, as it computes the distance of the same substrings many times. We can avoid this by storing the distance of all possible prefixes in an array `d[][]` where `d[i][j]` is the distance between the first `i` characters of string `s` and the first `j` characters of string t. When the table has been built, the desired distance is `d[len_s][len_t]`. It should be noted that this algorithm does not count a transposition as a primitive operation.

However, we only need the last two generated rows, so we can save some memory by tweaking the algorithm. Here is my implementation in Python:

{% highlight python %}
def lev( s, t ):
	len_s, len_t = len( s ), len( t )

	if s == t: return 0;
	if len_s == 0: return len_t
	if len_t == 0: return len_s

	v0, v1 = [], []

	for i in range( len_t + 1 ):
		v0.append( 0 )
		v1.append( 0 )

	for i in range( len_s ):
		v1[0] = i + 1

		for j in range( len_t ):
			cost = 0 if s[i] == t[j] else 1
			v1[j+1] = min( v1[j] + 1, v0[j + 1] + 1, v0[j] + cost )

		for j in range( len( v0 ) ):
			v0[j] = v1[j]

	return v1[len_t]
{% endhighlight %}

Okay, this is working for me so far. Checking "kitten" against "sitting" took a whopping 0.0009 seconds, but realistically, we'll never search a pool of 1 entries. To crank it up a notch, I compiled a dictionary list of 100 words and calculated the distance between "kitten" and each word in the dictionary. After each word has its score calculated, the `{score, word}` pairs were stored in a new array. The results array can be sorted by score, and there you have it. Fast, easy, fuzzy searching.

	$ python search.py
	Searching for term "kitten" in a list of 100 words.
	kitten: 0
	kittens: 1
	kitties: 2
	skittles: 2
	bitters: 3
	kitkat: 3
	sitting: 3
	acuminated: 4
	apostille: 4
	bailey: 4
	birdlimed: 4
	Search completed in 0.0163431874628 seconds

My full implementation can be found [here](https://gist.github.com/jsrn/6193311#file-search-py).

I will examine how this algorithm can be put to use searching larger data sets in another post.

### Sources

[Wikipedia: String Searching Algorithm](http://en.wikipedia.org/wiki/String_searching_algorithm)  
[Wikipedia: Approximate String Matching](http://en.wikipedia.org/wiki/Approximate_string_matching)  
[Wikipedia: Levenshtein Distance](http://en.wikipedia.org/wiki/Levenshtein_distance#Computing_Levenshtein_distance)