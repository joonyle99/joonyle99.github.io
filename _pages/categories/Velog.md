---
title: "개발 일지"
layout: archive
permalink: categories/velog/
author_profile: true
---

{% assign posts = site.categories.Velog %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}