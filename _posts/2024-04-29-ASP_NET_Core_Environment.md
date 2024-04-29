---
layout: post
title: ASP.NET Core 設定生產環境或測試環境
date: 2024-04-29 21:00:00 +0800
categories:  [ASP.NET]
---

因為程式開發時多少會有一些 Bug，為了讓服務不會常常爆炸，我們不會把開發中的程式直接搬到生產 (正式) 的伺服器上，而是經過測試和部屬等步驟，才將伺服器的程式更新為新版本。本篇介紹 ASP.NET Core 如何設定開發、測試和生產環境，讓開發時的除錯紀錄、行為等不會套用到正式環境。

### 開發、測試與生產環境解釋

- Development, DEV: 開發環境，Bug 最多的時候，產出的程式可能常常會修改。
- Testing, TEST: 測試環境，交給 QA 人員進行測試，發現問題時交給開發人員修正。
- Staging, STG: 試運行環境，上線前檢查部署到生產環境的組態，測試負載能力。
- Production, PROD: 生產環境，正式上線，開放使用。

定義會因為各個開發團隊制定的流程而略有差異。

參考資料：

- [【專案開發】專案開發中dev、test、stage、prod是什麼意思？ ~ 萊恩的隨手筆記 - Ryan's Notebook](https://ryanisagoodguy.blogspot.com/2020/02/devteststageprod.html)
- [釐清Development 、Testing、Staging、Production Environment - by Watson Wang - Medium](https://watson-john.medium.com/釐清development-testing-staging-production-environment-772566f2249e)

### ASP.NET Core 設定環境

方式有幾種：

- 設定 launchSettings.json，在環境變數的屬性裡面加入 `ASPNETCORE_ENVIRONMENT` ，並設定其值為 `Development` , `Staging`  或 `Production` 。 透過 Visual Studio 執行時，即會使用 JSON 檔案內的 Profile，並覆寫目前的環境變數設定。
    - 可以在偵錯按鈕右方下拉箭頭，選擇要使用哪一種 Profile。![選擇下拉箭頭](/assets/imgs/2024-04-29/debug.png)

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:59481",
      "sslPort": 44308
    }
  },
  "profiles": {
    "EnvironmentsSample": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": true,
      "applicationUrl": "https://localhost:7152;http://localhost:5105",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

以上 JSON 檔案為微軟提供的範例 (並稍作修改)，在環境變數中加入了 `"ASPNETCORE_ENVIRONMENT": "Development"` 。 

- 在系統設定 > 進階設定 > 環境變數 ... > 按下「新增...」或「編輯...」設定系統變數。效果會套用到整台電腦。

![環境變數](/assets/imgs/2024-04-29/system_environment.png)  

以下的設定方式，請查閱參考資料：

- 在 IIS 內設定 (對所有 IIS 應用程式生效)。
- 使用 dotnet 命令設定環境 (對該命令視窗執行的應用程式生效)。
- 編輯 web.config (對所屬的應用程式生效)。
- 在 Azure App Service 上設定。
- 在 Linux 或 MacOS 作業系統設定。

參考資料：

- [\[.NETCore\] ASP.NET Core - ENVIRONMENT 使用環境變數 ~ m@rcus 學習筆記](https://marcus116.blogspot.com/2019/04/netcore-aspnet-core-environment.html)
- [在 ASP.NET Core 中使用多個環境 - Microsoft Learn](https://learn.microsoft.com/zh-tw/aspnet/core/fundamentals/environments?view=aspnetcore-8.0)

### ASP.NET Core 判斷環境

可以用 `IsDevelopment()` 、`IsProduction()` 程式碼判斷，以下是範例：

```csharp
var app = builder.Build();
if (app.Environment.IsDevelopment() == false){
    // 不是開發環境時，要做的事情
}
```

參考資料：[How to remove swagger production .net core 2.1 - Stack Overflow](https://stackoverflow.com/questions/55046587/how-to-remove-swagger-production-net-core-2-1)