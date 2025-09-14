---
layout: post
title: 進階檢索摘要 (RAG, BERT)
date: 2025-09-14 18:00:00 +0800
categories: [Machine Learning]
---  

在資料檢索中，除了傳統的全文檢索以外，近年來透過機器學習，可以加強用自然語言查詢資料的能力。以下是這些檢索技術的相關研究。

### RAG 檢索增強生成

RAG 全名為 Retrieval-Augmented Generation，中文為「檢索增強生成」，是一種結合了搜尋檢索和生成能力的自然語言處理架構。

使用者可以用自然語言發問，藉由 RAG 檢索得到的結果交給 LLM，由 LLM 產生合適的答案，不像以前使用者要從檢索結果找出需要的內容。

- [RAG 基礎 - 常見向量資料庫整理-黑暗執行緒](https://blog.darkthread.net/blog/vector-db-survey/)
- [RAG 跟傳統搜尋不一樣的地方 · YWC 科技筆記](https://ywctech.net/ml-ai/difference-between-search-and-rag/)

### BERT

BERT 全名為 Bidirectional Encoder Representations from Transformers。

BERT 用來做字詞嵌入(Word Embedding)，將多組字詞放到相同的向量內，與全文檢索 (Full-Text Search) 相比，可以做到類似定義的查詢，代替關鍵字匹配 (Keyword Matching)。

例如以下的例子 (引用自 [取代關鍵字匹配 -- Sparse Vector 與 BM42 · YWC 科技筆記](https://ywctech.net/ml-ai/replace-keyword-match-by-sparse-vector/) )，有機會由 BERT 解決：

>  
> 
> - 錯字：使用者查 straw**barry** 找不到個體 straw**berry**
> - 同義詞：查**口紅**，找不到**唇膏**
> - 語意：查**圓桌**，但找到**圓桌武士**
> - 分詞：查**香水**，但找到 **「香」氛「水」氧機**

- [LeeMeng - 進擊的 BERT：NLP 界的巨人之力與遷移學習](https://leemeng.tw/attack_on_bert_transfer_learning_in_nlp.html)
- [\[Day 25\] Spotify 使用 NLP 以助於 Podcast 搜尋 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10306957)