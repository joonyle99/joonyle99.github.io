---
title: "개발 일지"
layout: archive
permalink: categories/develog/
author_profile: true
---

{% assign posts = site.categories.Develog %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}