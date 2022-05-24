---
layout: post
title: C# 資料過濾
date: 2021-05-30 12:00:00 +0800
categories:  [Database, C#]
--- 

本篇會介紹 DataView 的 RowFilter 和 DataTable 的 Select。

### 介紹

在 DataView 中 (如 DefaultView ) 的使用方法為 `DataView.RowFilter(expression)` ， *expression* 為 SQL 中的 Where 子句，例如 `Age > 35`。

使用 DataView 的 RowFilter 時，不會傳回資料，過濾的結果直接在 DataView 內呈現。

DataTable 本身提供 Select 方法，使用方法為 `DataTable.Select(expression)`，*expression* 一樣為 Where 子句。

使用 DataTable 的 Select 時，會傳回 DataRow 陣列。

根據參考資料的測試，使用 DataView 的耗時較低。

### 參考資料

- [DataView的RowFilter與DataTable的Select與Linq - Jeff 隨手記 - 點部落](https://dotblogs.com.tw/jeff-yeh/2010/09/28/17972)