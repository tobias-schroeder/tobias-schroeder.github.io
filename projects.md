---
layout: single
title: Projects
permalink: /projects/
---

<div class="projects-grid">
  {% assign sorted_projects = site.projects | sort: "order" %}
  {% for project in sorted_projects %}
  <a class="project-card{% if project.image %} project-card--has-image{% endif %}" href="{{ project.url | relative_url }}">
    {% if project.image %}
    <div class="project-card__image">
      <img src="{{ project.image | relative_url }}" alt="{{ project.title }}">
    </div>
    {% endif %}
    <div class="project-card__body">
      <h3 class="project-card__title">{{ project.title }}</h3>
      <p class="project-card__summary">{{ project.excerpt | strip_html | truncatewords: 45 }}</p>
      <span class="project-card__link">Read more →</span>
    </div>
  </a>
  {% endfor %}
</div>