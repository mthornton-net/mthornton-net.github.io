---
layout: page
title: Bookmarks
description: Useful Bookmarks that I have used or found over the years.
---

Useful bookmarks that I have used or found over the years:

{% for category in site.data.bookmarks %}
{% assign items = category[1] | sort_natural: "name" %}
### {{ category[0] | capitalize }}
{% for item in items %}
* [{{ item.name }}]({{ item.link }}){:target="_blank"}
  * {{ item.description }}
{% endfor %}
{% endfor %}

----

{% if site.comments_repo %}
{% include comments.html %}
{% endif %}
