---
layout: post
title: C# this 關鍵字
date: 2024-01-28 12:00:00 +0800
categories:  [C#]
--- 

`this` 關鍵字在 C# 中，有幾種不同的用途：指定類別內的欄位、傳遞自身作為參數，以及類別的擴充方法。

### this 的用途

this 關鍵字可以用來指定類別內的欄位，避免與傳入的欄位名稱混淆。

例如在建構式中，指定參數給類別內的變數時，就可以使用：

```csharp
public class Animal{
    private string name;
    public Animal(string name){
        this.name = name;
    }
}
```

或是將物件自身傳遞給其他類別：

```csharp
public class Animal{
    private string name;
    Bird bird;
    public Animal(string name){
        this.name = name;
        bird = new Bird(this); // 將自己傳遞給 Bird 類別
    }
}
public class Bird{
    private string name;
    public Bird(Animal animal){
        this.name = animal.name;
    }
}
```

也可作為擴充方法使用：如果方法是宣告為 public，且為 static 的方法。可以在參數的類型前方加入 this 關鍵字，作為該類型的擴充方法。範例請見下節。

參考資料：[C# this Keyword - Dot Net Perls](https://www.dotnetperls.com/this)  

### 檢查集合是否為 Null 或空白

`Microsoft.IdentityModel.Tokens.CollectionUtilities` 類別提供了一個實用的擴充方法：`IsNullOrEmpty<T>(this IEnumerable<T> enumerable)`。

其內容如下：

```csharp
/// <summary>
/// Checks whether <paramref name="enumerable"/> is null or empty.
/// </summary>
/// <typeparam name="T">The type of the <paramref name="enumerable"/>.</typeparam>
/// <param name="enumerable">The <see cref="IEnumerable{T}"/> to be checked.</param>
/// <returns>True if <paramref name="enumerable"/> is null or empty, false otherwise.</returns>
public static bool IsNullOrEmpty<T>(this IEnumerable<T> enumerable)
{
    return enumerable == null || !enumerable.Any();
}
```

參數中使用了 this 關鍵字和泛型，因此我們可以使用以下方式檢查集合是否為 Null 或空白：

```csharp
List<int> myList;
// do something ...
bool isEmpty = myList.IsNullOrEmpty();
```