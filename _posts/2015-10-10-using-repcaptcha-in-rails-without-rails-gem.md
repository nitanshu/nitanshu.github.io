---
layout: post
title:  Using recaptcha in rails without gem
excerpt: "Recaptcha is the best module for using captcha in your application"
tags: [sample post, code, highlighting]
modified: 2015-09-26
comments: true
---


### Introduction

Google recaptcha is one of the best captcha modules to use for captcha type of thing in your application.

### How to use

To use google recaptcha in your application you need to obtain a key value pair which includes public and private key.

To get the the keys [click here](https://www.google.com/recaptcha/intro/index.html)

It will redirect you to google reCAPTCHA home page -> Get reCAPTCHA -> fill the form where label is the label for your site and domain is the domain name of your site.
![Alt text](/images/bio-photo-2.jpg)

then you will get the keys site key and secret key.Now put this where you want the google recaptcha
{% highlight html %}

     <script src='https://www.google.com/recaptcha/api.js'></script>

     <div class="g-recaptcha" data-sitekey="site-key"></div>

{% endhighlight %}

Then clicking on the checkbox will open a default select image box:

{% highlight js %}
        var verifyCallback = function (response) {
            $.ajax({
                method: 'POST',
                dataType: 'html',
                url: '<%= '#' %>'
     })
    };
        var onloadCallback = function () {
            grecaptcha.render('grecaptcha', {
                'sitekey': '#',
                'callback': verifyCallback
     });
    };

{% endhighlight %}

#### Html

Html for recaptcha

{highlight html}

<script src="https://www.google.com/recaptcha/api.js?onload=onloadCallback&render=explicit" async defer>
</script>
<div id="grecaptcha", class="grecaptcha pull-right"></div>

{endhighlight}

This verify callback will give you the response which is handled with an ajax call in rails or it can be used in any kind of application.
