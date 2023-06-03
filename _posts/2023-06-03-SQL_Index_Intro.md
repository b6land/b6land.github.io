---
layout: post
title: SQL 索引簡介
date: 2023-06-03 12:00:00 +0800
categories: [SQL]
---

本文簡介 MS-SQL 中的索引 (Index)，以及常見的幾種索引類別。

### 什麼是索引

在 SQL 中，如果有大量資料存放在資料表的話，索引可以改善查詢時的效能，減少查詢時間。如同大部分的書本最前面都有目錄，我們可以透過目錄快速地找到想要的內容。

不過，不適當地使用索引的話，會額外占用磁碟或記憶體的空間，而沒有實際的改善查詢效能；而且在新增、修改、刪除資料時，都會耗費時間更新索引。

資料庫的索引使用的是一種叫 B+ Tree 的樹狀資料結構，搜尋的行為類似於二元樹 (Binary Tree)，但是較為矮胖，可以避免造訪的葉節點過長導致 IO 次數過多的問題，也較沒有重新平衡樹狀結構導致的效能問題。

### 常見的索引類型

以下是常見的索引類型：

Hash: 利用雜湊表 (Hash Table) 存取資料。(速度較快)

Clustered: 資料經過排序並使用樹狀結構儲存。可藉由存在節點中的鍵值 (Key) 快速地取得資料。

Nonclustered: 資料列的索引有經過排序，但關聯的資料列不保證有經過排序，除非建立 Clustered 索引。

Unique: 保證沒有重複的資料，可以是 Clustered 或 Nonclustered 索引的屬性之一。

### 參考資料

- [Indexes - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/relational-databases/indexes/indexes?view=sql-server-ver16)
- [SQL Server and Azure SQL index architecture and design guide - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/sql-server-index-design-guide?view=sql-server-ver16)
- [30-11 之資料庫層的核心 - 索引結構演化論 B+樹 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10221111)