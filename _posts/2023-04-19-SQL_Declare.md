---
layout: post
title: SQL 的宣告變數與使用
date: 2023-04-19 12:00:00 +0800
categories: [SQL Server]
---

在 SQL 中，可以使用 `DECLARE` 宣告變數，方便作為查詢的條件或查詢的預設值使用。

### 宣告變數與使用

1. 使用 DECLARE 搭配 `@` 符號宣告變數，並設定型別。
2. 可以在同一個 DECLARE 描述裡宣告多個變數。
3. 尚未設定值之前，會是 NULL，因此要使用 SET 或 SELECT 設定變數的值。
4. 在查詢時使用變數。

範例:

```sql
DECLARE @QueryID AS VARCHAR(20), @SetID AS INT; -- 宣告變數與型別
SET @QueryID = '00001'; -- 設定變數值
SET @SetID = 50;

SELECT ClassID, ClassName, ClassType, @SetID
WHERE ClassID = @QueryID -- 相當於 WHERE ClassID = '00001'
```

### 效能的影響

- 根據參考資料 2，使用變數作為條件式時，可能會導致執行計畫判斷時更容易誤判預測的資料量，而導致查詢效能的降低。解決方式是將放入 `sp_executesql` 參數化，讓 Query Optimizer 能正確預測資料量。
- 若作為查詢、連接時的條件，使查詢範圍大幅減少時，仍可以加快連接的速度。

### 參考資料

1. [筆記。隨手: [MS SQL] 變數宣告與使用](http://sorryicannot.blogspot.com/2018/12/ms-sql.html "‌")
2. [Impact of SQL Variables on Performance (sqlshack.com)](https://www.sqlshack.com/impact-of-sql-variables-on-performance/)
3. [如何在 T-SQL 中宣告變數 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10009411)