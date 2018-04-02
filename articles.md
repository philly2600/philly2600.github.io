--- 
title: Articles
layout: default
---
# {{ page.title }}

Here is a selection of articles written by members of Philly 2600 that have appeared in [2600 Magazine](https://www.2600.com/).

{% for post in site.categories.articles %}
{% assign currentdate = post.date | date: "%Y-%m-%d" %}
{{ currentdate }} - <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
{% endfor %}
