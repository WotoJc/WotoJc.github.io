---
layout: page
permalink: /daily/
title: 文章
description: 码字经验
---



<ul class="post-list">
{% for dairy in site.daily reversed %}
    <li>
        <h2><a class="daily-title" href="{{ dairy.url | prepend: site.baseurl }}">{{ dairy.title }}</a></h2>
        <p class="post-meta">{{ dairy.date | date: '%B %-d, %Y — %H:%M' }}</p>
      </li>
{% endfor %}
</ul>
