## SQL 筆記 (GROUP BY, DELETE FROM, Remoting Format)

這篇文章會簡介 SQL 中 GROUP BY 中常忽略的欄位說明、刪除資料時要如何搭配篩選條件，以及 T-SQL 資料傳輸的格式。

### GROUP BY

- 使用 GROUP BY 時，需要包含非彙總函數以外的所有欄位，否則會產生 MSG 8120 錯誤。

> SELECT ID, Name, SUM(Score) <br>
> FROM Student <br>
> GROUP BY ID

以上的語法會出現錯誤，需改成以下語法:

> SELECT ID, Name, SUM(Score) <br>
> FROM [Student] <br>
> GROUP BY **ID, Name**

- 參考資料: [（经典）使用group by出现错误.要注意什么?　 - 钱途无梁 - 博客园](https://www.cnblogs.com/qiantuwuliang/archive/2009/05/31/1492823.html)

### DELETE FROM 搭配篩選條件

- 在一般使用 DELETE FROM 時，通常會使用以下的方式篩選出需要刪除的資料。

> DELETE FROM [Table_Name] <br>
> WHERE Column_Name **operator value**

- 如果想要用更複雜的條件過濾需刪除的資料，如刪除在別的資料表中存在的資料，可以用 DELETE FROM + SELECT 子句，例如以下查詢語句。

> DELETE FROM [Table_1] <br>
> WHERE RowID IN ( <br>
> &nbsp;&nbsp;SELECT RowID FROM [Table_1] T1 <br>
> &nbsp;&nbsp;WHERE RowID NOT EXISTS IN ( <br>
> &nbsp;&nbsp;&nbsp;&nbsp;SELECT ID FROM [Table_2] <br>
> &nbsp;&nbsp;) <br>
> ) <br>

- 除了 DELETE FROM 以外，更新 (UPDATE) 語法也能套用以上 SELECT 子句。
- 參考資料: [How to write a SQL DELETE statement with a SELECT statement in the WHERE clause? - Stack Overflow](https://stackoverflow.com/questions/17548751/how-to-write-a-sql-delete-statement-with-a-select-statement-in-the-where-clause)

### 資料傳輸

- 適用於 ADO.NET 中，要傳輸 DataTable 的情形。
- 當傳送 DataTable 時，若為本機至遠端，可使用 `RemotingFormat` 屬性搭配 `BinaryFormatter`  序列化 DataTable，減少傳輸的檔案大小。
- 參考資料: [Reincarnation of DataTable in ADO.NET 2.0](https://www.c-sharpcorner.com/article/reincarnation-of-datatable-in-ado-net-2-0/)

