---
title: "아카이브"
layout: archive
permalink: /categories/archive
author_profile: true
---

{% assign posts = site.categories.archive %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}