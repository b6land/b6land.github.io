## C# 關鍵字 3

本文章紀錄 C# 的相關關鍵字說明。

### 關鍵字說明

- `.ctor` 函式：建構子 (Constructor) 的縮寫。可能在解譯 C# 程式時看到。
- `?.` 運算子 : Null 條件運算子 (`a` 評估為 null 時， `a?.x` 或 `a?[x]` 的評估結果也會是 null，不為 null 時則為原本的值)。
- `?` 運算子：在敘述中做簡易的判斷 (變數 = 條件式 ? 成立 : 不成立，如 `a = b > 0 ? 1 : 0`)。
- `int? a` 宣告：表示該變數可以被指定為 null。
- `var` 和 `dynamic` 的差別：var 是編譯時會帶入型態 (如 string)，實際上執行時和直接指定型別是相同的，dynamic 是執行時被指定數值後才決定其型態。
- 使用 `$` 插值：可以使用大括號插入數值，如 `string text = $"{user}'s ID is:{id}"`，作為除了 `string.format()` 以外的另一種字串插入變數的方式，可以避免需要維護 `string.format()` 中索引的問題。 

### 參考資料
- [c# - What is the meaning of CTOR? - Stack Overflow](https://stackoverflow.com/questions/4614099/what-is-the-meaning-of-ctor)
- [Member access operators and expressions - C# reference - Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-)
- [c# - What is the purpose of a question mark after a type (for example: int? myVariable)? - Stack Overflow](https://stackoverflow.com/questions/2690866/what-is-the-purpose-of-a-question-mark-after-a-type-for-example-int-myvariabl)
- [#CSharp(C#)：var與dynamic - 新罪楓翼☆灆洢騎士 - 點部落](https://dotblogs.com.tw/knightzone/2013/09/08/116725)
- [c# - What's with the dollar sign ($"string") - Stack Overflow](https://stackoverflow.com/questions/32878549/whats-with-the-dollar-sign-string)
- [C# Interpolated Strings 字串插值-黑暗執行緒](https://blog.darkthread.net/blog/c-interpolated-string/)