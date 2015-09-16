---
layout: post
title: On demand loading of content while scroll the browser to the bottom
excerpt: "Loading content while scroll the bar to the bottom is normal"
tags: [sample post, code, highlighting]
modified: 2015-09-11
comments: true
---

### Why not to use plugins

There are so many plugins which provides the functionality of loading content while scroll to the bottom but you can't depend on the plugins because they are limited to some extent.

### When to code for this kind of stuff

As plugins are limited to some extent but may be your requirement demands more than a plugin can provide the you need to code by your own.

### application.css

{% highlight css %}

  *= require cropper

{% endhighlight %}

I wrote a js file for scrolling to the bottom and loading content.

### scroll_sheet.js

{% highlight js %}

function scroll_sheet(total_sheets, load_all_sheets_url) {
    jQuery.ajaxSetup({async:false});
    var scroll_counter = 0;
    if (total_sheets > $('.sheet-block').size()) {
        $(window).scroll(function (e) {
            if (scroll_counter < 3) {
                if (($(window).scrollTop() + $(window).height()) > ($(document).height() - 2)) {
                    setTimeout(function(){
                        var url = $($('.pagination .page a')[scroll_counter]).attr('href');
                        scroll_counter ++;
                        if (typeof(url) != 'undefined') {
                            $.get(url);
                        }
                    }, 300);
                }
            } else {
                $(window).unbind('scroll');
                if (total_sheets > $('.sheet-block').size()) {
                    $('.load_all').append('<a class="btn btn-default btn-lg load_all_content" href="javascript:void(0)">Load all sheets</a>').click(function () {
                        $.get(load_all_sheets_url, {
                            sheet_token: $('.sheets_container img:last').attr('sheet_token'),
                            success: function () {
                                $('.load_all_content').remove();
                            }
                        });
                    });
                }
            }
        });
    }
}


{% endhighlight %}

Now I am calling this function from the page where I need to load my content on scroll to the bottom.

{% highlight js %}

scroll_sheet(<%#= project.sheets.count %>, '<%#= load_balance_sheets_project_path format: :js %>');

{% endhighlight %}

> Thank you for reading this blog.

