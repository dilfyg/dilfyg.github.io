---
layout: archive
permalink: /projects/
title: "Projects"
excerpt: "Here I have my projects I have worked on collated by tags."
author profile: false
header:
  overlay_image: "/images/header3.jpg"
---

{% include base_path %}
{% include group-by-array collection=site.posts field="tags" %}

{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ tag | slugify }}" class="archive__subtitle">{{ tag }}</h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
