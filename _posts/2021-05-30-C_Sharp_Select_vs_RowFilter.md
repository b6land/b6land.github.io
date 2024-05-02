---
layout: post
title: C# 資料過濾 - DataView.RowFilter 和 DataTable.Select
date: 2021-05-30 12:00:00 +0800
categories:  [C#]
--- 

在 C# 中，DataTable 可以代表記憶體中的單一資料表，而 DataView 是 DataTable 的檢視 (表)，允許你過濾 和排序資料。這兩種結構都可以過濾資料，本篇會介紹相關的 `DataView.RowFilter` 和 `DataTable.Select` 語法。

### 介紹

在 DataView 中 (如 DefaultView ) 想要過濾資料，可以使用 `DataView.RowFilter(expression)` 方法， *expression* 為 SQL 中的 Where 子句，例如 `Age > 35`。

使用 DataView 的 `RowFilter` 時，不會傳回資料，過濾的結果直接在 DataView 內呈現。

DataTable 本身提供 `Select` 方法，使用方法為 `DataTable.Select(expression)`，*expression* 一樣為 Where 子句。

使用 DataTable 的 `Select` 時，會傳回 DataRow 陣列。

根據參考資料的測試，DataTable 需要花較多時間找尋資料，使用 DataView 的耗時較低，而 Dictionary 的耗時則會是最少的。

### 參考資料

- [DataView的RowFilter與DataTable的Select與Linq - Jeff 隨手記 - 點部落](https://dotblogs.com.tw/jeff-yeh/2010/09/28/17972)
- [c# - What is the difference between dataview and datatable? - Stack Overflow](https://stackoverflow.com/questions/7382932/what-is-the-difference-between-dataview-and-datatable)
- [.net - DataView.RowFilter Vs DataTable.Select() vs DataTable.Rows.Find() - Stack Overflow](https://stackoverflow.com/questions/2832304/dataview-rowfilter-vs-datatable-select-vs-datatable-rows-find)