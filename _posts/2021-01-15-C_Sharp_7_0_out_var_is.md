---
layout: post
title: C# 7.0 新特性：out var 與 is
date: 2021-01-15 12:00:00 +0800
categories:  [C#]
--- 

以下介紹 C# 7.0 中新增的兩個關鍵字 `out var` 和 `is`。

### out var

原本的 `out` 語法可以傳入已經宣告的變數到方法內，且該方法一定會修改變數，常用來回傳兩個或更多的變數。

如下方程式碼：

```cs
static void Main(string[] args)
{
    int sum, sub;
    Math(5, 10, out sum, out sub);
    Console.WriteLine("Sum : {0}, Sub :{1}", sum, sub);
}

public static void Math(int a, int b , out int sum, out int sub) // 宣告參數時加入 out 修飾，表示參數會被修改
{
    sum = a + b;
    sub = b - a;            
}
```

C# 7.0 開始可以使用 `out [變數型別]` 傳入變數，例如 `out int`，不必先宣告變數，語法更加簡潔。

上方呼叫 `Math` 方法的程式碼可修改如下：

```cs
Math(5, 10, out int sum, out int sub);
```

我們也可以改成用 `out var` 傳入變數，效果和 `out [變數型別]` 相同：

```cs
Math(5, 10, out var sum, out var sub);
```

*(以上程式碼修改自參考資料)*

*(C# 7.0 開始若要回傳多個變數，也可參考拙作 [C# Tuple](/C_Sharp_Tuple/))*

### is

`is` 用於檢查某個變數是否相容於某個類別 (或型別)。

下面是微軟的官方範例，展示了 Nullable 的 `int` 型別變數、被封裝成 `object` 的 `int` 變數，使用 `is` 判斷，都能正確識別是 `int`。在 C# 7.0 中，當判斷是 `int` 時，能用 Declaration Pattern 直接宣告為變數 `a` 和 `b`。

```cs
int i = 23;
object iBoxed = i;
int? jNullable = 7;
if (iBoxed is int a && jNullable is int b)
{
    Console.WriteLine(a + b);  // 輸出 30
}
```

除此之外，也可以用來檢查某個變數是不是 `null`，而在 C# 9.0 以後，還可以加入 `not` 做反向檢查 (套用 Negation Pattern):

```cs
if (a is null)
{
// ……
}

if (a is not null)
{
// ……
}
```

`is` 還有幾種在新版本 C# 可辨識的 pattern，請查閱參考資料。

*(2024/05 更新為鐵人賽文章的內容)*

### 參考資料

- [Out Parameters In C# 7.0](https://www.c-sharpcorner.com/article/out-parameters-in-c-sharp-7-0/)
- [is operator - C# reference - Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/is)
- [Patterns - C# reference - Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/patterns#declaration-and-type-patterns)