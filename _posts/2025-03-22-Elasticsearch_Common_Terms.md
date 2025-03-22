---
layout: post
title: Elasticsearch 常用字效能調整
date: 2025-03-22 10:30:00 +0800
categories: [Elasticsearch]
---  

在 Elasticsearch 內，如果有每個文件都會 (或常常) 出現的字詞，例如英文文章的「is, the, not」，中文新聞文章的「相關、報導、可能」，可將這些字稱為「常用字」或「高頻字」。通常會將這些字詞加入停用字字典，避免效能降低。

### 處理方式

由於出現頻率很高，當查詢到常用字時，會造成有很多文件需要計算相關性，耗費大量計算資源，而造成效能的下降。因此有幾種處理方式：

1. 加入停用字 (stopword) 字典：Elasticsearch 本身提供停用字字典，也可以針對不同語言設定，Analyzer 也可能提供停用字的機制 (ex. IK Analyzer)。但是在查詢時，就不會納入計分。
2. 送關鍵字到 Elasticsearch 前，先減少不需要的常用字。
3. 查詢時加入 `minimum_should_match`  參數：在 [Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html) 內設定此參數，要求須匹配一定比例或個數的字詞，才納入結果。因為減少了計算數量，可以折衷的解決效能問題。

### Minimum\_should\_match 參數說明

那麼，讓我們來看一下怎麼使用 `minimum_should_match` 參數，以下是一個該參數的範例：

```json
      "query": {
        "multi_match": {
          "query": "To be or not to be",
          "minimum_should_match": "75%",
          "fields": [ "article" ]
        }
      }
```

可以輸入百分比或詞語個數。輸入個數時，表示有幾個詞語是可以忽略 (負數則表示必須匹配)。輸入百分比時，表示有多少比例的詞語是必須匹配 (負百分比表示可被忽略的比例)。此外，也可以組合百分比和個數，請參考官方的參數選項說明。

如果需要使用在多個欄位上，請參考 [Query string query - Elasticsearch Guide \[8.16\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-min-should-match) 的說明。

### 參考資料

- 常用詞的幾種處理方式：[ElasticSearch进阶之停用词处理 - 东哥IT笔记](https://donggeitnote.com/2021/10/16/elasticsearch-stopwords/ "https://donggeitnote.com/2021/10/16/elasticsearch-stopwords/")
- 官方說明 (參數選項)：[minimum\_should\_match parameter - Elasticsearch Guide \[8.16\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-minimum-should-match.html)
- ChatGPT

### 附註

- 中文新聞報導的常用字，可參考此篇論文：[以文字探勘技術分析臺灣四大報文字風格](http://csyue.nccu.edu.tw/ch/Taiwan%20Newspapers%20\(2020\).pdf)
- 由於 2021 年 Elasticsearch 改版後使用 BM25 演算法，不再需要 common\_terms 查詢，也拿掉了 `cutoff_frequency`  的參數，相關討論：[Surprised by deprecation of common\_terms query. What about its relevance features? - Elastic Stack / Elasticsearch - Discuss the Elastic Stack](https://discuss.elastic.co/t/surprised-by-deprecation-of-common-terms-query-what-about-its-relevance-features/270416)。
- 舊版本的 Common Terms 查詢參考：[常用术语查询(Common Terms Query) - Elasticsearch 高手之路](https://xiaoxiami.gitbook.io/elasticsearch/ji-chu/35query-dsldslfang-shi-cha-8be229/quan-wen-sou-7d2228-full-text-search/chang-yong-zhu-yu-cha-8be228-common-terms-query)