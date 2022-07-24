---
layout: post
title: LINQ 與 Lambda 筆記
date: 2022-04-10 12:00:00 +0800
categories:  [C#, LINQ, Lambda]
---

在寫 C# 程式時，常常會看到結合 LINQ 和 Lambda 語法的程式碼，以下是簡單的介紹。

### LINQ 語法

- 使用類似 SQL 的語法，常撰寫資料庫語法的人，可提高熟悉度。
- 可直接對物件操作，查詢結果也為可操作的物件集合。
- 常用的運算子： `FROM`, `SELECT`, `WHERE` 等，以下是一個基本的查詢語法。

``` csharp
var p = from a in emp
where a.country.Equals("america")
select a.name;
```


### Lambda 語法

- 可用來建立匿名函式。透過委派的方式傳入需要的資料。以下是一個基本的語法，從中取得集合 a 裡國家為台灣的資料，並以匿名型別呈現：

``` csharp
var q = emp.Where(x => x.country.Equals("taiwan"));
```

[Lambda 運算式 - C# 參考 - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/csharp/language-reference/operators/lambda-expressions)

### 範例程式碼

``` csharp
using System;
using System.Linq;
					
public class Program
{
	public class employee{
		public string name;
		public string country;
		
		public employee(string n, string c){
			this.name = n;
			this.country = c;
		}
	}
	
	public static void Main()
	{
		employee[] emp = new employee[3];
		emp[0] = new employee("jacky", "taiwan");
		emp[1] = new employee("annie", "america");
		emp[2] = new employee("jeff", "taiwan");
		var q = emp.Where(x=>x.country.Equals("taiwan"));
		
		foreach(var t in q.AsEnumerable()){
			Console.WriteLine(t.name + ": " + t.country);
		}
		
		var p = from a in emp
			where a.country.Equals("america")
			select a.name;
		
		foreach(var t in p.AsEnumerable()){
			Console.WriteLine(t.ToString());
		}
	}
}
```

### 曾遇到的須注意部分

- 要從 LINQ 的查詢結果取得第一筆資料，使用 `.First()` 或 `.FirstOrDefault` 時，如果沒有資料會拋出例外。另外也要避免重複呼叫 `.First()` 或 `.Count()` 方法，避免重複進行查詢。

### 參考資料

- [深入探索LINQ :: 2018 iT 邦幫忙鐵人賽](https://ithelp.ithome.com.tw/users/20107789/ironman/1574)
> 可以更加瞭解 C# 的委派和 LINQ 的關係
- [LINQ寫法：類SQL查詢語法 vs 方法串接-黑暗執行緒](https://blog.darkthread.net/blog/linq-sql-query-vs-methods/)
- [在撰寫 LINQ to SQL 時應注意的幾個小地方 - The Will Will Web](https://blog.miniasp.com/post/2008/05/16/Tips-and-Tricks-in-LINQ-to-SQL-Coding)
- [[C#.NET][LINQ] Query DataTable - 余小章 @ 大內殿堂 - 點部落](https://dotblogs.com.tw/yc421206/2014/07/14/145944)