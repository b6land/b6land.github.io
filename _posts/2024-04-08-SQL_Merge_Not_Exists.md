---
layout: post
title: SQL 更新資料─使用 MERGE 或 NOT EXISTS
date: 2024-04-08 12:00:00 +0800
categories: [SQL]
--- 

本文介紹「有資料時更新，沒有資料時插入」時適用的 `MERGE` 語法。「沒有資料時才插入」則可以用 `INSERT` 語法搭配 `NOT EXISTS` 條件達成。

### 神秘的 MERGE 魔法

撰寫條件比對來源、目標資料表的資料，如果條件符合時，就執行 `UPDATE` / `DELETE` 語法；否則執行 `INSERT`。

常見的語法如下：

```sql
MERGE INTO 目標資料表
USING 來源資料表
ON 符合的條件
WHEN MATCHED THEN 
-- 符合條件時，更新或刪除語法
WHEN NOT MATCHED THEN 
-- 不符合條件時，插入語法
;
```

需要留意以下規則：

- 最後一定要有 ; 號。
- `MATCHED` 符合條件只能寫更新或刪除語法； `NOT MATCHED` 只能寫插入語法。
- `WHEN NOT MATCHED` 條件預設判斷目標資料表是否有符合的資料，可改為依據來源資料表判斷，但會執行插入語法。
- 使用了和 `INSERT` 等語法不同的鎖定機制，須留意平行作業時是否正常運作。

另外尚有許多需要留意的部分，可參閱官方文件。

先建立以下的表格：

```sql
-- 建立範例資料表
CREATE TABLE ExampleTable (
    ID INT PRIMARY KEY,
    Name NVARCHAR(50),
    Age INT,
    Gender NVARCHAR(10)
);

-- 插入範例資料
INSERT INTO ExampleTable (ID, Name, Age, Gender) VALUES (1, 'John Doe', 30, 'Male');
INSERT INTO ExampleTable (ID, Name, Age, Gender) VALUES (2, 'Jane Smith', 25, 'Female');
INSERT INTO ExampleTable (ID, Name, Age, Gender) VALUES (3, 'Sam Brown', 40, 'Male');
```

接著來嘗試 `MERGE INTO` 語法：

```sql
-- 宣告要插入 / 更新的資料
DECLARE @ID AS INT = 4;
DECLARE @Name AS NVARCHAR(50) = 'Real Lazy Coder';
DECLARE @Age AS INT = 20;
DECLARE @Gender AS NVARCHAR(10) = 'Male';

MERGE INTO ExampleTable Ex
USING (
    SELECT @ID AS ID
) Source ON Ex.ID = Source.ID -- 檢查 ID 欄位是否和插入的 ID 相同
WHEN MATCHED -- 符合條件時，更新 Name, Age 和 Gender 欄位
THEN
UPDATE SET Name = @Name, Age = @Age, Gender = @Gender
WHEN NOT MATCHED -- 不符條件時，新增資料
THEN
INSERT(ID, Name, Age, Gender)
VALUES(@ID, @Name, @Age, @Gender);
```

如果不習慣 `MERGE` 語法，可以改用 `IF` 搭配 `EXISTS` 判斷，請參閱參考資料 3。

### INSERT 搭配 NOT EXISTS 條件

「哇！我只是不想插入重複資料，需要用到這麼複雜的語法嗎？」

只要避免插入重複資料的話，也可以在 `INSERT` 內應用 `WHERE NOT EXISTS` 關鍵字，達到「找不到資料時才插入資料」的效果。範例如下：

```sql
-- 宣告要插入的資料
DECLARE @ID AS INT = 4;
DECLARE @Name AS NVARCHAR(50) = 'Lazy Coder';
DECLARE @Age AS INT = 30;
DECLARE @Gender AS NVARCHAR(10) = 'Male';

-- 使用原本的插入語法
INSERT ExampleTable (ID, Name, Age, Gender)
SELECT Source.ID, Source.Name, Source.Age, Source.Gender
FROM (
  SELECT @ID AS ID, @Name AS Name, @Age AS Age, @Gender AS Gender
) Source 
WHERE NOT EXISTS ( -- 若 ID 不存在 ExampleTable 內時，才插入資料
  SELECT ID
  FROM ExampleTable Ex
  WHERE Source.ID = Ex.ID -- 檢查的條件
) 
```

### 參考資料

1. [SQL高级知识——MERGE INTO - 知乎](https://zhuanlan.zhihu.com/p/69776710)
2. [SQL MERGE vs INSERT, UPDATE, DELETE Performance Considerations](https://www.mssqltips.com/sqlservertip/7590/sql-merge-performance-vs-insert-update-delete/) (用實驗告訴你 MERGE 和一般 `INSERT` , `UPDATE` 和 `DELETE`  的效能影響)
3. [How to update if row exists else insert in SQL Server - My Tec Bits](https://www.mytecbits.com/microsoft/sql-server/update-if-row-exists-else-insert) (使用 `IF` ... `EXISTS` 更新資料的做法)
4. [MERGE (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/t-sql/statements/merge-transact-sql?view=sql-server-ver16) (說明 `MERGE` 各項參數並提供範例，也在一開頭就告訴你可以用更簡單的 `NOT EXISTS` 語法)
5. [SQL 用 NOT EXISTS 取得不存在於子查詢中的資料](/SQL_Not_Exists/) (拙作)
