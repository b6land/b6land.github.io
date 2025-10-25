---
layout: post
title: Elasticsearch 函數分數查詢
date: 2025-10-25 23:00:00 +0800
categories: [Elasticsearch]
---  

使用 function\_score 函數分數查詢，允許去修改查詢所檢索到的文件的分數。

### 函數分數簡介

- 支援多種計算方式，也可以自訂算式。
- 可以合併全文查詢一起使用。

各種選項的說明如下：

| 選項  | 說明  |
| --- | --- |
| weight<br> | 可搭配 filter，為特定關鍵字增加分數。<br> |
| random\_score<br> | - 用於產生隨機結果。<br>- 例如有多筆原始分數相同的結果，可以隨機排序，常用於商品查詢上 (讓類似商品都有曝光的機會)。<br>- 產生 0 ~ 1 之間的隨機小數。<br>- 可以透過 `seed`  和 `field`  欄位，產生可複製的隨機結果。<br><br><br> |
| decay function<br>(衰減函數) | - 適用地理位置、數值、日期等差距計算。<br>- 分為 Gauss, Linear, Exp 三種。<br>- 數字介於 0 ~ 1 之間。<br>- `decay` : 衰減分數。<br>- `scale` : 衰減程度。<br>- `offset` : 以 origin 為中心，在偏移量內都是 1 分。<br>- `origin` : 中心點。<br><br><br> |
| field\_value\_factor<br> | 可用來增減數值欄位的權重。支援多種計算方式，詳細的計算方式請參考官方說明。<br> |
| boost\_mode <br> | 可以調整 ，設定查詢分數和函數分數要如何結合，例如相加、相乘。<br> |
| score\_mode <br> | 可以設定各函數之間的結合方式。<br> |

可參考：[Function score query - Elasticsearch Guide \[8.14\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-function-score-query.html ) (有詳細的計算範例)。

### script\_score: 自訂計算函數

可以使用 script\_score 方法自訂計算的公式，script 可以使用 Elasticsearch 專用的 Painless 語法，

但是寫太多奇怪的 script，會導致 Elasticsearch 查詢的速度變慢，因此 script 的內容應該保持精簡。

Painless 的語法可以在 [Painless Guide - Painless Scripting Language \[8.15\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/painless/current/painless-guide.html) 找到說明，想要找到更詳細的語法，如數學計算方法 (ex. `Math.log()` )，可以參考 [Shared API for package java.lang - Painless Scripting Language \[8.15\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/painless/current/painless-api-reference-shared-java-lang.html#painless-api-reference-shared-Math ) 網頁。另外有幾個撰寫時的小訣竅：

1. Painless 語法長度最長為 65,536 個字元，其說明與如何提升計算速度的網頁在 [Scripts, caching, and search speed - Elasticsearch Guide \[8.15\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/scripts-and-search-speed.html )。
2. 將變數寫在 params 內，有助於將計算方式快取起來。如果 script 常常變動，就無法快取，而減低查詢效率。
3. 由於 JSON 不支援換行，因此必須將所有 Script 寫在同一行內。不過在 Kibana 的 Dev Tools 內，可以搭配 """ 符號輸入多行文字。
4. Elasticsearch 另外推薦使用特化的 Script Score 查詢代替 Fucntion Score，內有詳細的 Script 撰寫說明，詳情可參考 [Script score query - Elasticsearch Guide \[8.15\] - Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-script-score-query.html )。

### 自訂某個字詞的權重

如果有某個詞在查詢時特別重要，可以在搜尋時，用 `^`  在特定字詞後方加入權重，例如：

```csharp
the women^2 went to the beach
```

參考資料：[Modification of term weight - Elastic Stack / Elasticsearch - Discuss the Elastic Stack](https://discuss.elastic.co/t/modification-of-term-weight/3318/6)


### 參考資料

- [Elasticsearch：使用 function\_score 及 script\_score 定制搜索结果的分数\_script score-CSDN博客](https://blog.csdn.net/UbuntuTouch/article/details/103643910)
- 在送到 Elasticsearch 前，多行的非正規 JSON 語法仍會被轉成單個引號、單行的資料：[\[Painless\] Stored script - Multiline - Parsing Error - Elastic Stack / Elasticsearch - Discuss the Elastic Stack](https://discuss.elastic.co/t/painless-stored-script-multiline-parsing-error/211091)
- Painless 語法的簡易使用方式，可以參考：[IT鐵人第22天 Elasticsearch 腳本語言 expression painless - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10249809)
- 使用案例：[ElasticSearch-function\_score簡介](https://kucw.io/blog/2018/7/elasticsearch-function_score/)