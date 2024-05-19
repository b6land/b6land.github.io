---
layout: post
title: SQL Server 查詢變慢時，檢查索引的提示
date: 2024-05-19 21:00:00 +0800
categories: [SQL]
--- 

SQL 查詢速度很慢的原因，通常和索引的使用有關。如果對於查詢效率變差這件事感到毫無頭緒，不妨從以下幾個提示著手。

### 檢查索引的提示

1. 遇到平常執行速度很快，但偶爾會跑得很慢的查詢時，應該更新 SQL 內的統計資訊 \[1\]，讓 SQL Server 的 Query Optimizer 可以根據正確的資訊，挑選最合適的執行計畫。
2. 索引的欄位順序會影響到查詢的效率。查詢語法所使用的欄位，如果在索引欄位的後方，而且前方有查詢語法未使用的欄位，將無法有效的利用索引，請參考實驗 \[2\]；此外，如果順序較前方的欄位內資料區別性不足，那麼會需要花更多時間搜尋索引的樹狀結構，加速查詢的效果會變差。
3. 若有多個索引，並發現 SQL Server 的 Query Optimizer 使用了不適當的索引，可用  `WITH(INDEX([INDEX_NAME]))`  語法強制指定索引 (INDEX\_NAME 代換成你想使用的索引名稱)，避免產生不恰當的執行計畫 \[3\]。
4. 若要建立新的索引，要先檢查目前的資料量，並確認建立索引的時間會不會過長，影響 SQL Server 資料庫的其它作業。如果使用企業版，可以使用 `WITH ONLINE=ON` 參數一邊建立索引、一邊執行其它作業 \[4\]。

### 參考資料

- \[1\] [sql server - SQL query suddenly takes a long time to run - Super User](https://superuser.com/questions/745679/sql-query-suddenly-takes-a-long-time-to-run)
- \[2\] [Performance Impacts of Data Volume](https://use-the-index-luke.com/sql/testing-scalability/data-volume)
- \[3\] [\[SQL\] 強制指定索引](http://jengting.blogspot.com/2014/01/force-use-index.html)
- \[4\] [sql server 2008 r2 - How long will an index take to create? - Database Administrators Stack Exchange](https://dba.stackexchange.com/questions/10775/how-long-will-an-index-take-to-create)
- \[5\] 其它資料：[如何尋找MS SQL效能不好的索引 - iTe2 Blog](https://blog.ite2.com/如何尋找ms-sql效能不好的索引/)