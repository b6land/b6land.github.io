---
layout: post
title: C# 定時器 (Timer)
date: 2018-08-26 12:00:00 +0800
categories: C#
---

本篇文章介紹 C# 中兩種不同的 Timer 使用情境、方式。

### Timer 的類別
`System.Windows.Form.Timer`：

- 要定期更新 UI 元件時，建立 `System.Windows.Form.Timer` 物件，將 EventHandler 指向要更新元件的程式碼即可。
- 和 UI Thread 相同的單一 Thread (執行緒)。
> 建立多個 UI Thread 會出錯。
-可參考 [How to: Run Procedures at Set Intervals with the Windows Forms Timer Component](https://docs.microsoft.com/zh-tw/dotnet/framework/winforms/controls/run-procedures-at-set-intervals-with-wf-timer-component)

`System.Timer.Timer` : 

- 使用 `System.Timer.Timer` 更新 UI 元件會出錯，因為其非 UI 更新執行緒。
- 建立 ThreadPool 管理的 Thread，多執行緒。
- 直接參考  [Timer Class (System.Timers)](https://msdn.microsoft.com/en-us/library/system.timers.timer(v=vs.110).aspx) 最下方的 Examples 即可。
- 使用 SynchronizingObject 屬性解決跨執行緒更新 UI 的問題
- 可參考 [[C#.NET][Thread] 執行緒定時器](https://dotblogs.com.tw/yc421206/2011/01/30/21141)