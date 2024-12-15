---
layout: post
title: SQL Server 索引加入 INCLUDE 欄位，減少查詢時間
date: 2024-12-15 17:00:00 +0800
categories: [SQL Server]
--- 

在 SQL Server 索引加入 INCLUDE 欄位，可以減少從資料表查閱的步驟，更新索引時效率也較高。

### 建立方式

可以透過 SQL Server Management Studio (SSMS) 新增索引，於新增索引的視窗將欄位加入「包含的 資料行」。

![包含的資料行](/assets/imgs/2024-12-15/Create_Index_Include_Example.png)  

若手動輸入語法，其一般的建立索引語法類似，要在後方加入 `INCLUDE`  關鍵字：

```sql
CREATE INDEX index2 ON table1 (col1) INCLUDE (col2, col3)
```

其中 table1 後面放入要搜尋的欄位， INCLUDE 後面放入要查閱的資料欄位。

### 差異

假如有以下的 SQL Server 索引：

```sql
CREATE INDEX index1 ON table1 (col1, col2, col3)
CREATE INDEX index2 ON table1 (col1) INCLUDE (col2, col3)
```

index1 適合以下的查詢語法：

```sql
SELECT * FROM table1 WHERE col1 = x AND col2 = y AND col3 = z
```

index1 會尋找索引 (index seek)，並透過索引鍵查閱 (bookmark lookup) 取得資料列的所有欄位資料。

index 2 適合以下的查詢語法：

```sql
SELECT col2, col3 FROM table1 WHERE col1 = x
```

因為 index2 的索引的葉頁面 (Leaf Page) 內提供了所有所需的資料，不會再去查詢資料表。從執行計畫可看到，會減少 bookmark lookup 的步驟。

另外，如果索引本身可以涵蓋所有要查詢的資料欄位，包含使用 INCLUDE ，可稱為 **Covering Index**。Covering Index 表示不需要額外的 bookmark lookup 取得其它欄位資料。

與一般的索引欄位相比，用 INCLUDE 包含的欄位，不是 Key 欄位，因此可以減少根頁面 (Root Page)、中繼頁面 (Intermediate) 的數量。此外，編輯資料時不必對 Include 的資料排序，可以減少更新索引的時間。

### 範例

以 AdventureWorks2022 範例資料庫為例，我們要查詢 SalesOrderDetail 資料表特定 ProductID 的資料：

_(此處使用 SSMS 操作，想使用指令的話，請參考上方語法或官方文件 ~ )_ 😅

1\. 新增 IX\_ProductID\_INCLUDE 索引，加入 ProductID 欄位。

![加入索引資料行](/assets/imgs/2024-12-15/Create_Index.png)  
 
2\. 在包含的資料行加入 SalesOrderDetailID。

![加入欄位至包含的資料行](/assets/imgs/2024-12-15/Create_Index_Include.png)  

3\. 執行查詢語法，從執行計畫可看到執行索引搜尋，只找了 242 筆資料，因為涵蓋了查詢的所有欄位，因此沒有索引鍵查閱。

![INCLUDE 欄位的執行計畫](/assets/imgs/2024-12-15/Query_Index_Include.png)  

4\. 接著建立另一個 IX\_SalesOrder 索引，將 ProductID、SalesOrderDetailID 都加入索引欄位 (不設定 INCLUDE 欄位)，並調整語法查詢所有欄位。可看到仍然有使用索引搜尋，但因為沒有涵蓋所有欄位，增加了索引鍵查閱的步驟：

![索引欄位的執行計畫](/assets/imgs/2024-12-15/Query_Index_All.png)  

![索引鍵查詢](/assets/imgs/2024-12-15/Query_Index_All_Lookup.png)  

5\. 為了對比，我們來看看沒有索引的狀況，可發現對整個資料表掃描了一遍。

![沒有建立索引的執行計畫](/assets/imgs/2024-12-15/Query_No_Index.png)  

### 參考資料

- 官方文件：[建立內含資料行的索引 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/indexes/create-indexes-with-included-columns?view=sql-server-ver16)
- 使用時機：[sql server 2005 - Index Key Column VS Index Included Column - Stack Overflow](https://stackoverflow.com/questions/3581294/index-key-column-vs-index-included-column )
- 差異說明：[SQL Worker: 索引建立時，將欄位放在key中跟用include涵蓋進來有什麼差異？](http://sqlworker.blogspot.com/2016/11/keyinclude.html )
- Covering Index 完整說明：[Using Covering Indexes to Improve Query Performance - Simple Talk](https://www.red-gate.com/simple-talk/databases/sql-server/learn/using-covering-indexes-to-improve-query-performance/#covering-indexes )
- Covering Index 簡易說明：[What are Covering Indexes and Covered Queries in SQL Server? - Stack Overflow](https://stackoverflow.com/questions/609343/what-are-covering-indexes-and-covered-queries-in-sql-server)
- [資料庫索引深入淺出(二) - 石頭的coding之路](https://isdaniel.github.io/dbindex-2/)

