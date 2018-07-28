---
layout: post
title: "Code Highlighting Examples"
tagline: "for personal reference"
description: ""
category: programming
tags: [code highlighting, syntax highlighting]
---

I decided to try to set up code highlighting for this blog using the inbuilt functionality and the monokai theme borrowed from [this repository](https://github.com/richleland/pygments-css).

#### PHP

{% highlight php %}
<?php
$animals = array( 'cat', 'dog', 'mouse' );
foreach( $animals as $animal )
{
     echo "Look, a {$animal}!";
}
?>
{% endhighlight %}

#### HTML

{% highlight html %}
<!doctype html>
<html lang="en">
<head>
     <meta charset="UTF-8">
     <title></title>
</head>
<body>
</body>
</html>
{% endhighlight %}

#### Javascript

{% highlight javascript %}
function redirect( url )
{
     document.location = url;
}
{% endhighlight %}

#### Python

{% highlight python %}
#!/usr/bin/env python
for i in range( 100 ):
     print i
{% endhighlight %}

Works like a charm, just be sure to include

{% highlight html %}
<link href="{{ BASE_PATH }}/stylesheets/stylesheet_of_choice.css" rel="stylesheet">
{% endhighlight %}

in your default.html, and GitHub pages will do the rest.