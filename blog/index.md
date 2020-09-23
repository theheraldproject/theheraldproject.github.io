---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: blog
title: Herald blog
description: Latest news blog
permalink: blog
---

# Recent posts


<ul>
  {% for post in site.posts %}
    <li>
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      by <a href="https://github.com/{{post.author}}">{{post.author}}</a><br><br>

      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>

