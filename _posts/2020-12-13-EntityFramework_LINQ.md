---
layout: post
title: C# Entity Framework 和 LINQ 摘要
date: 2020-12-13 12:00:00 +0800
categories: C#
tags: [C#, LINQ, Entity Framework]
--- 

本文是練習 Entity Framework 和 LINQ 時的教學網站與筆記整理。

### Entity Framework (EF)

- 使用 Entity Framework 前必須從 Visual Studio 安裝程式新增 Microsoft SQL Server Data Tools 功能。
- 如果要搭配 LocalDB 使用的話，必須從 SQL Server 安裝程式新增 LocalDB 功能。
- 在 ASP\.NET MVC 下可參考的教學：[認識Model - Model First - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10160947)，ASP\.NET MVC 可透過 Scaffold 語法自動產生 CRUD 表單。
- 建立 C# Windows Form 資料應用程式：[使用 ADO.NET 建立簡單的資料應用程式 - Visual Studio - Microsoft Docs](https://docs.microsoft.com/zh-tw/visualstudio/data-tools/create-a-simple-data-application-by-using-adonet?view=vs-2019)，建立模型後會透過**自動產生**的 Stored Procedure 插入資料。

### LINQ

- 全名為 Language-Integrated Query，提供類似於 SQL 語法的關鍵字，可在 C# 語言環境中提供可操作的物件。
- LINQ to SQL: LINQ 會將語法轉為 SQL 查詢語法，與 LINQ to Entities 不同，可參考 [LINQ to SQL - ADO.NET - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/framework/data/adonet/sql/linq/)。
- LINQ to Entities: LINQ 針對 Entity Framework 模型撰寫的查詢語法，可參考 [LINQ to Entities - ADO.NET - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities)。

### 其它名詞解釋、參考資料

- ADO\.NET: ADO\.NET 是 Microsoft 開發用於操作資料庫的函式庫，.NET 上的程式語言可用該技術存取關聯式資料庫 (ex. SQL) 與非資料庫型檔案 (ex. XML, Excel 檔案)，或獨立出來作為處理應用程式資料的類別物件。
- ADO 全名為 ActiveX Data Objects。
- 參考資料：[ADO.NET - 維基百科，自由的百科全書](https://zh.wikipedia.org/wiki/ADO.NET)、[ADO.NET簡介](http://www.ezonesoft.com.tw/WebDatabase/ADONET.htm)
- ADO\.NET 如何操作的基礎步驟收錄：[【程式學習之路：Day28】C#ADO.NET：Entity Framework 物件關聯對應、LINQ - by 莎莉 Sally - Medium](https://medium.com/sally-thinking/4b27943af679)