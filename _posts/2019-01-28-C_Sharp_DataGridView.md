---
layout: post
title: C# DataGridView 的使用
date: 2019-01-28 12:00:00 +0800
categories:  [C#]
---

本篇文章包含 DataGridView 元件的綁定方式，以及各種不同的屬性修改方法。

### 如何綁定資料至 DataGridView 元件

DataGridView 可以使用 SQL Command 直接讀取和綁定資料，不過若要修改其中的資料 (如將原本的整數欄位改成英文名稱 )，自行從 SQL 讀取並自訂資料結構的作法較佳。

1. 建立 Class。要注意**須提供 get 和 set 存取子**。

``` csharp
public class TableData
{                 
    private string mOwner { get; set; }
    private string mPet { get; set; }
    private string mPetname { get; set; }
}
```

2. 建立該 Class 的 List 或 Array。
3. 指定該 List 或 Array 至 Data Source。
4. 預設的 Header 即為自訂類別的屬性名稱。

``` csharp
TableData td = new TableData();
td.Owner = "Frank Lloyd Wright";
td.Pet = "Cow";
td.PetName = "Felix";

TableData td2 = new TableData();
td2.Owner = "George Washington";
td2.Pet = "Dog";
td2.PetName = "Monty";

TableData[] arr = new TableData[4];
arr[0] = td;
arr[1] = td2;

DataGridView1.DataSource = arr;
```

ref : [Bind Objects to a DataGridView Control](https://www.c-sharpcorner.com/article/bind-objects-to-a-datagridview-control/)

或是直接建立 DataTable 類別，將資料庫的查詢結果放入 DataTable，並指定為 Data Source，

``` csharp
DataTable dataTable = new DataTable(); 
SqlDataAdapter da = new SqlDataAdapter(cmd);
da.Fill(dataTable);
GridView1.DataSource = dataTable; //告訴GridView資料來源為誰
GridView1.DataBind();//綁定
conn.Close(); //連線關閉       
```

ref: [C# 自行撰寫sql連線及綁定Gridview - chi's coding life - 點部落](https://dotblogs.com.tw/chichiblog/2017/10/16/163211)

### DataGridView 的事件列表

ref : [C# DataGridView 事件 - 自我LV1 - 點部落](https://dotblogs.com.tw/jain/2010/05/13/15206)

### 改變 DataGridView 裡每列的背景色

``` csharp
foreach (DataGridViewRow row in vendorsDataGridView.Rows)
{
    row.DefaultCellStyle.BackColor = Color.Red;
}
```

ref : [c# - How to change row color in datagridview? - Stack Overflow](https://stackoverflow.com/questions/2189376/how-to-change-row-color-in-datagridview)

### 如何改變每欄的標題文字 (Header Text)

``` csharp
dataGridView1.Columns[column_index].HeaderText = "New Grid Column Name";
```

ref : [c# - Changing DataGridView Header Text At Runtime - Stack Overflow](https://stackoverflow.com/questions/27267176/changing-datagridview-header-text-at-runtime)

### 自動調整欄位寬度的簡單方法

``` csharp
dataGridView1.AutoResizeColumns(DataGridViewAutoSizeColumnsMode.Fill);
```

ref: [c# - How do you automatically resize columns in a DataGridView control AND allow the user to resize the columns on that same grid? - Stack Overflow](https://stackoverflow.com/questions/1025670/how-do-you-automatically-resize-columns-in-a-datagridview-control-and-allow-the)

### 調整欄位寬度

``` csharp
//設定DataGridView中欄位的寬度
dgvEntityHardware.Columns[0].Width = 110;
```

ref: [[C#]-DataGridView 小技巧 - 程式設計筆記byChris - 點部落](https://dotblogs.com.tw/chris0920/2010/12/27/20418)