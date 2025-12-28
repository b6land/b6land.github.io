---
layout: post
title: 在 C# 中使用 Task
date: 2020-01-25 12:00:00 +0800
categories:  [C#]
---

本文介紹 C# 的 Task，包含範例程式碼。

### 目錄

- [為什麼使用 Task](#為什麼使用-task)
- [如何建立 Task 並等待至完成](#如何建立-task-並等待至完成)
- [取消 (中斷) Task 的執行](#取消-中斷-task-的執行)
- [簡單傳入 Task 參數的方法](#簡單傳入-task-參數的方法)
- [避免執行到一半被關閉 (適用於 ASP.Net Core)](#避免執行到一半被關閉-適用於-aspnetcore)
- [其它參考資料](#其它參考資料)

### 為什麼使用 Task

- 提供比 Thread 更多的控制能力，如等候 (Wait)、取消 (Cancel)、接續 (ContinueWith) 以及錯誤處理 (exception handling)。不必再透過執行緒手動控制。
- Task 會被排入 ThreadPool，並在幕後提供負載平衡，以最大化輸出結果。

### 如何建立 Task 並等待至完成

和 Thread 類似，在建構式傳入要執行的方法，使用 `Start()` 開始執行工作，並使用 `Wait()` 等待工作完成。

```cs

int n;

private void AddN(){
    for(int i = 0;i < 50;i++){
        n = n + 1;
    }
}

public void Run(){
    Task task = new Task(AddN);
    task.Start();
    task.Wait();
    Console.WriteLine(n.ToString());
}
```

另可參考 [明確建立和執行工作 - Microsoft Docs](https://learn.microsoft.com/zh-tw/dotnet/standard/parallel-programming/task-based-asynchronous-programming#creating-and-running-tasks-explicitly) 裡的範例。這段程式碼透過 Lambda 運算式執行，呼叫要執行的方法。

### 取消 (中斷) Task 的執行

1. 建立一個 `AddNWithCancel()` 的方法，傳入 `CancellationToken` 。方法內檢查 Token 的 `IsCancellationRequested` ，即取消被呼叫時，使用 Token 的 `ThrowIfCancellationRequested()` 拋出例外。
2. 在執行這個方法前，建立 `CancellationTokenSource` 和 `CancellationToken` 物件。使用 `Task.Run()` 執行工作，並使用 await 等待工作完成。由於我們在 `Task.Run()` 執行後，立刻呼叫 Token 的取消 (`Cancel()`) 方法，因此在底下的 try...catch... 區塊內，會接收到例外，並顯示「工作已取消」訊息。

```cs
    private void AddNWithCancel(CancellationToken cancelToken){
        int a = 0;
        for(int i = 0;i < 1000;i++){
            a = a + 1;
            
            if(cancelToken.IsCancellationRequested){
                cancelToken.ThrowIfCancellationRequested();
            }
        }
    }
    
    public async void Run(){

        var tokenSource = new CancellationTokenSource();
        CancellationToken cancelToken = tokenSource.Token;

        var task2 = Task.Run(()=> AddNWithCancel(cancelToken), cancelToken);
        tokenSource.Cancel();
        try{
            await task2;
        }
        catch(OperationCanceledException e){
            Console.WriteLine("工作已取消");
        }
    }
```

[範例程式碼](https://github.com/b6land/ithelp_2022_example/blob/8315e2fc5f9492bcd562ff7874dc8cb7278bcce9/Day15_Task.cs)

也請參考 [工作取消 - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/standard/parallel-programming/task-cancellation) 裡的程式碼。與上方例子類似，拋出 `OperationCanceledException` 例外，然後結束 Task 程式的執行。

[C# 學習筆記：多執行緒 (5) - 工作の取消和逾時 - Huan-Lin 學習筆記](https://www.huanlintalk.com/2013/06/csharp-notes-multithreading-5.html) 的範例程式碼，則只要判斷 `CancellationToken` 內的 `IsCancellationRequested` 是否為 `True`，再適當的撰寫程式碼，結束 Task 即可，不透過拋出例外的方式取消 Task 的執行。

### 簡單傳入 Task 參數的方法

想在 Task 內用 Lambda 呼叫方法，而且傳入參數的話，可以用：

```csharp
bool result = await Task.Run(() => ItemFound(param1, param2, param3) );
```

裡面的 `param1`, `param2`, `param3` 即是要傳入的參數。

需要回傳資料的話，可改寫成下列形式：

<br>

```csharp
Task<bool> t = new Task<bool>(() => ItemFound(param1, param2, param3) );
```

<br>

請參考 [c# - Task.Run with Input Parameters and Output - Stack Overflow](https://stackoverflow.com/questions/49587439/task-run-with-input-parameters-and-output) 。

### 避免執行到一半被關閉 (適用於 ASP.Net Core)

如果是透過 API 呼叫 Task 去執行要跑很久的作業，很有可能會遇到執行到一半莫名結束的情況。這是因為 網站或 API 會有閒置的設定，當一段時間沒有請求時，就會閒置狀態。英文中有專有名詞 Graceful Shutdown，常用於後端程式語言。

根據不同情境，有數種方式可以避免執行中程式被關閉。 

對我來說，能有效解決的方法有：


1. 從 IIS 的應用程式集區，點選進階設定，調整「閒置逾時」到需要的時間。![閒置逾時設定](/assets/imgs/2025-12-28/IIS_App_Pool.png)
2. 從程式中設定 Shutdown 時間，例如：

```csharp
builder.WebHost.UseShutdownTimeout(TimeSpan.FromSeconds(5400));
```

可以參考以下兩個討論：

- [c# - Graceful shutdown and currently processing requests - Stack Overflow](https://stackoverflow.com/questions/51482858/graceful-shutdown-and-currently-processing-requests)
- [c# - How to apply HostOptions.ShutdownTimeout when configuring .NET Core Generic Host? - Stack Overflow](https://stackoverflow.com/questions/52915015/how-to-apply-hostoptions-shutdowntimeout-when-configuring-net-core-generic-host)

個人是兩種方法都同時設定，才有發生效果。

另外，使用 HostedService 或一般 Class 的狀況，也可以參考：[ASP.NET Core 避免工作執行到一半強制被關閉 - Yowko's Notes](https://blog.yowko.com/aspdotnetcore-graceful-shutdown/ "https://blog.yowko.com/aspdotnetcore-graceful-shutdown/")

### 其它參考資料

- [C# 學習筆記：多執行緒 (6) - TPL - Huan-Lin 學習筆記](https://www.huanlintalk.com/2013/06/csharp-notes-multithreading-6-tpl.html)