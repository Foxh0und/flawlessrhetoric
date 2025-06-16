---
layout: page
title: directory
---
<div id="item-wrapper">
  {% assign sorted_tags = site.tags | sort %}
  {% for tag_pair in sorted_tags %}
    {% assign tag_name = tag_pair[0] %}
    {% assign tag_posts = tag_pair[1] %}
    <div class="tag-item">
      <h2>{{ tag_name | downcase }}</h2>
      <ul>
        {% for post in tag_posts %}
          <li><a href="{{ post.url }}">{{ post.title }}</a></li>
        {% endfor %}
      </ul>
    </div>
  {% endfor %}
</div>
