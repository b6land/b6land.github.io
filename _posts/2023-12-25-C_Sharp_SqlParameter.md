---
layout: post
title: C# 使用 SqlParameter
date: 2023-12-25 12:00:00 +0800
categories: [C#]
---

C# 中有 SqlCommand 類別可以放入 Raw SQL，查詢時可以使用 SqlParamter 類別輕鬆的加入查詢的條件，並避免 SQL Injection。

### 用法

- SQL 查詢語法中，要加入的查詢條件設定名稱，並在前方加上 @ 符號。
- 用 SqlCommand 設定查詢語法，並在 Parameters.Add 的方法傳入 SqlParameter，第一個參數為查詢條件的名稱，第二個參數為查詢的變數。
- 遇到 SQL Injection 的控制符號時，會拋出例外。

**範例**

``` cs
SqlCommand command = new SqlCommand("SELECT * FROM Dogs1 WHERE Name LIKE @Name", connection);
command.Parameters.Add(new SqlParameter("Name", dogName));
```

更詳細的用法，請參閱：[C# SqlParameter Example - Dot Net Perls](https://www.dotnetperls.com/sqlparameter)