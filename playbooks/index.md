---
layout: default
title: Playbooks
summary: Repeatable operating procedures for practical AI work.
---

# Playbooks

Repeatable operating procedures for practical AI work.

{% assign pages = site.pages | where: "published", true | where: "type", "playbook" | sort: "title" %}
{% if pages.size > 0 %}
{% for item in pages %}
- [{{ item.title }}]({{ item.url | relative_url }}){% if item.summary %} — {{ item.summary }}{% endif %}
{% endfor %}
{% else %}
No published playbooks yet.
{% endif %}
