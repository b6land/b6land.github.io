---
layout: post
title: SQL Server 執行計畫中的資料表連接方式
date: 2022-12-20 12:00:00 +0800
categories: [SQL]
---

平常我們在使用 SQL 語法執行查詢時，會使用 `JOIN` 關鍵字連接不同資料表。但實際連接資料表時，其實有幾種不同的連接方式，以下是這些方式的介紹。 (適用於SQL Server)

### 連接方式

如果在 SQL Server Management Studio (SSMS) 或其它軟體內，在查詢時取得執行計畫，可以觀察到資料表的連接由以下三種方式構成：

- Nested Loop Join: 直接掃描資料表連接，適合資料量較少的狀況，資料量變大就會掃描很久。
- Hash Join: 先建立雜湊表，根據雜湊值快速取得資料，但是較耗記憶體空間。
- Merge Join: 需要使用已排序過的資料作為查詢來源，適合已經建立索引的資料表

![Hash Match](/assets/imgs/hash_match.png)
*(使用 Hash Match 連接的例子)*

在實際執行時，SQL 會自行依據預估的資料量，選擇合適的連接方式，以獲得最佳的執行效率，不需要我們自行去指定。

如果發現某種連接方式耗費了過多時間，應該試著建立索引，讓 SQL Server 能更快查詢到需要的資料，或在編譯執行計畫時能夠更準確地估計資料量，而使用合適的連接方式。

也可透過條件篩選需要的資料，減少資料的連接數量與耗費的時間。

### 參考資料

- [Internals of Physical Join Operators (Nested Loops Join, Hash Match Join & Merge Join) in SQL Server](https://www.sqlshack.com/internals-of-physical-join-operators-nested-loops-join-hash-match-join-merge-join-in-sql-server/)

- 雖然也可以強制連接時使用特定的連接方式，*但應該由有經驗的開發人員作為最後手段使用*:  [Join Hints (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/t-sql/queries/hints-transact-sql-join?view=sql-server-ver16)