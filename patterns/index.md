---
layout: page
title: Patterns
summary: Reusable AI workflow and system-design patterns.
---

# Patterns

Reusable AI workflow and system-design patterns.

{% assign pages = site.pages | where: "published", true | where: "type", "pattern" | sort: "title" %}
{% if pages.size > 0 %}
{% for item in pages %}
- [{{ item.title }}]({{ item.url | relative_url }}){% if item.summary %} — {{ item.summary }}{% endif %}
{% endfor %}
{% else %}
No published patterns yet.
{% endif %}
