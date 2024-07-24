---
layout: post
title: SQL Server 的字串分割
date: 2023-01-24 12:00:00 +0800
categories: [SQL Server]
---

這篇文章會提到在 SQL Server 中實作字串分割的幾種方式，達到類似 C# Split 字串處理方法的效果。

### 分割字串的方法

SQL Server 2016 以前的版本，並沒有提供字串分割的函式。因此要藉由複雜一點的方法來達成： 

1. 先轉成 XML
2. 查詢 XML，以達成欄位分割

程式碼如下：

```sql
DECLARE @Name VARCHAR(1000);
SET @Name = 'alice,bob,candy';
DECLARE @xml AS XML;
SET @xml = CONVERT(XML, '<n>' + replace(@Name, ',', '</n><n>') + '</n>') -- 將字串轉成 XML，並加上 n 標籤

SELECT T.n.value('.','varchar(10)') AS Name -- 使用 value 函式查詢 XML 的 n 節點
FROM @xml.nodes('n') T(n) 
```

`CONVERT` 可以轉換資料至不同資料類型，包含 XML。

`value` 則對 XML 執行 XQuery，並傳回 SQL 類型的值。XQuery 是一種用來查詢 XML 的語言，用在被放在 SQL 資料庫的 XML 資料。

執行後，上述程式碼的結果為：

| Name |
| --- |
| alice |
| bob |
| candy |

#### 參考資料

- [T-SQL使用逗號分隔字串當作WHERE IN條件-黑暗執行緒](https://blog.darkthread.net/blog/sql-where-in-csv-values/) *(程式碼引用自參考資料，並部分修改)*
- [CAST 和 CONVERT (Transact-SQL) - SQL Server - Microsoft Docs](https://docs.microsoft.com/zh-tw/sql/t-sql/functions/cast-and-convert-transact-sql?view=sql-server-ver15)
- [value() 方法 (xml 資料類型) - SQL Server - Microsoft Docs](https://docs.microsoft.com/zh-tw/sql/t-sql/xml/value-method-xml-data-type?view=sql-server-ver15)
- [XQuery Tutorial](https://www.w3schools.com/xml/xquery_intro.asp)

### 分割為左右兩欄，並取得右 (左) 方的資料

當字串被逗號分隔成左右兩欄時，以下程式碼可以用來取得右邊的資料！

```sql
DECLARE @Memo AS VARCHAR(10) = '1,2';
SELECT SUBSTRING(@Memo, CHARINDEX(',', @Memo) + 1, LEN(@Memo)) AS SecondColumn
```

以上的程式碼，分別使用：

1. `CHARINDEX` 找到逗號所在的位置。
2. `LEN` 取得字串長度。
3. 再使用 `SUBSTRING` 擷取逗號後面 ~ 字串結尾的字為子字串。

若稍作調整，改成從 0 取到逗號位置前面的字，就可以改成取得字串左邊的資料！

#### 參考資料

- 取得特定字元的位置：[SQL Server CHARINDEX() Function](https://www.w3schools.com/sql/func_sqlserver_charindex.asp)
- 取得字串長度：[SQL Server LEN() Function](https://www.w3schools.com/sql/func_sqlserver_len.asp)
- 取得子字串：[SQL Server SUBSTRING() Function](https://www.w3schools.com/sql/func_sqlserver_substring.asp)


### 其它方法

- 也可以使用 SQLCLR 進行字串分割。
- SQL Server 2016 和以後的版本，可以使用 `String_split` 函式分割字串，請參考 [SQL2016新函數String_Split - Rock的SQL筆記本 - 點部落](https://dotblogs.com.tw/rockchang/2017/04/06/001827)。