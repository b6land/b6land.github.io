---
layout: post
title: C# 泛型 (Generic)
date: 2025-02-27 22:00:00 +0800
categories: [C#]
--- 

寫了多年程式，仍然感覺到對泛型不夠熟悉 OTZ

### 泛型的優點

1. 提高程式碼的重複使用度：一個泛型類別 / 方法可以支援多個不同類型的資料。
2. 型別安全 (type safe): 例如同樣功能的 `ArrayList` 和 `List<T>` 結構，`ArrayList` 可以加入各種類型的資料，而 `List<T>` 只允許加入一種型別的資料，可以在編譯期間就確保沒有其它類型資料加入。

``` cs
ArrayList list = new ArrayList();
list.Add(123);
list.Add("123");
List<int> intList = new List<int>();
intList.Add(123);
intList.Add("123"); // 提示錯誤
```

### C# 內常見的泛型用途

- `System.Collections.Generic` 裡面就有包含了多個泛型的資料結構，如字典 (Dictionary)、佇列 (Queue) 和堆疊 (Stack)。
- LINQ 語法的使用上也會用到，對各種不同類型的資料進行查詢。

### 自己寫泛型類別、方法

微軟的官方說明 [泛型類別和方法 - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/csharp/fundamentals/types/generics) 裡，有教你怎麼設計自己的泛型類別和方法：


```csharp
// Type parameter T in angle brackets.
public class GenericList<T>
{
    // The nested class is also generic, and
    // holds a data item of type T.
    private class Node(T t)
    {
        // T as property type.
        public T Data { get; set; } = t;

        public Node? Next { get; set; }
    }

    // First item in the linked list
    private Node? head;

    // T as parameter type.
    public void AddHead(T t)
    {
        Node n = new(t);
        n.Next = head;
        head = n;
    }

    // T in method return type.
    public IEnumerator<T> GetEnumerator()
    {
        Node? current = head;

        while (current is not null)
        {
            yield return current.Data;
            current = current.Next;
        }
    }
}
```

上面的大寫 `T` ，在實際建立類別會被代換，例如 `GenericList<int>`  ，`T`  會被代換成 `int` 。

### 參考資料

- [變來變去的Generic Type: 泛型介紹 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10194019)
- [.NET 的泛型 - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/standard/generics/)
- 自己的鐵人賽文章：[Day 10: C# 泛型 (Generic) 概念 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10295622)