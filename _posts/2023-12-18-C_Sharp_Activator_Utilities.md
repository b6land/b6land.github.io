---
layout: post
title: C# 服務注入與 ActivatorUtilities 類別
date: 2023-12-18 12:00:00 +0800
categories: [C#]
---

在 .NET Core 下，重度使用 Dependency Injection 呼叫服務。.NET 的 DI 容器需要管理服務的註冊、生命週期等。Activator Utilities 則是微軟在 .NET Core 提供的工具類別，可以方便的在多種不同的情形下，初始化類別的實例。

### 解決依賴

如何解決依賴：

``` cs
// 從 ServiceProvider 解析
serviceProvider.GetService<MyController>(); // FAIL

// 手動建立實例
new MyController(new TransientService()); // SUCCESS!

// 建立實例，透過 DI (依賴注入) 解析參數
new MyController(serviceProvider.GetService<TransientService>()); // SUCCESS!
```

問題：如果不知道要建立的類別，只知道建構子；或是類別和建構子都不知道時，要怎麼建立實例？


解決方式：

1. 使用 `Activator.CreateInstance()` 或使用 `Invoke()` 建立實例。
2. 或使用 `ActivatorUtilities.CreateInstance()` 建立實例，優點是更加方便。


### ActivatorUtilities 類別

`ActivatorUtilities.CreateInstance()` 的使用範例：

``` cs
class MyController {
    public MyController() {
        Console.WriteLine("Feeling empty!");
    }
    [ActivatorUtilitiesConstructor] // 指定 ActivatorUtilities 要使用的建構子，不指定時，會自行挑選合適的建構子使用
    public MyController(TransientService service) {
        Console.WriteLine("I've been served!");
    }
}

var subject = ActivatorUtilities.CreateInstance<MyController>(ServiceProvider); // 在指定物件類別和建構子的狀況下，建立實例 (此範例未傳入服務)，使用 IServiceProvider 建立的 Provider 解析
```

可以接受指定多個參數，或是在不指定物件類別和架構子下，建構實例：

``` cs
var controller = ActivatorUtilities.CreateInstance(ServiceProvider, typeof(Subject)); // 在不知道要建構的類型為何的時候使用
```

### 參考資料

- 本篇程式碼來源，以及更詳細的解說：[Activator utilities: activate anything! - On The Drift](https://onthedrift.com/posts/activator-utilities/)
- 使用多參數建構式：[ASP.NET Core DI 之多建構式問題-黑暗執行緒](https://blog.darkthread.net/blog/aspnet-core-di-multi-constructors/)