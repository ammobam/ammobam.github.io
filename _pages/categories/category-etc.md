---
layout: archive
title: "Posts by etc"
permalink: /categories/etc
author_profile: true
toc: true
---
{% for category in site.categories %}
  {% if category[0] == "etc" %}
    {% for post in category[1] %}
      {% include archive-single.html type=list %}
    {% endfor %}
  {% endif %}  
{% endfor %}