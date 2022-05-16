---
layout: post
title: SQL 除以 0
date: 2022-04-16 12:00:00 +0800
categories: SQL
tags: [SQL]
---

在 SQL 計算除法時，如果遇到除以 0 的狀況要如何處理。

### 介紹

遇到除法可能會除以 0 的情境時 (例如比較前後金額的花費比例)，SQL 會發出錯誤訊息並結束查詢。可以用 `ISNULL` 函式搭配 `NULLIF` 函式，除以 0 時以特定值代替。

{% highlight sql %}
ISNULL(Money1 / NULLIF(Money2, 0), 0)
{% endhighlight %}

- `NULLIF(exp, exp)`: 當第一個運算式 (exp) 和第二個運算式相等的時候，回傳 NULL；否則回傳第一個運算式。
- `ISNULL(exp, value)`: 當第一個運算式等於 NULL 時，以 value 取代；否則回傳第一個運算式。

### 參考資料

- [德瑞克：SQL Server 學習筆記: SQL Server：認識 NULLIF 運算式](http://sharedderrick.blogspot.com/2012/07/t-sql-nullif.html)
- [ISNULL (Transact-SQL) - SQL Server - Microsoft Docs](https://docs.microsoft.com/zh-tw/sql/t-sql/language-elements/nullif-transact-sql?view=sql-server-ver15)