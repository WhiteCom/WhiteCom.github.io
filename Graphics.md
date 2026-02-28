---
layout: archive
title: "Graphics"
permalink: /categories/graphics/
author_profile: true
---

{% assign posts = site.categories.Graphics %}
{% for post in posts %}
  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
{% endfor %}