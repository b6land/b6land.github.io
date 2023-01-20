---
layout: post
title: C# Tuple
date: 2023-01-20 12:00:00 +0800
categories: [C#]
---

Tuple 是 C# 的語法，可以解決以往呼叫方法後，如何回傳多個變數的問題。

### 介紹

以往方法要回傳多個變數時，通常需要以下方式：

1\. 使用 `ref` 或 `out` 方式，傳入多個參數。但是參數會變得非常多，也一定要傳入。
```cs
public void AMethod(ref int i, ref string str){
    i = 123;
    str = "text";
}

void run(){
    int i = 0;
    string str = "";
    AMethod(ref i, ref str);
    Console.WriteLine(i + ", " + str);
}
```
2\. 建立一個類別，裡面包含要回傳的變數。缺點是需要設計額外的類別 (而且可能只用這麼一次)。
```cs
public class AClass{
    public int i;
    public string str;
}

public AClass AMethod(){
    AClass a = new AClass();
    a.i = 123;
    a.str = "text";
    return a;
}

void run(){
    AClass ret = AMethod();
    Console.WriteLine(ret.i + ", " + ret.str);
}
```

Tuple 可以取代以上兩種回傳方式，寫出更簡潔的程式碼。

```cs
public (int i, string str) AMethod(){
    return (1, "text");
}

void run(){
    var ret = AMethod();
    Console.WriteLine(ret.i + ", " + ret.str);
}
```

### 參考資料
- [C# 7 新增的 Tuple 語法 - Huan-Lin 學習筆記](https://www.huanlintalk.com/2017/04/c-7-tuple-syntax.html)
- [Tuple 類型 - C# 參考 - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/csharp/language-reference/builtin-types/value-tuples)