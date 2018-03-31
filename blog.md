--- 
title: Blog
layout: default
---
# {{ page.title }}

{% for post in site.categories.posts %}
{% assign currentdate = post.date | date: "%Y-%m-%d" %}
{{ currentdate }} - <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
{% endfor %}
