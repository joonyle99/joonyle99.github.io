---
title: "카테고리"
layout: archive
permalink: categories
author_profile: true
---

---

{% for category in site.categories %}

<span style="font-size: 18px;">{{ category[0] }}</span>

  {% assign posts = category[1] %}
    {% for post in posts %}
        {% include archive-single2.html type=page.entries_layout %}
    {% endfor %}
{% endfor %}
