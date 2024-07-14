---
layout: post
title: SQL 的鎖定擴大 (Lock Escalation)
date: 2023-01-01 12:00:00 +0800
categories: [SQL Server]
---

在檢查 SQL 語法發生死結的原因時，發現是鎖定擴大 (Lock Escalation，或稱鎖定升級) 造成死結，究竟是怎麼回事呢？ (適用於 SQL Server)

### 怎麼觀察死結的發生

1. 開啟 SQL Profiler。
2. 在建立追蹤時，在「使用範本」的欄位選擇 **TSQL_Locks** 範本。
3. 按下「執行追蹤」，並開始執行可能產生死結的語法，觀察是否產生 Lock 類別的事件。
4. 可檢查 Lock 事件相關的語法或 Deadlock Graph。

- 參考資料: [[SQL]紀錄 SQL Server 死結( Deadlock ) 的方法 - 五餅二魚工作室 - 點部落](https://dotblogs.com.tw/jamesfu/2021/09/25/Deadlock)

### 關於 Lock Escalation (鎖定擴大)

- 從 SQL Profiler 觀察到 Lock Escalation 事件時，可對照 [Lock:Escalation Event Class - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/relational-databases/event-classes/lock-escalation-event-class?view=sql-server-ver16) 檢查鎖定的類型 (Type)，以 Table Lock 來說， Type 會是 5 = OBJECT (table level)。
- 此現象將目前的鎖定層級提高。比如說 DELETE 或 UPDATE 作業通常是 Row Lock，當 SQL Server 因為存取量過大，而執行鎖定擴大時，將由 Row Lock 提升至 Table Lock。若此時有其它語法同時存取同一個 Table，有機會造成 DeadLock。
- 參考資料: [Lock Escalation (Database Engine) - Microsoft Learn](https://learn.microsoft.com/en-us/previous-versions/sql/sql-server-2008-r2/ms184286(v=sql.105)?redirectedfrom=MSDN)

### 避免鎖定擴大的方法

1. 把一次大量的批次作業分成多次較小的作業，例如把一次刪除 2022 年資料的語法，改寫成每次刪除 1000 筆 2022 年的資料，直到沒有資料為止。
2. 讓查詢盡可能有效率，避免大量的資料掃描或書籤查詢 (Bookmark Lookups) 導致鎖定擴大。做法是根據查詢的欄位建立合適的索引，讓查詢能夠只檢查少數的資料列，避免鎖定擴大。
3. 查詢時使用 SARGable 的條件，並搭配包含查詢條件欄位的索引。
4. 如果一定要執行大量的批次作業，可以建立語法使用 `WAITFOR DELAY` 關鍵字，可以使特定資料表保持 IX Lock (內部互斥鎖) 一段時間，避免鎖定擴大的發生。
- 參考資料 (MSDN): [Prevent lock escalation](https://learn.microsoft.com/en-us/troubleshoot/sql/performance/resolve-blocking-problems-caused-lock-escalation#prevent-lock-escalation)