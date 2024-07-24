---
layout: post
title: [SQL Server 效能搶救] 2. 執行計畫 - 產生、檢視與一點點背景知識
date: 2024-07-24 11:00:00 +0800
categories: [SQL Server 效能搶救]
--- 

每個 SQL 查詢都會被轉成執行計畫，因此要找出效能瓶頸，可以先從取得執行計畫開始。

### 產生執行計畫

要產生執行計畫，需要使用 SQL 語法，以下是幾種取得語法的方式：

1. 從程式或 SQL 檔案內取得 SQL 查詢語法。
2. 使用 SQL Profiler 取得資料 (2024 年也不錯的方法)。
3. 使用 ORM 查詢，通常也可以取得 SQL 語法。例如 [\[Data Access\] ORM 原理 (11): 效能議題 - 小朱® 的技術隨手寫 - 點部落](https://dotblogs.com.tw/regionbbs/2012/08/23/performance_considerations_in_orm_framework) 所提到的，Entity Framework 和其它 ORM，都會提供方法取得產生的語法。

取得語法後，接著使用 SQL Server Management Studio (SSMS) 執行查詢語法，並在查詢時取得估計和實際的執行計畫，以便檢查執行計畫中成本最高的一或多個查詢語法。

另一種方法是 Query Store，從 SQL Server 2016 後開始支援，啟用後容量上限內的執行計畫會被保存下來。這部份本篇不細談，請參考 [Monitor performance by using the Query Store - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store?view=sql-server-ver16) 和 [SQL Server Execution Plan 執行計畫 - The Skeptical Software Engineer](https://sdwh.dev/posts/2022/09/SQL-Server-Execution-Plan/)。

### 檢視執行計畫的工具

- SSMS 本身
- Azure Data Studio
- Sentry Plan Explorer
    - 免費工具。
    - 可以快速的載入大型的執行計畫。
    - 提供查詢中各項語法依欄位排序的功能，可用來找出最耗費成本的幾項查詢語法。
    - 官方網站：[Plan Explorer – SQL Query Analysis - SolarWinds](https://www.solarwinds.com/free-tools/plan-explorer)

### 查詢時如何產生執行計畫



1. 剖析 SELECT 語法，分成關鍵字、表達式、運算子、識別碼。
2. 建立成查詢樹，用以描述如何將來源資料變成查詢結果。
3. Query Optimizer 分析存取來源資料表的不同方法。然後從中選擇一系列的步驟，其使用較少資源且能最快的取得查詢結果。查詢樹會更新以記錄這一系列的步驟。最佳版本的查詢樹被稱為「執行計畫」。
4. Relational Engine 開始執行「執行計畫」，當取得資料的步驟已被處理後，Relational Engine 要求 Storage Engine 取得需要的資料列集合。
5. Relational Engine 處理從 Storage Engine 取得的資料，將資料轉成查詢結果需要的格式，並回傳至 Client。

重新改寫自 [Query Processing Architecture Guide - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/relational-databases/query-processing-architecture-guide?view=sql-server-ver16)。