---
layout: post
title: C# Lambda 語法介紹
date: 2023-10-18 12:00:00 +0800
categories: [C#, Lambda]
---

轉載自己在 IT 邦幫忙發表的文章 [Day 13: C# Lambda 語法介紹 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10297407) 和 [Day 10: C# 再寫一次 Lambda - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10326389)。

## Part 1: Lambda 語法介紹

### 什麼是 Lambda 語法

可用來建立匿名函式。透過委派的方式傳入需要的資料。以下是一個基本的語法，從中取得集合 emp 裡國家為台灣的資料，並以匿名型別呈現：

``` cs
var q = emp.Where(x => x.country.Equals("taiwan"));
```

其實效果相當於以下的匿名函式:

```cs
var anonymous = emp.Where(delegate (Employee x) {return x.country.Equals("taiwan"); });
```

相比兩段語法，會發現 Lambda 語法在傳遞參數時，不一定需要明確的指定參數型別。

### 進一步的 Lambda 語法說明

- 如果需要傳入多個參數時，必須加上括號 (只有一個參數時，括號可以省略)。

``` cs
int a = 3, b = 5;
Func<int, int, int> rect = (x, y) => x * y; // Func<T, T, TResult> 委派
Console.WriteLine(rect(a, b));
```

- 如果需要指定明確型別時 (例如編譯器無法判斷輸入參數的類型)，也要使用括弧。

``` cs
Func<int, string, bool> isEqualLength = (int x, string s) => s.Length == x;
Console.WriteLine(isEqualLength(3, "super"));
```

- 只有單行時不必用 `{}` 包住程式，稱為 Lambda Expression；有多行時需用 `{}` 包住，稱為 Lambda Statements。

### 範例程式碼

``` cs
using System;
using System.Linq;
                    
public class Program
{
    // 員工類別
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
        // 建立測試資料
        employee[] emp = new employee[3];
        emp[0] = new employee("jacky", "taiwan");
        emp[1] = new employee("annie", "america");
        emp[2] = new employee("jeff", "taiwan");

        // 使用 Lambda 語法查詢國家中等於 taiwan 的資料
        var q = emp.Where(x=>x.country.Equals("taiwan"));
        
        foreach(var t in q.AsEnumerable()){
            Console.WriteLine(t.name + ": " + t.country);
        }
}
```

### 參考資料

- [Lambda 運算式 - C# 參考 - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/csharp/language-reference/operators/lambda-expressions)
- [Lambda運算式介紹 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10193784)

## Part 2: 使用 Lambda 查詢數量和排序

用 Lambda 找出集合中特定條件下的數量，可使用 `Count` 語法。
以下範例可找出 Receiver 類別中，name 以 J 開頭的物件數量：

``` csharp
    protected class Receiver{
    	public string name;
    	public string email;

    	public Receiver(string n, string m){
        	name = n;
        	email = m;
    	}
	}

	public void Run(){
    	List<Receiver> receiver = new List<Receiver>();

    	receiver.Add(new Receiver("Jack", "Jack@gmail.com"));
    	receiver.Add(new Receiver("Justin", "Justin@gmail.com"));
    	receiver.Add(new Receiver("Eddy", "Eddy@hotmail.com"));
    	receiver.Add(new Receiver("Jeff", "Chen.Jeff@gmail.com"));

    	int jCount = receiver.Count(n => n.name.StartsWith("J"));
    	Console.WriteLine("Receiver start with J:" + jCount);

    }

```

結果為：

```
Receiver start with J:3
```

若要使用 Lambda 排序，可以使用 `OrderBy()`、`OrderByDescending()` 對欄位分別作遞增、遞減排序。如果需要接著對其它欄位排序，還可以使用 `ThenBy()`、`ThenByDescending()` 。

延續上方的例子，修改內部的 Run 方法，要求以 Receiver 類別的 email 欄位遞增、遞減排序：

``` csharp
    public void Run(){
    	List<Receiver> receiver = new List<Receiver>();

    	receiver.Add(new Receiver("Jack", "Jack@gmail.com"));
    	receiver.Add(new Receiver("Justin", "Justin@gmail.com"));
    	receiver.Add(new Receiver("Eddy", "Eddy@hotmail.com"));
    	receiver.Add(new Receiver("Jeff", "Chen.Jeff@gmail.com"));

    	IOrderedEnumerable<Receiver> AscReceiver = receiver.OrderBy(n => n.email);
    	foreach(Receiver r in AscReceiver){
        	Console.WriteLine(r.email + "(" + r.name + ")");
    	}

    	IOrderedEnumerable<Receiver> DescReceiver = receiver.OrderByDescending(n => n.email);
    	foreach(Receiver r in DescReceiver){
        	Console.WriteLine(r.email + "(" + r.name + ")");
    	}
	}
```

輸出結果為：

```
Chen.Jeff@gmail.com(Jeff)
Eddy@hotmail.com(Eddy)
Jack@gmail.com(Jack)
Justin@gmail.com(Justin)

Justin@gmail.com(Justin)
Jack@gmail.com(Jack)
Eddy@hotmail.com(Eddy)
Chen.Jeff@gmail.com(Jeff)
```

### 參考資料

[C# 3.0 極簡風 - Lambda Expression-黑暗執行緒](https://blog.darkthread.net/blog/lambda-expression/)
[c# - Multiple Order By with LINQ - Stack Overflow](https://stackoverflow.com/questions/2318885/multiple-order-by-with-linq)
[c# - Get item count of a list<> using Linq - Stack Overflow](https://stackoverflow.com/questions/3853010/get-item-count-of-a-list-using-linq)