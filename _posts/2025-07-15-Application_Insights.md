---
layout: post
title: Azure Application Insights 的介紹
date: 2025-07-15 21:00:00 +0800
categories: [C#]
--- 

Applicaiton Insights 可以用來收集和分析遙測 (Telemetry) 資料，為應用程式賦予強大的可觀測性 (Observability)。

### 介紹

Applicaiton Insights 主要分成 Investigate, Monitoring, Usage 和 Code analysis 等功能，各功能的詳細介紹可以參考 [Application Insights OpenTelemetry observability overview - Azure Monitor - Microsoft Learn](https://learn.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview)。這些功能涵蓋了可觀測性的 Logs, Metrics, Tracing 三元素：

- Logs 紀錄獨立的作業，例如有呼叫請求。
- Metrics 是指計數數量或測量結果，例如完成請求的數量。
- Tracing 表示在分散式系統裡追蹤活動和請求。

OpenTelemetry 則是跨平台的收集、發送遙測資料的標準。

更詳細資料可以參考 [.NET Observability with OpenTelemetry - .NET - Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/observability-with-otel)。

### 安裝與啟用

- 若要在 ASP.NET 應用程式裡加入 Application Insights 的遙測，可照著這篇操作：[Configure monitoring for ASP.NET and ASP.NET Core with Application Insights - Azure Monitor - Microsoft Learn](https://learn.microsoft.com/en-us/azure/azure-monitor/app/asp-net)。
- 如果要使用 OpenTelemetry for NET 套件，則可照著這篇操作：[Enable OpenTelemetry in Application Insights - Azure Monitor - Microsoft Learn](https://learn.microsoft.com/en-us/azure/azure-monitor/app/opentelemetry-enable?)。
  - 範例 Github 專案：[azure-sdk-for-net/sdk/monitor/Azure.Monitor.OpenTelemetry.Exporter/tests/Azure.Monitor.OpenTelemetry.Exporter.Demo at main · Azure/azure-sdk-for-net · GitHub](https://github.com/Azure/azure-sdk-for-net/tree/main/sdk/monitor/Azure.Monitor.OpenTelemetry.Exporter/tests/Azure.Monitor.OpenTelemetry.Exporter.Demo)