---
layout: post
title: 在 C# 中使用 Task
date: 2020-01-25 12:00:00 +0800
categories: C#
tags: [C#]
---

本文介紹 C# 的 Task，包含範例程式碼。

### 為什麼使用 Task

- 提供比 Thread 更多的控制能力，如等候 (Wait)、取消 (Cancel)、接續 (ContinueWith) 以及具強韌性的錯誤處理 (exception handling)。
- Task 會被排入 ThreadPool，並在幕後提供負載平衡，以最大化輸出結果。

### 如何建立 Task 並等待至完成

{% highlight csharp %}
      // Create a task and supply a user delegate by using a lambda expression. 
      Task taskA = new Task( () => Console.WriteLine("Hello from taskA."));
      // Start the task.
      taskA.Start();

      // Output a message from the calling thread.
      Console.WriteLine("Hello from thread '{0}'.", 
                        Thread.CurrentThread.Name);
      taskA.Wait();

      // The example displays output like the following:
      //       Hello from thread 'Main'.
      //       Hello from taskA.
{% endhighlight %}

以上為 Microsoft Docs 裡的程式碼。
這段程式碼透過 Lambda 運算式執行，以具名的方式呼叫要執行的方法。

### 取消 (中斷) Task 的執行

{% highlight csharp %}
    static async Task Main()
    {
        var tokenSource2 = new CancellationTokenSource();
        CancellationToken ct = tokenSource2.Token;

        var task = Task.Run(() =>
        {
            // Were we already canceled?
            ct.ThrowIfCancellationRequested();

            bool moreToDo = true;
            while (moreToDo)
            {
                // Poll on this property if you have to do
                // other cleanup before throwing.
                if (ct.IsCancellationRequested)
                {
                    // Clean up here, then...
                    ct.ThrowIfCancellationRequested();
                }

            }
        }, tokenSource2.Token); // Pass same token to Task.Run.

        tokenSource2.Cancel();

        // Just continue on this thread, or await with try-catch:
        try
        {
            await task;
        }
        catch (OperationCanceledException e)
        {
            Console.WriteLine($"{nameof(OperationCanceledException)} thrown with message: {e.Message}");
        }
        finally
        {
            tokenSource2.Dispose();
        }

        Console.ReadKey();
    }
{% endhighlight %}

以上為拋出 `OperationCanceledException` ，然後結束 Task 的程式碼，來自 Microsoft Docs。

{% highlight csharp %}
static void MyTask(CancellationToken token)
    {
        for (int i = 0; i < 1000; i++)
        {
            if (token.IsCancellationRequested)
            {
                Console.WriteLine("使用者要求取消工作!");
                return;
            }
            Console.WriteLine(i);
            Thread.Sleep(200);
        }
        Console.WriteLine("MyTask 工作執行完畢。");
    }
{% endhighlight %}

上面的第二組程式碼，則只要判斷 `CancellationToken` 內的 `IsCancellationRequested` 是否為 `True`，再適當的撰寫程式碼，結束 Task 即可，不透過拋出例外的方式取消 Task 的執行。

### 參考資料
- [工作型非同步程式設計 - .NET - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/standard/parallel-programming/task-based-asynchronous-programming)
- [工作取消 - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/standard/parallel-programming/task-cancellation)
- [C# 學習筆記：多執行緒 (5) - 工作の取消和逾時 - Huan-Lin 學習筆記](https://www.huanlintalk.com/2013/06/csharp-notes-multithreading-5.html)
- [C# 學習筆記：多執行緒 (6) - TPL - Huan-Lin 學習筆記](https://www.huanlintalk.com/2013/06/csharp-notes-multithreading-6-tpl.html)