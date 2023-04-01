---
layout: static
title: "Contents"
date: 2023-02-19
permalink: contents.html
---

[```arc```](/tag/arc.html)
[```java```](/tag/java.html)
[```k8s```](/tag/k8s.html)
[```maintainability```](/tag/maintainability.html)
[```microservices```](/tag/microservices.html)
[```nosql```](/tag/nosql.html)
[```oop```](/tag/oop.html)
[```oss```](/tag/oss.html)
[```reactive```](/tag/reactive.html)

This is a full list of all blog posts published:

{% assign sorted_posts = site.posts | sort: 'date' %}{% for post in sorted_posts %}{%if post.categories contains category[0]%}
<div class="posts-list-item">
<span class="posts-list-item-name float-left"><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></span>
<span class="posts-list-item-date float-right">{{ post.date | date: "%Y-%m-%d" }}</span>
</div>