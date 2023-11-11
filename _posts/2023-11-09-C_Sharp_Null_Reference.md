---
layout: post
title: C# Nullable Reference Types
date: 2023-11-09 12:00:00 +0800
categories: [C#]
---

C# Nullable Reference Types 是在 C# 8.0 提供的功能。可以選擇是否要啟用。(.NET 6.0 以後預設啟用)

### 介紹

啟用前，參考型別預設可為 `Null`，但是容易在傳入 `Null`` 時，引發 `NullReferenceException`。
啟用後，參考型別預設變得不能 `Null`，必須在型別後面加入 ?，宣告成 Nullable 型別才可為 Null。

範例可參考 [C# 8 的 Nullable Reference Types - Huan-Lin 學習筆記](https://www.huanlintalk.com/2020/03/csharp-8-nullable-reference-types.html) 的程式碼：

```cs
string str1 = "hello"; // str1 是不可為 null 的字串
string? str2 = null;   // str2 是可為 null 的字串
```

部分舊的程式碼在編譯時可能會出現類似以下的警告：

```
Warning CS8618: Non-nullable field 'field-name' is uninitialized.
```

除了改寫以外，可以使用前置處理指示詞 (須謹慎使用)：

```
#pragma warning disable CS8618 
...
#pragma warning restore CS8618
```

可進一步參考：[Mastering Null In C# 11 and .NET 7 - Jon D Jones](https://www.jondjones.com/programming/aspnet-core/fundamentals/mastering-null-in-c-11-and-net-7/)