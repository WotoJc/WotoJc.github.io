---
layout: page
permalink: /album/
title: 相册
description: 旅游 & 摩天
---


<ul class="post-list">
{% for photo in site.album reversed %}
    <li>
        <h2><a class="daily-title" href="{{ photo.url | prepend: site.baseurl }}">{{ photo.title }}</a></h2>
        <p class="post-meta">{{ photo.date | date: '%B %-d, %Y — %H:%M' }}</p>
      </li>
{% endfor %}
</ul>
