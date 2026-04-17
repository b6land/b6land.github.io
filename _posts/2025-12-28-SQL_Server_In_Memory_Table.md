---
layout: post
title: SQL Server In-memory Table
date: 2025-12-28 22:00:00 +0800
categories: [SQL Server]
--- 

SQL Server 2014 開始可以建立 In-Memory Table，以加速讀寫。

### 儲存方式

In-Memory Table 的資料可以設定為永久儲存 (Durability = `SCHEMA_AND_DATA` )，不會因為關機等原因，造成資料揮發。資料會分別儲存在記憶體和磁碟，所以在永久儲存的同時，也能透過記憶體存取加快讀寫速度。

此外也可以設定為只存放在記憶體 (Durability = `SCHEMA_ONLY` )，SQL Server 重新啟動服務後，資料會消失，若有設定用 IDENTITY 遞增，也會重設為 1，但是資料表結構仍會保留。

### 原生編譯的預存程序(Native Compilation Stored Procedure)

可以搭配原生編譯的預存程序 (下稱 Native SP) 存取資料，以獲得較佳的效能。不過不能用 ALTER 修改 Native SP，一部分的函數也不能在 Native SP 上執行，例如 `LOWER`  和 `REPLACE` 。如果有需要的話，還是可以用一般的 SP 存取 In-Memory Table，但效能可能會稍差。

### 注意事項

在 In-Memory Table 上，不適用一般資料表的 `UPDLOCK` , `HOLDLOCK`  等鎖定方式。

建立 In-Memory Table 時，需要為 Key Column 設定 Bucket Count 的大小，建議應比實際資料數量多。因為會建立每筆資料的 HASH (雜湊)，如果 Bucket Count 設得太小，會容易導致查詢 HASH 時遇到碰撞的問題，而大幅降低效能。

建立 In-Memory Table 時，不支援 Sequence 的 `NEXT VALUE FOR` ，例如建立一般資料表的：

```sql
CREATE TABLE dbo.Orders (
    OrderID INT PRIMARY KEY DEFAULT (NEXT VALUE FOR dbo.MySequence),
    OrderDate DATETIME DEFAULT GETDATE()
);

```

會發生 10794 錯誤「記憶體最佳化資料表 不支援 'NEXT VALUE FOR'」。

```plain
查詢 In-Memory Table 的資料時，不能夠和其它資料庫的資料表 (包含用同義字查詢) 一起查詢，否則會出現「41317 A user transaction that accesses memory optimized tables or natively compiled modules cannot access more than one user database or databases model and msdb, and it cannot write to master.」 的錯誤。
```

### 與 Redis 的不同

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
5. [為記憶體最佳化的物件定義持久性 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/in-memory-oltp/defining-durability-for-memory-optimized-objects?view=sql-server-ver17)
6. [記憶體最佳化資料表和原生編譯的預存程序 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure?view=sql-server-ver17)
7. [Index Architecture and Design Guide - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/relational-databases/sql-server-index-design-guide?view=sql-server-ver17&redirectedfrom=MSDN#memory-optimized-hash-index-architecture)
8. ChatGPT