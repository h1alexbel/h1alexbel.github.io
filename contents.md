---
layout: static
title: "Archive"
date: 2023-02-19
permalink: contents.html
---
[```arc```](/tag/arc.html)
[```business```](/tag/business.html)
[```career```](/tag/career.html)
[```devops```](/tag/devops.html)
[```future```](/tag/future.html)
[```github```](/tag/github.html)
[```java```](/tag/java.html)
[```maintainability```](/tag/maintainability.html)
[```microservices```](/tag/microservices.html)
[```oop```](/tag/oop.html)
[```pets```](/tag/pets.html)
[```process```](/tag/process.html)
[```quality```](/tag/quality.html)
[```reactive```](/tag/reactive.html)

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
