---
layout: post
title: NLog 套件的設定
date: 2024-01-22 12:00:00 +0800
categories:  [Programming]
--- 

NLog 是一套 Open Source 的 Log 工具，支援多種 .Net 平台，使用上簡單又具備調整的彈性。

可以跟著 [NLog](https://nlog-project.org/download/) 官網教學安裝。安裝好 NLog 後，可以藉由調整 `nlog.config` 來調整 Log 的格式。以下介紹 `nlog.config` 檔案的常用設定值 (也可直接查詢 [Configuration file · NLog/NLog Wiki](https://github.com/NLog/NLog/wiki/Configuration-file))。

### Rule 功能

Rule 功能可以限制不同等級的訊息輸出到特定的目標 (Target) ，如檔案或網路位址。

部分會用到的 Rule 元素如下：

|     |     |
| --- | --- |
| 元素  | 說明  |
| name <br> | 要記錄的類別名稱，可接受萬用字元 \* 和 ?<br> |
| minlevel <br> | 要記錄的最小等級 |
| maxlevel <br> | 要記錄的最大等級<br> |
| writeTo <br> | 要寫入的 Target |
| final <br> | (此類別) 最後適用的規則，列在後方的不處理 |

### Layout 功能  

Layout 功能是依照特定標籤和格式顯示 Log，標籤可稱為 Layout Renderer，包含多種實用的除錯內容。

部分會使用到的 Layout 標籤如下：

|     |     |     |
| --- | --- | --- |
| 標籤<br> | 描述  | 連結  |
| aspnet-request-url<br> | 顯示需求的 URL，預設顯示 Scheme, Host 和 Path，可另外顯示 Port 和 Query String | [AspNetRequest Url Layout Renderer · NLog/NLog Wiki · GitHub](https://github.com/NLog/NLog/wiki/AspNetRequest-Url-Layout-Renderer)<br> |
| activity<br> | .NET 分散式追蹤的列舉，可以加入 property=TraceId 顯示個別 API 請求的 Trace ID | [GitHub - NLog/NLog.DiagnosticSource](https://github.com/NLog/NLog.DiagnosticSource)<br> |
| level | 訊息等級 | [Level layout renderer · NLog/NLog Wiki · GitHub](https://github.com/NLog/NLog/wiki/Level-Layout-Renderer)<br> |
| longtime | 以 yyyy-MM-dd HH:mm:ss.ffff 格式顯示時間 | [Longdate Layout Renderer · NLog/NLog Wiki · GitHub](https://github.com/NLog/NLog/wiki/LongDate-Layout-Renderer )<br> |
| message | 訊息內容 | [Message Layout Renderer · NLog/NLog Wiki · GitHub](https://github.com/NLog/NLog/wiki/Message-Layout-Renderer )<br> |
| pad<br> | 顯示時內容固定寬度，不足時以空格補充 | [Pad Layout Renderer · NLog/NLog Wiki · GitHub](https://github.com/NLog/NLog/wiki/Pad-Layout-Renderer)<br> |
| replace-newlines<br> | 取代換行字元，預設為空白 | [Replace NewLines Layout Renderer · NLog/NLog Wiki · GitHub](https://github.com/NLog/NLog/wiki/Replace-NewLines-Layout-Renderer)<br> |
| truncate<br> | 內容超過指定長度時，截斷後方內容 | [I want to truncate message layout renderer after 30 characters in database logging.Any suggestion? · Issue #3040 · NLog/NLog · GitHub](https://github.com/NLog/NLog/issues/3040 )<br> |

詳細列表可以參考：[Layout Renderers - Config - NLog](https://nlog-project.org/config/?tab=layout-renderers)

### 加入 Trace ID

若想加入每個 API 請求的 Trace ID，以增加除錯的觀察性，可以安裝 `NLog.DiagnosticSource`  套件：[GitHub - NLog/NLog.DiagnosticSource](https://github.com/NLog/NLog.DiagnosticSource) 。這個套件是 [Microsoft Activity Trace](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) (.NET 分散式追蹤) 的 NLog Layout Renderer。

可以透過 NuGet 管理員安裝套件，並在 nlog.config 內加入：

```xml
<extensions>
    <add assembly="NLog.DiagnosticSource"/>
</extensions>
```

即安裝完成。


