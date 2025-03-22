---
layout: post
title: 介紹 Elasticsearch 的 Shard 機制
date: 2025-03-09 09:00:00 +0800
categories: [Elasticsearch]
---  

在 Elasticsearch 中，Shard (或可翻譯為「分片」) 可以用來加速查詢或產生備用的資料。

### Shard 介紹

Shard 可以分成 Primary Shard (PS) 和 Replica Shard (RS) 兩種。

可以使用多個 Primary Shard，加速搜尋速度，因為可以透過 Key 搜尋較小的 Index。但是如果分隔成太多 Shard的話，反而會在合併搜尋結果時降低效能。 

另外也建議產生額外的 Replica Shard，如果架設的 Elasticsearch 服務有多個節點，當每個節點各自存放不同的資料，且其中一個節點發生錯誤時，剩餘的正常節點可以從 Replica Shard 取得資料，維持服務的運作。

可以在建立 Index 時設定 Primary 和 Replica 的數量。如果想要後來才設定的話，可以使用分隔的 API (\_split)，詳細的使用方式請見參考資料。

### Primary Shard 與分數計算

分隔 Primary Shard 後，預設是對各個 Primary Shard 內的文件進行查詢，再合併所有 Primary Shard 的查詢結果。由於 Primary Shard 內只有包含一部分的文件，因此單詞頻率會和所有文件的單詞頻率不太一樣。

如果要在查詢多個 Primary Shard 時，套用所有文件的單詞頻率，可以在查詢 API 的 URL 套用 `?search_type=dfs_query_then_fetch` 參數，套用分散式詞頻搜尋 (DFS, Distributed Frequency Search)，但可能會增加 API 查詢的時間。例如：

```
https://localhost:9200/xxx/_search?search_type=dfs_query_then_fetch
```

不過可能還是會遇到同分但不同順序的情形。

### 參考資料

- Shard 的介紹：[Day 07 Elasticsearch - Primary Shard 主要分片 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10324741)  
- Split API ：[Split index API - Elasticsearch Guide \[8.14\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-split-index.html)
- 在 Shard 內計算相關性分數的說明： [Practical BM25 - Part 1: How Shards Affect Relevance Scoring in Elasticsearch - Elastic Blog](https://www.elastic.co/blog/practical-bm25-part-1-how-shards-affect-relevance-scoring-in-elasticsearch)  
- 相關性分數的舉例：[Elasticsearch BM25相关度评分算法超详细解释 - 夜色微光 - 博客园](https://www.cnblogs.com/novwind/p/15177871.html)  
