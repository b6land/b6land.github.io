---
layout: post
title: C# 用 Delegate、Func 和 Action 傳遞方法參數
date: 2023-01-23 12:00:00 +0800
categories: [C#]
---

C# 中，如果要將方法作為參數傳遞，可以透過 Delegate (委派)，以及更方便的 `Func` 和 `Action` 語法。

### 微軟的委派介紹

根據我對 [A tour of C# - Major language areas - Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/features#delegates-and-lambda-expressions) 中委派的解讀，委派即是「有特定參數清單和回傳類別的方法的參考」。

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

### 更方便的 `Func` 和 `Action`

可以不用每次都宣告委派，C# 提供強型別的語法傳入方法參數。簡單來說，可以使用 `Func` 傳入方法，並指定傳入方法的參數數量、回傳型別；若不需要回傳物件，則可以用 `Action` 指定傳入方法的參數數量。

`Func <T1, T2, TResult> arg1` 表示要傳入兩個參數，類型分別為 `T1` 和 `T2`，並回傳 `TResult` 類別，而 `Action <T1, T2> arg1` 表示傳入 `T1` 和 `T2` 兩個參數。更詳細的規格可參考 [Strongly Typed Delegates - C# - Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/csharp/delegates-strongly-typed)。

`Func` 的範例程式碼如下 (擷取自 StackOverflow 的問題 [.net - Pass Method as Parameter using C# - Stack Overflow](https://stackoverflow.com/questions/2082615/pass-method-as-parameter-using-c-sharp) 與回答，並略微編輯)：

```csharp
public class Class1
{
    public int Method1(string input)
    {
        //... do something
        return 0;
    }

    public int Method2(string input)
    {
        //... do something different
        return 1;
    }

    public bool RunTheMethod(Func<string, int> myMethodName)
    {
        //... do stuff
        int i = myMethodName("My String");
        //... do more stuff
        return true;
    }

    public bool Test()
    {
        return RunTheMethod(Method1);
    }
}
```

### 其它參考資料

- [DAY 17 委派 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10205614)