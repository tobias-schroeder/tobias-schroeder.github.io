---
layout: page
title: Projects
permalink: /projects/
---

This page now uses a responsive tile layout to keep the design minimal and readable, while making it easy to add project detail pages or external paper links.

<div class="project-grid">
{% for project in site.data.projects %}
  <article class="project-card">
    <a href="{{ project.details_path | default: project.external_link }}" class="project-card-link" aria-label="{{ project.title }}">
      <img src="{{ project.image }}" alt="{{ project.title }}" loading="lazy" />
    </a>
    <div class="project-card-content">
      <h3 class="project-card-title">
        {% if project.details_path %}
          <a href="{{ project.details_path }}">{{ project.title }}</a>
        {% else %}
          {{ project.title }}
        {% endif %}
      </h3>
      <p>{{ project.summary }}</p>
      <p class="project-card-tags">{% for tag in project.tags %}<span>{{ tag }}</span>{% unless forloop.last %}, {% endunless %}{% endfor %}</p>
      <p class="project-card-actions">
        {% if project.external_link %}<a href="{{ project.external_link }}" class="project-btn" target="_blank" rel="noopener">Paper / Report</a>{% endif %}
        {% if project.details_path %}<a href="{{ project.details_path }}" class="project-btn">Read more</a>{% endif %}
      </p>
    </div>
  </article>
{% endfor %}
</div>

> Tip: create a Markdown post under `_posts/` with the URL path in `details_path` and that page will be linked from each card for extended project stories.

