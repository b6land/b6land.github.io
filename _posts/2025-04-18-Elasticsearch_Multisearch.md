---
layout: post
title: Elasticsearch 多重搜尋
date: 2025-04-18 22:00:00 +0800
categories: [Elasticsearch]
---  

想要做多種搜尋結果的整合，或是熱門景點搜尋等等，可以參考以下的文章和概念。

### Multi Search API 用法

內建的 Multi Search API，可以在一次 API Request 執行多次查詢，節省多次 Request 來回的時間與額外負載。

例如：

```csharp
GET my-index-000001/_msearch
{ }
{"query" : {"match" : { "message": "this is a test"}}}
{"index": "my-index-000002"}
{"query" : {"match_all" : {}}}
```

每組搜尋語法分為 header 和 body，header 可填寫要搜尋的索引，body 是查詢語法，可填寫的參數，請參考 [Multi search API - Elasticsearch Guide \[8.17\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-multi-search.html) 。

可能須將原本的多行 JSON 合併在一行內，Elasticsearch 才能正確回應。

請注意，在 Request Body 後面，需要加一行空白行表示結束。

### 關於 Curation

Curation 指的是查詢多個索引，並整合查詢結果，達到查詢熱門景點排序在前的效果：[用 App search 的 curation 概念解決熱門景點的搜尋問題 - Funliday Tech Blog](https://techblog.funliday.com/2022/11/22/%E7%94%A8-App-search-%E7%9A%84-curation-%E6%A6%82%E5%BF%B5%E8%A7%A3%E6%B1%BA%E7%86%B1%E9%96%80%E6%99%AF%E9%BB%9E%E7%9A%84%E6%90%9C%E5%B0%8B%E5%95%8F%E9%A1%8C/)。

不過 Curation 的主要目的，其實是提升或隱藏搜尋結果，在 Elastic Cloud 的 App Search 內可以看到：

- 鐵人賽文章介紹：[【少女人妻的30天Elastic】Day 7 : Elastic App Search\_搜尋優化\_趴萬 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10241974)
- 底層的查詢方式：[Engine 的 Search 進階剖析篇 - 喬叔的 Elastic Stack 專業教育訓練](https://training.onedoggo.com/tech-sharing/uncle-joe-teach-es-elasticsearch/xiang-app-search-xue-xi-zen-mo-yong-elasticsearch/engine-de-search-jin-jie-pou-xi-pian)
- Elasticsearch 官方說明：[Curations - App Search documentation \[8.17\] - Elastic](https://www.elastic.co/guide/en/app-search/current/curations-guide.html)