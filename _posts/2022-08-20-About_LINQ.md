---
layout: post
title: LINQ 是不是一種 ORM ?
date: 2022-08-20 12:00:00 +0800
categories:  [C#]
---

本篇介紹 LINQ 與 ORM 之間的關係，並另外描述在 .NET 執行 SQL 邏輯的三種方法。

### LINQ 是不是一種 ORM 呢 ?
- ORM 是 Object Relation Mapper 的縮寫，可以用來輔助開發人員更簡單、安全的操作資料庫。
- LINQ 是一組查詢方法的集合，例如它可以用來查詢物件或資料表等。
- LINQ to SQL 是一個簡易的 ORM ，雖然缺乏了很多 ORM 應有的功能。進階的 Entity Framework (EF) 提供 LINQ 方法，且具備 ORM 的功能。
- 參考資料: [ls LINQ an ORM (Object Relational Mapper)? - Stack Overflow](https://stackoverflow.com/questions/8163513/ls-linq-an-orm-object-relational-mapper)

### 在 .NET 執行 SQL 邏輯的三種方法: 
- EF (LINQ): 可以避免一些寫純 SQL 遇到的錯誤，並避免 SQL Injection。
- 純 SQL: 能妥善利用 SQL 特性查詢、更新複雜資料，而且與 .NET 能互相合作 (ex. 查詢取得資料表後，利用迴圈處理資料)。
- Store Procedure: 效能最好，利於跨平台引用。
- 參考資料: [閒聊：用 LINQ 還是自己寫 SQL？-黑暗執行緒](https://blog.darkthread.net/blog/linq-or-direct-sql/)

### 其它

- [短小精悍的.NET ORM神器 -- Dapper-黑暗執行緒](https://blog.darkthread.net/blog/dapper)
