---
layout: post
title: 寫出好讀的 SQL
date: 2021-07-18 12:00:00 +0800
categories: SQL
--- 

為了讓其它人 (或好幾個月後的自己) 接手維護時能更加方便，一般的程式語言 (ex. C, C#, Java) 會被要求寫得容易閱讀，而 SQL 的查詢語法，也可透過幾個簡易規則寫得容易閱讀。

### 規則

1. 盡可能讓宣告的關鍵字獨立一行。
2. 以大寫撰寫宣告關鍵字，如 `SELECT`, `WHERE` 等。
3. 試著減少子查詢的數量，減少修改查詢時的困難。
4. 撰寫註解 - 無論在哪種程式語言都很重要。
5. 使用短而精確的表格別名。
6. 在子查詢上使用縮排。
7. 建立新表格存放資料。

另外也應配合開發團隊內的規定或約定俗成的撰寫模式。

以下是包含規則 1, 2, 4, 6 的 SQL 查詢語法。

> -- 使用 DELETE FROM 刪除表格 1 中不包含在表格 2 內的資料 <br>
> DELETE FROM [Table_1] <br>
> WHERE RowID IN ( <br>
> &nbsp;&nbsp;SELECT RowID FROM [Table_1] <br>
> &nbsp;&nbsp;WHERE RowID NOT EXISTS IN ( <br>
> &nbsp;&nbsp;&nbsp;&nbsp;SELECT ID FROM [Table_2] <br>
> &nbsp;&nbsp;) <br>
> ) <br>

### 參考資料

- [Creating readable SQL. · Vicki Boykis](http://veekaybee.github.io/2015/06/02/good-sql/)

