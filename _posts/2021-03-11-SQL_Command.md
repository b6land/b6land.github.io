---
layout: post
title: SQL 語法 - WITH (NOLOCK)、暫存資料表、Join
date: 2021-03-11 12:00:00 +0800
categories: SQL
--- 

在這一篇當中，會簡介 SQL 語法的 `WITH (NOLOCK)`、暫存資料表與 Select Into，以及 Join 的差異。 

### With (NoLock)

- 當只需要查詢時，可使用 `WITH (NOLOCK)` 增加查詢速度，因為不用讓之後的查詢等待本次查詢結束。但需要注意使用的情形：如果剛好有資料正在寫入、更新或刪除時，可能會導致資料出錯。
- 使用的範例：
> Select * From Table_a With(NoLock)
- 參考資料：[[SQL SERVER]小心使用With NoLock(續) - RiCo技術農場 - 點部落](https://dotblogs.com.tw/ricochen/2014/05/04/144966)

### 暫存資料表與 Select Into

- 在 SQL Server 中的暫存表分為 3 個類型： 

1. [#Table] 暫存資料表：只有建立者可以使用
2. [##Table] 暫存資料表：所有人都可以使用
3. [@Table] 資料表變數：批次資料執行完後立即從記憶體被刪除

- 暫存表的插入方式：
> Select * into [#Table_name] from [Tables] where [Conditions]
- Select Into 可以將查詢的結果插入至一個新的資料表，不能插入至已存在的資料表。
- 參考資料：[[iT鐵人賽Day33] SQL Server 暫存表(@ # ##)與CTE (Common Table Expressions) - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10225120)
- 參考資料：[SQL SELECT INTO - SQL 語法教學 Tutorial](https://www.fooish.com/sql/select-into.html)

### Inner Join 和 Left Join

- Inner Join：等值連接，只會返回符合連接結果 (條件) 的資料；
- Left Join：對於左方的表格 (Table 1)，無論是否有符合連接結果，都會返回資料，不存在的欄位為 NULL。 
- 參考資料：[SQL INNER JOIN 內部連接 - SQL 語法教學 Tutorial](https://www.fooish.com/sql/inner-join.html)

### Where 和 Inner Join

- 當連接多個資料表時，使用 `Inner Join` 會比 `Where` 來得更容易閱讀。這是因為

> Select * From table_a As a
>
> Inner Join table_b As b On a.f_key=b.p_key
>
> Where a.id='A01'

只單純的包含了在哪些值相同時連接資料，而 `Where` 則可以同時包含限定條件 (Which) 和連接 (Where-from)，因此有多組資料表連接時，會比較需要花時間閱讀與理解。

> Select * From table_a As a, table_b As b
>
> Where a.f_key=b.p_key And a.id='A01'

- 參考資料：[sql - INNER JOIN ON vs WHERE clause - Stack Overflow](https://stackoverflow.com/questions/1018822/inner-join-on-vs-where-clause)