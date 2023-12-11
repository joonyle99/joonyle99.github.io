---
title: "책 리뷰"
layout: archive
permalink: categories/book/
author_profile: true
---

{% assign posts = site.categories.Book %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}