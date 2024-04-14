---
layout: post
title: ASP.NET Core 的設定檔案、Secret Manager
date: 2024-04-06 12:00:00 +0800
categories:  [ASP.NET]
--- 

本文要介紹 ASP.NET Core 專案的設定檔案，其與舊有的 ASP.NET 專案有很大的不同。最大的差異是改用了 JSON 格式，比舊有的 XML 格式體積更小、更容易閱讀。

### 設定檔案

可以透過 `appsettings.json` 檔案設定專案需要的設定值。以下是一個 `appsettings.json`  範例檔案：

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AppSettings": {
    "AppTitle": "MyApp",
    "Environment": "Development",
    "Version": "1.0"
  }
}
```

可以使用以下的程式碼取得 `appsettings.json`  內的設定值：

```csharp
public class Repository: IRepository
{
    // 需要加入 using Microsoft.Extensions.Configuration;
    private readonly IConfiguration Configuration;

    // 注入 IConfiguration 類別
    public Repository(IConfiguration configuration)
    {
        Configuration = configuration;
    }

    public void DoSomething()
    {
        var title = Configuration["AppSettings:AppTitle"];
        var defaultLogLevel = Configuration["Logging:LogLevel:Default"];
    }
}
```

也可以使用 `builder.Configuration.AddJsonFile()` 方法加入自訂的設定檔。

### 用 Secret Manager 儲存秘密設定

為了避免將機密簽入至版本控制系統，不能將帳號、密碼或是其它重要的資訊存放在 `appsettings.json`  內。可以建立 `secrets.json`  解決此問題。

在專案目錄下，使用命令列執行初始化和設定的指令，就會建立預設的 `secrets.json` 檔案，其效用和 `appsettings.json` 相同。需注意此檔案仍是使用明碼儲存資料，存放的密碼仍建議要先加密過。

```powershell
dotnet user-secrets init
dotnet user-secrets set "userName" "lazy"
```

產生時會告知一組隨機的 user\_secrets\_id，預設會存放在 `%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json` 內，可自行編輯。

### 存取順序

1. secrets.json
2. appsettings.{Environment}.json，例如 appsettings.Production.json
3. appsettings.json  

有重複的設定值時，優先使用數字較低的設定檔內的設定值。

### 參考資料

- [ASP.NET Core 的設定 - Microsoft Learn](https://learn.microsoft.com/zh-tw/aspnet/core/fundamentals/configuration/?view=aspnetcore-8.0)
- 延伸閱讀之 ASP.NET 的設定檔讀取方式：[如何透過 C# 類別庫讀取 Web.config 或 App.config 的參數設定值 - The Will Will Web](https://blog.miniasp.com/post/2015/11/23/How-Class-library-read-config-from-webconfig-or-appconfig-file)
- [Safe storage of app secrets in development in ASP.NET Core - Microsoft Learn](https://learn.microsoft.com/en-us/aspnet/core/security/app-secrets?view=aspnetcore-7.0&tabs=windows)
- [如何使用 Secret Manager 保護 .NET Core 專案的機密設定](https://blog.poychang.net/microsoft-user-secret-manager-with-dotnet-core/)
- [使用 dotnet user-secrets 儲存專案專屬秘密設定(API Key、密碼)-黑暗執行緒](https://blog.darkthread.net/blog/dotnet-secrets-manager/)