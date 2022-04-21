---
layout: post
title: SQL ROW_NUMBER() 和 ISNULL() 函數
date: 2021-03-26 12:00:00 +0800
categories: SQL
--- 

本篇包含 SQL 中的 `ROW_NUMBER()` 和 `ISNULL()` 的介紹。

### ROW_NUMBER()

- 為每筆資料產生 1 ~ N 的遞增整數。
- 搭配 OVER 函數，根據特定的資料欄位排序與產生整數。
> SELECT ID, Score, Class, ROW_NUMBER() OVER(ORDER BY Score DESC) N'RowNumber'
>
> FROM Student
- 可搭配 OVER PARTITION BY 關鍵字，根據資料欄位的分組結果產生分組的整數。
> SELECT ID, Score, Class, ROW_NUMBER() OVER(PARTITION BY Class ORDER BY Score DESC) N'RowNumber'
>
> FROM Student
- 也有與 `ROW_NUMBER()` 類似的**次序函數**可以使用，如 `RANK()` (資料值相同時賦予相同整數，會跳號)、`DENSE_RANK()` (資料值相同時賦予相同整數，*不*跳號)
- 參考資料：[SQL QnA: 次序函數](http://sqlqna.blogspot.com/2018/01/blog-post_25.html)
- 參考資料：[德瑞克：SQL Server 學習筆記: SQL Server：認識「次序函數(Window Ranking Functions)」(2)](http://sharedderrick.blogspot.com/2012/10/sql-serverwindow-ranking-functions2.html)

### ISNULL()

- 當使用彙總函數 (如 `SUM()`) 時，可能會遇到部分資料的欄位數值為空的狀況，此時可以用 `ISNULL()` 函數將 NULL 取代為特定的值。例如下面指令會將價格欄位加總，遇到沒有價格的資料時，以 0 代替。
> SUM(ISNULL(Price, 0))
- 加總時，若相加的欄位可能為 NULL 值，應以 `ISNULL(column, 0)` 代替，因為 NULL 加上任何數字都會為 NULL ，而導致計算錯誤。
> SELECT ISNULL(NULL, 0) + 100 -- 結果為 100
> SELECT NULL + 100 -- 結果為 NULL
- 參考資料：[德瑞克：SQL Server 學習筆記: SQL Server：認識 ISNULL 函數](http://sharedderrick.blogspot.com/2012/06/t-sql-isnull.html)