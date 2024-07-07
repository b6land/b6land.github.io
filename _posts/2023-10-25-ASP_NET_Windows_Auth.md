---
layout: post
title: ASP.NET Windows 驗證模式小記
date: 2023-10-25 12:00:00 +0800
categories: [ASP.NET]
---

本篇介紹 ASP.NET 中的 Windows 驗證模式，是一種簡易的網站使用者驗證方式。因為依賴 Windows 和 AD 的安全機制，不須撰寫額外的驗證邏輯，有著簡單和高安全性的特色。適合用於內部網路（Intranet）應用。

### ASP.NET 專案設定 Windows 驗證

在 ASP.NET 內的 Web.Config 內加入認證模式 = Windows，在啟用 Active Directory (AD) 的情形下，使用 AD 的使用者名稱 / 密碼進行驗證。以下是最基本的必要設定值。

```xml
<configuration>
  <system.web>
    <authentication mode="Windows" />
  </system.web>
</configuration>
```

也要搭配 IIS 一同設定站台的認證設定。

1. 開啟 IIS 管理員，選擇需要設定的站台。
2. 在首頁窗格中的「安全性」區段，按兩下「驗證」。
3. 在「驗證」窗格中，選取「Windows 驗證」，按下右方「動作」窗格中的「啟用」。

### Windows 驗證的相關程式碼

`User.Identity.IsAuthenticated` 可以判斷使用者是否是「已登入」狀態。

可以透過 `User.Identity.Name` 判斷目前是否已登入，若尚未登入，則此屬性為 Null，存取時會引發例外。

```cs
string strLoginID = User.Identity.Name;
```

登入失敗時，則回應 HTTP Response 401 (認證無效而拒絕存取)。

### 參考資料

- [使用 Windows 驗證驗證使用者 (C#) - Microsoft Learn](https://learn.microsoft.com/zh-tw/aspnet/mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs)
- [Windows 驗證 < windowsAuthentication> - Microsoft Learn](https://learn.microsoft.com/zh-tw/iis/configuration/system.webserver/security/authentication/windowsauthentication/)
- [部署內部網站並使用 Windows 驗證登入的標準作業流程 - The Will Will Web](https://blog.miniasp.com/post/2014/01/12/Deployment-Intranet-Sites-using-Windows-Authentication-SOP)
- [簡介 ASP.NET 表單驗證 (FormsAuthentication) 的運作方式 - The Will Will Web](https://blog.miniasp.com/post/2008/02/20/Explain-Forms-Authentication-in-ASPNET-20)