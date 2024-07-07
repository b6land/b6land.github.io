---
layout: post
title: C# 的字串插補符號 $
date: 2024-07-07 18:00:00 +0800
categories:  [C#]
--- 

`$` 符號從 C# 6.0 開始出現，可以搭配大括號插入變數、字串，以下為使用的範例和解說。

### 範例與解說

```cs
string userName = "Ming";
string id = "A001";
string text = $"{userName}'s ID is:{id}";
Console.WriteLine(text);
```

上方程式碼的 `text` 變數在指派時，前方加上 `$` 後，就能直接加入變數名稱，插入裡面的內容。

這是除了 `string.format()` 以外的另一種字串插入變數的方式，優點是插入數值更加簡潔和直覺，也可以避免需要維護 `string.format()` 中索引位置的問題。

另外：

1. 如果同時要使用 `@` 符號，在 C# 8.0 以前必須依照順序 $@；C# 8.0 以後則不用依照順序出現。
2. 在編譯時，通常會編譯成 `String.Format`，有串接行為時，可能以 `String.Concat` 取代；如果被插入的變數是 `IFormattable` 或 `FormattableString` 類型，會呼叫 `FormattableStringFactory.Create`。

### 參考資料

- 本篇曾發表於 [Day 6: C# 關鍵字: $ - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10292299)
- [c# - What's with the dollar sign ($"string") - Stack Overflow](https://stackoverflow.com/questions/32878549/whats-with-the-dollar-sign-string) 
- [C# Interpolated Strings 字串插值-黑暗執行緒](https://blog.darkthread.net/blog/c-interpolated-string/)
- [$ - 字串插補 - C# 參考 - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/csharp/language-reference/tokens/interpolated)