---
layout: post
title: 一些 SQL 效能最佳化、正規化的網頁整理
date: 2023-08-21 12:00:00 +0800
categories: [SQL Server]
---

本文紀錄一些 SQL 效能最佳化、正規化的網頁連結，部份並附註個人心得。

### 最佳化

- [15個優化你的sql Query的方式 - Davidou的 Blog](http://blog.davidou.org/archives/609)
- [查詢 3 百萬筆資料的速度問題](https://social.msdn.microsoft.com/Forums/zh-TW/fac7d4a4-0870-46ea-b789-b1073facec78/2659735426-3-30334338363155836039260093034036895242302183938988?forum=240)
> 建立合適的索引，可以有效的改善 SQL Server 查詢效能。如果遇到查詢效能不佳的情形，都建議優先檢查索引是否有確實幫助到查詢。
- [德瑞克：SQL Server 學習筆記: SQL 效能調教：規劃記憶體(RAM Capacity)到比資料表、資料庫還要大的容量](http://sharedderrick.blogspot.com/2016/03/sql-ram-capacity.html)
- [MSSQL SQL效能的知識 - kinanson的技術回憶 - 點部落](https://dotblogs.com.tw/kinanson/2013/09/12/118235)

### 正規化

- [資料庫正規化](http://cc.cust.edu.tw/~ccchen/doc/db_04.pdf)
> 資料表要正規化，才能減少重複資料的數量，有效利用儲存媒體空間，以及避免重複資料導致的插入、更新和刪除錯誤。
- [後疫情時代的數位轉型關鍵心法 ─ 資料庫篇 – CIO Taiwan](https://www.cio.com.tw/key-heart-law-in-the-post-outbreak-age-database/)
> 但是正規化也帶來連接表格 (Join) 導致的效能影響，改善的做法包含：
> 1. 將常用的資料改以 JSON 儲存，建立在同一資料表後，以 NoSQL 查詢。
> 2. 善用 Stored Procedure 不須編譯、可參數化的特性。