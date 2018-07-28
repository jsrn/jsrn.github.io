---
layout: post
title: "Collage.py"
tagline: "quick image tiling"
description: ""
category: programming
tags: [python, image manipulation]
---

On an online game I play, we take a lot of screenshots to document our events. To condense the screenshots into something easy to take in, we generally crop them to the meaningful part and tile them into a larger image. I decided to try my hand at automating the process.

**Goal:** Given `n` images of size `{W1H1},{W2H2},...,{WnHn}`, arrange these images on a canvas such that:

* Left-to-right order is maintained
* Top-to-bottom order is maintained
* Minimal blank space is left on the canvas

This is a simple task when you have images of roughly the same size. However, this isn't always going to be the case. It may be useful to be able to determine the variation in images sizes across the set. We can start with an array of images sizes, e.g.:

	image_sizes = [ [100,100], [60,80], [200,50], [10,2] ]

and some functions:

	get_max_width() 			get_max_height()
	get_min_width() 			get_min_height()
	get_avg_width() 			get_avg_height()
	get_width_range() 			get_height_range()

When I say that it's ideal to have images of the same size, this is only really half true. As the viewer of the image will be reading left to right, it is much more important that the resultant image have uniform rows. Uniform columns are a distant second.

#### The Easy Way

This is the easy route. It doesn't guarantee that the output will look good, but hey, at least it's *something*, right? Well, we can make that judgement after it's coded. Rather than the comparatively more complex processing of **The Hard Way**, this method just requires three simple steps:

* Calculate the mean height
* Scale all images to the mean height
* Tile
* (optional) Marvel at the distorted chimera of an image you have inflicted upon your screenshot collection

This was relatively easy to accomplish, and the full code for Collage.py comes in at a mere 57 lines. The code as it was at this point can be found [here](/files/Collage.py.txt). Actually, with images that are already a similar size, this actually produces some decent results. However, the cracks definitely begin to show with certain images that are too far away from the mean. We can improve upon this by applying some more in depth techniques in **The Hard Way**.

#### The Hard Way

So for now, we cease worrying about width constraints and focus on height. As long as aspect ratio is maintained, we can resize images a reasonable amount before the image is too distorted to be worth viewing. Since the sprites used in the game are quite small to begin with, we can start with the following constraints:

* An image can be reduced to 75% of its former size to fit with the others.
* An image can be increased to 150% of its former size to fit with the others.

If resizing images, the smaller image should increase 2px for every 1px that the large image shrinks.

***Example***:

We have two images, one is 100px tall, the other is 126px tall.

	top = 126
	bottom = 100
	while top > bottom:
		bottom += 2
		top -= 1
	print "Top: " + str(top) 			# 117
	print "Bottom: " + str(bottom) 		# 118

If we take the lowest value as the new height, we know to scale both images to 117px tall. This makes our final image much tidier. However, this doesn't take into account our earlier resizing constraints.

If the smaller image can no longer be increased in size, we should make a note to leave it at its maximum size, and fill in the blank space with background colour to keep the rows uniform in height. This requires keeping the new height of each image separately, as they will not always be the same.

So, here's a rough psuedocode outline of the entire program:

	# Load images from folder
	images = [ 'img1.png', 'img2.png', ..., 'imgn.png' ]

	mean_height = get_mean_height( images )

	# Resize images
	for img in images:
		if img.height < mean_height:
			while img.height < img.max_height:
				img.height += 2
		else:
			while img.height > img.min_height:
				img.height -= 1
			img = pad_image( img, mean_height )

	tile_images()

There is a problem here. We can reduce huge images to their minimum size, and we can pad small images with blank space so as to keep rows uniform without distorting the image. However, there will be situations where the size difference is so vast that these images still look absurd next to eachother.

Project code will be made available [here](https://github.com/jsrn/Collage.py) as I work on it.