---
layout: post
title: Elasticsearch 檢查與改善效能
date: 2025-03-22 10:00:00 +0800
categories: [Elasticsearch]
---  

如果想要提升 Elasticsearch 的查詢效能，例如減少查詢的時間，以下有幾個方法可以達成。

### 檢查索引資訊

我們可以透過檢查索引資訊、系統負載，先了解目前的系統狀態。

檢查索引資訊的 API (請自行代換網址)：

```powershell
https://localhost:9200/_cat/indices?v
```

檢視系統負載的 API。不過在部分作業系統或執行環境下，可能無法顯示所有欄位的資訊 (請自行代換網址)：

```powershell
https://localhost:9200/_nodes/stats/os
```
- 參考：[Nodes stats API - Elasticsearch Guide \[8.16\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-nodes-stats.html)  

### 使用 Kibana 的 Search Profiler 檢查查詢時間

接著我們可開啟 Kibana，透過 Management → DevTools 找到 Search Profiler，這個工具可以用來分析查詢時間的成分。

Search Profiler 是透過呼叫 Profiler API 取得測量結果，並視覺化呈現。View details 內的項目說明可參考[解釋](https://xiaoxiami.gitbook.io/elasticsearch/ji-chu/33-apis/343sou-suo-apis-search-apis/profile-api/profiling-queries) (不知道為什麼只有簡體的說明 …. )。

PS. 由於增加了分析的紀錄，查詢時間花得比一般查詢更久是正常的。

- Search Profiler 請參考：[Profile queries and aggregations - Kibana Guide \[8.15\] - Elastic](https://www.elastic.co/guide/en/kibana/8.15/xpack-profiler.html)
- 關於 Search Profiler 和其它效能微調的說明：[Tune for search speed - Elasticsearch Guide \[8.15\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/tune-for-search-speed.html#_tune_your_queries_with_the_search_profiler)

### 使用 Reindex 改進查詢效能

Reindex 除了可以改變原有資料的結構、索引配置以外，也能調整 Shard (分片) 的數量，或重新分片，使分片內的資料分布更平均，增進查詢效能。

- 更詳細的說明：[Elasticsearch Reindexing: When, How, and Best Practices - Rockset](https://rockset.com/blog/elasticsearch-reindexing-when-to-reindex-best-practices-and-alternatives/)

### 改善效能的幾種做法

1. 如果每個文件都有特定的字詞，那麼搜尋到該字詞時，會需要計算所有文件的分數，速度就會變慢。
2. 不要濫用 Script 撰寫計分或判斷邏輯，很容易拖慢時間。
3. 要調整 Primary Shard，適度分隔資料，以增加搜尋效率。

另外也可以參考以下網頁：

- [Searching 搜尋效能優化 - 喬叔的 Elastic Stack 專業教育訓練](https://training.onedoggo.com/tech-sharing/uncle-joe-teach-es-elasticsearch/elasticsearch-de-you-hua-ji-qiao/searching-sou-xun-xiao-neng-you-hua)
- [我的 ElasticSearch 調校之旅. 分享 MaiCoin SRE 團隊在久病成良醫的 Elastic… - by smalltown - Starbugs Weekly 星巴哥技術專欄 - Medium](https://medium.com/starbugs/89c380b5673c )
- [最大程度提高向集群添加节点后的 Elasticsearch 性能 - Elastic Blog](https://www.elastic.co/cn/blog/maximizing-elasticsearch-performance-when-adding-nodes-to-a-cluster)