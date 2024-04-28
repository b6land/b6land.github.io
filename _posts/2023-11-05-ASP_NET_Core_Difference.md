---
layout: post
title: ASP.NET Core 與舊版 ASP.NET 的差異
date: 2023-11-05 12:00:00 +0800
categories: [ASP.NET]
---

本文章摘要 ASP.NET Core 和舊版 ASP.NET 的差異。

### 特性

- 跨平台，支援 Linux/Mac 等平台。
- Open Source。
- 效能較佳: 相較 ASP.NET MVC 綁定 IIS 更加輕巧，所以能處理更多連線要求 (Request)。

[ASP.NET Core 值得學嗎？-黑暗執行緒](https://blog.darkthread.net/blog/is-aspnetcore-worth-learning/)

### 技術

- 依賴注入: 大量地使用依賴注入 (Dependency Injection, DI) 概念，減少程式間的耦合。
- 路由: .NET Core 中，可以使用的路由包含 `app.UseRouting()` 和傳統的 ASP.NET WebAPI 2 的路由設定方式。
- 設定檔: .NET Core 中，不再如 WebAPI 2 使用 App_Start 資料夾或 Global.asax 檔案設定組態，而是在 startup.cs 中設定組態。web.config 會在發布後才產生。

### 其它技術細節

- [ASP.NET與ASP.NET Core差異 - 攻城獅的學習筆記 - 點部落](https://dotblogs.com.tw/cotton/2021/08/02/113844)
- [App startup in ASP.NET Core - Microsoft Learn](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/startup?view=aspnetcore-5.0)
- [Routing differences between ASP.NET MVC and ASP.NET Core - .NET - Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/architecture/porting-existing-aspnet-apps/routing-differences)

### 功能

- Filter: 在送入請求 (Request) 後，實際執行請求的動作 (Action) 前，可以透過 Filter 檢查請求內容。
- Background Service: 可以透過實作託管服務介面 (IHostedService) 快速建立背景服務的程式。請參考拙作 [ASP.NET Core 使用 Hosted Service 建立背景服務](/ASP_NET_Core_Hosted_Service)。