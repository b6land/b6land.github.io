---
layout: post
title: 在 C# 中使用 Task
date: 2020-01-25 12:00:00 +0800
categories:  [C#]
---

本文介紹 C# 的 Task，包含範例程式碼。

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

### 其它參考資料

- [C# 學習筆記：多執行緒 (6) - TPL - Huan-Lin 學習筆記](https://www.huanlintalk.com/2013/06/csharp-notes-multithreading-6-tpl.html)