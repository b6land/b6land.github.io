---
layout: post
title: SQL 用 ROW_NUMBER 和其它次序函數產生名次
date: 2021-03-26 19:00:00 +0800
categories:  [SQL]
--- 

本篇介紹 SQL Server 中的 `ROW_NUMBER`、`RANK`、`DENSE_RANK` 函數，可以用來產生名次，最常用來幫成績排名。

### ROW_NUMBER()

`ROW_NUMBER()` 是一個 SQL 中常用的次序函數，用於為每筆資料產生 1 到 N 的遞增整數，可以用來計算名次或進行排序。這個函數可以與 `OVER` 子句搭配使用，根據特定的資料欄位排序並產生整數。範例如下：

1\. 建立包含學生的 ID、分數（Score）和班級（Class） 的資料表。

``` sql
CREATE TABLE Student (
    ID INT,
    Score INT,
    Class NVARCHAR(50)
);

INSERT INTO Student (ID, Score, Class) VALUES
(1, 90, 'A'),
(2, 85, 'A'),
(3, 90, 'B'),
(4, 70, 'B'),
(5, 95, 'A'),
(6, 80, 'B');

```

2\. 根據 Score 欄位的值遞減排序，並為每筆資料分配一個遞增整數。

``` sql
SELECT ID, Score, Class, ROW_NUMBER() OVER(ORDER BY Score DESC) N'RowNumber'
FROM Student
```

結果為：

|ID | Score | Class | RowNumber|
|--- | --- | --- | ---|
|5 | 95 | A | 1|
|1 | 90 | A | 2|
|3 | 90 | B | 3|
|2 | 85 | A | 4|
|6 | 80 | B | 5|
|4 | 70 | B | 6|

3\. 可搭配 `OVER PARTITION BY` 關鍵字，根據資料欄位的分組結果產生分組的整數。以下的查詢會根據 Class 欄位進行分組，然後在每個分組內依據 Score 欄位的值遞減排序，並為每個分組內的資料分配遞增的整數。

``` sql
SELECT ID, Score, Class, ROW_NUMBER() OVER(PARTITION BY Class ORDER BY Score DESC) N'RowNumber'
FROM Student
```

結果為：

|ID | Score | Class | RowNumber|
|--- | --- | --- | ---|
|5 | 95 | A | 1|
|1 | 90 | A | 2|
|2 | 85 | A | 3|
|3 | 90 | B | 1|
|6 | 80 | B | 2|
|4 | 70 | B | 3|

4\. 也有與 `ROW_NUMBER()` 類似的次序函數可以使用，如 `RANK()`：資料值相同時賦予相同整數，會跳號。請見以下範例：

```sql
SELECT ID, Score, Class, RANK() OVER(ORDER BY Score DESC) AS Rank
FROM Student;
```

結果為：

|ID | Score | Class | Rank|
|--- | --- | --- | ---|
|5 | 95 | A | 1|
|1 | 90 | A | 2|
|3 | 90 | B | 2|
|2 | 85 | A | 4|
|6 | 80 | B | 5|
|4 | 70 | B | 6|

5\. `DENSE_RANK()`：資料值相同時賦予相同整數，*不*跳號。請見以下範例：

```sql
SELECT ID, Score, Class, DENSE_RANK() OVER(ORDER BY Score DESC) AS DenseRank
FROM Student;

```

結果為：

|ID | Score | Class | DenseRank|
|--- | --- | --- | ---|
|5 | 95 | A | 1|
|1 | 90 | A | 2|
|3 | 90 | B | 2|
|2 | 85 | A | 3|
|6 | 80 | B | 4|
|4 | 70 | B | 5|

### 參考資料

- [SQL QnA: 次序函數](http://sqlqna.blogspot.com/2018/01/blog-post_25.html)
- [德瑞克：SQL Server 學習筆記: SQL Server：認識「次序函數(Window Ranking Functions)」(2)](http://sharedderrick.blogspot.com/2012/10/sql-serverwindow-ranking-functions2.html)
