---
layout: post
title: SQL 計算與上一筆的差值
date: 2023-03-07 12:00:00 +0800
categories: [SQL Server]
---

這篇文章介紹如何計算一組資料中，該筆與上一筆的差值，例如計算當月與上個月的金額差多少。

### 計算方式

0. 假設我們要計算每個月存款的差異，先建立每月存款資料表與測試資料。

``` sql
CREATE TABLE MonthSaving(
    YearMonth VARCHAR(7),
    Saving MONEY
)

INSERT INTO MonthSaving (YearMonth, Saving)
VALUES ('2022-08', 10060), ('2022-09', 10236), ('2022-10', 10361), ('2022-11', 10765), ('2022-12', 10987)
```

1. 首先，先依照需要的欄位排序並呼叫 `ROW_NUMBER()` 取得序號欄位，此處為 YearMonth 欄位為例。

``` sql
SELECT YearMonth, Saving
    , ROW_NUMBER() OVER (ORDER BY YearMonth ASC) AS ID 
FROM MonthSaving M
```

2. 將上述的資料存入暫存表，或寫為 CTE，此處寫為 CTE。

``` sql
WITH MonthSavingOrder (YearMonth, Saving, ID)
AS
(
    SELECT YearMonth, Saving
        , ROW_NUMBER() OVER (ORDER BY YearMonth ASC) AS ID 
    FROM MonthSaving M
)
```

3. 從原本資料表取得所有資料，並使用 `INNER JOIN` 依照識別欄位連接暫存表或 CTE (此處使用 YearMonth)，此暫存表再用 `LEFT JOIN` 連接自己，但序號 + 1。然後再計算兩個暫存表的差異即可。

``` sql
SELECT MS.YearMonth, MS.Saving, MSO.Saving - MSO2.Saving AS Income
FROM MonthSaving MS
INNER JOIN MonthSavingOrder MSO ON MS.YearMonth = MSO.YearMonth
LEFT JOIN MonthSavingOrder MSO2 ON MSO.ID = (MSO2.ID + 1) 
```

上面的範例將計算上個月到本月間存入的金額，並命名為 Income 欄位。

4. 處理 NULL 欄位。

### 參考資料

- 另一種做法: [[SQL]計算與上一筆的差值 - topcat 姍舞之間的極度凝聚 - 點部落](https://dotblogs.com.tw/topcat/2020/06/02/DiffWithPreRow)
- [[進階SQL]With As進行子查詢(CTE)[SQL-004] - jimmy-wang - Medium](https://medium.com/jimmy-wang/e045147f0317)

### 完整範例

``` sql
CREATE TABLE MonthSaving(
    YearMonth VARCHAR(7),
    Saving MONEY
)

INSERT INTO MonthSaving (YearMonth, Saving)
VALUES ('2022-08', 10060), ('2022-09', 10236), ('2022-10', 10361), ('2022-11', 10765), ('2022-12', 10987)
;
WITH MonthSavingOrder (YearMonth, Saving, ID)
AS
(
    SELECT YearMonth, Saving
        , ROW_NUMBER() OVER (ORDER BY YearMonth ASC) AS ID 
    FROM MonthSaving M
)
SELECT MS.YearMonth, MS.Saving, MSO.Saving - MSO2.Saving AS Income
FROM MonthSaving MS
INNER JOIN MonthSavingOrder MSO ON MS.YearMonth = MSO.YearMonth
LEFT JOIN MonthSavingOrder MSO2 ON MSO.ID = (MSO2.ID + 1) 
;
DROP TABLE MonthSaving
```