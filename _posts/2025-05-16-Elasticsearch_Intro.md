---
layout: post
title: Elasticsearch 介紹
date: 2025-05-16 21:30:00 +0800
categories: [Elasticsearch]
---  

Elasticsearch 可用來進行全文查詢 (全文檢索)，常聽到的 ELK 是指 Elasticsearch, Logstash, Kibana 的縮寫。

### 簡介

| 產品 | 描述 |
| --- | --- |
| Elasticsearch<br> | 分散式 JSON 搜尋與分析引擎<br> |
| Logstash<br> | 連結與轉換資料<br> |
| Kibana<br> | 資料視覺化與管理工具<br> |

另一個較早出現的是 SQL Server 全文查詢功能。

Elasticsearch 和 SQL Server 全文查詢都用 Invert Index 作為索引，比起使用 SQL Server 直接用 `LIKE %Keyword%` 做為查詢條件更加快速和有彈性。

- [ElasticSearch 全文字搜尋引擎 — 基本概念介紹. 前言 - by vic - Happy Friday - Medium](https://medium.com/happy-friday/f38a0cab9717)

### 和 SQL Server 全文查詢的差異

- SQL Server 本身包含 LIKE 語法和全文查詢功能。
- SQL Server 全文查詢能自行控制的部分較少。
- SQL Server 不容易擴大運行規模，只能在該台主機上加強硬體配備，以達成結果。

Elasticsearch 需要更多維護的成本，但提供更多功能與控制權，如分詞的方式、提供查詢 API 與介面 (含多種查詢方式和選項)、多語系的支援、提供解釋的指令、容易調整同義字和辭庫、整合機器學習，以及地理查詢等等，且分散式系統易於擴充規模和備援。

- [c# - ElasticSearch vs SQL Full Text Search - Stack Overflow](https://stackoverflow.com/questions/20515069/elasticsearch-vs-sql-full-text-search)
- [Full-text search with MySQL and Elasticsearch - by Azraar Azward - Medium](https://medium.com/@mazraara/full-text-search-with-mysql-and-elasticsearch-b48e79a8ad4a)
- 這篇文章有提到一部份傳統資料庫的優點：[Elasticsearch vs. Traditional Databases: Diving into Elastic search's Strengths - by Rajeev Kumar - Medium](https://medium.com/@rajeevprasanna/elasticsearch-vs-traditional-databases-diving-into-elastic-searchs-strengths-c6f55b9b449f)
- Bonus: MySQL 和 Elasticsearch 的討論：[MySQL Full Text Search versus Elasticsearch : r/devops](https://www.reddit.com/r/devops/comments/av704k/mysql_full_text_search_versus_elasticsearch/)

### 衍生的 OpenSearch

Elasticsearch 還有從 Amazon 分支出來的 OpenSearch，特色為開源授權。(不過 Elasticsearch 也在近日重新轉為開源授權)

- [AWS分叉Elasticsearch重新命名為OpenSearch - iThome](https://www.ithome.com.tw/news/143812)
- [Elastic與AWS授權爭議落幕，Elasticsearch重新提供開源授權 - iThome](https://www.ithome.com.tw/news/164799)

### MeiliSearch 全文檢索引擎

MeiliSearch 是另一個簡單、輕量化的替代方案。

在中文匹配方面，有無法分辨同音字的嚴重問題，推測是只有使用拼音查詢資料，到目前還沒有解決，因此不適合套用在產品。

- [\[推薦\] 輕量級全文檢索搜尋引擎 Meilisearch - 辛比誌](https://xenby.com/b/316-%E6%8E%A8%E8%96%A6-%E8%BC%95%E9%87%8F%E7%B4%9A%E5%85%A8%E6%96%87%E6%AA%A2%E7%B4%A2%E6%90%9C%E5%B0%8B%E5%BC%95%E6%93%8E-meilisearch)
- [There seems to be a problem with matching Chinese · Issue #4075 · meilisearch/meilisearch · GitHub](https://github.com/meilisearch/meilisearch/issues/4075)
- [Chinese Pinyin search is not accurate in few Chinese character · Issue #3655 · meilisearch/meilisearch · GitHub](https://github.com/meilisearch/meilisearch/issues/3655)

### 常用 API 的介紹

- [Elasticsearch 教學 - API 操作](https://yuanchieh.page/posts/2020/2020-07-15-elasticsearch-%E6%95%99%E5%AD%B8-api-%E6%93%8D%E4%BD%9C/)
- [ElasticSearch 教學- by Gary Ng - Medium](https://gary840227.medium.com/elasticsearch-%E6%95%99%E5%AD%B8-fdbb9fdf3225)