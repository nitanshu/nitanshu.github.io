---
layout: post
title: Implementing Cropper in rails with fixed crop box
excerpt: "Cropper is a rich jquery plugin for cropping while zoom in and out resize crop area and many more options"
tags: [sample post, code, highlighting]
modified: 2015-08-28
comments: true
---

##Cropper

To implement cropper in rails for cropping and zoom in and out you

### application.css

{% highlight css %}

  *= require cropper

{% endhighlight %}

### application.js

{% highlight js %}

 //= require cropper

{% endhighlight %}

Now get your image on which you need to apply cropper here what I am doing is on double clicking a div it opens a file selector and it selects a file and I am reading a file locally and showing it on my browser

The div where I applied the cropper or my image is showing is