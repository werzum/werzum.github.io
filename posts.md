---
layout: page
title: Posts
permalink: /posts/
---

# Posts

A list containing the blog-posts made on this site.

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
