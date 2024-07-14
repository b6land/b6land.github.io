---
layout: post
title: SQL 用 SET STATISTICS 陳述式查看統計資訊
date: 2024-05-13 22:00:00 +0800
categories: [SQL Server]
--- 

在 MS-SQL 中，SET 陳述式 (SET Statement) 可以設定要不要輸出一些特定的資訊。`SET STATISTICS` 提供好幾種陳述式，可用來顯示查詢的統計資訊，快速確認查詢的效能。

### SET STATISTICS IO ON

可顯示出該次查詢的讀取資訊，對於判斷執行效率來說很實用。

| 項目  | 中文  | 說明  |
| --- | --- | --- |
| Scan count | 掃描計數 | 到達葉 (leaf) 層級後，搜尋 (seek) 或 掃描 (scan) 取得資料的次數，用於建立輸出結果。 |
| logical reads | 邏輯讀取 | 從資料快取讀取的分頁數量。 |
| physical reads | 實體讀取 | 從磁碟中讀取的分頁數量。 |
| read-ahead reads | 讀取前讀取 | 為了本次查詢，將資料放入快取的分頁數量。 |
| lob logical reads | LOB 邏輯讀取 | 從資料快取讀取的 LOB 分頁數量。 |
| lob physical reads | LOB 實體讀取 | 從磁碟中讀取的 LOB 分頁數量。 |
| lob read-ahead reads | LOB 讀取前讀取 | 為了本次查詢，將資料放入快取的 LOB 分頁數量。 |

- 分頁：SQL 資料儲存的基本單位。
- LOB：Large OBject 的縮寫，表示可以存放大量內容的資料類型，包含 `TEXT`, `NTEXT`, `IMAGE`, `VARCHAR(MAX)`, `NVARCHAR(MAX)`, `VARBINARY(MAX)` 等。
- 掃描計數的次數：使用 Unique Index 或 Clustered Index 查詢主鍵 (Primary Key)，而且只搜尋一個值的狀況，為 0；在 Non-Clustered Index 上搜尋一個值時，為 1；在葉層級找到索引鍵值，開始向左、右掃描的次數，為 N。

![掃描計數、邏輯讀取等統計資訊](/assets/imgs/2024-05-13/IO.png)

#### 參考資料

- 有掃描計數的語法範例：[試著看懂 SQL Server IO 統計資訊 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10185710 )  
- 官方說明：[SET STATISTICS IO (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/t-sql/statements/set-statistics-io-transact-sql )  
- LOB 大型物件的解釋：[What is LOB data in an SQL server? - Quora](https://www.quora.com/What-is-LOB-data-in-an-SQL-server)  
- 若想知道分頁是什麼，可以參考設計指南：[分頁與範圍架構指南 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/pages-and-extents-architecture-guide)

### SET STATISTICS TIME ON

可以顯示當次執行語法的 CPU 計算時間 / 查詢時間。

| 項目  | 中文  | 說明  |
| --- | --- | --- |
| CPU Time | CPU 時間 | CPU 查詢資料的時間 |
| Elapsed Time | 經過時間 | 查詢完成的時間，包含 CPU 時間，以及 IO、延遲、鎖定等其它耗費的時間 |

-  若經過時間 < CPU 時間，是因為有多個 CPU 核心平行查詢造成的。

![經過時間、CPU 時間](/assets/imgs/2024-05-13/Time.png)

#### 參考資料

- 官方說明：[SET STATISTICS TIME (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/t-sql/statements/set-statistics-time-transact-sql)  

- CPU 時間、經過時間的解釋：[Lesson Learned #452: Understanding CPU Time and Elapsed Time in SQL Query Execution - Microsoft Community Hub](https://techcommunity.microsoft.com/t5/azure-database-support-blog/lesson-learned-452-understanding-cpu-time-and-elapsed-time-in/ba-p/3986949)  

### 其它的 SET 陳述式

| 陳述式 | 說明  | 好文參考 |
| --- | --- | --- |
| SET ANSI\_NULLS ON | 可以用來設定查詢條件使用 `=`  和 `<>`  比較 NULL 時，比較的結果為何。 | [SET ANSI\_NULLS (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/t-sql/statements/set-ansi-nulls-transact-sql) |
| SET QUOTED\_IDENTIFIER ON | 可以決定要不要使用 ”  雙引號作為字串邊界。 | [\[SQL SERVER\]善用 QUOTED\_IDENTIFIER - RiCo技術農場 - 點部落](https://dotblogs.com.tw/ricochen/2014/11/02/147166 ) |
| SET NOCOUNT ON | 可以不顯示「N 筆資料列受到影響」，節省一點點網路流量。 | [德瑞克：SQL Server 學習筆記: 減少網路傳輸量，進而提升效能，以設定 SET NOCOUNT ON 不要回傳所影響的資料列筆數之訊息為例](http://sharedderrick.blogspot.com/2010/06/set-nocount-on.html ) |

- 也可以參考： [SET 陳述式 (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/t-sql/statements/set-statements-transact-sql?view=sql-server-ver16 )  

  
