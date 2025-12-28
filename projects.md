---
layout: single
title: "Projects"
permalink: /projects/
author_profile: true
---

{% for project in site.projects %}
  <article class="archive__item">
    <h3 class="archive__item-title">
      <a href="{{ project.url | relative_url }}">{{ project.title }}</a>
    </h3>
    {% if project.excerpt %}
      <p class="archive__item-excerpt">{{ project.excerpt | strip_html }}</p>
    {% endif %}
  </article>
{% endfor %}
