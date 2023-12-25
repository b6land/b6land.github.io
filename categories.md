---
layout: page
permalink: /categories/
title: 分類
---

## 所有分類

<div id="category_list">
{% for category in site.categories %}
  {% capture category_name %}{{ category | first }}{% endcapture %}
  <a href="#{{ category_name }}"
    style="font-size: {{ category | last | size | times: 4 | plus: 80  }}%">
    {{ category_name | replace: ' ', '&nbsp;'}}&nbsp;({{ category | last | size }})
  </a>
  
{% endfor %}
</div>

## 分類文章

<div id="archives">
{% for category in site.categories %}
  <div class="archive-group">
    {% capture category_name %}{{ category | first }}{% endcapture %}
    <div id="#{{ category_name | slugize }}"></div>
    <p></p>
    <h3 class="category-head">{{ category_name }}</h3>
    <a name="{{ category_name | slugize }}"></a>
    {% for post in site.categories[category_name] %}
    <article class="archive-item">
      <h4><a href="{{ site.baseurl }}{{ post.url }}">{% if post.title and post.title != "" %}{{post.title}}{% else %}{{post.excerpt |strip_html}}{%endif%}</a></h4>
    </article>
    {% endfor %}
  </div>
{% endfor %}
</div>