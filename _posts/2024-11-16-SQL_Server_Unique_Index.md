---
layout: post
title: SQL Server 唯一索引 (Unique Index) 的優點和範例
date: 2024-11-16 13:00:00 +0800
categories: [SQL Server]
--- 

本文介紹唯一索引，並使用 SQL Server Management Studio (SSMS) 示範如何建立。

### 唯一索引的特性

唯一索引是指索引包含唯一 (UNIQUE) 屬性，用途是防止索引內使用的欄位有多個重覆值，可以和其它索引共存。

建立唯一索引後，如果要插入重覆的值，會導致錯誤，並退回目前的 INSERT 結果。

與「設計資料表時建立 UNIQUE 條件約束」相比，具有一樣的資料驗證方式。

### 有哪些優點

- 保證索引鍵的每一個資料組合都是唯一的。
- 可以保證資料完整性 (假如你儲存的資料其特性是「不會有重複組合」)。
- 提供額外資訊給 Query Optmizer，有助於產生更有效率的執行計畫。

### 範例

一開始的資料表，包含兩個非唯一索引。

![資料表概觀，已有兩個非叢集索引](/assets/imgs/2024-11-16/ssms_unique_index_table.png)  

在資料表的索引點右鍵，選擇「新增索引 → 非叢集索引 (或叢集索引)」。

![資料表右鍵選單](/assets/imgs/2024-11-16/ssms_unique_index_menu.png)

在「新增索引」的視窗，勾選「唯一」，並加入要索引的資料欄位，然後按下「確定」建立索引。

![新增索引時勾選唯一欄位](/assets/imgs/2024-11-16/ssms_unique_index_create.png)  

這時如果還想再加入一個 Smith, Will 的話 (這世界上的 Will Smith 特別多，除了影帝以外，大聯盟還有[兩位 Will Smith 連續五年獲得世界大賽冠軍](https://www.cna.com.tw/news/aspt/202410310196.aspx))，就會出現以下錯誤，並提醒你插入了重複的資料為何：

![不能插入重複的索引鍵資料列](/assets/imgs/2024-11-16/ssms_unique_index_message.png)  

如果沒有唯一索引，就可以加入另一個  Smith, Will。

### 參考資料

- 官方介紹特色和使用方式：[建立唯一索引 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/indexes/create-unique-indexes?view=sql-server-ver16)