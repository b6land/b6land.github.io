---
layout: post
title: C# Dapper 查詢時，使用 IN 設定多個條件
date: 2024-04-28 15:00:00 +0800
categories:  [C#]
--- 

在 SQL 內的 WHERE 可以使用 IN 查詢包含多個數字、字串 ... 等條件的資料，在 Dapper 裡面其實也可以查詢多個參數條件。

### 使用方式

以下是一個查詢範例：

```csharp
var queryNames = new[] { "Alice", "Bob", "Candy" };
var sql = "SELECT * FROM Person WHERE Name IN @Names;";
var products = connection.Query(sql, new {Names = queryNames });
```

直接在 Query 方法的引數傳入陣列即可，參數名稱要和 SQL 查詢語法內的變數名稱相同。

### 一定要傳參數查詢多個條件嗎？

如果不傳參數，而是直接組合查詢語法與條件，可能會遇到 SQL Injection 的問題。Learn Dapper 網站提供一個登入查詢範例，回傳帳號和密碼：

```sql
SELECT * FROM users WHERE username = 'mike' AND password = 'pass1'
```

有心人士可以在密碼傳入字串 `' or ''='` ，查詢變成

```sql
SELECT * FROM users WHERE username = 'mike' AND password = '' or ''=''
```

就能查詢到所有的資料 (或因此順利登入系統)。

真的很危險！

透過 Dapper 傳入參數，會幫你確保安全，避免以上 SQL Injection 的情形發生。

### 參考資料

- 本篇文章的查詢語法，皆來自：[Dapper Parameter, SQL Injection, Anonymous and Dynamic Parameters](https://www.learndapper.com/parameters)
- 回答提供簡單的範例：[.net - SELECT \* FROM X WHERE id IN (...) with Dapper ORM - Stack Overflow](https://stackoverflow.com/questions/8388093/select-from-x-where-id-in-with-dapper-orm)