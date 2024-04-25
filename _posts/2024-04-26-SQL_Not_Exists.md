---
layout: post
title: SQL 用 NOT EXISTS 取得不存在於子查詢中的資料
date: 2024-04-26 07:00:00 +0800
categories: [SQL]
--- 

`NOT EXISTS` 可以將**不存在**於子查詢中的資料列出來。

### 用途

假設今天有會員資料表、訪談資料表，想從中找出還沒有訪談的會員，就可以使用 `NOT EXISTS` 語法。

### 範例

以下會列出包含在 Member 會員清單資料表，但不在 Interview 訪談資料表內的紀錄：

1\. 加入範例資料。

``` sql
-- 建立 Member 資料表
CREATE TABLE Member (
    ID INT PRIMARY KEY,
    Name VARCHAR(50)
);

-- 插入範例資料到 Member 資料表
INSERT INTO Member (ID, Name) VALUES 
(1, 'John'),
(2, 'Alice'),
(3, 'Bob');

-- 建立 Interview 資料表
CREATE TABLE Interview (
    ID INT PRIMARY KEY,
    MemberID INT,
    InterviewDate DATE,
    InterviewNotes NVARCHAR(255),
    FOREIGN KEY (MemberID) REFERENCES Member(ID)
);

-- 插入範例資料到 Interview 資料表
INSERT INTO Interview (ID, MemberID, InterviewDate, InterviewNotes) VALUES
(101, 1, '2023-01-15', N'非常滿意現有的服務'),
(102, 2, '2023-02-20', N'需要第二次訪談');
```

2\. 使用 `NOT EXISTS` 找出還沒有訪談的會員。

``` sql
SELECT ID, [Name]
FROM Member M
WHERE NOT EXISTS (
    SELECT M.ID
    FROM Interview I
    WHERE M.ID = I.MemberID -- 要關聯欄位
);
```

3\. 結果如下：

| ID | Name |
|----|------|
| 3 | Bob |

### 參考資料

- 使用方法：[法蘭雞的學習筆記: SQL NOT EXISTS 怎麼用？](http://frankiestudy.blogspot.com/2012/01/sql-not-exists.html)
- 進階用法，請參考拙作：[SQL 更新資料─使用 MERGE 或 NOT EXISTS](/SQL_Merge_Not_Exists/)