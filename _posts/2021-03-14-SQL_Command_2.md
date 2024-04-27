---
layout: post
title: SQL 語法 - UNION、NOT EXISTS、CASE
date: 2021-03-14 12:00:00 +0800
categories:  [SQL]
--- 

這篇文章介紹 `UNION` 和 `UNION ALL` 語法的作用與範例，`NOT EXISTS` 與 `CASE` 語法的簡介，以及其它的 SQL 語法，包含註解的使用，以及如何取得目前時間。

### UNION 和 UNION ALL

`UNION` 可以將上下兩句 SQL 語法的查詢結果合併起來，使用 `SELECT` 查詢的結果欄位需要完全相同。如下範例：

``` sql

CREATE TABLE Person (
    Name VARCHAR(50),
    Sex VARCHAR(10),
    City VARCHAR(50)
);

CREATE TABLE Member (
    Name VARCHAR(50),
    Sex VARCHAR(10),
    Account VARCHAR(50),
    City VARCHAR(50)
);

INSERT INTO Person (Name, Sex, City) VALUES
('John', 'Male', 'New York'),
('Alice', 'Female', 'Los Angeles'),
('Bob', 'Male', 'Chicago'),
('Emily', 'Female', 'Houston');

INSERT INTO Member (Name, Sex, Account, City) VALUES
('Alice', 'Female', 'alice123', 'Los Angeles'),
('David', 'Male', 'david456', 'San Francisco'),
('Emily', 'Female', 'emily789', 'Houston'),
('Frank', 'Male', 'frank012', 'Seattle');
```

接著執行合併資料的 SELECT 語法：

``` sql
SELECT [Name] FROM Person
UNION
SELECT [Name] FROM Member
```

結果如下：

```
Name
-----
Alice
Bob
David
Emily
Frank
John

```

可以看見重複的 Alice 和 Emily 只會出現一次。

`UNION ALL`: 與 `UNION` 的作用類似，上下兩句查詢結果合併在一起，差異在於 `UNION ALL` 會列出包含重複資料的所有結果，而 `UNION` 僅列出不重複的所有結果。

因為 `UNION ALL` 不需要排序、找出重複的資料並刪除，效率會比 `UNION` 更好。如果確認要合併的資料不會重複，或是重複了也沒關係，就可以使用 `UNION ALL`。

以上面的範例資料為例：

``` sql
SELECT [Name] FROM Person
UNION ALL
SELECT [Name] FROM Member
```

結果如下：

```
Name
-----
John
Alice
Bob
Emily
Alice
David
Emily
Frank

```

參考資料：[SQL UNION ALL - 1Keydata SQL 語法教學](https://www.1keydata.com/tw/sql/sqlunionall.html)

### NOT EXISTS 與 CASE

- NOT EXISTS 可以將**不存在**於子查詢中的資料列出來，其用法範例如下：

```sql
SELECT 資料欄位
FROM 資料表1
WHERE NOT EXISTS (
    SELECT 1
    FROM 資料表2
    WHERE 資料表1.欄位 = 資料表2.關聯欄位
);
```

- `CASE` 是類似其它程式語言中 `if` 的寫法，其格式如下：

```sql
CASE
    WHEN 條件成立 THEN 結果
    [其它的 WHEN ...]
    [ELSE 結果]
END;
```
- 詳細範例請參考拙作：
  - [SQL 用 NOT EXISTS 取得不存在於子查詢中的資料](/SQL_Not_Exists/)
  - [SQL 更新資料─使用 MERGE 或 NOT EXISTS](/SQL_Merge_Not_Exists/)
  - [SQL 的 CASE 條件敘述](/SQL_Case/)

### 其它

- SQL 註解：區塊註解為 `/**/`，單行註解為 `--`
- SSMS 顯示行號的方式：從上方選單選取「工具 > 選項」，於左邊窗格展開至「文字編輯器 > 所有語言」，於「顯示」勾選「行號」。
- 參考資料：[德瑞克：SQL Server 學習筆記: 讓 SQL Server Management Studio 的「查詢編輯器」視窗，顯示「行號」](http://sharedderrick.blogspot.com/2008/11/sql-server-management-studio.html)
- 取得目前時間可使用 `GETDATE()`函數，參考資料： [GETDATE (Transact-SQL) - SQL Server - Microsoft Docs](https://docs.microsoft.com/zh-tw/sql/t-sql/functions/getdate-transact-sql?view=sql-server-ver15)
