---
layout: post
title: SQL Server 用 PATINDEX 找出特定模式文字的位置
date: 2024-12-24 22:00:00 +0800
categories: [SQL Server]
--- 

SQL Server 中，PATINDEX 語法可以用簡化的表達式語法，查詢特定模式 (規則) 文字的位置。

### 語法說明與範例

以下是語法說明：

```csharp
PATINDEX ('%pattern%', expression )  
```

`'%pattern%'`  表示要查詢的模式 (規則)，`expression`  表示被查詢的字串，例如查詢 ter 出現的位置：

```sql
SELECT PATINDEX('%ter%', 'interesting data') AS Position;  
```

查詢結果 Position = 3。下方則用另外一個簡易表達式，查詢「一二三」中任一字元出現的位置：

```sql
SELECT PATINDEX(N'%[一二三]%', N'第三天') AS Position;  
```

其回傳結果為 2，因為匹配到「三」。

下面的表達式可找出不匹配任一英文字母，其回傳結果為 4，因為第一個不匹配的字元為「1」。

```sql
SELECT PATINDEX('%[^A-Za-z]%', 'abc123') AS Position;  
```

### 可以使用的 match 運算子

SQL Server 沒辦法像其它程式語言一樣使用所有的正規表達式 (Regex) 符號，以下列出可以用的符號，又稱為 match 運算子：

| 運算子 | 說明  | 模式範例 | 符合模式範例的文字 |
| --- | --- | --- | --- |
| %   | 匹配 1 ~ 多個字元 | %ter% | pattern, theater |
| \[\] | 匹配任一個字元 | %\[一二三\]% | 星期一, 第二天 |
| \[^\] | 不匹配任一個字元 | %\[^A-Za-z\]%<br> | abc123, 2024 |
| \_  | 匹配單一任意字元 | m\_d%<br> | model, medium |

這些運算子適用於 `PATINDEX` 和 `LIKE` 語法。

### 參考資料

- 官方說明與語法範例：[PATINDEX (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/t-sql/functions/patindex-transact-sql?view=sql-server-ver16)
- 可以使用的字串運算子：[String operators (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/t-sql/language-elements/string-operators-transact-sql?view=sql-server-ver16)
- 使用範例：[\[MSSQL\] 在 T-SQL 使用 PATINDEX 搜尋關鍵字 ~ m@rcus 學習筆記](https://marcus116.blogspot.com/2019/03/mssql-t-sql-patindex.html)