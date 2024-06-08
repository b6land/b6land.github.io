---
layout: post
title: C# 用 var 和 dynamic 不指定型別宣告變數
date: 2024-06-08 09:00:00 +0800
categories:  [C#]
--- 

在 C# 當中，一般在宣告變數時，都要指定型別，例如基本的 `int` 或是類別名稱 (如 `StringBuilder`)，可是 `var` 和 `dynamic` 能夠在宣告時不指定型別，一起來看看以下的範例。(本篇改寫自個人的鐵人賽文章：[Day 5: C# 關鍵字: var 和 dynamic - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10292004))

### var 的說明與範例

```cs
Dictionary<int, string> books = new Dictionary<int, string>();
books.Add(1,"C# Tutorial");
books.Add(2,"Python 1-2-3");
books.Add(3,"The Basic of C++");
        
foreach(var pair in books){
// foreach(KeyValuePair<int, string> pair in books){ // 效果與 var 相同
    Console.WriteLine("索引: " + pair.Key + ", 書名: " + pair.Value);    
}

var query = from book in books
            where book.Value.Contains("C#")
            select book.Value;
Console.WriteLine("查詢結果: " + string.Join(", ", query));
```

`var` 可以省略掉長長的類別名稱，使程式碼更簡潔；另外也可以方便的接收 LINQ 的執行結果，使開發更加的便利且具備彈性。

### dyanmic 的說明與範例

```cs
public class Computer{
    public void TurnOn(){
        Console.WriteLine("電腦運行中 ...");    
    }
}
    
public class Tablet{
    public void TurnOn(){
        Console.WriteLine("平板電腦已啟動");    
    }
}
    
public static void Main()
{
    dynamic pc1 = new Computer();
    pc1.TurnOn();
    dynamic tab1 = new Tablet();
    tab1.TurnOn();
}
```

`dynamic` 是執行時才知道變數的類型，因此不同類別的物件有相同的方法、屬性時，可以用相同名字呼叫或使用，缺點則是沒辦法在編譯前先知道型別的錯誤。時常被用在動態載入的類別上。

### var 和 dynamic 的差別

`var` 是編譯時會帶入型態 (如 `string`)，實際上執行時和直接指定型別是相同的；而 `dynamic` 是執行時被指定數值後才決定其型態。

### 參考資料

- [#CSharp(C#)：var與dynamic - 新罪楓翼☆灆洢騎士 - 點部落](https://dotblogs.com.tw/knightzone/2013/09/08/116725)
- [C# var資料型態之詳解 - programlin - 點部落](https://dotblogs.com.tw/programlin/2010/05/19/15335)