---
layout: post
title: C# 三種定時器 (Timer) 的使用情境
date: 2018-08-26 12:00:00 +0800
categories:  [C#]
---

C# 提供了數種 Timer，分別適用於不同的使用情境。本文將介紹 System.Windows.Forms.Timer、System.Timers.Timer 和 System.Threading.Timer 的簡介、使用方法，最後並整理它們的差異。

## System.Windows.Form.Timer

通常用於 Windows Forms 應用程式中，用來定期更新 UI 元件，例如顯示計時器或即時資料。

1. 建立 Timer 物件。
2. 設定 Timer 的間隔時間（以毫秒為單位）。
3. 綁定事件處理函數（EventHandler），指向要更新元件的程式碼。
4. 啟動 Timer。

### 範例

```csharp
using System;
using System.Windows.Forms;

public class TimerExample
{
    private System.Windows.Forms.Timer myTimer;
    public Label myLabel {get; set;} // 給外部 Form 存取

    public TimerExample()
    {
        myLabel = new Label();
        
        myTimer = new System.Windows.Forms.Timer();
        myTimer.Interval = 1000; // 設置間隔為 1 秒
        myTimer.Tick += new EventHandler(UpdateLabel);
        myTimer.Start();
    }

    private void UpdateLabel(object sender, EventArgs e)
    {
        myLabel.Text = DateTime.Now.ToString("HH:mm:ss");
    }
}
```

### 註記

- 只能建立一個 Timer，否則會發生例外。
- 可參考 [How to: Run Procedures at Set Intervals with the Windows Forms Timer Component](https://docs.microsoft.com/zh-tw/dotnet/framework/winforms/controls/run-procedures-at-set-intervals-with-wf-timer-component)

## System.Timer.Timer 

可以同時執行多個 Timer，適用需要多個定時發送事件的狀況。當應用程式需要高精度的計時功能或在多執行緒環境中運行時，可以考慮使用 `System.Timers.Timer`，會比 `System.Windows.Form.Timer` 更加適合。

1. 建立 Timer 物件。
2. 設定 Timer 的間隔時間。
3. 綁定事件處理函數（ElapsedEventHandler）。
4. 啟動 Timer。

### 範例

```csharp
using System;
using System.Timers;

public class TimerExample
{
    private static System.Timers.Timer myTimer;

    public static void Main()
    {
        myTimer = new System.Timers.Timer(1000); // 設置間隔為 1 秒
        myTimer.Elapsed += OnTimedEvent;
        myTimer.AutoReset = true;
        myTimer.Enabled = true;

        Console.WriteLine("Press [Enter] to exit the program.");
        Console.ReadLine();
    }

    private static void OnTimedEvent(Object source, ElapsedEventArgs e)
    {
        Console.WriteLine("The Elapsed event was raised at {0:HH:mm:ss.fff}", e.SignalTime);
    }
}
```

### 註記

- 可使用 `SynchronizingObject` 屬性解決跨執行緒更新 UI 的問題。
- 可參考  [Timer Class (System.Timers)](https://msdn.microsoft.com/en-us/library/system.timers.timer(v=vs.110).aspx) 最下方的 Examples 與 [[C#.NET][Thread] 執行緒定時器](https://dotblogs.com.tw/yc421206/2011/01/30/21141)

### System.Threading.Timer

提供了一種基於執行緒集區 (Threading Pool) 的定時器機制，適合在多執行緒環境中執行計時任務。當需要在背景執行緒中執行計時任務，且不需與 UI 交互時，可以使用 `System.Threading.Timer`。

1. 建立 Timer 物件，並設定 Callback 函數和狀態。
2. 設定 Timer 的間隔時間。

### 範例

```csharp
using System;
using System.Threading;

public class TimerExample
{
    private static System.Threading.Timer myTimer;

    public static void Main()
    {
        myTimer = new System.Threading.Timer(Callback, null, 0, 1000); // 設置 Callback 函數，初始延遲和間隔為 1 秒

        Console.WriteLine("Press [Enter] to exit the program.");
        Console.ReadLine();
    }

    private static void Callback(Object state)
    {
        Console.WriteLine("Timer callback executed at {0:HH:mm:ss.fff}", DateTime.Now);
    }
}
```

## 總結

用下表列出 Timer 的比較與選擇：

| Timer 類型 | 優點 | 缺點 | 使用情境 |
| --- | --- | --- | --- |
| System.Windows.Forms.Timer | 簡單易用，專為 UI 更新設計  | 只能在 UI 執行緒中使用，精度較低 | 需要定期更新 Windows Forms UI 元件 |
| System.Timers.Timer | 高精度，可在多執行緒中運行，適合服務應用程式 | 使用稍微複雜，需要處理跨執行緒問題 | 需要高精度計時或多 Timer 的情況 |
| System.Threading.Timer | 基於執行緒集區，使用上較為簡單 | 沒有內建的 UI 更新機制 | 背景執行緒的計時任務，不需要與 UI 互動 |

作者學疏才淺，請參考 [受控執行緒集區 - .NET - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/standard/threading/the-managed-thread-pool) 和 [Timer 類別 (System.Threading) - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/api/system.threading.timer?view=net-8.0) 瞭解更詳細的知識。
