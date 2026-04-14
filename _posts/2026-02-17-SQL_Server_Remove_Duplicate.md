---
layout: post
title: SQL Server 刪除重複資料
date: 2026-02-17 11:00:00 +0800
categories: [SQL Server]
--- 

如果想要刪除重複資料，可以用 `ROW_NUMBER()` 來為重複資料分組排序，並將每一組不是第一筆的重複資料移除掉。

### 程式碼

```sql
WITH TmpOrderdTable
AS
(
  SELECT 
    GroupID = ROW_NUMBER() OVER (PARTITION BY 欄位1, 欄位2, 欄位3 ORDER BY 主索引鍵)
  FROM
    [dbo].[MyTable]
)

DELETE FROM TmpOrderdTable WHERE GroupID > 1
```

NOTE: 這段程式碼有記錄在保哥 [如何刪除 SQL Server 資料庫中重複的資料 (兩種不同解法) - The Will Will Web](https://blog.miniasp.com/post/2010/08/11/Remove-duplicate-rows-from-a-table-in-SQL-Server) 文章裡，以下是自己理解的說明。

- ORDER BY 填入**數值類型**的**主索引鍵 (**例如：int)，例如 ID 欄位。
- 決定何謂重複資料： 若判斷重複資料的依據是看其中 3 個欄位，那就將這些欄位全部列入 `PARTITION BY`  子句中。

### 原理

1. 用 `ROW_NUMBER()` 先幫每個 ID 分組並排序。
2. 將分組結果存入 CTE。
3. 只保留第一筆 (`GroupID = 1`)，其它 (`GroupID > 1`) 的視為重複 → 刪掉。

#### ROW\_NUMBER() 視窗函數

- 它會根據 `PARTITION BY` (分組) 和 `ORDER BY` (排序) 的規則，幫每一筆資料編號。
- 舉例：如果 `MainTable` 裡有多筆相同的 `ID`，那麼 `ROW_NUMBER()` 就會從 1 開始依序編號。

#### CTE (Common Table Expression)

- SQL 的一個暫時結果集 (像是暫時的表格)。我們用它來「標記哪些是重複資料」。