---
layout: default
title: Archive
permalink: /archive/
---

# Archive

Posts from my old blog (jld-stats.com).

{% assign sorted_posts = site.archive | sort: "date" | reverse %}
{% assign current_year = nil %}

{% for post in sorted_posts %}
  {% assign post_year = post.date | date: "%Y" %}
  {% if post_year != current_year %}
    {% if current_year != nil %}</ul>{% endif %}
    {% assign current_year = post_year %}
<h2>{{ current_year }}</h2>
<ul>
  {% endif %}
  <li><a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
{% if current_year != nil %}</ul>{% endif %}

<!-- MathJax for titles with LaTeX -->
<script>
  MathJax = {
    tex: { inlineMath: [['$', '$'], ['\\(', '\\)']] },
    svg: { fontCache: 'global' }
  };
</script>
<script async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-svg.js"></script>
