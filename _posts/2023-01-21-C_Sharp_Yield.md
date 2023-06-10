---
layout: post
title: C# Yield 關鍵字
date: 2023-01-21 12:00:00 +0800
categories: [C#]
---

什麼是 `yield` ? 在 C# 中有什麼作用呢 ?

### 內文

在 C# 中有 `foreach` 語法，可以快速的列舉集合 (ex. List) 中的所有元素。

而 `foreach` 的背後，其實就是集合的類別實做了 IEnumerable 介面，即 Iterator 設計模式。讓其他人操作此類別的集合時，不必知道集合列舉的邏輯，就能一一的列舉元素。

但如果要自己實作 IEnumerable 介面，會需要寫很多程式碼，因此可以透過 `yield` 精簡程式碼，寫出更直覺的語法。

從 [IEnumerable\<T\> Interface (System.Collections.Generic) - Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1?view=net-7.0) 的例子，可以看到要實作 IEnumerable 介面，就要實作 Current 屬性, Dispose()、MoveNext()、Reset() 等方法。

使用 `yield` 的話，參考 [yield statement - provide the next element in an iterator - Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/yield?redirectedfrom=MSDN) 的例子，只需要在 IEnumerable 介面內撰寫列舉的邏輯，並加入 `yield` 關鍵字即可。

### 其它參考資料

- [C# IEnumerator, IEnumerable, and Yield](https://dev.twsiyuan.com/2016/03/csharp-ienumerable-ienumerator-and-yield-return.html)
- [[.NET]快快樂樂學LINQ系列前哨戰－IEnumerable, IEnumerator, yield, Iterator - In 91 - 點部落](https://dotblogs.com.tw/hatelove/2012/05/10/introducing-foreach-ienumerable-ienumerator-yield-iterator)
- [仔細體會yield的甜美: yield介紹 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10193586)