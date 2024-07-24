---
layout: post
title: [SQL Server 效能搶救] 3. 執行計畫 - 數值解說
date: 2024-07-24 11:00:00 +0800
categories: [SQL Server 效能搶救]
--- 

當你移到執行計畫的某個節點時，會顯示以下資訊，來看看這些項目分別代表什麼意思。

### 重點說明

**連接方式**

雜湊連接、巢狀迴圈連接、合併連接等。

**索引方式**

索引搜尋、索引掃描 ─ 叢集、非叢集。

**實際的資料列數目**

實際查詢取得的資料量。

**估計的資料列數目**

基於統計計算的估計值。

- 應該減少至合理的資料量 (ex. 如果只要處理某部門的員工資料，不應該把整間公司的員工都抓進來)。
- 差異太大時，表示統計資訊老舊，可能產生不合適的執行計畫。

**Missing Index**

提示你可以建立什麼樣的索引，以增進查詢效能 (個人經驗: 請考慮使用的場合是否有大量修改、寫入或刪除，避免改善查詢後，反而導致其它動作的效能變差)

**排序**  

如果有合適已排序索引，會被拿來使用。盡可能只保留需要的資料欄位作排序。  
  
**述詞 (Predicate)**  

每個運算子的搜尋條件。

- Predicate: 會藉由「資料表掃描」篩選欄位內的資料。
- Seek Predicate: 搜尋條件，會藉由「索引搜尋」篩選欄位內的資料。

延伸閱讀：[sql server 2014 - Difference between Seek Predicate and Predicate - Database Administrators Stack Exchange](https://dba.stackexchange.com/questions/174860/difference-between-seek-predicate-and-predicate)

**警告**  

例如：隱含型別轉換 (轉型會導致效能降低，且無法利用索引)、排序警告 (如果因為資料量大時，需要從磁碟的 tempdb 排序)  
  
**估計 CPU 與 IO 成本**  

整個執行計畫花費多少百分比執行此項作業。請留意 0 % 的狀況，仍有耗費一點點成本。  
利用估計值計算出可能的 CPU 與 IO 成本，有可能不是實際上的資源耗費成本，例如，執行過程有呼叫 function。

### 參考資料

- [~楓花雪岳~: \[SQL\] 觀看執行計畫重點](https://jengting.blogspot.com/2013/12/executionplan-keypoint.html)  

- [Internals of Physical Join Operators (Nested Loops Join, Hash Match Join & Merge Join) in SQL Server](https://www.sqlshack.com/internals-of-physical-join-operators-nested-loops-join-hash-match-join-merge-join-in-sql-server/ )