---
layout: post
title: C# async 與 await 的用法
date: 2023-10-09 12:00:00 +0800
categories: [C#]
---

本文介紹 C# 中非同步程式的寫法─使用 `async` 和 `await` 關鍵字。

### `async` 和 `await`

使用 `async` 和 `await` 關鍵字，可以將同步程式改為非同步程式。

程式將使用 `Task` 執行宣告為 `async` 的非同步方法，並使用 `await` 等待非同步程式執行完成；或是使用 `Task.Result` 阻擋程式往下執行，直到非同步方法完成。

以下是範例程式與解說。

```cs
using System.Net;

/// <summary>
/// 非同步程式測試
/// </summary>
public class AsyncPractice{
        /// <summary>
    /// 非同步執行
    /// </summary>
    public async void Run(){
        
        string url = "https://b6land.github.io/";
        var downloader = await DownloadPage(url); // 以 await 呼叫，會先跳出 Run() 執行其它方法 (若有的話)，直到 DownloadPage(url) 執行完成，才繼續往下執行

        string content = downloader;
        Console.WriteLine(content.Substring(0, 100));

        string urlMsn = "https://msn.com/";
        var downloaderMsn = DownloadPage(urlMsn); // 沒有 await 時，程式會往下執行

        string urlYahoo = "https://tw.yahoo.com/";
        var downloaderYahoo = DownloadPage(urlYahoo); 
        
        string contentYahoo = downloaderYahoo.Result; // 阻擋，程式執行到此處停住，直到 DownloadPage(urlYahoo) 執行完成，才繼續往下執行
        Console.WriteLine(contentYahoo.Substring(0, 100));

        string contentMsn = downloaderMsn.Result; 
        Console.WriteLine(contentMsn.Substring(0, 100));
    }

    /// <summary>
    /// 以非同步方式下載網頁內容
    /// </summary>
    /// <param name="url"> 網址 </param>
    /// <returns> 網頁內容字串 </returns>
    private async Task<string> DownloadPage(string url){
        HttpClient client = new HttpClient();
        string content = await client.GetStringAsync(url);
        Console.WriteLine("Download finished: " + url);
        return content;
    }
}

AsyncPractice asyncPractice = new AsyncPractice();
asyncPractice.Run();
Console.WriteLine("Execute finished.");
```

### 參考資料

- async/await 詳細解說，與很棒的流程圖：[async 與 await - Huan-Lin 學習筆記](https://www.huanlintalk.com/2016/01/async-and-await.html)
- 如何用 HttpClient 取代 WebClient 的取得網頁內容：[c# - How to Replace WebClient with HttpClient? - Stack Overflow](https://stackoverflow.com/questions/33020657/how-to-replace-webclient-with-httpclient)
- 使用 Lambda 撰寫非同步語法，且可傳入多參數、回傳參數：[c# - Task.Run with Input Parameters and Output - Stack Overflow](https://stackoverflow.com/questions/49587439/task-run-with-input-parameters-and-output)

### 補充：await 和 GetAwaiter()

`await`  和 `GetAwaiter()`  本質上是一樣的，同樣可用來等待非同步程式完成工作。

在編譯器內， `await`  也會被編譯成 `GetAwaiter()` ，因此編譯器比較常使用 `GetAwaiter()` 。而在程式碼內，適合用 `async`  / `await`  處理非同步邏輯。  

不過，使用  `GetAwaiter().GetResult()`，有容易導致 Deadlock / Thread Stravation 的風險。詳細資訊可參考：[閒聊 - GetAwaiter 到底能不能用？-黑暗執行緒](https://blog.darkthread.net/blog/getawaiter-or-not/)

### 補充：寫多個 await 好嗎 ?

在同一個 async 方法內，寫多個 await 是好的，因為在等待 await 程式完成的期間，程式可以先跳出 async 方法，回到呼叫的程式那邊執行其它的程式，直到 await 的程式完成。

關於這個議題的討論如下：[c# - Effects of using multiple awaits in the same method? - Stack Overflow](https://stackoverflow.com/questions/36976810/effects-of-using-multiple-awaits-in-the-same-method)、[c# - Multiple Awaits in a single method - Stack Overflow](https://stackoverflow.com/questions/18445279/multiple-awaits-in-a-single-method)。

### 補充：不要 await Task.FromResult

不要撰寫 `await Task.FromResult`  ，因為程式本身會等待 Task 程式完成，不是一個非同步作業。這樣的寫法只會增加多餘的程式碼 ([Task.FromResult 用法與注意事項 - Huan-Lin 學習筆記](https://www.huanlintalk.com/2016/01/await-taskfromresult.html))。

### 補充：在 ASP.NET WebAPI 的 API 端點內，非同步方法避免使用  result 取得結果

在 WebAPI 中，API 端點的方法內部若有 `await`  的程式碼，例如在底層呼叫其它 API，但在 API 端點使用 `Task.Result`  取得資料，編譯器不會發出警告，但會發生 API 端點沒有回傳結果的問題，這與死結有關。
  
ASP.NET 中使用 `SynchronizationContext`  類別的物件來控制非同步方法的控制流，但是若在 `Task.Result`  阻擋物件以後，`await`  的程式碼 (例如從外部資源取得資料) 才結束工作，想取用該物件處理後續作業，就會造成死結。

在新版的 ASP.NET Core 不會有這樣的問題。

請參考以下網頁：

- 詳細說明，包含 WinForm、WebAPI 的死結範例：[.NET 程式鎖死與 SynchronizationContext - Huan-Lin 學習筆記](https://www.huanlintalk.com/2016/01/asyc-deadlock-in-aspbet.html)
- 這篇的問題過程比較好懂：[await與Task.Result/Task.Wait()的Deadlock問題-黑暗執行緒](https://blog.darkthread.net/blog/await-task-block-deadlock/)
- 另一個 API 死結的範例：[c# - 'await' works, but calling task.Result hangs/deadlocks - Stack Overflow](https://stackoverflow.com/questions/17248680/await-works-but-calling-task-result-hangs-deadlocks)
