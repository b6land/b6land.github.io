---
layout: post
title: C# 結構 (struct)
date: 2024-01-11 12:00:00 +0800
categories: [C#, Interview]
---

結構 (struct) 在部分的程式語言裡很常使用，在物件導向的 C# 語言內，也可以用來存放輕量的內容。

### 結構的幾個重要特性

- 可以使用方法、屬性等大部分的成員，不過有以下限制：
    - 無法像類別被繼承，但可以從 interface 實作。
    - 無法實作 finalizer (完成項，舊譯為解構函式)。
    - C# 11 之前，建構子必須初始化結構內所有欄位。
- struct 是數值型別，class 是參考型別。struct 適合用來定義輕量型物件，因為數值型別在配置記憶體上，成本較參考型別來得小；然而，當需要複製資料，或是對資料進行封裝或傳遞時，參考型別的成本較低。
- 可使用具有參數的建構函數，但不能自己寫預設 _(無參數)_ 的建構函數

### 使用方式

建立方式如下：

```cs
public struct Coords
{
    public int x, y;

    public Coords(int p1, int p2)
    {
        x = p1;
        y = p2;
    }
        
    public void add(Coords c)
    {
        x = x + c.x;
        y = y + c.y;
    }
}
```

使用方式如下：

```cs
Coords c1 = new Coords();
c1.x = 5;
c1.y = -3;
Coords c2 = new Coords(10, 10);
        
c2.add(c1);

Console.WriteLine($"Coords 1: {c1.x}, {c1.y}");
Console.WriteLine($"Coords 2: {c2.x}, {c2.y}");
```

### 結構與類別怎麼選擇 ?

官方的建議：

1. 當內容比較簡單，且生命週期較短，或是嵌入在其它類別內時可以使用結構。
2. 在邏輯上趨近於基本型別，不常被封裝，為不可變，且大小不大於 16 bytes 可以使用結構。

其它狀況建議使用類別。

### 參考資料

- [結構 - C# 程式設計手冊 - Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/structs)
- [Choosing Between Class and Struct - Framework Design Guidelines - Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/choosing-between-class-and-struct)
- [\[C#\] 結構 - .NET 隨筆 - 點部落](https://dotblogs.com.tw/atowngit/2009/08/28/10292)