---
layout: page
title: Archive
permalink: /archive/
header: "https://jawad.ca/images/unsplash/photo-1505663912202-ac22d4cb3707.jpeg"
---

<div>
  <ul>
    {% for post in site.posts %}
      <li>{{ post.date | date: "%F" }} -  <a href="{{ post.url | relative_url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
</div>
