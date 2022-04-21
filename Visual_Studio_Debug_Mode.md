---
layout: post
title: Visual Studio 偵錯模式技巧
date: 2021-12-04 12:00:00 +0800
categories: VisualStudio
--- 

本篇介紹 Visual Studio 的偵錯技巧。

以前上課時，只會知道可以下中斷點，以偵錯模式執行程式時會停在該處，讓你可以檢查各個變數。但實際上在 Visual Studio 內，還有進階的用法，增加除錯的效率。

### 在中斷點內設定條件式

可以在中斷點按下右鍵，選擇「設定條件...」，並輸入結果為布林值的判斷式，例如「`i==3`」。

設定完成後，偵錯時會達到指定條件才進入中斷點。

### 在 Foreach 下檢查特定的迭代

雖然 Foreach 每次迭代都是一個物件，但是可以使用 `List.IndexOf()` 語法，列出目前迭代項目在原本的列表中的位置，並依此找到發生問題的列表中物件。

### Hot Reload

從 Visual Studio 2019 16.11 和 .Net 6 開始，可以使用 Hot Reload 的功能。這個功能可以讓開發者從 Visual Studio 執行程式時，不用重新編譯，只要按下 Hot Reload 的按鈕以後，就會套用程式碼變更，而不必中斷執行。

這個功能目前實際使用上，遇到部分如「修改參數方法」或大量的程式碼變更時，可能會告知無法套用 Hot Reload，但大部分狀況下仍是很實用的功能。

### 參考資料

[.net - Debugging a foreach loop in C#: what iteration is this? - Stack Overflow](https://stackoverflow.com/questions/3293051/debugging-a-foreach-loop-in-c-what-iteration-is-this)
[Introducing the .NET Hot Reload experience for editing code at runtime - .NET Blog](https://devblogs.microsoft.com/dotnet/introducing-net-hot-reload/)