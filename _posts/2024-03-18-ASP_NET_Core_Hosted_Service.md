---
layout: post
title: ASP.NET Core 使用 Hosted Service 建立背景服務
date: 2024-03-18 12:00:00 +0800
categories:  [ASP.NET]
---

ASP.NET Core 有內建 Hosted Service (託管服務)，可用來執行背景的排程工作，不像舊的 ASP.NET 需要自行設定 Windows 排程，或是使用 Hangfire 執行排程工作。

### 介紹

為了建立 Hosted Service，需要實作 `IHostedService` 介面。這個介面需要新增的方法包含 `Task StartAsync(CancellationToken)` 和 `Task StopAsync(CancellationToken)`。

`StartAsync` 方法會在程式啟動時被觸發，可以在內部設定 Timer 或執行緒，以執行背景作業。

`StopAsync` 方法會在程式結束時被觸發，通常用於 Unmanaged 資源的回收 (如果是意外關閉，就不會觸發此方法)。

### 範例程式

以下的程式是一個 Asp.Net Core MVC 專案，會在 `Program.cs` 註冊一個自行建立的 `HostedService` 類別。這個類別實作 `IHostedService` 介面，包含 `StartAsync` 和 `StopAsync` 方法。

當程式啟動時，`StartAsync` 方法會啟動一個 Timer，每 5 秒鐘在終端機顯示目前時間；當程式結束時，則會透過 `StopAsync` 方法設定 Timer，使其永遠不被觸發。

`HostedService.cs`

```cs
using System;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Extensions.Hosting;

namespace Practice
{
    public class HostedService : IHostedService, IDisposable
    {
        private Timer _timer;

        public Task StartAsync(CancellationToken cancellationToken)
        {
            _timer = new Timer(DoWork, null, TimeSpan.Zero, TimeSpan.FromSeconds(5));
            return Task.CompletedTask;
        }

        private void DoWork(object state)
        {
            var now = DateTime.Now;
            Console.WriteLine($"{now:yyyy-MM-dd HH:mm:ss.fff}");
        }

        public Task StopAsync(CancellationToken cancellationToken)
        {
            _timer?.Change(Timeout.Infinite, 0);
            return Task.CompletedTask;
        }

        public void Dispose()
        {
            _timer?.Dispose();
        }
    }
}
```

`Program.cs`

```cs
using Practice;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddControllersWithViews();
builder.Services.AddHostedService<HostedService>();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Home/Error");
    // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();

app.UseRouting();

app.UseAuthorization();

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

app.Run();
```

### 參考資料

- [在 ASP.NET Core 網站執行定時排程-黑暗執行緒](https://blog.darkthread.net/blog/aspnet-core-background-task/)
- [在 ASP.NET Core 中使用託管服務的背景工作 - Microsoft Learn](https://learn.microsoft.com/zh-tw/aspnet/core/fundamentals/host/hosted-services?view=aspnetcore-8.0&tabs=visual-studio)