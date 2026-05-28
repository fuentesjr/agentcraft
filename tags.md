---
layout: page
title: Tags
summary: Topic tags used by published Field Guide pages.
---

{% assign tag_list = site.pages | where: "published", true | map: "tags" | join: "," | split: "," | uniq | sort %}

{% if tag_list.size > 0 %}
{% for tag in tag_list %}
{% assign clean_tag = tag | strip %}
{% if clean_tag != "" %}
- `{{ clean_tag }}`
{% endif %}
{% endfor %}
{% else %}
No tags yet.
{% endif %}
