---
layout: post
title: C# Delegate
date: 2023-01-23 12:00:00 +0800
categories: [C#]
---

C# 有一個特色稱之為 Delegate (委派)。以下是我對 [A tour of C# - Major language areas - Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/features#delegates-and-lambda-expressions) 中委派的解讀。

### 微軟的介紹

根據介紹，委派即是「有特定參數清單和回傳類別的方法的參考」。

委派將方法視為物件的實體，且可以做為參數傳入。在其它的程式語言中 (例如 C 語言)，有類似的「函式指標」概念。但是委派為物件導向且型別安全 (Type-safe)。

例如網頁裡的例子:

```cs
delegate double Function(double x); // 宣告委派，需傳入 double 參數，並回傳 double 變數

class Multiplier
{
    double _factor;

    public Multiplier(double factor) => _factor = factor; 

    public double Multiply(double x) => x * _factor; 
}

class DelegateExample
{
    static double[] Apply(double[] a, Function f) // 傳入 double 陣列和委派方法
    {
        var result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    public static void Main()
    {
        double[] a = { 0.0, 0.5, 1.0 };
        double[] squares = Apply(a, (x) => x * x); // 傳入 Lambda 運算式
        double[] sines = Apply(a, Math.Sin); // 傳入 Math.Sin 方法
        Multiplier m = new(2.0);
        double[] doubles = Apply(a, m.Multiply); // 傳入物件的 Multiply 方法
    }
}
```

上述程式碼傳入一個委派方法，該委派方法需要傳入一個 double 參數，並回傳 double 變數作為結果。

如果傳入的方法是物件方法，則呼叫方法期間 `this` 會指向該委派的物件。

### 參考資料

- [DAY 17 委派 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10205614)