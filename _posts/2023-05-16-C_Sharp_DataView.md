---
layout: post
title: DataView 如何取得不重覆、排序的資料
date: 2023-05-16 12:00:00 +0800
categories: [C#]
---

在 C# 中，DataView 可以用來排序和過濾 DataTable 內的資料。

### 使用方式

- DataTable.DefaultView 即是一個 DataView。

- 可以指定 `DataView.Sort` 的欄位名稱，根據欄位進行排序。可以在 DefaultView 檢視排序完後的結果。

  ```cs
  DataTable table = new DataTable();
  table.Columns.Add("Height", typeof(int));
  table.Columns.Add("Name", typeof(string));
  
  table.Rows.Add(170, "Alice");
  table.Rows.Add(171, "Bob");
  table.Rows.Add(172, "Candy");
  table.Rows.Add(172, "Candy");

  table.DefaultView.Sort = "Height";
  ```

- 延續上個範例，可以呼叫 `DataView.ToTable()` 方法，根據特定欄位取得不重複的資料，並存為 DataTable 物件，請參考以下程式碼:

  ```cs
  string[] columnNames = new string[] { "Name" };
  DataTable distinctTable = table.DefaultView.ToTable(true, columnNames);
  ```

### 參考資料

- [利用DataView來過過濾DataTable達到SQL Query的Distinct效果 - Kenny Hus' blog - 點部落](https://dotblogs.com.tw/kennyshu/2010/07/07/16449)

- [C# DataView Example (Sort) - Dot Net Perls](https://www.dotnetperls.com/dataview)

*(程式改寫自以上參考資料)*