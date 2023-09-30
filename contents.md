---
layout: static
title: "Contents"
date: 2023-02-19
permalink: contents.html
---

 <li>{% for tag in page.tags %}
  <a href="/tag/{{ tag }}.html">
    <code>
      <nobr>{{ tag }}</nobr>
    </code>
  </a>
</li>

Total: {{ site.posts.size }}.

This is a full list of all blog posts published:
  <ul class="categories-list">
    {% assign sorted_posts = site.posts %}{% for post in sorted_posts %}
      <div class="posts-list-item">
          <span class="posts-list-item-name float-left"><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></span>
        <span class="posts-list-item-date float-right">{{ post.date | date: "%Y-%m-%d" }}</span>
      </div>
    {% endfor %}
  </ul>
