---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: posts
title: Herald blog
description: Latest news blog
permalink: blog
---

<div class="content">

<h1>Recent posts</h1>

<div>
  {% for post in site.posts %}
      <div>
      <h2><a href="{{site.baseurl}}{{post.url}}">{{ post.title }}</a></h2>
      <div class="post-single-meta-author">
        {% if post.author_avatar %}
        <div class="post-single-meta-author-avatar">
          <img src="{{ post.author_avatar }}" alt="{{ post.author_name }}">
        </div>
        {% endif %}
        <div class="post-single-meta-author-name">
            {% if post.author_url %}
            <a href="{{ post.author_url }}">{{ post.author_name }}</a>
            {% else %}
            <a href="/tags/{{ post.author_name | urlencode }}">{{ post.author_name }}</a>
            {% endif %}
        </div>
      </div>
      {{ post.excerpt }}
              <br/>
      </div>
  {% endfor %}
</div>

</div>