---
layout: post
title: "Automatic Screenshot Cropping for Ultima Online"
description: ""
category: programming
tags: [cropping, python, image processing, text detection, images]
---

To avoid the tedium of endless screenshot arrangement I created <a href="http://github.com/jsrn/Collage.py">Collage.py</a> to automatically tile cropped screenshots. This turned out to be a big time saver, and helped me to easily share screenshot compilations with others.

The process I currently use is still a long one, though. Each screenshot has to be cropped to the "interesting" parts before the folder can be fed into Collage.py for the end result. How great would it be if we could have a program determine the interesting parts for us? I've noticed that my screenshots are usually in one of two forms.

The first and most frequent type is a screenshot of someone saying something clever or interesting. Generally, these screenshots contain the text and the speaker, along with, ideally, those being spoken to and any interesting responses. We can assume the largest block of text on the screen is the focus of the screenshot. We also want the speaker in the shot, so the crop would have to, at least, extend downwards to encompass the speaker.

The second type of screenshot is the action shot. This generally features a bunch of people running about fighting, and centres on the player. These shots do not have to be cropped quite so tidily, and so we will focus on the first type for now.

To get screenshots of interesting text, we need some way to detect when such text is present. Fortunately, the focus of this exercise, online roleplaying game Ultima Online, has very uniform text. Unlike captchas, this text is never obscured or distorted. We should be able to detect it in screenshots fairly simply.

**Things we know:**

 1. Screen resolution
 2. The bounds of the game window
 3. The text size and font used for in game speech

**Constraints:**

 1. Desired crop size

**Things we hope to detect:**

 1. Text placement
 2. Character placement

If we hope to detect text in the screenshots, it is very useful to us that we know the exact size and shape of all letters that can be rendered on screen. With this knowledge, we can compare any set of pixels to our letter templates to see which ones match. We can do this effectively by translating each letter outline to a matrix of integers corresponding to the letter size. A value of 1 means that the pixel is on, a value of 0 means that it is off. The following example is that of the lower case a:

<img src="/images/uoautocrop/template.png">

For each pixel, and for each matrix, we simply check that all pixels corresponding to a 1 in the matrix are the same colour. If so, we can somewhat reliably assume that the letter is present. In order to build the matrices, I copied the letters from the font into the following template:

<img src="/images/uoautocrop/letters.png">

Since we're not (currently) trying to determine what the text is, only that it is present, it is not necessary for us to order our array of letter templates, or to know which template corresponds to which letter.

{% highlight python %}
letter_height = 17
letter_width = 22
rows, columns = 4, 20

x, y = 1, 1
matrices = {}
for r in rows:
	for c in columns:
		matrix = [][]
		for w in letter_width:
			for h in letter_height:
				if pix( w, h ) is black:
					matrix[w][h] = 1
				else:
					matrix[w][h] = 0
		matrices.append( matrix )
{% endhighlight %}

The end result is that we now have a complete list of matrices representing each letter template. Now we can start looking for text in our screenshot.

{% highlight python %}
texts = []
for y in pic:
	for x in pic:
		for matrix in matrices:
			if is_present( matrix, x, y ):
				texts.append( (x,y) )
{% endhighlight %}

The advantage of knowing that each pixel should be is that we don't need to check each pixel. We can duck out at the first failure. On average, we should be able to rule out the presence of a letter within the first two rows, or 44 pixels.

{% highlight python %}
def matrix_is_present( matrix, x, y ):
	found = None
	for ( x, y ) in matrix:
		if matrix( x, y ) = 1:
			if found == False:
				found = colours( x, y )
			else if colours( x, y ) != found:
				return False
	return True
{% endhighlight %}

*To be continued.*