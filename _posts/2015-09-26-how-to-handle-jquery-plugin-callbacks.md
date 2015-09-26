---
layout: post
title:  Callbacks in jquery plugin
excerpt: "A plugin requires callbacks and you need to know how to create a callback and handle it."
tags: [sample post, code, highlighting]
modified: 2015-09-26
comments: true
---

### How to create a plugin callback

There is nothing creating a callback in a jquery plugin. You just need to define the callback in options that's it like this:

{% highlight js %}

   $.scrollHappen = {
        defaults: {
            elementsToLoadInitially: 8,
            elementSet: 4,
            scrollCycle: 3,
            loadingHtml: '<small>loading...</small>',
            ajaxSettings: {
                async: false,
                global: false,
                type: 'GET',
                dataType: 'script'
            },
            loadingHtmlAttrs: {
                class: 'loading'
            },
            loadRestButtonAttrs: {
                class: 'load_rest',
                type: 'button'
            },
            // This is your callback option
            loadRestButton: function () {
            }
        }
    };

{% endhighlight %}

You have created a callback for your plugin now you just need to call it properly later in your plugin after extending it to the settings or options of your plugin.

### Extending your callback option in plugin settings

{% highlight js %}

var settings = $.extend(true, {}, $.scrollHappen.defaults, options)

{% endhighlight %}

### Providing attributes to the callback function and calling it

It's not necessary to pass attributes to your callback it's depend on the requirement here I am providing two attributes one is offset and another is limit for fetching data from controller with the help of these parameters.

{% highlight js %}

 $(function () {
        $('.sheets_container').scrollHappen({
            url: '<%= project_path %>',
            total_elements: <%= project.sheets.count %>,
            loadRestButton: function (offset, limit) {
                load_all('<%= project_path %>', offset, limit);
            }
        });
    });

function load_all(url, offset, limit) {
    $.ajax({
        url: url,
        dataType: 'script',
        data: {elementSet: limit, offset: offset}
    });
}

{% endhighlight %}

Now you have called the callback function with some specific parameters which you will handle from the your jquery plugin like this

### handling jquery plugin callback and calling it from the plugin

{% highlight js %}

function handle_callback() {
   $('.' + settings.loadRestButtonAttrs.class + '').click(function (e) {
     if (settings.loadRestButton !== undefined) {
        settings.loadRestButton(offset, limit);
        $('.' + settings.loadRestButtonAttrs.class + '').remove();
     }
   });
}

{% endhighlight %}

> Thank you for reading this blog.

