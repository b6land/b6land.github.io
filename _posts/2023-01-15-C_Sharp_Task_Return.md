---
layout: post
title: C# Task 回傳值
date: 2023-01-15 12:00:00 +0800
categories: [C#]
---

本篇介紹如何從 Task 中傳回數值。

### 作法

建立泛型的 Task 類別，並在 Lambda 運算式裡回傳結果。

```cs

///<summary> 物品類別 </summary>
class Item 
{
    public string? Name { get; set; }
    public int ID { get; set; }
}

///<summary> 執行程式 </summary>
public void Run(){
    // 以 Lambda 運算式建立 Task
    Task<Item> task3 = Task<Item>.Factory.StartNew(() =>
    {
        string s = "Printer";
        int id = 15;
        return new Item {ID = id, Name = s};
    });
    Item printer = task3.Result; // 取得 Task 執行結果
    Console.WriteLine($"{printer.ID}-{printer.Name}");
}
```

也可以不用 Lambda 運算式達成：

``` cs
///<summary> 物品類別 </summary>
class Item
{
    public string? Name { get; set; }
    public int ID { get; set; }
}

/// <summary> 建立新物品 </summary>
private Item NewItem(){
    Item newItem = new Item();
    newItem.ID = 30;
    newItem.Name = "Mouse";
    return newItem;
}

///<summary> 執行程式 </summary>
public void Run(){
    Task<Item> task4 = new Task<Item>(NewItem); // 傳入方法建立 Task
    task4.Start();
    Item mouse = task4.Result; // 取得 Task 執行結果
    Console.WriteLine($"{mouse.ID}-{mouse.Name}");
}
```

### 參考資料

[How to: Return a Value from a Task](https://docs.microsoft.com/zh-tw/dotnet/standard/parallel-programming/how-to-return-a-value-from-a-task)

