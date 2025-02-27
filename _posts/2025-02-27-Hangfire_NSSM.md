---
layout: post
title: 建立服務的工具: Hangfire 和 NSSM 服務管理員
date: 2025-02-27 22:30:00 +0800
categories: [Other]
---  

以下介紹兩個建立 .NET 服務時會用到的工具：常用在排程、背景服務的 Hangfire，以及比 Windows 內建服務工具更強大的 NSSM。

### Hangfire 排程執行工作

Hangfire 可以在 .NET 或 .NET Core 平台上執行排程或背景服務。撰寫簡單的語法，就能設定一次性、重複、一段時間後執行的工作，也能執行接續性的工作。另外也包含網頁式的管理介面和失敗重試的功能。

以下收錄 Hangfire 的相關文章：

- 介紹在較新的 .NET 環境設定 Hangfire：[.NET 6 使用 HangFire 執行排程任務 - TWJOIN 哲煜科技](https://blog.twjoin.com/fc3bc143b7d4 )
- 「黑暗執行緒」網站有針對較舊的 ASP.NET Hangfire 的一系列文章：[黑暗執行緒 分類檢視：hangfire](https://blog.darkthread.net/blog/category/hangfire)。此處想特別紀錄 [Hangfire 筆記2 - 執行定期排程-黑暗執行緒](https://blog.darkthread.net/blog/hangfire-recurringjob-notes/)，想透過 ASP.NET Hangfire 排程執行工作，前提為「應用程式 (網站) 永遠保持運作」，可以透過 IIS 設定「應用程式初始化」功能，使網站不會進入閒置 (Idle) 狀態。
- 官方網站：[Hangfire – Background jobs and workers for .NET and .NET Core](https://www.hangfire.io/)

### NSSM 服務管理員

Non-Sucking Service Manager 的縮寫，可以幫你註冊 Windows 服務，功能強大且可幫忙處理例外狀況。

- NSSM 的介紹：[介紹好用工具：優異的 nssm 服務管理員 (Non-Sucking Service Manager) - The Will Will Web](https://blog.miniasp.com/post/2021/09/15/Useful-tools-the-Non-Sucking-Service-Manager)
- 官方說明：[NSSM - the Non-Sucking Service Manager](https://nssm.cc/usage)