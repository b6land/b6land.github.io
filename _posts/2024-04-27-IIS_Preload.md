---
layout: post
title: IIS 閒置後初次呼叫速度很慢的問題
date: 2024-04-27 20:00:00 +0800
categories: [ASP.NET]
---

這篇文章簡介「IIS 閒置後初次呼叫速度很慢」問題的原因，並整理幾種解決方法的文章連結。

### 簡介

閒置超過一定時間 (預設為 20 分鐘) 時，IIS 會釋放應用程式的資源，此時再次呼叫應用程式，就會需要花時間重新載入 (IIS 剛啟動程式時，也要花時間載入應用程式)，導致過好幾秒才會回應。(根據 [Google Adsense 的說明](https://support.google.com/adsense/answer/7450973?hl=zh-Hant)，行動網頁載入時間若超過 3 秒，就可能有 53% 的訪客跑掉！)

這個問題可以藉由設定「預先載入」功能解決，但該功能的安裝、設定方式，會因為作業系統、IIS 版本而有所不同。在新版 IIS 中，這個功能通常被包含在「Application Initialization (應用程式初始化)」內，舊版則是「 Application Warm-Up 」。

### 安裝與設定

如果使用 Windows 2019 Server 的話，可參考以下文章安裝「應用程式初始化」功能：

- [IIS 使用 應用程式初始化(Application Initialization)套件有效提升冷啟動和首頁載入速度](https://www.ruyut.com/2023/03/iis-application-initialization-speed-up-website-loading-time.html)
- [如何設定 ASP.NET Core 網站應用程式持續執行在 IIS 上 - by Kodofish - Medium](https://kodofish.medium.com/eb32ffc94179)
- 在 IIS 8.0 下，安裝「應用程式初始化」模組的方式：[\[IIS\] 如何解決網站第一個請求 Request 特別慢 ? ~ m@rcus 學習筆記](https://marcus116.blogspot.com/2018/12/why-iis-application-so-slow-on-first-request.html)

### 使用 Web.Config 設定

除了在 IIS 內設定網站的「預先載入」功能以外，也可以在網站的 `web.config`  內設定 doAppInitAfterRestart ，此選項表示重新啟動時，是否要預先啟動應用程式。此外尚有其他數個選項可供調整：

- [利用IIS Application Initialization預啟動功能，來縮短ASP.NET網頁第一次的載入時間](https://slashview.com/archive2018/20180316.html)

已經有安裝 IIS 7.5 和 Application Warm-Up 的情形下，分別檢查應用程式集區、站台的設定：

> AppPool-> Advanced Settings. Set StartMode to AlwaysRunning. Increase Idle Time-out minutes as per your requirements. Go to your\_site-> Advanced Settings. Set Preload Enabled to True.

- [c# - First call to backend (WebAPi) is really slow - Stack Overflow](https://stackoverflow.com/questions/71231222/first-call-to-backend-webapi-is-really-slow)

### 留意

不應該直接將逾期時間從 20 分鐘改為 0 分鐘，除了可能會導致「使用者斷線後，伺服器不釋放資源」以外，若有敏感資料，因為持續地保留著，會提高資安風險 (例如使用者未登出即離開，下一位使用者接著使用該服務的狀況，常見於銀行、政府機關等服務)。

- [c# - WebAPI Slow first call to service - Stack Overflow](https://stackoverflow.com/questions/36056536/webapi-slow-first-call-to-service)
- [ASP.NET 小技巧 - 防止 Session 逾時與網頁閒置偵測-黑暗執行緒](https://blog.darkthread.net/blog/prevent-aspnet-session-timeout/)

### 其它參考資料

- 應用程式初始化 (Application Initialization) 官方頁面：[Application Initialization <applicationInitialization> - Microsoft Learn](https://learn.microsoft.com/en-us/iis/configuration/system.webserver/applicationinitialization/)
- [Use IIS Application Initialization for keeping ASP.NET Apps alive - Rick Strahl's Web Log](https://weblog.west-wind.com/posts/2013/oct/02/use-iis-application-initialization-for-keeping-aspnet-apps-alive#IIS-Configuration-for-Application-Initialization)