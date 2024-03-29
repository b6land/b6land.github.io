---
layout: post
title: 「思考物件導向」系列文心得
date: 2024-01-16 12:00:00 +0800
categories: [Programming]
---

本文章為「思考物件導向」系列文章 (作者：蔡學鏞) 的閱讀心得。

### 類別、方法、欄位的能見度

類別裡的資料要不要徹底的隱藏，只能由內部類別設定 ? 類別的方法能不能被別的類別呼叫或繼承 ?

徹底隱藏帶來極大的安全性，可是容易有擴充性不佳，導致無法實現特定設計的情形。但是若不進行一定程度的隱藏，會導致軟體開發時的複雜度提高、程式容易出錯、非 Thread-Safe ... 等情形。

### 物件導向程式設計能增進開發效率嗎？

物件導向程式設計並非軟體開發的萬靈丹，也不一定能大幅促進開發效率。而且物件導向還有概念上 (如設計模式)、語法上 (不同的 Framework) 的學習門檻。

### 領域特定語言的優缺點

領域特定語言 (Domain Specific Language, DSL) 可能因為更高階的抽象，而增加生產力。例如 SQL 包含資料查詢、資料定義、資料操作、資料控制等語法，使用者不需要理解底層指令就能查詢和操作資料庫。

然而，缺點是使用者需要另外學習一門語言，且設計不良的 DSL 可能會吸收多種語言的缺點，產生使用繁瑣等問題，可參見 [DSL 的误区](https://www.yinwang.org/blog-cn/2017/05/25/dsl) 一文的經驗談。

### 系列文章連結

- [思考物件導向(1)物件導向與封裝 - iThome](https://www.ithome.com.tw/node/45903)  
- [思考物件導向(2)繼承與其階段性任務 - iThome](https://www.ithome.com.tw/node/46085)
- [思考物件導向(3)從多型看物件導向的真面目 - iThome](https://www.ithome.com.tw/node/46202)