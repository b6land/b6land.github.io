---
layout: post
title: SQL Server 篩選索引 (Filtered Index) 特色與範例
date: 2024-12-08 12:00:00 +0800
categories: [SQL Server]
--- 

篩選索引指的是在建立 Index 時，使用 WHERE 條件。下面介紹篩選索引的特色、如何新增，以及實際的索引效果。

### 特色

1. 大小比一般的索引來得小。
2. 只有更動到相關條件的資料時，才會維護索引，減少維護成本。
3. 因為只包含符合條件的資料，索引的統計值來得更加精確。

### 用處

- 如果資料欄位絕大部分的值都是 NULL，而你只想找到非 NULL 的資料，
- 會週期性的尋找資料表中標示為「待處理」的資料，並標示為「已處理」。在一段時間後，大部分的資料都是「已處理」，此時找出「待處理」的篩選索引就很有用。
- 如果資料表有各種不同類型資料，透過篩選索引專注建立特定範圍資料的索引，減少索引大小。

以下使用 AdventureWorks 資料庫來做測試，我們要從 SalesOrderDetail 資料表找出 ProductID = 776 的資料。

### 新增篩選索引

1\. 在資料表的「索引」項目上點右鍵，新增一個索引。

![新增索引功能表](/assets/imgs/2024-12-08/index_context_menu.png)  

2\. 新增要索引的資料行。

![新增索引，選擇資料行](/assets/imgs/2024-12-08/create_index_column.png)  

3\. 切換到「篩選」頁籤，輸入查詢條件。

![輸入查詢條件](/assets/imgs/2024-12-08/create_index_filter.png)  

4\. 按下確定來建立索引。

### 篩選索引的執行計畫和大小

1\. 我們建立了一個索引資料行為 ProductID、篩選條件為 `ProductID = 776`  的篩選索引 (此處名稱為 `NonClusteredIndex_20241208_100200` )。可以看到下方的查詢語法內，使用了篩選索引來加速取得結果，只讀取 228 筆資料。

![有使用篩選索引](/assets/imgs/2024-12-08/filtered_index_plan.png)  

2\. 如果修改查詢條件，不符合篩選條件的話，就不會使用篩選索引處理，讀取了約 12 萬筆資料。

![未使用篩選索引](/assets/imgs/2024-12-08/non_filtered_index_plan.png)  

3\. 如果建立的是含 ProductID 的一般索引 (此處名稱為 `IX_SalesOrderDetail_ProductID` )，可以看到產生的執行計畫和篩選索引一樣，但不限定篩選條件。

![一般索引](/assets/imgs/2024-12-08/normal_index_plan.png)  

4\. 查詢索引大小，可以看到篩選索引大小較小，表示內部儲存的資料少，因此除了不占容量，更新索引的速度也會比一般索引快。

![篩選索引占用容量較小](/assets/imgs/2024-12-08/index_size.png)  


註：如果同時建立了同一組欄位的一般索引和篩選索引， SQL Server 在查詢時會自行決定要使用哪種索引。

### 參考資料

- [建立篩選索引 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/indexes/create-filtered-indexes?view=sql-server-ver16)
- [資料庫索引深入淺出(二) - 石頭的coding之路](https://isdaniel.github.io/dbindex-2/)