---
layout: post
title: C# 7.0 新特性：out var 與 is
date: 2021-01-15 12:00:00 +0800
categories: C#
tags: [C#]
--- 

以下介紹 C# 7.0 中新增的兩個關鍵字。

### out var

在 C# 中，常使用以下的方式將變數試圖剖析為特定型別的變數：

```
bool value;
bool.TryParse(object input, out object value);
```

在 C# 7.0 中，可以使用以下的語法，減少程式碼的使用：

```
int.TryParse(input, out var value);
```

`out var` 會幫你宣告該 `value` 變數，效果沒有改變，是一種語法糖。

### is

用於檢查某個變數是否相容於某個類別 (或型別)。
在 C# 7.0 中，可以在檢查類別 (或型別) 時，建立區域變數。

```
int i = 23;
object iBoxed = i;
int? jNullable = 7;
if (iBoxed is int a && jNullable is int b)
{
    Console.WriteLine(a + b);  // output 30
}
```

### 參考資料
- [C# Language - out var聲明 - c# Tutorial](https://riptutorial.com/zh-TW/csharp/example/6326/out-var%E8%81%B2%E6%98%8E)
- [is - C# 參考 - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/csharp/language-reference/keywords/is)
- [Type-testing and cast operators - C# reference - Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/type-testing-and-cast)