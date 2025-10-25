---
layout: post
title: C# Static 優缺點、變數生命週期
date: 2023-11-08 12:00:00 +0800
categories: [C#]
---

C# 的 `static` 關鍵字，可以宣告類別或方法為靜態，那麼和一般的類別、方法有什麼不一樣呢？

### 關於 static

- 可以宣告靜態類別或方法，不必使用 new() 建立物件就能使用。
- 只能有一份實例 (Instance)，在應用程式啟動時即產生，直到程式結束。
- 難以套用多型物件，以及在運行中動態注入資料物件。

### Web 專案內使用 Static 的優缺點

- 可以保持只有一份實例，不需要 GC 機制。
- 但是比較適合公用程式或參數，例如紀錄 Log、計算分數的程式等。
- 導致較難撰寫單元測試。
    - 例如外部依賴 (網路、資料庫) 的類別與方法，無法藉由實作介面，在測試期間建立模擬 (Mock) 物件，並動態注入。

### 一般類別裡 static 變數的生命週期

如果 static 變數放在一般的類別裡，它會一直存在嗎 ?

例如以下的範例：

```csharp
using System;
					
public class Program
{
	public static void Main()
	{
		Employee a = new Employee("Alice");
		a.Show();
		
		Employee b = new Employee("Berry");
		b.Show();
		
		a = null;
		b = null;
		
		Console.WriteLine("Count: " + Employee.count);
	}
}

public class Employee
{
	public static int count = 0;
	string name;
	
	public Employee(string n){
		name = n;	
		count ++;
	}
	
	public void Show(){
		Console.WriteLine($"{count}: {name}")	;
	}
}
```

它的輸出結果是：

```csharp
1: Alice
2: Berry
Count: 2
```

答案：是的，會一直存在，直到應用程式結束才會被清除。

### 參考資料

- [static 修飾詞 - C# reference - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/csharp/language-reference/keywords/static)
- [c# - Benefit of using static methods in web application - Stack Overflow](https://stackoverflow.com/questions/7338275/benefit-of-using-static-methods-in-web-application)