---
title: "Coding Test"
layout: archive
permalink: /categories/cote
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.Cote %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}