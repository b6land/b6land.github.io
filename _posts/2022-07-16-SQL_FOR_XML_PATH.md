---
layout: post
title: 用 FOR XML PATH 將多筆資料合併成一筆
date: 2022-07-16 12:00:00 +0800
categories: [SQL]
---

在 SQL Server 中，可以用 `FOR XML PATH` 語法，將多筆資料合併成一筆，輸出至同一欄位。

假設 Options 資料表有以下資料：

|ID|City|Area|
|--|----|----|
|1|桃園|北|
|2|台北|北|
|3|台中|中|

以下是基本語法，用逗號將找到的所有選項合併在同一欄位: 

``` sql
SELECT ',' + A.City FROM Options A
FOR XML PATH('')
```

會輸出下列結果

> ,桃園,新竹,台中

可以加入子查詢 (需使用 `DISTINCT` 過濾重複資料)，使用其它欄位分類要合併的欄位，在此使用 Area 欄位分類:

``` sql
SELECT DISTINCT B.Area, (  
	SELECT ',' + A.City FROM Options A
  	WHERE A.Area = B.Area
	FOR XML PATH('')
) AS City
FROM Options B
```

結果會是：

|Area|City|
|----|----|
|北|,桃園,台北|
|中|,台中|

可以再使用 `STUFF` 指令 *(將第一個字串刪除指定長度的字元，結尾插入另一個字串)* 去除一開始的逗號。

``` sql
SELECT DISTINCT B.Area, STUFF((  
	SELECT ',' + A.City FROM Options A
  	WHERE A.Area = B.Area
	FOR XML PATH('')
),1,1,'') AS City
FROM Options B
```

|Area|City|
|----|----|
|北|桃園,台北|
|中|台中|

### 參考資料

- [使用 FOR XML PATH 將多筆資料組合成一個字串](https://www.tpisoftware.com/tpu/articleDetails/763)
- [[SQL]將多筆資料合併為一筆顯示(FOR XML PATH) - kevinya - 點部落](https://dotblogs.com.tw/kevinya/2012/06/01/72553) *(另一種做法)*