---
layout: post
title: Elasticsearch 評分方式、文字相似度與模糊搜尋
date: 2026-04-14 23:00:00 +0800
categories: [Elasticsearch]
---  

在全文搜尋系統中，搜尋結果的排序通常是根據文字的相關性決定。Elasticsearch 提供多種方式來計算相關性，例如：  

1. 關鍵字頻率 (BM25)
2. 模糊搜尋 (Fuzzy Search)
3. 文字相似度 (Vector)

本文將簡單介紹這幾種技術。

### Elasticsearch 在全文查詢使用的 BM25 評分方法

BM25 (Best Match 25) 是 Elasticsearch 的相似度評分方式，改善自 TF/IDF 演算法，重點如下：

1. 加總所有單詞的匹配結果
2. 對於每個單詞： 
    - 在單一文件內，單詞出現頻率越高，該欄位單詞總數越少，越高分
    - 在所有文件內，單詞出現頻率越少，越高分

詳細的說明可以參考：

- [文字探勘之前處理與TF-IDF介紹](https://www.cc.ntu.edu.tw/chinese/epaper/0031/20141220_3103.html)
- [BM25 实用详解 - 第 2 部分：BM25 算法及其变量 - Elastic Blog](https://www.elastic.co/cn/blog/practical-bm25-part-2-the-bm25-algorithm-and-its-variables)
- [白話圖解 - 什麼是 TF-IDF · YWC 科技筆記](https://ywctech.net/ml-ai/what-is-tfidf/)

### 模糊搜尋

模糊搜尋 (Fuzzy Search) 的原理是計算文字編輯距離，找出符合特定距離的詞彙。例如 `fuzziness` 為 1 的情形下，「box」和「fox」是相同的。

Elasticsearch 支援模糊搜尋，可以使用專屬的 Fuzzy 查詢：[Fuzzy query - Elasticsearch Guide \[8.15\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-fuzzy-query.html)。其它的查詢語法 (如 `match` )，也有相關屬性可以開啟和調整模糊搜尋，可參考：[Match query - Elasticsearch Guide \[8.15\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html)。

常用的參數如下：

1. `fuzziness` ：設定模糊查詢的最大文字編輯距離。
2. `transpositions` ：能不能接受兩個字母互換位置，預設是 `true` 。

請留意：

1. 模糊搜尋功能可能不支援中文。
2. 模糊查詢應用在英文時，會造成查詢速度變慢。

### 用文字相似度查詢

文字相似度與 Word Embdding 有關，用向量表示詞語間的相關性，例如 car 和 automobile 的語意相近，和 flower 的語意較不接近。

若要在 Elasticsearch 內用文字相似度查詢，可參考這篇文章的介紹，[使用向量场进行文本相似性搜索 - Elasticsearch Labs](https://www.elastic.co/search-labs/cn/blog/text-similarity-search-with-vectors-in-elasticsearch)，裡面有介紹如何搭配向量模型查詢資料。

Note：目前的文字相似度計算方式，分成：用模型計算，例如 Word2Vec；根據詞語與上下文重複的頻率計算，例如 GloVe。想更進一步了解原理，可以參考：[\[常見的自然語言處理技術\] 文本相似度(II): Cosine Similarity - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10268777)。

### 延伸應用：推薦系統

在電商平台中常見的推薦商品，是透過推薦系統 ([深入淺出常用推薦系統演算法 Recommendation System - by 學．誌](https://chriskang028.medium.com/42f2437e3e9a)) 達成。通常該系統也會用到 TF/IDF 演算法，實作的經驗談可參考：[Build a recommender system with Spark: Content-based and Elasticsearch – I Failed the Turing Test](https://vinta.ws/code/build-a-recommender-system-with-spark-content-based-and-elasticsearch.html)，另外 Elasticsearch 本身也有實作 More Like This 功能。