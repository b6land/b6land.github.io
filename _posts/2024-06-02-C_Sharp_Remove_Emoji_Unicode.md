---
layout: post
title: C# 如何移除表情符號 (Emoji)，以及相關 Unicode 知識
date: 2024-06-02 17:00:00 +0800
categories:  [C#]
--- 

這篇文章除了告訴你怎麼移除表情符號 (Emoji) 以外，還附帶 Unicode 和 UTF-16 的愛恨交織唷 (誤)。

### C# 內移除表情符號 (Emoji)

在 C# 中，`char` 的大小是 16 位元，範圍是 U+0000 到 U+FFFF，代表 Unicode UTF-16 字元。這個範圍收錄了各國文字 (包含英文、中日韓字元)、標點符號等。大部分的 Unicode 文字都可以用一個 `char` 表示，不過少部分文字，如表情符號 (Emoji) 🦄，因為編碼從 U+10000 開始，需要兩個 `char` 表示 \[2\]。

因此想要在 C# 中移除表情符號，可以移除 UTF-16 一部分範圍的字元，可稱為 Surrogate Character (替代字元)。不過有一部分的表情符號不一定屬於此種字元，例如 U+2764 (愛心 ❤)。

可以使用正規表達式移除表情符號，語法如下。其中的 Cs 表示 Unicode 的 Surrogate 分類字元\[3\]：

```csharp
string result = Regex.Replace(text, @"\p{Cs}", "");
```

### 關於 Unicode 和 UTF-16

Unicode (萬國碼) 是一種標準，定義世界上絕大部分的文字如何編碼。在 Unicode 中的定義中，BMP 稱之為基本多語言平面 (Basic Multilingual Plane)，其內包含多種語言的字元集 \[4\]，例如中日韓字元集\[5\] (CJK)。

而 Non-BMP 也可稱為輔助平面 (Supplementary Planes)。上述提到的表情符號 (Emoji) 即為 Non-BMP 字元。

UTF-16 是實作 Unicode 定義的編碼方式。雖然只有 16 個位元，但是可以透過代理字元，使用兩個字元表示 U+10000 以後的 Non-BMP 字元。

另外，如果要移除 BMP 內相關的符號，可以參考 \[6\] 的字元編碼範圍。

### 參考資料

- \[1\] [Char 類型 - C# reference - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/csharp/language-reference/builtin-types/char)
- \[2\] [Introduction to character encoding in .NET - .NET - Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/standard/base-types/character-encoding-introduction)
- \[3\] [c# - How do I remove emoji characters from a string? - Stack Overflow](https://stackoverflow.com/questions/28023682/how-do-i-remove-emoji-characters-from-a-string)
- \[4\] [UTF-16 - Wikipedia](https://zh.wikipedia.org/zh-tw/UTF-16)
- \[5\] [CJK characters - Wikipedia](https://en.wikipedia.org/wiki/CJK_characters)
- \[6\] [unicode - How to remove emoji code using javascript? - Stack Overflow](https://stackoverflow.com/questions/10992921/how-to-remove-emoji-code-using-javascript)