---
layout: page
title: Templates
summary: Reusable prompts, checklists, scaffolds, and supporting material.
---

# Templates

Reusable prompts, checklists, scaffolds, and supporting material.

{% assign pages = site.pages | where: "published", true | where: "type", "template" | sort: "title" %}
{% if pages.size > 0 %}
{% for item in pages %}
- [{{ item.title }}]({{ item.url | relative_url }}){% if item.summary %} — {{ item.summary }}{% endif %}
{% endfor %}
{% else %}
No published templates yet.
{% endif %}
