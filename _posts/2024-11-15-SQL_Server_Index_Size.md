---
layout: post
title: SQL Server 檢查索引大小
date: 2024-11-15 23:30:00 +0800
categories: [SQL Server]
--- 

本文介紹 SQL Server 檢查索引大小的兩種方法。

### SSMS 圖形化報表

使用 SSMS (SQL Server Management Studio) 的狀況下，若想要檢查索引的大小，可以在物件總管內，對著資料庫點右鍵，選擇報表 → 標準報表，使用「依資料表的磁碟使用量」或是「依分割區的磁碟使用量」。

![從報表選單取得索引大小](/assets/imgs/2024-11-15/ssms_index_size_menu.png)  

可以看到每個資料表的各個索引大小：

![取得索引大小報表](/assets/imgs/2024-11-15/ssms_index_size_report.png)  

### 查詢語法

沒有安裝 SSMS 的話，可以用以下的語法檢查資料庫內每個資料表的各個索引大小：

```sql
SELECT tn.[name] AS [Table name], ix.[name] AS [Index name],
SUM(sz.[used_page_count]) * 8 AS [Index size (KB)]
FROM sys.dm_db_partition_stats AS sz
INNER JOIN sys.indexes AS ix ON sz.[object_id] = ix.[object_id] 
AND sz.[index_id] = ix.[index_id]
INNER JOIN sys.tables tn ON tn.OBJECT_ID = ix.object_id
WHERE sz.[index_id] > 0 
GROUP BY tn.[name], ix.[name]
ORDER BY tn.[name]
```

上方的語法會從動態管理檢視 (DMV)的 `sys.dm_db_partition_stats`  取得頁面的數量，並藉由連接索引資訊 `sys.indexes` ，取得索引物件並計算大小，最後連接 `sys.tables`  取得資料表名稱。

![以查詢語法取得索引大小](/assets/imgs/2024-11-15/query_index_size.png)  

更多索引大小的查詢變化，可以參考：[How to monitor total SQL Server indexes size](https://www.sqlshack.com/how-to-monitor-total-sql-server-indexes-size/)