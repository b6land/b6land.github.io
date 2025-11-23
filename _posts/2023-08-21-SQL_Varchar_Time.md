---
layout: post
title: SQL 資料類型 - VARCHAR 基礎知識 & 調整欄位長度、time 的介紹
date: 2023-08-21 12:00:00 +0800
categories: [SQL Server]
---

本文介紹常見的 `varchar` 和 `time` 屬性的部份特性。

### `varchar` 基礎知識

`nvarchar` , `varchar` , `nchar` , `char`  都可以用來儲存文字，char 前面的字表示了它們的差異：

- `n` : 儲存 Unicode。
- `var` : 長度可變動，需要額外用 2 bytes 儲存長度。
    - 在建立索引時，`(n)varchar`  會因此會占用額外的容量。

效能上，因為存取 `nchar`  , `char`  不必再確認長度，會略快一點，但通常差距很小。

這些類型的後方都可以接(數字)，表示可容納的長度，例如：

- `varchar(8000)`  表示可以存放 8000 個 Ansi 字元。
- `varchar(max)`  可存放 2GB 的 Ansi 字元。

### 調整 `varchar` 欄位長度的語法

如果需要調整 `(n)varchar` 的最大長度，可以用以下語法：

```sql
-- 修改欄位長度
ALTER TABLE dbo.Addresses
ALTER COLUMN PKey NVARCHAR(255) NOT NULL;

-- 重建單一索引
ALTER INDEX IX_Addresses_PKey ON dbo.Addresses REBUILD;
```

- `ALTER COLUMN ...`  直接把 `PKey`  改為 `nvarchar(256)` ，可視需要修改長度。
- `ALTER INDEX <index_name> ...` 重建單一索引。
- `REBUILD` 會把索引資料頁重新整理，並且壓縮碎片。

### time 的介紹

可以用來儲存時間，`time(7)` 表示的是秒後面的小數要表示到第幾位。

| 型別 | 精確度 & 位數 | 大小 (Bytes) | 範例 | 備註 |
| --- | --- | --- | --- | --- |
| time | 16,7 | 5 | 09:31:35.6170000 | // 和 time(7) 相同 |
| time(0) | 8,0 | 3 | 09:31:36 |  |
| time(3) | 12,3 | 4 | 09:31:35.617 |  |
| time(7) | 16,7 | 5 | 09:31:35.6170000 |  |

### 相關參考資料

- [SQL欄位nvarchar, varchar, nchar, char比較 - 只是個打字的](https://blog.typeart.cc/SQL%E6%AC%84%E4%BD%8Dnvarchar,%20varchar,%20nchar,%20char%E6%AF%94%E8%BC%83/)
- [\[MSSQL\] 欄位開立(1) - nvarchar, varchar, nchar, char的抉擇 - 資料庫黑洞 - 點部落](https://dotblogs.com.tw/henryli/2014/05/27/145277)
- [Index Architecture and Design Guide - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/relational-databases/sql-server-index-design-guide?view=sql-server-ver17&utm_source=chatgpt.com#nonclustered-index-design-guidelines)
- [Resizing a VARCHAR/NVARCHAR column with indexes – SQLServerCentral Forums](https://www.sqlservercentral.com/forums/topic/resizing-a-varcharnvarchar-column-with-indexes)
- [Difference between varchar(max) and varchar(8000) – SQLServerCentral](https://www.sqlservercentral.com/forums/topic/difference-between-varcharmax-and-varchar8000)
- ChatGPT
- [Time Data Type in SQL Server - TekTutorialsHub](https://www.tektutorialshub.com/sql-server/time-data-type-in-sql-server/)