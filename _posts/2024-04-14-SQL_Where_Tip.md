---
layout: post
title: SQL WHERE 語法的查詢條件
date: 2024-04-14 12:00:00 +0800
categories: [SQL Server]
--- 

這篇文章是 [C# LINQ to Entities 與 SQL 語法使用技巧](/SQL_and_LINQ_to_Entities/) 的重新整理再出發，收錄其中的 SQL WHERE 語法小技巧，並加入範例解說。

### SQL 語法的查詢條件: 中文、CONCAT、大小寫

- SQL 下查詢中文字串無資料、寫入中文變成問號 ? 時，可透過加入 N 在查詢字串前面，告知 SQL 的查詢語法需轉換為 Unicode 的形式。
- 可使用 `CONCAT` 語法 (MS-SQL 語法為 `+` 號) 同時查詢多欄位的字串。
- 測試資料：

``` sql
-- 建立 TestTable 表格
CREATE TABLE TestTable (
    ID INT,
    LastName NVARCHAR(50),
    FirstName NVARCHAR(50)
);

-- 插入資料 (將中文轉為 Unicode)
INSERT INTO TestTable (ID, LastName, FirstName)
VALUES (1, N'王', N'小明'),
       (2, N'李', N'大同'),
       (3, 'Wang', 'XiaoHua'),
       (4, 'wang', 'DaRen');

```

- 結合上方語法的查詢範例： 

``` sql
SELECT LastName, FirstName 
FROM dbo.TestTable 
WHERE LastName + FirstName LIKE N'%王小明%'
-- 結果 
-- LastName FirstName
-- 王       小明
```

- MS-SQL 的查詢條件預設是沒有大小寫區別的，如果需要在查詢時區分大小寫的查詢結果，可以嘗試以下的定序 (Collation)：

``` sql
SELECT LastName, FirstName 
FROM TestTable 
WHERE LastName = 'Wang' COLLATE Latin1_General_CS_AS; -- 區分大小寫
-- 結果 
-- LastName FirstName
-- Wang     XiaoHua

SELECT LastName, FirstName 
FROM TestTable 
WHERE LastName = 'Wang' COLLATE Latin1_General_CI_AS; -- 不區分大小寫
-- 結果 
-- LastName FirstName
-- Wang     XiaoHua
-- wang     DaRen
```

### 參考資料

- [SQL下查詢中文字串無資料或寫入中文 變成問號? @ SAP之鬼~~ :: 痞客邦 ::](https://saperp.pixnet.net/blog/post/4078081)
- [[T-SQL] 資料庫資料型態與數學符號，前置詞N.. - 炎龍牙 - 點部落](https://dotblogs.azurewebsites.net/dragoncancer/2016/02/02/173247)
- [SQL Concatenate 函数 - 1Keydata SQL 語法教學](https://www.1keydata.com/tw/sql/sql-concatenate.html)
- [個人筆記 - Character-Based型態的資料在SQL裡面儲存的Columns要選用什麼DataType？為何SQL在字串前面要加N？ - 己心亦凡 - 點部落](https://dotblogs.com.tw/eason/2011/05/25/26116)
- [SQL server ignore case in a where expression - Stack Overflow](https://stackoverflow.com/questions/1224364/sql-server-ignore-case-in-a-where-expression)
- [用定序解決SQL Server查詢 未分大小寫的問題 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10313369)