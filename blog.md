--- 
title: Blog
layout: default
---
# {{ page.title }}

Philly 2600's blog posts. Philly 2600 members can contribute posts via [this GitHub repo](https://github.com/philly2600/philly2600.github.io).

{% for post in site.categories.posts %}
{% assign currentdate = post.date | date: "%Y-%m-%d" %}
{{ currentdate }} - <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
{% endfor %}
