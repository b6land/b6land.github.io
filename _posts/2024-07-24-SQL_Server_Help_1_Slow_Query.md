---
layout: post
title: [SQL Server 效能搶救] 1. 一個速度慢的查詢具備哪些因素
date: 2024-07-24 11:00:00 +0800
categories: [SQL Server 效能搶救]
--- 

「反過來想，總是反過來想」─查理·蒙格。讓我們列出很慢的查詢會具備的因素。

### 因素

- 要處理的資料太多。
- 沒有建立索引。
- 無法有效利用索引：不適當的查詢條件。
- 取得資料的邏輯有問題。
    - 例如：明明只要取一個月的資料，但是取的時候都把一整年的資料都取出來？只需要查詢姓名、電話的通訊錄，卻取得整張基本資料表的欄位？
    - 不理解你的商業邏輯，做效能優化是事倍功半。
- 不適當的資料表設計。
    - 資料重複過多的資料表 (沒有正規化) vs 切得太細 (取得資料要使用多次 Join，需要反正規化)。這個問題可能需要交給 DBA 解決。

### 參考資料

- [文章摘要 - 搶救資料庫效能大作戰 – Lazy Coding](/SQL_Article_Improve_Database_Performance/)
- [什麼是資料庫反正規化？優缺點是什麼 - ExplainThis](https://www.explainthis.io/zh-hant/interview-guides/backend/database-denormalization)