---
layout: post
title: SQL Decimal 類型筆記
date: 2024-02-29 12:00:00 +0800
categories:  [SQL Server]
--- 

SQL 的 decimal 可以用來儲存特定位數的整數與小數。與 float 不同的地方，在於 decimal 儲存精確的數字，而 float 則採取二進位儲存的方式，因此僅能表現出近似值。

### 類型說明

可以表示成以下形式：

decimal \[(p \[,s\])\]

- p 為精確度 (Precision) ，表示數值位數，含小數點前後的位數。
- s 為刻度 (Scale)，表示小數點後的位數。

decimal 和 numeric 在 MS-SQL 中，功能是完全相同的。

### decimal 與 float 的應用場景

- decimal 適合用於需要高精度的計算，如金額加減、科學計算。
- float 適合用於對精度要求不高，且需要非常大數值範圍的情況，如測量數據，因為其可以接受到 1.79E+308 (1.79 乘以 10 的 308 次方) 的大小。

### 程式範例

```sql
-- 建立表格
CREATE TABLE FinancialData (
    Amount decimal(10, 2)
);

-- 插入 decimal 資料
INSERT INTO FinancialData (Amount) VALUES (123.45);
INSERT INTO FinancialData (Amount) VALUES (123456789.12); -- 會發生算術溢位錯誤
INSERT INTO FinancialData (Amount) VALUES (0.123456789); -- 儲存為 0.12

-- 更新資料
UPDATE FinancialData
SET Amount = 999.99
WHERE Amount = 123.45;

-- 檢查結果
SELECT Amount FROM FinancialData

-- 移除測試用表格
DROP TABLE FinancialData
```

### Bonus: 轉換 numeric 到資料類型 numeric 時發生算術溢位錯誤

若在執行 SQL Script 時發生這項錯誤，可能是因為整數或小數的長度設定錯誤導致的，例如欄位定義為 decimal \[9,6\] 時：

- 123.456789 可以正常輸入
- 123456789 會發生錯誤
- 0.123456789 會儲存為 0.123456

因此要擴充欄位的長度，或檢查資料是否輸入正確，才能解決此問題。

### 參考資料

- [decimal and numeric (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/t-sql/data-types/decimal-and-numeric-transact-sql?view=sql-server-ver16)
- [Understanding the SQL Decimal data type](https://www.sqlshack.com/understanding-sql-decimal-data-type/)
- [浮點數計算結果更接近正解？-黑暗執行緒](https://blog.darkthread.net/blog/float-vs-decimal-case/)