---
layout: post
title: C# 資料結構的使用
date: 2019-02-09 12:00:00 +0800
categories:  [C#]
---

本文紀錄了 Dictionary、Struct 和 List 的一部分使用方式。

### 檢查 Dictionary 是否包含某個 Key

可以使用 `ContainsKey(key)` ：

``` csharp
if (dict.ContainsKey(key)) { ... }
```

或 `TryGetValue`：

``` csharp
dict.TryGetValue(key, out value);
```

參考資料：[exchangewebservices - How can I detect if this dictionary key exists in C#? - Stack Overflow](https://stackoverflow.com/questions/2829873/how-can-i-detect-if-this-dictionary-key-exists-in-c)

### Dictionary 如何使用 foreach

使用 `KeyValuePair` 即可分別取出 Key 和 Value。

``` csharp
_DicList = new Dictionary<string, string>();
_DicList.Add("s1", "test1");
_DicList.Add("s2", "test2");
_DicList.Add("s3", "test3");

foreach (KeyValuePair<string, string> item in _DicList) {
    Console.WriteLine(item.Value);
}
```

參考資料：[[C#] Dictionary 如何使用 foreach (KeyValuePair)](https://dotblogs.com.tw/atowngit/2010/07/30/blogseo-to-beat-a-dead-horse)

### 使用結構 (struct)

在特定情形下，使用 `struct` 代替類別儲存資料，能夠節省使用的記憶體空間。

- **可以使用方法。**
- `struct` 是實值型別，`class` 是參考型別，因此 `struct` 適合用來定義輕量型物件。
- 可使用具有參數的建構函數，但不支援預設 *(無參數)* 建構函數

建立方式如下：

``` csharp
public struct Coords
{
    public int x, y;

    public Coords(int p1, int p2)
    {
        x = p1;
        y = p2;
    }
}
```

使用方式如下：

``` csharp
// Initialize:   
Coords coords1 = new Coords();
Coords coords2 = new Coords(10, 10);
```

參考資料：

[使用結構 - C# 程式設計手冊 - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/csharp/programming-guide/classes-and-structs/using-structs)

[結構 - C# 程式設計手冊 - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/csharp/programming-guide/classes-and-structs/structs)

### 如何複製 List

若 List<Type> 的元素是實值的話，直接作為參數傳入新 List 的建構式即可：

``` csharp
List<YourType> newList = new List<YourType>(oldList);
```

若元素是參考的話，則可能需要逐一元素進行複製。

[How do I clone a generic list in C#? - Stack Overflow](https://stackoverflow.com/questions/222598/how-do-i-clone-a-generic-list-in-c)

