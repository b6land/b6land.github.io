---
layout: post
title: C# Regex 正規表示式常見語法
date: 2025-04-03 12:00:00 +0800
categories: [C#]
--- 

正規表示式 (Regular Expression, Regex)，可以用來查詢與取代符合格式的字串，許多的編輯器和程式語法都支援，C# 內常用的用法如下。

### 常用語法

以下的程式需要引用 `using System.Text.RegularExpressions;`  命名空間。

`Regex.IsMatch`  方法可以檢查字串內是否有符合表示式的子字串，回傳為 bool。

```csharp
Regex.IsMatch("100號", "[0-9]+號"); // true
Regex.IsMatch("信義路100號", "[0-9]+號"); // true
Regex.IsMatch("信義路號", "[0-9]+號"); // false
```

`Regex.Match`  方法則可以取得匹配的結果，其回傳類別是 `System.Text.RegularExpressions.Match` ，使用命名空間後也是 `Match` 。

`Match`  類別的 `Index`  屬性，可以取得字串中匹配的位置。

`Regex.Matches(text, pattern).Count`  則可以計算符合表示式的子字串出現次數，例如以下程式碼可以計算出現了幾個中文字：

```csharp
Regex.Matches("中文字123", "[\u4e00-\u9fff]").Count // Count = 3
```

(註：UTF-8 的中日韓漢字即位於 U+4E00..U+9FFF 之間，請參考 [整理 Unicode 經常會使用到的內碼區域並透過 Regex 自動比對文字 - The Will Will Web](https://blog.miniasp.com/post/2019/01/02/Common-Regex-patterns-for-Unicode-characters))

### 參考資料

- 維基百科：[正規表示式 - 維基百科，自由的百科全書](https://zh.wikipedia.org/zh-tw/%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F)
- 官方說明：[.NET 規則運算式 - .NET - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/standard/base-types/regular-expressions)
- 使用 Match 找到匹配的位置：[c# - Is there a function that returns index where RegEx match starts? - Stack Overflow](https://stackoverflow.com/questions/1851795/is-there-a-function-that-returns-index-where-regex-match-starts)
- 測試 Regex 的網站：[RegExr: Learn, Build, & Test RegEx](https://regexr.com/)，可以一次測試多組資料。