---
layout: post
title: SQL Server 的 DATETIME 欄位會造成索引的效率變低嗎？
date: 2024-11-17 17:00:00 +0800
categories: [SQL Server]
--- 

透過疑問來探討 SQL Server 的 `DATETIME`  欄位如何儲存資料，要怎麼索引較為恰當。

### 疑問

如果在檢查 SQL Server 的效能問題時，發現索引或查詢語法中包含 `DATETIME`  欄位，或許會懷疑，`DATETIME`  欄位因為包含了日期與時間，是否需要在儲存時省略紀錄時、分、秒，來避免查詢索引的效率降低？要不要將日期和時間分成兩個欄位呢？

在 2023 年時，Reddit 上也有人表達了疑問 ([Index on Datetime column? : r/SQL](https://www.reddit.com/r/SQL/comments/1126849/index_on_datetime_column/))，裡面提到了更久以前在 StackOverflow 上的文章 ([How to improve performance for datetime filtering in SQL Server? - Stack Overflow](https://stackoverflow.com/questions/17381875/how-to-improve-performance-for-datetime-filtering-in-sql-server)) ，但是 Reddit 的鄉民卻說 StackOverflow 的熱門回答可能有問題 ...

一起來搞清楚是怎麼回事！

### 釐清

一開始需要先知道，SQL Server 的索引是採用稱為 B+ Tree ([Wikipedia](https://en.wikipedia.org/wiki/B%2B_tree)) 的樹狀結構，有一些關聯式資料庫的索引也採用類似的結構。

![B-Tree](https://upload.wikimedia.org/wikipedia/commons/3/37/Bplustree.png)  

B+Tree 示意圖，作者為 [Grundprinzip from Wikimedia](https://en.wikipedia.org/wiki/B%2B_tree#/media/File:Bplustree.png )  

這個結構可以快速地執行精準搜尋、範圍搜尋，而不需要掃描全部的索引。

而根據微軟官方的說明：[SQL Server 及 Azure SQL 索引架構與設計指南 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/sql-server-index-design-guide?view=sql-server-ver16#column-considerations)，這個結構在數值分散時 (例如 1, 2, 3)，加速效果會比數值集中 (例如 3, 3, 3) 來得好。因此我們可以知道「在儲存時省略時、分、秒」，反而會導致 `DATETIME`  欄位的資料變得更集中，而導致索引效率變差 (例如在省略分、秒的狀況下，`2024-04-22 13:24:56`  和 `2024-04-22 13:36:45`  都會被轉成 `2024-04-22 13:00:00` )。

[How SQL Server stores data types: dates and times - Born SQL](https://bornsql.ca/blog/how-sql-server-stores-data-types-datetime-date-time-and-datetime2/) 這篇文章提到，`DATETIME`  欄位的資料在資料庫內部會以二進位方式儲存。例如 `2020-04-22 00:00:00.000` 會被儲存為 `0x0000ABA500000000`，`2020-04-22 00:00:01.703`  會被存成 `0x0000ABA5000001FF`，佔據 8 個 Byte。而一般的 `DATE`  欄位只有 3 個 Byte，例如 `0x12190B` 是 `1992-04-27` 。因此若資料只需要表示日期的話，使用 `DATE`  欄位型別，有助於減少索引的大小，增加索引查詢速度；若同時需要紀錄日期與時間，則可繼續使用 `DATETIME`  型別。

因此我們的結論是：通常狀況下，儲存 `DATETIME` 欄位時，不需要省略時、分、秒的資訊，因為不會造成索引效率降低。若資料表還在設計階段，可視情況調整資料欄位型別。根據資料的分佈狀況，也可以嘗試不同的資料表設計、索引欄位、查詢語法，取得最佳的效能表現。

### 延伸閱讀

- 回答有 B+ Tree 的特性簡介：[How to create B+ tree index Microsoft SQL - Microsoft Q&A](https://learn.microsoft.com/en-us/answers/questions/620224/how-to-create-b-tree-index-microsoft-sql "https://learn.microsoft.com/en-us/answers/questions/620224/how-to-create-b-tree-index-microsoft-sql")
- 來實際試試看 [B+ Tree Visualization](https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html "https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html") 是怎麼找資料的吧！
- `DATETIME`  欄位與索引的範圍搜尋：[sql server - Understanding about SQL index on DateTime Column - Database Administrators Stack Exchange](https://dba.stackexchange.com/questions/280450/understanding-about-sql-index-on-datetime-column "https://dba.stackexchange.com/questions/280450/understanding-about-sql-index-on-datetime-column")
- 學習資源 1：[The Search Tree (B-Tree) Makes the Index Fast](https://use-the-index-luke.com/sql/anatomy/the-tree)
- 學習資源 2：[SQL Server index structure and concepts](https://www.sqlshack.com/sql-server-index-structure-and-concepts/)