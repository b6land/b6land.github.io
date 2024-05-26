---
layout: post
title: C# 用 ? 運算子判斷條件與 Null
date: 2024-05-27 07:00:00 +0800
categories:  [C#]
--- 

`?`  運算子在 C# 裡面，可以用來做簡單的條件判斷，還有判斷變數是不是 `null`，以及宣告變數為 Nullable。(改寫自己的鐵人賽文章 [Day 4: C# 關鍵字: ? - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10291159))

### 三元運算子

`?` 可作為三元運算子 (ternary operator)，在敘述中做簡易的判斷，以簡化原本要用 `if` 表示的邏輯。用法為：**變數 = 條件式 ? 成立 : 不成立**，例如：

```cs
Int a = 0;
Int b = 0;
a = b > 0 ? 1 : 0;
```

第三行的邏輯是，假如 `b` > 0，則 `a` = 1，否則為 0。以上的例子中，`a` 將為 0。

### Null 條件運算子

在物件的後方輸入 `?.` ，可作為 Null 條件運算子使用。

如果 `a` 是 `null` 時， `a?.x` 或 `a?[x]` 的評估結果也會是 `null`，不為 `null` 時則等同於原本的值 `a.x` 或 `a[x]`。

根據微軟官方文件，假如有多個變數或成員以 `?.` 連接，那麼最後的變數或執行方法不會被評估。 

因為看起來有點複雜，試著寫一個 Null 條件運算子的範例：

```cs
Car? car = null;
car?.Name.ToUpper(); // 最後的 ToUpper() 方法不會被執行
try {
    (car?.Name).ToUpper();    
}
catch (NullReferenceException) {
    Console.WriteLine("NullReferenceException");
]}; // 會觸發 Null Reference Exception

public class car{
    public string? Name {get; set;};
}
```

### 宣告 Nullable 型別

若變數宣告時，在行別後方加入 `?` ，例如 `int? a` ，表示該變數是可以被指定為 `null` (Nullable Type)，可用在資料庫中有 null 欄位的處理 (例如資料庫中的 `int` 欄位，可以是數字或 `null`)。在 C# 8.0 以後，物件宣告時，預設都需要在後方加上 `?` ，讓編譯器更嚴格的檢查可能造成 null 的狀況。

Nullable 型別本身提供 `hasValue` 屬性，可判斷是不是 `null` 值，且可以使用 `Value` 屬性取得數值。相關的範例，請參考下方 Stack Overflow 問題的回答。

### 參考資料

- [Member access operators and expressions - C# reference - Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-)
- [c# - What is the purpose of a question mark after a type (for example: int? myVariable)? - Stack Overflow](https://stackoverflow.com/a/2690890)