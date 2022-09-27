---
layout: post
title: C# 定時器 (Timer)
date: 2018-08-26 12:00:00 +0800
categories:  [C#]
---

本篇文章介紹 C# 中兩種不同的 Timer 使用情境、方式。

### Timer 的類別
`System.Windows.Form.Timer`：

- 要定期更新 UI 元件時，建立 `System.Windows.Form.Timer` 物件，將 EventHandler 指向要更新元件的程式碼即可。
- 只能建立一個 Timer，否則會發生例外。
- 可參考 [How to: Run Procedures at Set Intervals with the Windows Forms Timer Component](https://docs.microsoft.com/zh-tw/dotnet/framework/winforms/controls/run-procedures-at-set-intervals-with-wf-timer-component)

`System.Timer.Timer` : 

- 可以同時執行多個 Timer，適合需要多定時發送事件的狀況。
- 能提供比 `System.Windows.Form.Timer` 更微小 (精確) 的間隔時間。
- 直接參考  [Timer Class (System.Timers)](https://msdn.microsoft.com/en-us/library/system.timers.timer(v=vs.110).aspx) 最下方的 Examples 即可。
- 可使用 SynchronizingObject 屬性解決跨執行緒更新 UI 的問題。
- 可參考 [[C#.NET][Thread] 執行緒定時器](https://dotblogs.com.tw/yc421206/2011/01/30/21141)