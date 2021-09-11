---
layout: archive
title: "Posts by Algorithm"
permalink: /categories/algorithm
author_profile: true
toc: true
---
{% for category in site.categories %}
  {% if category[0] == "Algorithm" %}
    {% for post in category[1] %}
      {% include archive-single.html type=list %}
    {% endfor %}
  {% endif %}  
{% endfor %}