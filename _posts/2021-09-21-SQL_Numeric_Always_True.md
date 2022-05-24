---
layout: post
title: SQL 數值處理與多重條件簡化
date: 2021-09-21 12:00:00 +0800
categories: SQL
tags: [SQL]
--- 

本文介紹如何處理含小數的數值進退位、`DATEDIFF` 與 `DATEADD` 等數種日期函式的應用，以及使用程式建立 SQL 語法時，如何簡化多重條件。

### SQL 四捨五入與進退位

- 包含 `CEILING(numeric_expression)` (無條件進位至整數)、`FLOOR(numeric_expression)` (無條件捨去至整數)、 `ROUND(numeric_expression,length)` (四捨五入至特定位數)。
- 使用 `ROUND` 時，建議先將數值轉成 `DECIMAL` 再處理，避免溢位。
- 參考資料: [SQL 數值進位處理與四捨五入 @ 月神的咖啡館 :: 痞客邦 ::](https://byron0920.pixnet.net/blog/post/56498636)

### 取得當月的 1 日

語法: 

``` sql
SELECT DATEADD(m, DATEDIFF(m, 0, '2020/10/15'), 0)
```

- `DATEDIFF(datepart, startdate, enddate)`: 上方語法中的 m 為月份的縮寫，表示要取得月份的差距，起始時間為 0，結束時間為 2020/10/15，將取得至今為止所有的月份差距。可使用的縮寫請參考官方語法說明。

- `DATEADD(datepart, number, date)`: 上方語法中的 m 為月份的縮寫，表示增加數量的單位，number 為增加的數量，date 為被計算的日期。將上方的 DATEDIFF 加入後，表示加入至今的所有月份至 0 上。結果即為當月的 1 日。

- 官方語法說明: [DATEDIFF (Transact-SQL) - SQL Server - Microsoft Docs](https://docs.microsoft.com/zh-tw/sql/t-sql/functions/datediff-transact-sql)。

### 取得當年的第 1 日

- 以下的語法，可以取得當年度的第一天，並轉為 `NVARCHAR` 型態。

``` sql
SELECT CONVERT(NVARCHAR, YEAR(GETDATE())) + '/01/01' 
```

- 使用 `GETDATE` 函數取得日期，接著以 `YEAR` 函數取得日期中的年份，並以 `CONVERT` 轉為 `NVARCHAR` 型態以後，再加上當年一月一日的字串。

- 參考資料: [Add and Subtract Dates using DATEADD in SQL Server](https://www.mssqltips.com/sqlservertip/2509/add-and-subtract-dates-using-dateadd-in-sql-server/)

### SQL 的多重條件簡化

- `1=1` 表示永遠成立。
- 可以應用在使用程式碼串接 SQL 的情況下，用於簡化程式碼。
- 若要選擇性的加入多個條件，需要寫出多個判斷式，以檢查是否要在前方加 `AND` 關鍵字。
- 加入 `1=1` 條件後，條件一律可在前面加 `AND`，條件效果不受影響，且能簡化判斷式。

``` sql
WHERE 1=1
AND ID = 'A123'
AND DATE = '2020/5/1'
```

- 參考資料: [[SQL] WHERE 1=1 做什麼用的? - CHF's note - 點部落](https://dotblogs.com.tw/invercent914/2013/09/16/118728)