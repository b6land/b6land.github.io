---
layout: post
title: SQL Server 的字串分割
date: 2023-01-24 12:00:00 +0800
categories: [SQL Server]
---

這篇文章會提到如何在 SQL Server 中實作字串分割 (類似 C# 字串的 Split 方法)。

### 摘要

在 SQL Server 中，並沒有直接提供字串分割的函式。因此要藉由複雜一點的方法來達成： 

1. 先轉成 XML
2. 查詢 XML，以達成欄位分割

### 程式碼

```sql
DECLARE @Name VARCHAR(1000);
SET @Name = 'alice,bob,candy';
DECLARE @xml AS XML;
SET @xml = CONVERT(XML, '<n>' + replace(@Name, ',', '</n><n>') + '</n>') -- 將字串轉成 XML，並加上 n 標籤
SELECT ID 
FROM Person P
WHERE P.Name IN (
    SELECT T.n.value('.','varchar(10)') -- 使用 value 函式查詢 XML 的 n 節點
    FROM @xml.nodes('n') T(n) 
)
```

`CONVERT` 可以轉換資料至不同資料類型，包含 XML。

`value` 則對 XML 執行 XQuery，並傳回 SQL 類型的值。XQuery 是一種用來查詢 XML 的語言，用在被放在 SQL 資料庫的 XML 資料。

### 參考資料

- [T-SQL使用逗號分隔字串當作WHERE IN條件-黑暗執行緒](https://blog.darkthread.net/blog/sql-where-in-csv-values/) *(程式碼引用自參考資料，並部分修改)*
- [CAST 和 CONVERT (Transact-SQL) - SQL Server - Microsoft Docs](https://docs.microsoft.com/zh-tw/sql/t-sql/functions/cast-and-convert-transact-sql?view=sql-server-ver15)
- [value() 方法 (xml 資料類型) - SQL Server - Microsoft Docs](https://docs.microsoft.com/zh-tw/sql/t-sql/xml/value-method-xml-data-type?view=sql-server-ver15)
- [XQuery Tutorial](https://www.w3schools.com/xml/xquery_intro.asp)

## 其它方法

- 也可以使用 SQLCLR 進行字串分割。
- SQL Server 2016 和以後的版本，可以使用 String_split 函式分割字串。
- 參考資料: [SQL2016新函數String_Split - Rock的SQL筆記本 - 點部落](https://dotblogs.com.tw/rockchang/2017/04/06/001827)