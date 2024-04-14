---
layout: post
title: ASP.NET Core Repository 適合的生命週期
date: 2024-04-14 12:00:00 +0800
categories:  [ASP.NET]
---

在 ASP.NET Core 內，依賴注入服務時，會需要指定該類別的生命週期。那麼存取外部資源的 Repository Class，適合哪種生命類別。 本文僅為網路和個人看法，沒有標準答案。

### 看法

關於生命週期的描述，請先參考 [\[.Net Core\] 服務存留期 (Service Lifetime)：叡揚部落格](https://www.gss.com.tw/blog/net-core-service-lifetime) 與拙作 [ASP.NET Core 的服務生命週期、註冊與使用 – Lazy Coding](https://b6land.github.io/ASP_NET_Core_Service_Lifetime/)。

使用 Singleton 的理由為：不需要每次有新的請求時，都重新注入、初始化。

使用 Scoped 的理由為：非同步的情形下，Repository 類別可能是不安全的，應該根據各請求的狀態分別產生 Repository 物件使用。

個人因為使用的類別為無狀態，而且會將資料存入快取，因此使用 Singleton 生命週期注入。

### 在 Singleton 類別內建立 Scoped 類別物件的作法

假設今天有 SingletonService 類別，已被註冊為 Singleton。若要在其中建立已經被註冊為 Scoped、實作 IScopedDbContext 介面的類別，可以參考以下程式碼：

```csharp
public class SingletonService : ISingletonService 
{
    private readonly IServiceProvider _serviceProvider;

    public SingletonService (IServiceProvider serviceProvider)
    {
        _serviceProvider = serviceProvider;
    }

    public async Task DoSomething()
    {
        using (var scope = _serviceProvider.CreateScope())
        {
             var context = scope.ServiceProvider.GetRequiredService<IScopedDbContext>();
             // 實作內容
        }
    }
}
```

但這可能是不好的設計 (Singleton 類別內使用的物件應該都要有相同的生命週期)，應盡量避免這樣做。

### 參考資料

- 使用 Singleton: [spring - Should service layer classes be singletons? - Stack Overflow](https://stackoverflow.com/questions/2173006/should-service-layer-classes-be-singletons )
- 使用 Scoped: [c# - Can repository class be scoped as singleton in ASP.NET - Stack Overflow](https://stackoverflow.com/questions/15706483/can-repository-class-be-scoped-as-singleton-in-asp-net )
- 如果有 Singleton 類別的服務要注入 Scoped 週期類別的需求，可行的替代做法: [c# - How to use DbContext in a singleton class? - Stack Overflow](https://stackoverflow.com/questions/72872402/how-to-use-dbcontext-in-a-singleton-class )
- [c# - Using a Scoped service in a Singleton in an Asp.Net Core app - Stack Overflow](https://stackoverflow.com/questions/55708488/using-a-scoped-service-in-a-singleton-in-an-asp-net-core-app )
