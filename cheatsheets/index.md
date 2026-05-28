---
layout: page
title: Cheatsheets
summary: Fast references for practical AI workflows, prompts, commands, and concepts.
---

# Cheatsheets

Fast references for practical AI workflows, prompts, commands, and concepts.

{% assign pages = site.pages | where: "published", true | where: "type", "cheatsheet" | sort: "title" %}
{% if pages.size > 0 %}
{% for item in pages %}
- [{{ item.title }}]({{ item.url | relative_url }}){% if item.summary %} — {{ item.summary }}{% endif %}
{% endfor %}
{% else %}
No published cheatsheets yet.
{% endif %}
