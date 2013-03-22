---
layout: page
title: SwarmBots Blog
type: blog
permalink: /blog/
---

#Recent Blog Posts
- - -

<ul class="block-list">
    {% for post in site.posts %}
    <li>
        <span class="post__time">{{ post.date | date: "%d %B, %Y" }}</span>
        <h2 class="post__title"><a href="{{ post.url }}">{{ post.title }}</a></h2>
    </li>
    {% endfor %}
</ul>