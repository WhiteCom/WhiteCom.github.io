---
layout: archive
title: "Computer-Language"
permalink: /categories/computer-language/
author_profile: true
---

{% assign posts = site.categories.Computer-Language %}
{% for post in posts %}
  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
{% endfor %}