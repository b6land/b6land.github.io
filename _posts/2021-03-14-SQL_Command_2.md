---
layout: post
title: SQL 語法 - UNION、NOT EXISTS、CASE
date: 2021-03-14 12:00:00 +0800
categories:  [SQL]
--- 

這篇文章介紹 Union 和 Union All 語法的作用，Not Exists 與 Case 語法的使用時機，以及其它的 SQL 語法，包含註解的使用，以及如何取得目前時間。

### Union 和 Union All

- `Union`: 將上下兩句 SQL 語法的查詢結果合併起來，使用 SQL SELECT 查詢的結果欄位需要完全相同。如下範例：

``` sql
-- 建立範例資料
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

-- 執行合併資料的 SELECT 語法
SELECT Name FROM Person
UNION
SELECT Name FROM Member
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

- `Union All`: 與 `Union` 的作用類似，上下兩句查詢結果合併在一起，差異在於 `Union All` 會列出包含重複資料的所有結果，而 `Union` 僅列出不重複的所有結果。
  - 因為 `Union All` 不需要排序、找出重複的資料並刪除，效率會比 `Union` 更好。如果確認要合併的資料不會重複，或是重複了也沒關係，就可以使用 `Union All`。
- 參考資料：[SQL UNION ALL - 1Keydata SQL 語法教學](https://www.1keydata.com/tw/sql/sqlunionall.html)

### Not Exists 與 Case

- 可以透過子查詢，將**不存在**於子查詢中的資料列出來。以下會列出在 Person 內，但不在 Member 資料表內的紀錄：

``` sql
SELECT * FROM Person
WHERE NOT EXISTS (
   SELECT ID FROM Member WHERE Person.ID=Member.ID)
```

- 參考資料：[法蘭雞的學習筆記: SQL NOT EXISTS 怎麼用？](http://frankiestudy.blogspot.com/2012/01/sql-not-exists.html)

- 類似程式語言中 `if` 的寫法：`CASE`，關鍵字包含 `WHEN`, `THEN`和 `ELSE`，語法範例如下：

``` sql
SELECT Name, CASE
WHEN IsMember=0, THEN 'Not Member'
WHEN IsMember=1, THEN 'Member'
END AS IsMember
FROM Person
```

- 參考資料：[SQL CASE - SQL 語法教學 Tutorial](https://www.fooish.com/sql/case.html)

### 其它

- SQL 註解：區塊註解為 `/**/`，單行註解為 `--`
- SSMS 顯示行號的方式：從上方選單選取「工具 > 選項」，於左邊窗格展開至「文字編輯器 > 所有語言」，於「顯示」勾選「行號」。
- 參考資料：[德瑞克：SQL Server 學習筆記: 讓 SQL Server Management Studio 的「查詢編輯器」視窗，顯示「行號」](http://sharedderrick.blogspot.com/2008/11/sql-server-management-studio.html)
- 取得目前時間可使用 `GETDATE()`函數，參考資料： [GETDATE (Transact-SQL) - SQL Server - Microsoft Docs](https://docs.microsoft.com/zh-tw/sql/t-sql/functions/getdate-transact-sql?view=sql-server-ver15)
