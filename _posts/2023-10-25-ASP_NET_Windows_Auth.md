---
layout: post
title: ASP.NET Windows 驗證模式小記
date: 2023-10-25 12:00:00 +0800
categories: [ASP.NET]
---

本篇介紹 ASP.NET 中的 Windows 驗證模式，是一種簡易的網站使用者驗證方式。

### Windows 驗證模式

在 Web.Config 內加入認證模式 = Windows，且有啟用 Active Directory (AD) 的情形下，會使用 AD 的使用者名稱 / 密碼進行驗證。
也要搭配 IIS 一同設定站台的認證設定。

```
<authentication mode="Windows" />
```

[使用 Windows 驗證驗證使用者 (C#) - Microsoft Learn](https://learn.microsoft.com/zh-tw/aspnet/mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs)
[部署內部網站並使用 Windows 驗證登入的標準作業流程 - The Will Will Web](https://blog.miniasp.com/post/2014/01/12/Deployment-Intranet-Sites-using-Windows-Authentication-SOP)

可以透過 `User.Identity.Name` 判斷目前是否已登入，若尚未登入，則此屬性為 Null，存取時會引發例外。

```
string strLoginID = User.Identity.Name;
```

登入失敗時，則回應 HTTP Response 401 (認證無效而拒絕存取)。

[簡介 ASP.NET 表單驗證 (FormsAuthentication) 的運作方式 - The Will Will Web](https://blog.miniasp.com/post/2008/02/20/Explain-Forms-Authentication-in-ASPNET-20)