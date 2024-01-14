---
layout: post
title: C# 繼承 (Inheritance) 
date: 2024-01-14 12:00:00 +0800
categories: [C#]
---

寫 C# 數年後，再重新複習一次繼承的概念與實作。

### 繼承介紹

- 從父類別繼承時，可以使用非私有的成員 (Member)，如屬性 (Field) 與方法 (Method)。
- 使用 `virtual`  宣告此方法可被子類別重新實作。
- 使用 `abstract`  時，父類別不實作方法內的邏輯，由子類別去實作。
- 使用 `override`  宣告此方法覆蓋父類別原本的方法。
- 以下的範例程式，雖然 Main 中宣告的是父類別 X，但因為實際建立的是子類別 Y，因此會呼叫子類別的 `Show()`  語法。
    - 這是一種多型 (Polymorphism) 的應用。

``` cs
using System;

public class X
{
	public virtual void Show(){
		Console.WriteLine("X Show");
	}
}

public class Y:X
{
	public override void Show(){
		Console.WriteLine("Y Show");
	}
}

public class Program
{
	public static void Main()
	{
		X x = new Y();
		x.Show();
	}
}
```

- 參考資料：[C# 物件導向程式設計(Object-oriented programming，OOP) (三) 繼承 - 阿夢的程式設計天地 - 點部落](https://dotblogs.com.tw/ace_dream/2016/01/12/inheritance)