---
layout: post
title: "Ruby Hash Default is the Same Object"
description: ""
category: programming
tags: [ruby, hash, default]
---

I was recently tripped up when I was trying to use the ruby hash value defaults. I wanted the default value of my hash to be an empty hash. That way I could do something like the following, without worrying about initializing the hashes:

{% highlight ruby %}
hash = Hash.new(Hash.new({}))
hash[:foo][:bar] = 1
# { foo: { bar: 1 } }
hash[:boo][:baz] = 2
# { foo: { bar: 1 }, boo: { baz: 2 } }
{% endhighlight %}

What I didn't realise is that when you set a default value in a ruby hash, _that value is a single object_.

{% highlight ruby %}
hash = Hash.new([])
hash[:foo].object_id
# => 17383520
hash[:bar].object_id
# => 17383520
{% endhighlight %}

This means that the actual behaviour I experienced was pretty weird.

{% highlight ruby %}
hash = Hash.new(Hash.new({}))
hash[:foo][:bar] = 1
hash[:boo][:baz] = 2
hash[:foo]
# { bar: 1, baz: 2}
hash[:boo]
# { bar: 1, baz: 2 }
hash[:noo]
# { bar: 1, baz: 2 }
hash.keys
# => []
{% endhighlight %}

So here we have a hash within a hash.

The strangest part is that, as you see, if you assign directly to the child hash without explicitly assigning the value of a key in the parent, no key is set on the parent!

{% highlight ruby %}
hash = Hash.new(Hash.new({}))
hash[:foo][:bar] = :baz
# => :baz
hash[:foo].keys
# => [:bar]
hash.keys
# => []
hash[:boo] = {}
hash.keys
 => [:boo]
{% endhighlight %}

In effect, we've hidden data within the hash.

If you want the behaviour I mentioned in the first place, you have to initialize your hash by passing a block to it.

{% highlight ruby %}
hash = Hash.new { |hash, key| hash[key] = {} }
hash[:foo][:bar] = 1
hash
# => { foo: { bar: 1 } }
hash[:boo][:baz] = 2
hash
# => {foo: { bar: 1 }, boo: { baz: 2 } }
hash.keys
# => [:foo, :boo]
{% endhighlight %}
