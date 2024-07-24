---
layout: post
title: SQL Server 效能搶救 (7) 改進查詢效能的心得、延伸閱讀
date: 2024-07-24 17:00:00 +0800
categories: [SQL Server 效能搶救]
--- 

以下是個人在微調語法、調整商業邏輯時的一些心得，以及延伸閱讀。

### 微調語法善用索引

檢查 `GROUP BY` 的運算有沒有使用索引。對實體資料表建立索引後，`GROUP BY` 若能應用索引，效率會提高。不過在網路上查詢相關資料時，也有遇到有人指出「效果不大」。因此根據不同的使用狀況，而需要自己的最佳化策略。

此外，若能將 `LIKE %XXX%` 改成使用 `LIKE XXX%`，`LIKE XXX%` 將可以適當的使用索引。

### 減少不需要的資料

只在需要時加入 Join 資料表。

盡可能在資料最少時進行計算，例如有利用暫存表不斷地計算資料時，排序等會增加 CPU 成本的動作，可以在最後篩選出結果後再做。

如果有子查詢或大量連接不同資料表時，可以先透過條件式篩選，減少資料筆數以後，再加入關聯的資料表 (與欄位)。

### 瞭解商業邏輯

去了解這段語法的商業邏輯和原本的目標，說不定可以找到冗餘、造成效能負擔的邏輯。如果商業邏輯較為複雜，且可能由多個人在不同時間維護，偶爾會發生重複取得、計算資料的情形。

最重要的是「資料結果要正確」。

### 調整結果的觀察

SQL 效能調整常常需要因查詢語法、資料表規格、資料的分布狀況等條件，制定自己的調整方式，以達到最佳的查詢效能。因此調整完成後，請觀察：

1. 效能真的有提高嗎？不只是查詢，也要考慮包含寫入、更新和刪除的情形。
2. 語法調整以後，資料的正確性如何，查詢結果有沒有和調整前相同？如果有連帶調整邏輯，是不是符合預期結果？

若本文件有誤，歡迎指正，謝謝。

### 延伸閱讀

每一個細節都能談得更多，以下是各領域的相關資料：

#### Microsoft Learn (官方)

- [Indexes - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/relational-databases/indexes/indexes?view=sql-server-ver16 )
- [SQL Server 及 Azure SQL 索引架構與設計指南 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/sql-server-index-design-guide?view=sql-server-ver16)
- [分頁與範圍架構指南 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/pages-and-extents-architecture-guide?view=sql-server-ver16)

#### 經典教學文件

- [SQL Server Query Performance Guidelines Tutorial](https://www.mssqltips.com/sqlservertutorial/3200/sql-server-query-performance-guidelines-tutorial/)
- [Slow in the Application, Fast in SSMS?](https://www.sommarskog.se/query-plan-mysteries.html#plangenerate)

#### 解讀執行計畫

- [VITO の 學習筆記: 建立索引(1)-叢集與非叢集索引](http://vito-note.blogspot.com/2013/05/blog-post_5510.html)
- [VITO の 學習筆記: 效能調校(2)-分析執行計畫](http://vito-note.blogspot.com/2013/05/blog-post_2862.html)
- [VITO の 學習筆記: Stored Procedures](http://vito-note.blogspot.com/2013/05/stored-procedures.html)
- [CaryHsu - 學無止盡: 如何解讀 SQL Server 的圖型式執行計畫](http://caryhsu.blogspot.com/2011/06/sql-server.html)
- [How to read an execution plan with all details](https://www.sqlshack.com/how-to-read-an-execution-plan-with-all-details/)

### 其它

- [Blog - Brent Ozar Unlimited®](https://www.brentozar.com/blog/)
- [Think as sql optimizer, then code for it. - DBA jungle - Medium](https://medium.com/dba-jungle/think-as-sql-optimizer-then-code-for-it-f0bb2036cfa6)
- 拙作：[一些 SQL 效能最佳化、正規化的網頁整理 – Lazy Coding](/SQL_Useful_Link/)
- [文章摘要 - 搶救資料庫效能大作戰 – Lazy Coding](/SQL_Article_Improve_Database_Performance/)