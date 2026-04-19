---
layout: post
title: HttpClient 的使用方式與重要觀念
date: 2026-04-19 15:30:00 +0800
categories: [C#]
---  

HttpClient 推出於 .NET 4.5，是 .NET 可用來代替 WebClient，取得網路資源的方法。以下簡介使用方式和觀念。

### 使用方式

```csharp
// HttpClient 是用作程式內的單一執行個體 (Instance)，而不是每次使用時建立新的執行個體。可參考微軟官方的說明或內文。
static readonly HttpClient client = new HttpClient();

static async Task Main()
{
    // 在 try/catch 區塊內呼叫非同步方法，以處理例外。
    try
    {
        using HttpResponseMessage response = await client.GetAsync("http://www.contoso.com/");
        response.EnsureSuccessStatusCode(); // 若沒有成功取得資料，則拋出例外
        string responseBody = await response.Content.ReadAsStringAsync();
        // 上面三行可以用以下的 helper 方法取代
        // string responseBody = await client.GetStringAsync(uri);

        Console.WriteLine(responseBody);
    }
    catch (HttpRequestException e)
    {
        Console.WriteLine("\n發生例外!");
        Console.WriteLine("訊息 :{0} ", e.Message);
    }
}

```

改寫自：[HttpClient 類別 (System.Net.Http) - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/api/system.net.http.httpclient?view=net-8.0)

### 重要觀念

雖然 HttpClient  有實作 `IDisposable` ，仍應宣告為 static 並重複利用，而非在每次連線時用 using 建立新的 `HttpClient` 連線，避免連線數量過大時，Dispose 後連線仍等待 240 秒後才被釋放，導致占用過多記憶體和 Socket Port 號碼。

HttpClient 的多數方法是 Thread-Safe 的，例如 `GetStringAsync` 、`PostAsync` ，因此可在多執行緒下執行。副作用則是發生在 DNS 重設後，會有無法取得新位置的問題。

在 .NET Framework 4.6 或之前的版本，可以用 ServicePoint 指定 `ConnetionLeaseTimeout` 和 `DnsRefreshTimeout` ，這兩個選項分別是：連線在一段時間未使用後會關閉；在一段時間後重新解析 DNS。範例如下：

```csharp
//設定 1 分鐘沒有活動即關閉連線
ServicePointManager.FindServicePoint(baseUri)
    .ConnectionLeaseTimeout = (int)TimeSpan.FromMinutes(1).TotalMilliseconds;
//設定 1 分鐘更新 DNS
ServicePointManager.DnsRefreshTimeout = (int)TimeSpan.FromMinutes(1).TotalMilliseconds;

```

若是使用 .Net Core 或 .NET Framework 4.6 等更新的版本，可改用 HttpClientFactory ，以在重複利用 HttpClient 的同時，避開 DNS 重設造成的問題。

但是 `HttpRequestMessage`  和 `HttpResponseMessage`  還是要在沒使用的時候，就把它們釋放掉。

### 參考資料

- [HttpClient，該 using 還是 static?-黑暗執行緒](https://blog.darkthread.net/blog/httpclient-sigleton/)
- [HttpClient 無法反應 DNS 異動的解決方式 - Yowko's Notes](https://blog.yowko.com/httpclient-not-respect-dns-change/)
- [在 .NET Core 與 .NET Framework 上使用 HttpClientFactory - Yowko's Notes](https://blog.yowko.com/httpclient/)
- [c# - When or if to Dispose HttpResponseMessage when calling ReadAsStreamAsync? - Stack Overflow](https://stackoverflow.com/questions/27715327/when-or-if-to-dispose-httpresponsemessage-when-calling-readasstreamasync)