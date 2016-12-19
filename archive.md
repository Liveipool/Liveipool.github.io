---
layout: page
title: "Archive"
description: "学在大三"
header-img: "img/liver.jpg"
---

<!-- <center>
    <p><img src="http://upload-images.jianshu.io/upload_images/3001083-30125fcb9b03aa58.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" align="center"></p>
</center>
 -->
<ul class="listing">
{% for post in site.posts %}
  {% capture y %}{{post.date | date:"%Y"}}{% endcapture %}
  {% if year != y %}
    {% assign year = y %}
    <li class="listing-seperator">{{ y }}</li>
  {% endif %}
  <li class="listing-item">
    <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
    <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
  </li>
{% endfor %}
</ul>