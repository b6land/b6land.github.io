---
layout: post
title: C# LINQ to Entities 與 SQL 語法使用技巧
date: 2021-01-06 12:00:00 +0800
categories:  [C#, LINQ, SQL]
--- 

本文是撰寫 ASP.NET MVC 專案時，留下的一些筆記。

### 以 LINQ to Entities 或 SQL 語法查詢資料 

- 撰寫 LINQ to Entities 的基礎 CRUD 語法時，除了 Read 方法 (`dbContext.Table.Find()`) 以外，基本上是以特定語法搭配 `dbContext.SaveChanges()` 儲存資料庫內容。
- 需要輸入原始的 MS-SQL 語法查詢時，可使用 `dbContext.Table.SqlQuery("SQL Command")` ，這個方法會回傳 Table class 的實體物件集合：

``` csharp
 using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```

- 查詢以外的寫入、更新資料庫等作業，可使用`Database.ExecuteSqlCommand()` 原生 SQL 語法執行資料庫的寫入指令。*(`Database.SqlQuery()` 可用原生 SQL 語法查詢 (SELECT) 資料)*
- 參考 [認識Model - LINQ to Entities 與 導覽屬性 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10161589)，雖然是以 ASP.NET MVC 為範例，但 Winform 也適用該語法。
- 參考資料: [原始 SQL 查詢-EF6 - Microsoft Docs](https://docs.microsoft.com/zh-tw/ef/ef6/querying/raw-sql)
- 參考資料: [如何使用 SQL Command 儲存、更新資料庫](https://docs.microsoft.com/zh-tw/ef/ef6/saving/transactions)

### SQL 語法的使用 (中文、CONCAT)

- SQL下查詢中文字串無資料或寫入中文變成問號 ? 時，可透過加入 N 在查詢字串前面，告知 SQL 的查詢語法需轉換為 Unicode 的形式。
- 可使用 CONCAT (MSSQL 語法為 + 號) 同時查詢多欄位的字串。
- 上方範例查詢語法： 
``` sql
SELECT * FROM dbo.TestTable WHERE Name+Address LIKE N'%小明%'
```

- 可以用以下方法查詢同一個欄位包含兩個不一樣值的紀錄 (Record)：

``` sql
SELECT
    a.transporter,
    b1.warehouseName as warehouse1,
    b2.warehouseName as warehouse2
FROM
    transportation a,
    warehouse b1,
    warehouse b2
WHERE
        a.warehouseId1 = b1.warehouseId
    AND
        a.warehouseId2 = b2.warehouseId
```


- 參考資料: [SQL下查詢中文字串無資料或寫入中文 變成問號? @ SAP之鬼~~ :: 痞客邦 ::](https://saperp.pixnet.net/blog/post/4078081)
- 參考資料: [SQL Concatenate 函数 - 1Keydata SQL 語法教學](https://www.1keydata.com/tw/sql/sql-concatenate.html)
- [個人筆記 - Character-Based型態的資料在SQL裡面儲存的Columns要選用什麼DataType？為何SQL在字串前面要加N？ - 己心亦凡 - 點部落](https://dotblogs.com.tw/eason/2011/05/25/26116)

### Entity Framework 與專案註記

- `DataDirectory` 用以替代應用程式目錄，在 ClickOnce 應用程式下則為產生的特定資料目錄。
- 以 Enttiy 建立資料庫後，需使用 "新增連接" 功能建立連線。
- 參考資料: [Where is DataDirectory ?](https://social.msdn.microsoft.com/Forums/sqlserver/en-US/dc31ea59-5718-49b6-9f1f-7039da425296/where-is-datadirectory-?forum=sqlce)
- 參考資料: [Model First-EF6 - Microsoft Docs](https://docs.microsoft.com/zh-tw/ef/ef6/modeling/designer/workflows/model-first)

### Entity Framework 的使用技巧

- 欲查詢多欄位的資料，先根據查詢的資料建立符合的類別，再使用以下語法：
``` csharp
var res = dbContext.Database.SqlQuery<TestEntity>("Select * from dbo.MyView").ToList();
```
- 增加 Entity Framework 讀寫效率的方法：1. 關閉自動偵測追蹤 2. 使用 `AddRange()` 一次加入所有資料再 `SaveChange()` 代替 `Add()` 一筆一筆加入資料。 

### 其它

- 用原生 SQL 好，還是 LINQ to Entities 好？: [閒聊：用 LINQ 還是自己寫 SQL？-黑暗執行緒](https://blog.darkthread.net/blog/linq-or-direct-sql/)
- 修改資料前，Entity Framwork 在背後自動執行 `Attach`: [天空的垃圾場: Entity Framework - 為什麼修改的資料不用Attach](http://blog.sanc.idv.tw/2014/08/entity-framework-attach.html)