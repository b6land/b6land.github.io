---
layout: post
title: 啟用 Jekyll 網誌的分頁和標籤功能 (適用 GitHub Pages)
date: 2022-05-29 12:00:00 +0800
categories:  [Jekyll]
---

本篇會介紹使用 Jekyll 在 GitHub Pages 服務建立網誌時，如何加入分頁 (Pagination) 功能和標籤 (Tag) 功能。

### 分頁 (Pagination)

可以參閱 Jekyll 說明文件 ([Pagination - Jekyll](https://jekyllrb.com/docs/pagination/)) ，直接啟用所介紹的 `jekyll-paginate` Plugin 來達成。

說明文件裡面有提到，如果要針對分類 (Category) 和標籤 (Tag) 頁面分頁的話，可以使用 ` jekyll-paginate-v2 ` Plugin，可惜目前 GitHub Pages 並不支援。在 GitHub Pages 裡面，如果不是事先建立好已分頁過的分類、標籤頁面，則只能對 `Posts` 裡面的文章分頁而已。

1. 修改 `_config.yml`，加入以下設定值：

- `paginate: 5` : 指定一頁要顯示的文章數目。
- `paginate_path: "/blog/page:num/"`: 每一頁的網址。

2. 建立 `index.html`，並加入以下程式碼，這段程式碼是從官方說明文件複製並加以修改：

{% raw %}
``` html
---
layout: default
title: My Blog
---

<!-- This loops through the paginated posts -->
{% for post in paginator.posts %}
  <h1><a href="{{ post.url }}">{{ post.title }}</a></h1>
  <p class="author">
    <span class="date">{{ post.date }}</span>
  </p>
  <div class="content">
    {{ post.content }}
  </div>
{% endfor %}

<!-- Pagination links -->
<div class="pagination">
  {% if paginator.previous_page %}
    <a href="{{ paginator.previous_page_path }}" class="previous">
      Previous
    </a>
  {% else %}
    <span class="previous">Previous</span>
  {% endif %}
  <span class="page_number ">
    Page: {{ paginator.page }} of {{ paginator.total_pages }}
  </span>
  {% if paginator.next_page %}
    <a href="{{ paginator.next_page_path }}" class="next">Next</a>
  {% else %}
    <span class="next ">Next</span>
  {% endif %}
</div>
```
{% endraw %}

想要自行修改語法，可先初步了解 HTML 和 Liquid 語法。

3. 如果建立 GitHub Pages 網誌時，有 `index.markdown` 的話，記得要刪除，不然第一頁會預設吃到這個檔案，然後就顯示**未分頁**的所有文章。

4. 預設不會為第一頁產生 page1 的網址 (資料夾)，如果需要 page1 的網址，應參考說明文件 *Beware the page one edge-case* 的區塊加入第一頁的程式碼語法。

### 標籤 (Tag)

Jekyll 官網的說明文件較少提到標籤、分類的實作，可以參考其他使用 Jekyll 網誌的作者，他們如何建立標籤、分類頁面，以及在文章上的顯示，以下簡介 codinfox 的作法：

1. 在每篇文章 (post) 的 Front Matter 裡加入 tags (及 category)。

```
layout: post
title: 如何使用 Jekyll 產生網誌
date: 2022-04-17 12:00:00 +0800
tags: [Jekyll]
```

2. 建立一個 HTML 檔案，放在 `baseurl` 下 (ex. */blog/*)，並輸入以下程式碼 (從 [codinfox 的教學文章](https://codinfox.github.io/dev/2015/03/06/use-tags-and-categories-in-your-jekyll-based-github-pages/) 複製並加以修改)：

{% raw %}
``` html
---
layout: default
title: 標籤
---

{% comment %}
=======================
The following part extracts all the tags from your posts and sort tags, so that you do not need to manually collect your tags to a place.
=======================
{% endcomment %}
{% assign rawtags = "" %}
{% for post in site.posts %}
	{% assign ttags = post.tags | join:'|' | append:'|' %}
	{% assign rawtags = rawtags | append:ttags %}
{% endfor %}
{% assign rawtags = rawtags | split:'|' | sort %}

{% comment %}
=======================
The following part removes dulpicated tags and invalid tags like blank tag.
=======================
{% endcomment %}
{% assign tags = "" %}
{% for tag in rawtags %}
	{% if tag != "" %}
		{% if tags == "" %}
			{% assign tags = tag | split:'|' %}
		{% endif %}
		{% unless tags contains tag %}
			{% assign tags = tags | join:'|' | append:'|' | append:tag | split:'|' %}
		{% endunless %}
	{% endif %}
{% endfor %}

{% comment %}
=======================
The purpose of this snippet is to list all the tags you have in your site.
=======================
{% endcomment %}
{% for tag in tags %}
	<a href="#{{ tag | slugify }}"> {{ tag }} </a> ‧ 
{% endfor %}

{% comment %}
=======================
The purpose of this snippet is to list all your posts posted with a certain tag.
=======================
{% endcomment %}
{% for tag in tags %}
	<h2 id="{{ tag | slugify }}">{{ tag }}</h2>
	<ul>
	 {% for post in site.posts %}
		 {% if post.tags contains tag %}
		 <li>
		 <h3>
		 <a href="{{ post.url }}">
		 {{ post.title }}
		 <small>{{ post.date | date_to_string }}</small>
		 </a>
		 </h3>
		 </li>
		 {% endif %}
	 {% endfor %}
	</ul>
{% endfor %}
```
{% endraw %}

這些程式碼可以顯示一個 `tags` 頁面，包含網誌所有的標籤列表，以及每個標籤下的所有文章。可自行將 `tags` 換成 `category`。

3. 也可以在 post 中顯示該篇文章有的 `tags`，插入以下片段至 `_layouts\post.html` (如果沒有可自行新增檔案)。

{% raw %}
```html
{% for tag in page.tags %}
	<a href="#{{ tag | slugify }}"> {{ tag }} </a>
{% endfor %}
```
{% endraw %}

### 其它的參考資料
- [在jekyll blog的文章內加入新tag - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10210700)
- [Jekyll Tags on Github Pages · Long Qian](https://longqian.me/2017/02/09/github-jekyll-tag/)