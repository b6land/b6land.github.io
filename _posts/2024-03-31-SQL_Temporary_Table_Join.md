---
layout: post
title: SQL 暫存資料表與資料表連接
date: 2024-03-31 12:00:00 +0800
categories:  [SQL]
--- 

這篇文章轉錄我在 IT 邦幫忙 2022 年鐵人賽的文章：[Day 23: SQL 暫存資料表與資料表連接](https://ithelp.ithome.com.tw/articles/10304040)，是對 [SQL 語法 - WITH (NOLOCK)、暫存資料表、Join](/SQL_Command/) 文章的補充，現在看起來也還是很實用！

### 暫存資料表與 SELECT INTO

在 SQL Server 中的暫存表分為 3 個類型：

1. [#Table] 暫存資料表：只有建立者可以使用
2. [##Table] 暫存資料表：所有人都可以使用
3. [@Table] 資料表變數：批次資料執行完後立即從記憶體被刪除

可以使用 `SELECT INTO` 的語法將 SELECT 的結果插入到新的暫存表，以供暫時性的紀錄或計算使用，其插入方式如下：
```sql
SELECT Column1, Column2… 
INTO [#Table_name] 
FROM [Tables]
```

不過 `SELECT INTO` 不能插入至已存在的資料表，如果需要覆蓋的話，應該要先使用 `DROP TABLE` 指令：

```sql
DROP TABLE [#Table_name]
```

另外，可以加上 `WHERE 0=1`，只複製資料表結構，而不複製裡面的內容。

```sql
SELECT Column1, Column2… 
INTO [#Table_name] 
FROM [Tables]
WHERE 0=1
```

可參考 [[iT鐵人賽Day33] SQL Server 暫存表(@ # ##)與CTE (Common Table Expressions) - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10225120)、[SQL SELECT INTO - SQL 語法教學 Tutorial](https://www.fooish.com/sql/select-into.html) 兩篇文章的講解！

### JOIN 的分別

- `INNER JOIN`：只會返回兩個資料表都符合連接結果 (條件) 的資料。
- `LEFT JOIN`：只要左方的資料表符合條件，就取回資料。右方資料表不存在的欄位為 `NULL`。
- `RIGHT JOIN`：只要右方的資料表符合條件，就取回資料。左方資料表不存在的欄位為 `NULL`。

*(對我來說，通常會將左方資料表稱為主資料表，右方資料表稱為子資料表，較能區分連接後的結果。)*

推薦 [SQL Joins](https://www.w3schools.com/sql/sql_join.asp) 文中的圖解，可以快速理解各種 JOIN 的關係。