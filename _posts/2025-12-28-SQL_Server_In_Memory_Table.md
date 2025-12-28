---
layout: post
title: SQL Server In-memory Table
date: 2025-12-28 22:00:00 +0800
categories: [SQL Server]
--- 

SQL Server 2014 開始可以建立 In-Memory Table，以加速讀寫。

### 簡介與限制

In-Memory Table 的資料可以永久儲存，不會因為關機等原因，造成資料揮發。資料會分別儲存在記憶體和磁碟，所以在永久儲存的同時，也能透過記憶體存取加快讀寫速度。

可以搭配原生編譯的預存程序 (Stored Procedure，下稱 SP) 存取資料，不過不能用 ALTER 修改原生編譯 SP，一部分的函數也不能在原生編譯 SP 上執行，例如 `LOWER`  和 `REPLACE` 。如果有需要的話，還是可以用一般的 SP，但效能可能會稍差。

在 In-Memory Table 上，不適用一般資料表的 `UPDLOCK` , `HOLDLOCK`  等鎖定方式。

建立 In-Memory Table 時，需要為 Key Column 設定 Bucket Count 的大小，建議應比實際資料數量多。因為會建立每筆資料的 HASH (雜湊)，如果 Bucket Count 設得太小，會容易導致查詢 HASH 時遇到碰撞的問題，而大幅降低效能。

與 Redis 不同的地方，主要在於 Redis 是外部的快取，而且不同系統可以透過 Client 端存取 Redis 內的資料；而 In-Memory Table 和 SQL Server 整合在一起，需要用 SQL 語法存取資料。

### 建立表格的範例

```sql
CREATE TABLE xxx.[dbo].[ItemID]
(
	SerialNumber VARCHAR(12) NOT NULL PRIMARY KEY NONCLUSTERED HASH WITH (BUCKET_COUNT=100000), -- 設定 Key Column 的 Bucket Count
	Name VARCHAR(32) NULL ,
	UpdateTime DATETIME NULL 
) WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA) -- 設定為 In-Memory Table，並永久儲存資料
GO
```

### 詳細的建立方式與說明

1. [CaryHsu - 學無止盡: SQL Server 2014 In-Memory OLTP測試與使用心得](https://caryhsu.blogspot.com/2015/06/sql-server-2014-in-memory-oltp.html)
2. [In-Memory OLTP overview and usage scenarios - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/relational-databases/in-memory-oltp/overview-and-usage-scenarios?view=sql-server-ver17&viewFallbackFrom=sql-server-2014&redirectedfrom=MSDN)
3. [\[SQL Server\]\[In-Memory OLTP\]記憶內資料表BUCKET\_COUNT預估 - 史丹利好熱 - 點部落](https://dotblogs.com.tw/stanley14/2016/10/28/234914)
4. [記憶體最佳化的資料表簡介 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables?view=sql-server-ver17)
5. [Index Architecture and Design Guide - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/relational-databases/sql-server-index-design-guide?view=sql-server-ver17&redirectedfrom=MSDN#memory-optimized-hash-index-architecture)
6. ChatGPT