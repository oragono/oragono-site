---
title: News
layout: default
meta-description: News about the Oragono IRC server.
---
{% include header.html %}

This page contains news about the Oragono IRC server, new releases, and plans for the future. We also have an RSS feed here, which you can use to keep updated: [feed.xml](https://oragono.io/feed.xml)

{% include hrpre.html %}

{% for post in site.posts %}
{% assign author = site.data.authors[post.autho] %}

<strong>~=~ {{ post.title }} ~=~</strong><br>
<span class="postinfo">{% if author.name %}[{{ author.name }}]({{ author.web }}) - {% endif %}{{ post.date | date_to_string }} - <a href="{{ site.liveurl }}{{ post.url }}">Link</a></span>

{{ post.content }}

{% if forloop.last %}{% else %}{% include hrpre.html %}{% endif %}
{% endfor %}