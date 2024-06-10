---
layout: post
title: SQL 避免除以 0 的錯誤 - ISNULL 和 NULLIF 函數
date: 2022-04-16 12:00:00 +0800
categories:  [SQL]
---

在 SQL 計算除法時，如果遇到除以 0 的狀況要如何處理呢？`ISNULL` 和 `NULLIF` 函數可以幫助你避免產生錯誤，算出正確的結果。

### 介紹

遇到除法可能會除以 0 的情境時 (例如比較前後金額的花費比例)，SQL 會發出錯誤訊息並結束查詢。可以用 `ISNULL` 函式搭配 `NULLIF` 函式，除以 0 時以特定值代替。

``` sql
DECLARE @Money1 AS INT = 100;
DECLARE @Money2 AS INT = 0;

SELECT ISNULL(Money1 / NULLIF(Money2, 0), 0)
```

- `NULLIF(exp, exp)`: 當第一個運算式 (exp) 和第二個運算式相等的時候，回傳 NULL；否則回傳第一個運算式。
- `ISNULL(exp, value)`: 當第一個運算式等於 NULL 時，以 value 取代；否則回傳第一個運算式。

### ISNULL 的其它用途

- 當使用彙總函數 (如 `SUM()`) 時，可能會遇到部分資料的欄位數值為空的狀況，此時可以用 `ISNULL()` 函數將 NULL 取代為特定的值。例如下面指令會將價格欄位加總，遇到沒有價格的資料時，以 0 代替。

``` sql
-- 建立 Products 資料表
CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    ProductName VARCHAR(50),
    Price DECIMAL(10, 2)
);

-- 插入示範資料
INSERT INTO Products (ProductID, ProductName, Price) VALUES
(1, 'Apple', 10.00),
(2, 'Orange', NULL),
(3, 'Banana', 5.00),
(4, 'Grape', NULL);

SELECT SUM(ISNULL(Price, 0))
```

資料表如下：

| ProductID | ProductName | Price |
|-----------|-------------|-------|
| 1         | Apple       | 10    |
| 2         | Orange      | NULL  |
| 3         | Banana      | 5     |
| 4         | Grape       | NULL  |

則結果為：15。

- 加總時，若相加的欄位可能為 NULL 值，應以 `ISNULL(column, 0)` 代替，因為 NULL 加上任何數字都會為 NULL ，而導致計算錯誤。

``` sql
SELECT ISNULL(NULL, 0) + 100 -- 結果為 100
SELECT NULL + 100 -- 結果為 NULL
```

### 參考資料

- [德瑞克：SQL Server 學習筆記: SQL Server：認識 NULLIF 運算式](http://sharedderrick.blogspot.com/2012/07/t-sql-nullif.html)
- [德瑞克：SQL Server 學習筆記: SQL Server：認識 ISNULL 函數](http://sharedderrick.blogspot.com/2012/06/t-sql-isnull.html)
- [ISNULL (Transact-SQL) - SQL Server - Microsoft Docs](https://docs.microsoft.com/zh-tw/sql/t-sql/language-elements/nullif-transact-sql?view=sql-server-ver15)