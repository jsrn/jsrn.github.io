---
layout: post
title: "Fuzzy Searching: Part 2"
description: ""
category: programming
tags: [search, algorithms, string searching, fuzzy searching, damerau-levenshtein]
---

[Previously]({% post_url 2013-08-09-fuzzy-searching %}), I described my start to a fast and flexible search box for a work project. While I'm pretty happy with the progress so far, there's definitely room for improvement. In the last post, I demonstrated my implementation of a method of getting the Levenshtein distance between two strings.

The Levenshtein distance takes into account the following operations: additions, subtractions, and replacements. The **Damerau-Levenshtein** number is similar, but takes the transposition of *adjacent* characters into account. In an article in the March 1964 edition of the Communications of the ACM magazine (which I am sadly unable to locate, thanks to academic paywalls), Damerau reported that the four primitive operations (addition, subtraction, substitution, transposition) account for 80% of human misspellings. I'm not sure how much of that is accounted for by transpositional errors, but using the Damerau-Levenshtein distance instead of the Levenshtein may yield better results.

	Searching for term 'kittne' in a list of 100 words.
	Levenshtein:        Damarau-Levenshtein:
	kitten: 2           kitten: 1
	kittens: 2          kittens: 2
	kitties: 2          kitties: 2
	kitkat: 3           kitkat: 3
	sitting: 3          sitting: 3
	skittles: 3         skittles: 3
	bitters: 4          bitters: 4
	pottage: 4          pottage: 4
	casino: 5           casino: 5
	inlit: 5            inlit: 5
	mani: 5             mani: 5

These are some pretty good results, But we can see that the Demarau-Levenshtein is going to give a more reliable result when it comes to minor misspellings. However, there must be a way to enhance these results. If we look at the following case:

*(This, and all future output will be from the D-L algorithm unless noted otherwise.)*

	Searching for term 'c' in a list of 100 words.
	nut: 3
	lehi: 4
	mani: 4
	prud: 4
	casino: 5

This is obviously not optimal. If we start a seach with "c", we are obviously not looking for "lehi". These results come first in the rankings simply because there are fewer steps between them, not because it's likely to be what we're looking for. Perhaps we can add additional weighting to those results in the set of which the search term is a substring.

To attempt to do this, I changed two things:

 1. **Increased the cost of substitution from 1 to 2.** This should help to prevent shorter words from being a closer match just because it would be easier to substitute every letter than it would be to increase the length of the word.
 2. **Reduced the cost of insertion to 0.** This makes it is inexpensive to increase word length, giving increased importance to substring matches.

Now to look at how that turned out:

	Searching for term 'casro' in a list of 101 words.
	Before:         After:
	casino  [2]     casinoroyale  [0]
	mani    [4]     casino        [1]
	alvera  [5]     ashlaring     [2]
	bailey  [5]     carpetbag     [2]

Much better. We can throw in substrings, and so long as they're in the right order, they'll give us a good answer. We get close to typing in the full word, and we're almost guaranteed a the right answer.

In the next post, I will look at including the modified Damarau-Levenshtein distance to allow for a good result when:

 1. The search string (S) contains multiple words
 2. The string being searched (T) against contains multiple words
 3. Both S and T contain multiple words

### Sources

[Wikipedia: Damerau-Levenshtein Distance](http://en.wikipedia.org/wiki/Damerau-Levenshtein_distance)