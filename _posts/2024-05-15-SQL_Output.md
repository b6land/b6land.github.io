---
layout: post
title: SQL Server 用 OUTPUT 指令輸出編輯結果
date: 2024-05-15 22:00:00 +0800
categories: [SQL Server]
--- 

`OUTPUT` 指令可以輸出被新增、修改、刪除的資料，適用於 `INSERT`、`UPDATE`、`DELETE`、`MERGE` 等語法。可用來檢查這些動作的結果是否正確*(請參閱[莫菲定律](https://en.wikipedia.org/wiki/Murphy%27s_law))*，或是結合 `INTO` 輸出結果至資料表內。

### 語法

`OUTPUT` 語法內可以使用保留字 `DELETED` 取得更改前的資料；`INSERTED` 取得更改後的資料，語法範例如下：

`DELETE`：

```sql
DELETE FROM [Table]
OUTPUT [DELETED].Column1, [DELETED].Column2,... -- 使用 DELETED 關鍵字取得刪除的值
WHERE [condition]
```

`INSERT`：

```sql
INSERT INTO [Table](Column1, Column2,...)
OUTPUT [INSERTED].Column1, [INSERTED].Column2,... -- 使用 INSERTED 關鍵字取得新增的值
VALUES (..., ..., ...)
```

`UPDATE`：

```sql
UPDATE [Table]
SET Column1=..., Column2=...
OUTPUT [INSERTED].Column1, ... -- 此處 INSERTED 是更新後資料
    [DELETED].Column1,... -- 此處 DELETED 是更新前資料
  INTO [Table2](Column1, Column2,...) -- 可以用 INTO 插入至別的資料表
WHERE [condition]
```

### 範例

```sql
-- 建立範例資料表
CREATE TABLE ExampleTable (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    Name NVARCHAR(50),
    Age INT,
    Gender NVARCHAR(10)
);

-- 插入資料並輸出自動產生的 ID
INSERT INTO ExampleTable (Name, Age, Gender) 
OUTPUT [INSERTED].ID 
VALUES ('John Doe', 30, 'Male'), ('Jane Smith', 25, 'Female'), ('Sam Brown', 40, 'Male');

-- 更新資料並輸出更新前後結果
UPDATE ExampleTable
SET Age = 45
OUTPUT [INSERTED].ID, [INSERTED].Name, [INSERTED].Age AS UpdatedAge, [DELETED].Age AS OriginAge
WHERE Name = 'Sam Brown'

-- 刪除資料並輸出其資料
DELETE FROM ExampleTable
OUTPUT [DELETED].ID, [DELETED].Name, [DELETED].Age, [DELETED].Gender
WHERE Age <= 25
```

插入資料後：

| ID  |
| --- |
| 1   |
| 2   |
| 3   |

更新後：

| ID  | Name | UpdatedAge | OriginAge |
| --- | --- | --- | --- |
| 3   | Sam Brown | 45  | 40  |

刪除後：

| ID  | Name | Age | Gender |
| --- | --- | --- | --- |
| 2   | Jane Smith | 25  | Female<br> |

### 參考資料

[\[小菜一碟\] 用 SQL Server 的 OUTPUT 語法撈出剛剛刪除的資料 - 軟體主廚的程式料理廚房 - 點部落](https://dotblogs.com.tw/supershowwei/2021/09/26/155836)
