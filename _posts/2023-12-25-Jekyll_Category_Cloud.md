---
layout: post
title: 製作 Jekyll 網誌的分類雲 (標籤雲)
date: 2023-12-04 12:00:00 +0800
categories:  [Jekyll in GitHub Pages]
---

標籤雲常見在網誌的側欄或上方，可以快速地看到網誌裡所有的標籤，並以大小表示含有標籤的文章數量多寡。本篇文章包含 Jekyll 分類雲 (Categories) 的範例、標籤雲 (Tags) 的相關資料，以及空格的處理。

### 建立分類雲

以下是本網誌的分類雲範例，放在列出所有分類的「分類」頁面內：

```
{% for category in site.categories %}
  {% capture category_name %}{{ category | first }}{% endcapture %}
  <a href="#{{ category_name }}"
    style="font-size: {{ category | last | size | times: 4 | plus: 80  }}%">
    {{ category_name | replace: ' ', '&nbsp;'}}&nbsp;({{ category | last | size }})
  </a>
  
{% endfor %}
```

這段 Liquid 標記語法和 HTML 語法結合，使用了迴圈取得 Jekyll 站台內的所有分類作為連結，並從 80% 開始，每個分類多一篇文章就增加 4% 的字體大小。連結包含每個分類的名稱與文章數量，按下連結後，會移至該分類所在的位置。

可以參考以下的網頁，加入分類雲或標籤雲，並自行調整顯示方式：[Generating Tag cloud on Jekyll blog without plugin - Super Dev Resources](https://superdevresources.com/tag-cloud-jekyll/)

### 不分行空格

實際製作分類的標籤雲以後，發現比較長的標籤在邊邊時會拆成兩行，造成顯示上的困惑。這個狀況可以透過將一般空格取代為 nbsp 空格 (no-break space)，將較長的標籤強制在一行顯示。

```
{{ category_name | replace: ' ', '&nbsp;'}}
```

除此以外還有其它不同的插入空白方式：[實用！在HTML插入空白的6種方式 – April Yang](https://aprilyang.home.blog/2020/02/14/6-ways-to-put-spaces-in-html)