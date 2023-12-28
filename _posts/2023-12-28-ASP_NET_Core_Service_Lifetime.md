---
layout: post
title: ASP.NET Core 的服務生命週期、註冊與使用
date: 2023-12-28 12:00:00 +0800
categories:  [ASP.NET]
---

ASP.NET Core 與 ASP.NET 間一個很大的差異，是大量使用依賴注入建立服務，對於服務類別的替換便更加容易，減少程式間耦合性。本篇文章介紹相關的服務特性。

## 服務生命週期

ASP.NET Core 的服務，可以用 DI (依賴注入, Dependency Injection) 註冊，改善了程式的可測試性。其包含三種服務生命週期：

|     |     |     |
| --- | --- | --- |
| 服務生命週期 | 特性  | 使用時機 |
| Singleton | 從程式開始時，只會建立一個實例 (Instance) ，該實例會在所有需要它的元件間共享，直到程式結束為止。 | 程式執行期間須維持狀態的服務 |
| Scoped | 在處理請求 (Request) 時建立 (通常指「接到瀏覽器請求至回傳結果前的期間」) 實例。 | 對個別請求有不同狀態的服務 |
| Transient | 每次要求元件時都建立一個新實例，絕不共享。 | 輕量、無狀態 (Stateless) 的服務 |

### 參考資料

- [筆記 - 不可不知的 ASP.NET Core 依賴注入-黑暗執行緒](https://blog.darkthread.net/blog/aspnet-core-di-notes/)
- [[.Net Core] 服務存留期 (Service Lifetime)：叡揚部落格](https://www.gss.com.tw/blog/net-core-service-lifetime)

## 使用服務的方式

### 註冊

因為 ASP.NET Core 採用依賴注入的模式，現在使用服務前，需要先註冊服務。

要註冊服務，可以使用以下的簡易做法，適用於實作的型別 = 服務型別的情形：

```cs
services.AddSingleton<GlobalService>();
services.AddScoped<MyService>();
services.AddTransient<DataService>();
```

如果實作型別 != 服務型別，例如服務型別為 IDataService 介面，實作型別為 DataService 類別，可以用以下方式註冊服務：

```cs
services.AddTransient<IDataService, DataService>();
services.AddSingleton<IStubRepository, StubRepository>();
```

### 注入

註冊服務後，要使用該服務時，可以透過以下幾種方式：建構子、呼叫 (Invoke) 參數、自行呼叫 RequestServices。

下方是要注入服務的類別。

```cs
public class App
{
    public App(){}

    public Task Invoke(HttpContext ctx){}
}
```

以下是三種注入的方式。

```cs
public class App
{
    private readonly IDataService _svc;

    public App(IDataService svc){ // 從建構子注入
        _svc = svc;
    }

    public Task Invoke(HttpContext ctx, IDataService svc2){ // 呼叫參數注入
        IDataService svc3 = ctx.RequestServices.GetService<IDataService>(); // 自行呼叫 GetService()
    }
}
```

`HttpContext` 內有 `RequestServices`，其為一個繼承 `IServiceProvider` 的 abstract class，因此可以透過 `ServiceProvider` 取得服務。

### 參考資料

- [How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
- [ASP.NET Core Dependency Injection Deep Dive - Joonas W's blog](https://joonasw.net/view/aspnet-core-di-deep-dive)
