---
layout: post
title: SQL 資料類型 - varchar, time
date: 2023-08-21 12:00:00 +0800
categories: [SQL Server]
---

本文介紹常見的 Varchar 和 Time 屬性的部份特性。

### Varchar

- Varchar(8000) 表示可以存放 8000 個 Ansi 字元。
- Varchar(max) 可存放 2GB 的 Ansi 字元。
- 參考資料: [Difference between varchar(max) and varchar(8000) – SQLServerCentral](https://www.sqlservercentral.com/forums/topic/difference-between-varcharmax-and-varchar8000)

### Time

- Time(7) 表示的是秒後面的小數要表示到第幾位。

>time	16,7	5	09:31:35.6170000 // 和 time(7) 相同
>time(0)	8,0	3	09:31:36
>time(3)	12.3	4	09:31:35.617
>time(7)	16.7	5	09:31:35.6170000

- 參考資料: [Time Data Type in SQL Server - TekTutorialsHub](https://www.tektutorialshub.com/sql-server/time-data-type-in-sql-server/)