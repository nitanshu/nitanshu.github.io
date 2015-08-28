---
layout: post
title: Implementing Cropper in rails with fixed crop box
excerpt: "Cropper is a rich jquery plugin for cropping while zoom in and out resize crop area and many more options"
tags: [sample post, code, highlighting]
modified: 2015-08-28
comments: true
---

##Cropper

To implement cropper in rails for cropping and zoom in and out you need to download it from [cropper home page](http://fengyuanchen.github.io/cropper/)

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

 {% highlight ruby %}
   <div class="form-group">
     <div class="form-group user_image_lg" , id="profile_pic_default" >
       <% if current_user.profile_pic? %>
         <%= image_tag current_user.profile_image, id: 'profile', title: 'Double click to change your profile picture'%>
       <% else %>
         <%= image_tag asset_path(current_user.profile_image), id: 'profile', title: 'Double click to select your profile picture' %>
       <% end %>
     </div>
   </div>
 {% endhighlight %}

Div where I am showing my uploaded or cropped image is

{% highlight ruby %}
    <div class="cropper-example-2">
      <img src="" style="display: none;" id="crop_profile_pic">
    </div>
     <%= form_for(resource, as: resource_name, url: crop_profile_pic_path, :html => {:multipart => true}, :remote => true, :method => 'patch') do |f| %>
       <%= f.file_field :profile_pic, id: 'profile_photo_upload',accept: "image/png,image/jpeg,image/gif", class:"pull-left" %>
       <%= hidden_field_tag 'x'%>
       <%= hidden_field_tag 'y'%>
       <%= hidden_field_tag 'width'%>
       <%= hidden_field_tag 'height'%>
       <div class="modal-footer">
         <div class="row">
            <%= f.submit 'Select', class: 'btn btn-primary', id:'submit_crop' %>
         </div>
       </div>
     <% end %>
{% endhighlight %}
