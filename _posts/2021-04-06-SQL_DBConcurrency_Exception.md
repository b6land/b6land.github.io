---
layout: post
title: C# 中的 SQL 並行違規錯誤
date: 2021-04-06 12:00:00 +0800
categories:  [C#]
--- 

在寫資料庫讀寫指令的過程遇到此錯誤，在此紀錄 SQL 並行違規發生的可能原因與處理方式。

### 錯誤訊息

錯誤為 System.Data.DBConcurrencyException，訊息可能如下：

- 並行違規: 批次作業的命令已經影響必須是 2 記錄的 0。
- 並行違規: UpdateCommand 已經影響必須是 1 記錄的 0。

*(數字可能會因為資料數量不同而有所變動)*

### 遇到的錯誤

- 從已有的 DataTable 複製 DataRow 時，連同狀態一同複製，且呼叫了更新資料的指令。由於該 DataTable 並沒有該筆資料，DataAdapter 原本應插入新 DataRow，卻變成試圖更新不存在的 DataRow，導致發生錯誤。在此情形下，可檢查 RowState 是否為 Added，若 RowState 為 Modified 或其他值，則可能發生錯誤。

### 其它可能原因

- 資料無法關聯，例如 Table B 的 Primary Key 無法對應到 Table A 的 Foreign Key。
- 在一次交易 (Transcation) 內多次的執行更新指令 (Update Command) 。
- 有兩個或以上的使用者同時存取同一筆紀錄時造成。

### 解決方式

- 由於當初是使用 `DataTable.ImportRow()` 複製資料列，並呼叫 DataAdapter 儲存包含新資料列的資料表，由於 RowState 也一同複製 (沒有改變為 Added)，因此發生錯誤。
- 改用其它方式複製 DataRow 即可解決。

### 參考資料

- [並行違規: UpdateCommand 已經影響必須是 1 記錄的 0 - 竹林濤聲 - 點部落](https://dotblogs.com.tw/wesleybamboo/2009/11/13/11655)
- [將資料更新到資料庫時，發生這個問題:Concurrency violation - MSDN](https://social.msdn.microsoft.com/Forums/sqlserver/zh-TW/f55763c5-4b53-4ad1-9ea7-d2d5b3532971/23559360392600926356260322104036039260092423526178652923033229?forum=238)
- [使用 DataAdapter 更新資料來源 - ADO.NET - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/framework/data/adonet/updating-data-sources-with-dataadapters)