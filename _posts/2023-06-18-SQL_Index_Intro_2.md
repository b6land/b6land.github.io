---
layout: post
title: SQL 索引簡介 2 - 設計索引的原則
date: 2023-06-18 12:00:00 +0800
categories: [SQL Server]
---

之前曾簡介 MS-SQL 中的索引 (Index) 與常見類別，今天要繼續介紹設計索引的原則。

### 設計索引的原則

1. 索引使用到的欄位應盡量的少，以減少 Insert, Update, Delete 和 Merge 指令調整索引導致的效能影響，以及使用的磁碟空間。
2. 索引適合建立在常讀取 (Select)，但不常增修的大量資料表，以增進讀取的效能。
3. 如果檢視 View 包含資料表連接 (Join)、彙總函數 (ex. SUM)，使用索引可以增進讀取的效能。
4. 盡量使用 SARGable 的查詢語法，以有效的利用索引。(可參考拙作的一部分說明: [SQL Not Equal 的效能影響](/SQL_Not_Equal/))
5. 盡量一次 (指令) 就完成 Insert 或 Update，才能利用到最佳化的索引維護方式。

以上改寫自官方的設計指南。

### 參考資料

- [SQL Server 及 Azure SQL 索引架構與設計指南 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/sql-server-index-design-guide?view=sql-server-ver16)
- [30-13 之資料庫層的優化 - 索引設計與雷區 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10221971)