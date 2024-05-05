---
layout: page
permalink: /categories/
title: 分類
---

## 所有分類

<div id="category_list">
{% assign categories = site.categories | sort %}
{% for category in categories %}
  {% capture category_name %}{{ category | first }}{% endcapture %}
  <a href="#{{ category_name }}"
    style="font-size: {{ category | last | size | times: 4 | plus: 80  }}%">
    {{ category_name | replace: ' ', '&nbsp;'}}&nbsp;({{ category | last | size }})
  </a>
{% endfor %}
</div>

## 分類文章

<div id="archives">
{% for category in categories %}
  <div class="archive-group">
    {% capture category_name %}{{ category | first }}{% endcapture %}
    <div id="#{{ category_name | slugize }}"></div>
    <p></p>
    <a name="{{ category_name | slugize }}"></a>
    <h2 class="category-head">{{ category_name }}</h2>
    
    {% for post in site.categories[category_name] %}
    <article class="archive-item">
      <li><a href="{{ site.baseurl }}{{ post.url }}">{% if post.title and post.title != "" %}{{post.title}}{% else %}{{post.excerpt |strip_html}}{%endif%}</a><span class="post_date-list"> — <time datetime="{{ post.date | date: "%Y-%m-%d" }}">{{ post.date | date: "%B %-d, %Y" }}</time></span></li>
    </article>
    {% endfor %}
  </div>
  <p><a href="#top" class="back_top">回到最上方 &#8593;</a></p>
{% endfor %}
</div>