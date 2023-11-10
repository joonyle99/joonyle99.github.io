---
title: "Computer Science"
layout: archive
permalink: /categories/cs
author_profile: true
---

<!-- 이 문서는 CS를 대표하는 카테고리 -->

{% assign posts = site.categories.CS %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}