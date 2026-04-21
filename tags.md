---
layout: page
title: Tags
permalink: /tags/
---

A clustering view of the notes. Every note is tagged with one or more short topic keywords — the sections below group notes by tag so adjacent observations live near each other. There is no formal taxonomy; tags are added as they arise, and the list will grow and shift as more notes accumulate.

{% assign notes = site.pages | where_exp: "p", "p.path contains 'notes/'" | where_exp: "p", "p.date != nil" %}
{% assign all_tags = "" | split: "" %}
{% for note in notes %}
  {% assign all_tags = all_tags | concat: note.tags %}
{% endfor %}
{% assign unique_tags = all_tags | uniq | sort %}

**Jump to:** {% for tag in unique_tags %}[{{ tag }}](#{{ tag | slugify }}){% unless forloop.last %} · {% endunless %}{% endfor %}

{% for tag in unique_tags %}
## {{ tag }}

{% assign tag_notes = notes | where_exp: "n", "n.tags contains tag" | sort: "date" | reverse %}
{% for note in tag_notes %}- [{{ note.title }}]({{ note.url | relative_url }}) ({{ note.date | date: "%Y-%m-%d" }})
{% endfor %}
{% endfor %}
