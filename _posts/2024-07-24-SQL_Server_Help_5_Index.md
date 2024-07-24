---
layout: post
title: SQL Server 效能搶救 (5) 什麼是索引
date: 2024-07-24 12:00:00 +0800
categories: [SQL Server 效能搶救]
--- 

索引可以改善 SQL 查詢時的效能，減少查詢時間。如同大部分的書本最前面都有目錄，我們可以透過目錄快速地找到想要的內容。

### 類型

那麼，有哪些索引的類型呢？  

| 類型 | 說明 |
| --- | --- |
| Hash | 利用雜湊表 (Hash Table) 存取資料。(速度較快) |
| Clustered | 資料經過排序並使用樹狀結構儲存。可藉由存在節點中的鍵值 (Key) 快速地取得資料。 |
| Non-clustered | 資料列的索引有經過排序，但關聯的資料列不保證有經過排序，除非建立 Clustered 索引。|
| Unique | 保證沒有重複的資料，可以是 Clustered 或 Non-clustered 索引的屬性之一。|

### 索引的查詢方式

**掃描 (Scan)**

對整個資料表進行掃描。

**索引掃描 (Index Scan) vs 索引搜尋 (Index Seek)**

- 索引掃描: 掃描整個非叢集索引，找出所需資料。
- 索引搜尋: 利用查詢參數搜尋非叢集索引，找出所需資料。(較快)

**叢集索引掃描 (Clustered Index Scan) vs 叢集索引搜尋 (Clustered Index Seek)**

- 叢集索引掃描: 掃描整個叢集索引，找出所需資料。
  - ex. 查詢沒有使用 SARGAble 的運算子
- 叢集索引搜尋: 利用查詢參數搜尋叢集索引，找出所需資料。
  - ex. 查詢條件有包含索引的欄位

### 建立索引

如果在執行計畫中，看到對整張資料表進行掃描，那麼用 `CREATE INDEX` 語法建立索引吧！

以下是該語法的範例。建立索引時，應該將查詢語法中會用到的資料庫欄位納入進去，查詢時才能用索引加速。

```sql
USE [AdventureWorks2019] -- 要使用的資料庫
GO

CREATE NONCLUSTERED INDEX Index_FirstName
ON [Person].[Person] ([FirstName]) -- 要建立索引的資料表與欄位，可以在括號中指定多個欄位和排序
GO
```

詳細用法，請參考[CREATE INDEX (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/t-sql/statements/create-index-transact-sql?view=sql-server-ver16)

### 特別注意

雖然看起來索引是萬靈丹，不適當地使用仍會有以下缺點：

- 額外占用磁碟或記憶體的空間，因此不要將資料表的所有欄位都加入索引。
- 沒有實際的改善查詢效能。
- 新增、修改、刪除資料時，都會耗費時間更新索引。

如果需要測試查詢語法在不同 Index 的效果，可以用 `WITH (INDEX ([IndexName]))` 提示強制指定要利用索引。

下一步，來改善查詢語法：[SQL Server 效能搶救 (6) 善用索引，使用 SARGAble 改進查詢條件 – Lazy Coding](/SQL_Server_Help_6_Sargable/)

也請參考拙作：[SQL Server 查詢變慢時，檢查索引的提示 – Lazy Coding](/SQL_Server_Check_Index/)