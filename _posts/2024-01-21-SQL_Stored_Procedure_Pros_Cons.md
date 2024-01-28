---
layout: post
title: SQL Stored Procedure 的優缺點與查詢內文
date: 2024-01-21 12:00:00 +0800
categories: [SQL]
---

Stored Procedure (儲存程序，也可稱為 SP) 是資料庫系統中儲存多個 SQL 邏輯，以便應用程式呼叫的資料庫物件，可作為子程式使用 \[1\]。

### Stored Procedure 的優缺點

優點：

1. 減少網路流量，不用傳送整串 raw SQL，只要呼叫 SP 名字就能使用並取得結果。
2. 如果有多個程式要呼叫一樣的查詢語法，不用重複多寫。
3. 封裝起來，隱藏複雜的商業邏輯，減少被修改的風險。\[2\]
4. 資料庫系統會對其預先產生執行計畫，不需要像 Raw SQL 每次執行時都重新產生執行計畫，增加執行時間。

缺點：

1. 因為不是寫在程式檔案內，不容易納入版本控制，無法追蹤每一次變更的內容。(替代作法：在 SP 內加入 Changelog，紀錄變更內容。)
2. 不像 ORM 一樣可以在 IDE 內透過偵錯模式執行，比較難找到邏輯上的錯誤。
3. 不容易透過搜尋找到所有需要修改的 SP 內容。

### 查詢含特定文字的Stored Procedure或View

因為 SP 沒辦法像程式專案一樣，可以透過 IDE 的「所有參考」找到使用物件的程式碼，或是用搜尋功能查詢整個專案，如果要一次查詢或修改多個 SP 的特定文字，需要透過以下的程式碼 \[3\] 查詢 SP 或 View 中的文字：

``` sql
SELECT DISTINCT o.name, c.*
FROM syscomments c
INNER JOIN sysobjects o ON c.id=o.id
WHERE (o.xtype = 'P'          --查SP
    OR o.xtype = 'V')              --查View
    AND (o.name LIKE '%Text%'  --查SP或View名稱
        OR c.text LIKE '%Text%')  --查SP或View內含文字
```

### 參考資料

- \[1\] [Stored procedure - Wikipedia](https://en.wikipedia.org/wiki/Stored_procedure)
- \[2\] [T-SQL筆記2\_Stored Procedure觀念\_為何要使用它\_如何建立](https://coolmandiary.blogspot.com/2018/02/t-sql2stored-procedure.html)
- \[3\] [\[MS SQL\]查詢含特定文字的Stored Procedure或View - AlenWu的程式學習筆記 - 點部落](https://dotblogs.com.tw/AlenWu_coding_blog/2019/03/12/102752)