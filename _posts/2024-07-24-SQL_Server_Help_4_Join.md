---
layout: post
title: SQL Server 效能搶救 (4) 執行計畫 - 連接方式
date: 2024-07-24 11:00:00 +0800
categories: [SQL Server 效能搶救]
--- 

為了將好幾個資料表的資料欄位一起呈現，我們會連接資料表。

例如 `INNER JOIN`, `LEFT JOIN`, `CROSS JOIN` 等等，這些連接是為了使用者的需求，可以稱之為 Logical Join (邏輯連接)。

SQL Server 包含了 Query Optimizer，會根據使用者的需求挑選最合適的 Physical Join (實體連接)方式。但是我們可以瞭解以下三種 Physical Join 在做什麼，當查詢變慢時才知道怎麼回事。

| 實體連接 | 說明 |
| --- | --- |
| Nested Loop Join | 直接掃描資料表連接，適合資料量較少的狀況，資料量變大就會掃描很久。 |
| Hash Join | 先建立雜湊表，根據雜湊值快速取得資料，但是較耗記憶體空間。 |
| Merge Join | 需要使用已排序過的資料作為查詢來源，適合已經建立索引的資料表。 |

### 減少連接的成本

連接時耗費過多時間要怎麼辦呢？

1. 試著建立查詢需要的索引，更快查詢到需要的資料。因為編譯執行計畫時，可以產生更合適的索引類型 (如 Merge Join)。
2. 連接時用條件篩選資料。減少資料的連接數量，就能減少耗費的時間

### 加入提示 (Hint)

為連接加入提示，可強制執行計畫使用特定連接方式。應該視為改善效能的最終手段，平時不使用。在某些 SQL 查詢內有用的提示，可能在其它查詢造成效能的反效果。  
  
強制使用特定連接方式的語法：

```sql
LEFT LOOP JOIN
INNER MERGE JOIN
LEFT HASH JOIN
```

接著來看索引的知識：[SQL Server 效能搶救 (5) 什麼是索引 – Lazy Coding](/SQL_Server_Help_5_Index/)

### 參考資料

- 微軟對三種連接的解釋：
  - [Understanding Merge Joins - Microsoft Learn](https://learn.microsoft.com/en-us/previous-versions/sql/sql-server-2008-r2/ms190967\(v=sql.105\)?redirectedfrom=MSDN)
  - [Understanding Hash Joins - Microsoft Learn](https://learn.microsoft.com/en-us/previous-versions/sql/sql-server-2008-r2/ms189313\(v=sql.105\)?redirectedfrom=MSDN)
  - [Understanding Nested Loops Joins - Microsoft Learn](https://learn.microsoft.com/en-us/previous-versions/sql/sql-server-2008-r2/ms191318\(v=sql.105\))
- [Introduction to Nested Loop Joins in SQL Server](https://www.sqlshack.com/introduction-to-nested-loop-joins-in-sql-server/)
- 拙作 [SQL Server 執行計畫中的資料表連接方式 – Lazy Coding](/SQLServer_Physical_Join/)