---
layout: post
title: C# LINQ 查詢多個列表 (List)
date: 2024-02-18 12:00:00 +0800
categories:  [C#]
--- 

LINQ 可以用來查詢多個列表、資料表連接後的資料，請見後方的說明。

### 撰寫方式

假設分別有 Status 類別的 status 列表、Parameters 類別的 parameters 列表，可以使用以下 LINQ 語法連接兩個列表，以檢索相關欄位。類似於 SQL 的 SELECT、WHERE 和 JOIN 運算子，不同的在 LINQ 使用 join 連接時要使用 equals 來連接，而且 select 語法需要擺在最後方：

```cs
var result = from s in status
             join p in parameters on s.ID equals p.ID
             where s.ID == userID && p.Class == userClass 
             select new { s.ID, s.IsOnline, n.Class, n.Level};
```

select 的結果是匿名類別的集合，可以使用一般操作清單的方式取得資料，並存取查詢結果的欄位。例如：

```cs
var first = result.First();
Console.WriteLine($"{first.ID}, {first.IsOnline} - {first.Class}, {first.Level}");
```

以上的結果相當於查詢資料庫時使用 INNER JOIN 連接資料表，如果需要寫出 LEFT JOIN 的話，則需要使用 `DefaultIfEmpty()`  方法回傳 null 物件。

### 參考資料

- [LINQ學習筆記(6) Join — 多表單多條件式 - by 莊創偉 - Medium](https://ad57475747.medium.com/4076c487264f)  

- [分享幾個 LINQ to SQL 執行各種 Join 查詢的技巧 - The Will Will Web](https://blog.miniasp.com/post/2010/10/14/LINQ-to-SQL-Query-Tips-INNER-JOIN-and-LEFT-JOIN)  

- [在LINQ中實踐多條件LEFT JOIN-黑暗執行緒](https://blog.darkthread.net/blog/linq-left-join/)

### 範例程式碼

``` cs
    public class Status
	{
    	public int ID { get; set; }
    	public bool IsOnline { get; set; }
	}

	public class Parameter
	{
	    public int ID { get; set; }
	    public string Class { get; set; }
	    public int Level { get; set; }
	}
	
	public static void Main()
	{
		List<Status> status = new List<Status>()
		{
		    new Status { ID = 1, IsOnline = true },
    		new Status { ID = 2, IsOnline = false },
    		new Status { ID = 3, IsOnline = true },
    		new Status { ID = 4, IsOnline = true }
		};

		List<Parameter> parameters = new List<Parameter>()
		{
    		new Parameter { ID = 1, Class = "A", Level = 3 },
    		new Parameter { ID = 2, Class = "B", Level = 2 },
    		new Parameter { ID = 3, Class = "A", Level = 1 },
    		new Parameter { ID = 4, Class = "C", Level = 3 }
		};

		int userID = 1;
		string userClass = "A";

		var result = from s in status
             join p in parameters on s.ID equals p.ID
             where s.ID == userID && p.Class == userClass 
             select new { s.ID, s.IsOnline, p.Class, p.Level};
		
		var first = result.First();
		Console.WriteLine($"{first.ID}, {first.IsOnline} - {first.Class}, {first.Level}");
	}
```