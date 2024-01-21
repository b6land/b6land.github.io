---
layout: post
title: 「學習單元測試」系列文的介紹與補充
date: 2024-01-21 12:00:00 +0800
categories:  [Unit Test, C#]
--- 

[學習單元測試 - 程式隨筆](https://toyo0103.github.io/categories/學習單元測試/) 系列文章是簡單易懂的單元測試教學文章。但因為發佈於 2017 年，已經有 API 結束營運的問題；如果要使用較新版本的 .Net 練習，也有些微要調整的地方。以下是兩點個人心得。

### 選擇使用不同的 API

文中介紹的 PTX (公共運輸整合資訊流通服務平台) 已經結束服務，可以從此文章列出的 API 中，挑選出合適的 API 來練習：[第三方 API 整理. Public API 列表 - by 彼得潘的 iOS App Neverland](https://medium.com/彼得潘的-swift-ios-app-開發問題解答集/第三方-api-整理-115868a56959)

以下是我有測試過的 API，可分為

- 不需要申請密鑰的 API

  - 非官方 MyAnimeList.com API: [Jikan - Unofficial MyAnimeList API](https://jikan.moe/)
  - 國家資訊 API: [REST Countries](https://restcountries.com/)

- 需要申請密鑰的 API

  - [TDX 運輸資料流通服務](https://tdx.transportdata.tw/)

### `InternalVisbleTo` 屬性

使用 .Net SDK 風格的專案檔案時，已經無法在原本的 `AssemblyInfo.cs`加入 `InternalVisbleTo` 屬性，有什麼其它方式，使被封裝的內部方法可被測試專案呼叫？

解決方法之一是在 csproj 專案檔內使用 `InternalsVisibleToAttribute` 屬性，並在參數加入要開放的專案名稱，範例如下 \[1\]： 

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net7.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
    <AssemblyAttribute Include="System.Runtime.CompilerServices.InternalsVisibleToAttribute">
      <_Parameter1>Practice.Tests</_Parameter1>
  </AssemblyAttribute>
  </ItemGroup>

</Project>
```

- \[1\] [.NET Project SDKs 設定 InternalsVisibleTo - 余小章 @ 大內殿堂 - 點部落](https://dotblogs.com.tw/yc421206/2021/01/22/declaring_InternalsVisibleTo_in_the_dot_net_project_sdk)