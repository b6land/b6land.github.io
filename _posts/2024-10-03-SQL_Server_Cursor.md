---
layout: post
title: SQL Server 用 Cursor (指標) 迴圈處理資料
date: 2024-10-03 14:00:00 +0800
categories: [SQL Server]
--- 

在 SQL Server 中，也有類似其它程式語言裡 For 語法的邏輯：Cursor (指標)，可以用來迴圈處理資料。

### 說明

要用迴圈處理資料，可以看以下 `Cursor` 的使用方式：

```sql
DECLARE cursor_name CURSOR -- 宣告指標與要處理的資料
    FOR [SELECT 語法]  

OPEN cursor_name -- 開啟指標
FETCH NEXT FROM cursor_name -- 擷取第一筆資料

WHILE @@FETCH_STATUS = 0 -- 如果擷取成功
BEGIN
    [要處理的邏輯]
    FETCH NEXT FROM cursor_name -- 擷取下一筆資料
END

CLOSE level_cursor -- 釋放指標關聯的資料
DEALLOCATE level_cursor -- 釋放指標
```

`Cursor` 的效能比原本的 SQL 資料處理語法來得差，如果可以的話，應盡量使用如 `UPDATE`, `DELETE` 的語法大量處理資料。

### 範例

先插入範例資料：

```sql
-- 建立會員等級折扣的表格
CREATE TABLE MemberLevel (
    ID INT PRIMARY KEY,
    LevelName NVARCHAR(100),
    DiscountPercent DECIMAL(6,2)
);

-- 插入範例資料
INSERT INTO MemberLevel (ID, LevelName, DiscountPercent)
VALUES
(1, 'Normal', 0),
(2, 'Bronze', 5),
(3, 'Silver', 10),
(4, 'Gold', 15);
```

接著開始逐筆處理資料：

```sql
-- 宣告變數
DECLARE @ID INT, @CurrentPercent DECIMAL(6,2), @NewPercent DECIMAL(6,2)

-- 宣告指標
DECLARE level_cursor CURSOR FOR
SELECT ID, DiscountPercent
FROM MemberLevel

-- 開啟指標
OPEN level_cursor

-- 擷取第一筆資料
FETCH NEXT FROM level_cursor INTO @ID, @CurrentPercent

-- 當指標內還有資料時繼續執行
WHILE @@FETCH_STATUS = 0
BEGIN
    -- 計算新價格，增加 10% (未使用)
    -- SET @NewPercent = @CurrentPercent * 1.10
    -- 計算新價格：前一級倍率 * 0.9 + 5 %，最低 5 %
    SET @NewPercent = @LastPercent * 0.9 + 5
    SET @LastPercent = @NewPercent

    -- 更新折扣
    UPDATE MemberLevel
    SET DiscountPercent = @NewPercent
    WHERE ID = @ID

    -- 擷取下一筆資料
    FETCH NEXT FROM level_cursor INTO @ID, @CurrentPercent
END

-- 釋放指標相關資源
CLOSE level_cursor
DEALLOCATE level_cursor
```

其結果如下：

| ID <br> | LevelName <br> | DiscountPercent<br> |
| --- | --- | --- |
| 1 <br> | Normal <br> | 5.00<br> |
| 2 <br> | Bronze <br> | 9.50<br> |
| 3 <br> | Silver <br> | 13.55<br> |
| 4 <br> | Gold <br> | 17.20<br> |

上述的例子裡，要依照前一筆資料計算目前的折扣，展現了批次處理的必要性。如果要依照目前資料  + 10 % (如上方註解的程式碼)，其實不必用到 `Cursor`，直接使用以下的 `UPDATE` 語法即可代替：

```sql
-- 使用標準 SQL 大量更新
UPDATE MemberLevel
SET DiscountPercent = DiscountPercent * 1.10
```

不僅執行效率較高，程式碼也簡單很多。

### 參考資料

- [\[MS SQL\]寫給新手的Cursor小筆記 - 程式宅急便](http://kyleap.blogspot.com/2013/12/ms-sqlcursor.html)
- [DECLARE CURSOR (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/t-sql/language-elements/declare-cursor-transact-sql?view=sql-server-ver16)