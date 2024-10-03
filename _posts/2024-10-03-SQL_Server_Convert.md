---
layout: post
title: SQL Server 用 CONVERT 轉換日期、數字為字串
date: 2024-10-03 08:00:00 +0800
categories: [SQL Server]
--- 

以下蒐集 SQL Server 中常用的 `CONVERT`  轉換語法，相信我，用得到！

### 語法範例

`CONVERT`  函數的語法如下：

```sql
CONVERT (資料類型 [ 長度 ], 表達式 [, 格式 ])
```

如何將 DATETIME 格式的資料轉為 YYYYMMDD (西元年月日) \[1\]：

```sql
DECLARE @Birthday AS DATETIME = '2024-10-01 18:00:00' -- 宣告測試用變數
SELECT CONVERT(VARCHAR(8), @Birthday, 112) AS OnlyDate
-- 輸出：20241001
```

112 指的是 ISO 標準的日期格式。更多日期、時間的格式，可以參考官方文件 \[4\] (永遠是好朋友！)。

除了產生不同的日期以外，也可以將數字轉為字串 \[3\]：

```sql
SELECT CONVERT(VARCHAR(8), 987654)
-- 輸出：987654
```

`CONVERT`  函數還可以：

- 轉成 XML，並分隔字串 \[5\]。
- 將 DATETIME 轉為 DATE，只保留日期，去除時間 \[6\]。

### 延伸閱讀  

- \[1\] [SQL Convert Date to YYYYMMDD](https://www.mssqltips.com/sqlservertip/6452/sql-convert-date-to-yyyymmdd/)  
- \[2\] 各種格式化日期/時間的方法 (不限於 Convert)： [How to Format Date/Time](https://networking.ringofsaturn.com/SQL/howtoformatdatetime.php)
- \[3\] [Convert Integer to String in SQL Server](https://www.c-sharpcorner.com/blogs/convert-integer-to-string-in-sql-server )  
- \[4\] [CAST and CONVERT (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/t-sql/functions/cast-and-convert-transact-sql?view=sql-server-ver16)
- \[5\] 拙作 [SQL Server 的字串分割](/SQL_Server_Split/)
- \[6\] 拙作 [SQL 取得當月第一天和最後一天](/SQL_First_Last_Date/)