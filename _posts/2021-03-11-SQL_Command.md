---
layout: post
title: SQL 語法 - WITH (NOLOCK)、暫存資料表、Join
date: 2021-03-11 12:00:00 +0800
categories:  [SQL]
--- 

在這一篇當中會介紹好幾個 SQL 的語法，會簡介 `WITH (NOLOCK)` 的語法提示 (Table Hint)、暫存資料表與 `SELECT INTO` 語法，以及幾種不同 JOIN 語法的差異。 

### WITH (NOLOCK)

- 當只需要查詢時，可使用 `WITH (NOLOCK)` (或只用 `(NOLOCK)` 也可以) 增加查詢速度。
  - 沒辦法使用在 `UPDATE` 或 `DELETE` 語法。
- 為什麼可以加速？即使目前在進行修改資料的作業，也可以不必等待作業完成，即讀取資料。
  - 避免 Block 的問題，即可以讀取目前被鎖定的資料。
  - 簡單來說，SQL Server 的鎖定機制是處理到變更中的資料，詳情可參考拙作：[SQL 的鎖定 (Lock) 和死結 (Deadlock)](/SQL_Lock_Deadlock/)。
- 小心使用！讀取的資料可能剛好被更新到一半，並非完整的資料 *(通常使用此語法提示，表示你不太在意資料正確性)*；如果剛好有資料正在寫入、更新或刪除時，可能會導致資料出錯。
- 使用的範例如下：

``` sql
SELECT Column_1, Column_2 
FROM Table_a WITH(NOLOCK)
```

#### 參考資料

- 微軟官方的說明：[Table Hints (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/t-sql/queries/hints-transact-sql-table?view=sql-server-ver16#readuncommitted)
- `NOLOCK` 語法的解說：[[SQL SERVER][Performance]小心使用With NoLock - RiCo技術農場 - 點部落](https://dotblogs.com.tw/ricochen/2011/04/15/22758)
- `NOLOCK` 語法的缺點與替代方案：[[SQL SERVER]小心使用With NoLock(續) - RiCo技術農場 - 點部落](https://dotblogs.com.tw/ricochen/2014/05/04/144966)

### 暫存資料表與 SELECT INTO

- 在 SQL Server 中的暫存表分為 3 個類型： 

1. [#Table] 暫存資料表：只有建立者可以使用
2. [##Table] 暫存資料表：所有人都可以使用
3. [@Table] 資料表變數：批次資料執行完後立即從記憶體被刪除

- 以下是從已有的資料表產生新的暫存表的範例語法，其中 Columns 表示要查詢的欄位，#Table_name 是想插入的暫存表名稱，Tables 表示來源資料表，Conditions 表示篩選的條件：

``` sql
SELECT [Columns] INTO [#Table_name] 
FROM [Tables] WHERE [Conditions]
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

#### 參考資料

- 簡單易懂的語法說明文章：[[iT鐵人賽Day33] SQL Server 暫存表(@ # ##)與CTE (Common Table Expressions) - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10225120)
- `SELECT INTO` 語法說明：[SQL SELECT INTO - SQL 語法教學 Tutorial](https://www.fooish.com/sql/select-into.html)

### JOIN 的分別

- `INNER JOIN`：只會返回兩個資料表都符合連接結果 (條件) 的資料。
- `LEFT JOIN`：只要左方的資料表符合條件，就取回資料。右方資料表不存在的欄位為 `NULL`。
- `RIGHT JOIN`：只要右方的資料表符合條件，就取回資料。左方資料表不存在的欄位為 `NULL`。

*(對我來說，通常會將左方資料表稱為主資料表，右方資料表稱為子資料表，較能區分連接後的結果。)*

#### 參考資料

- `INNER JOIN` 語法說明：[SQL INNER JOIN 內部連接 - SQL 語法教學 Tutorial](https://www.fooish.com/sql/inner-join.html)
- 可透過文中圖解，快速理解各種 JOIN 的關係：[SQL Joins](https://www.w3schools.com/sql/sql_join.asp) 

### 要使用 WHERE 還是 INNER JOIN 連接資料 ?

適用狀況：你寫了連接一堆資料表的語法，需要設定連接時的條件。

當連接多個資料表時，使用 `INNER JOIN` 會比 `WHERE` 來得更容易閱讀。

``` sql
SELECT * FROM table_a AS a
INNER JOIN table_b AS b ON a.f_key=b.p_key
WHERE a.id='A01'
```

以上述語法來說，`INNER JOIN` 只包含在哪些值相同時連接資料。而 `WHERE` 則可以同時包含限定條件 (Which) 和連接 (Where-from)，因此有多組資料表連接時，會比較需要花時間閱讀與理解，如下方語法。

``` sql
SELECT * FROM table_a AS a, table_b AS b
WHERE a.f_key=b.p_key AND a.id='A01'
```
#### 參考資料

- Stack Overflow 問答：[sql - INNER JOIN ON vs WHERE clause - Stack Overflow](https://stackoverflow.com/questions/1018822/inner-join-on-vs-where-clause)