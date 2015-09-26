---
layout: post
title: Handling callback in jquery plugin
excerpt: "A plugin requires callback events how to create a callback and handle it"
tags: [sample post, code, highlighting]
modified: 2015-09-26
comments: true
---

### How to create a plugin callback

There is nothing creating a callback in a jquery plugin. You just need to define the callback in options that's it like this:

{% highlight html %}

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

### When to code for this kind of stuff

As plugins are limited to some extent but may be your requirement demands more than a plugin can provide the you need to code by your own.

What this code will do it will load content while scroll to the bottom for three times and then it will append a button to load all which will load rest of the content.

###Setting up div where the scrolling js would apply

{% highlight html %}

<div class="row sheets_container">
  <%= render partial: 'sheets/sheet', collection: @sheets, locals: { project: @project } %>
  <%= paginate @sheets, params: { format: :js } %>
</div>
<div class="load_all text-right form-group">
</div>

{% endhighlight %}

I wrote a js file for scrolling to the bottom and loading content.

### scroll_sheet.js

{% highlight js %}

function scroll_sheet(total_sheets, load_all_sheets_url) {
    jQuery.ajaxSetup({async:false});
    var scroll_counter = 0;
    if (total_sheets > $('.sheet-block').size()) { // Here $('.sheet-block').size() is a class where the content are paginated
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

###controller

This is your controller's action where you are fetching the sheets for each scroll.

{% highlight ruby %}

 def show
    @sheets = @project.sheets.page(params[:page]).per(8)
 end

{% endhighlight %}

This is the action where you need to load rest of the content.

{% highlight ruby %}

  def load_balance_sheets
    if @sheet = @project.sheets.where(token: params[:sheet_token]).take
      @sheets = @project.sheets.where(id: (@sheet.id + 1)..Float::INFINITY)
      render :action => 'show'
    else
      render :js => "alert('Unable to fetch sheets.');"
    end
  end

{% endhighlight %}


If you need to made this js available throughout the application then you need to do this:

### application.js

{% highlight js %}

  // require scroll_sheet

{% endhighlight %}

Now I am calling this function from the page where I need to load my content on scroll to the bottom.

{% highlight js %}

scroll_sheet(<%#= project.sheets.count %>, '<%#= load_balance_sheets_project_path format: :js %>');

{% endhighlight %}

> Thank you for reading this blog.

