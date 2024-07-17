---
title: "Mini Project"
layout: archive
permalink: categories/mini
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.mini %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}
