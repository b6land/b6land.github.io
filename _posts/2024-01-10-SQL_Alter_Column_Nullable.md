---
layout: post
title: SQL 變更資料表欄位為 Nullable
date: 2024-01-10 12:00:00 +0800
categories: [SQL]
---

如何在不 DROP 資料表的情形下，將資料表中的某一個欄位轉成可為 NULL？

### 語法

其語法如下：

```
ALTER TABLE myTable ALTER COLUMN myColumn {DataType} NULL
```

將其中的 {DataType} 改為需要的資料型態，例如 `varchar(20)` 或 `int`；myTable 和 myColumn 分別改成要修改的資料表和欄位名稱。

參考資料: [sql server - Can I change a column from NOT NULL to NULL without dropping it? - Stack Overflow](https://stackoverflow.com/questions/7407650/can-i-change-a-column-from-not-null-to-null-without-dropping-it)