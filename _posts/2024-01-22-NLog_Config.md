---
layout: post
title: C# NLog 套件的設定
date: 2024-01-22 12:00:00 +0800
categories:  [C#]
--- 

NLog 是一套 Open Source 的 Log 工具，支援多種 .Net 平台，使用上簡單又具備調整的彈性。

可以跟著 [NLog](https://nlog-project.org/download/) 官網教學安裝。安裝好 NLog 後，可以藉由調整 `nlog.config` 來調整 Log 的格式。以下介紹 `nlog.config` 檔案的常用設定值 (也可直接查詢 [Configuration file · NLog/NLog Wiki](https://github.com/NLog/NLog/wiki/Configuration-file))。

### Target 功能

指定訊息要輸出的目標，常用的 target 如下：

| Target | 描述  |
| --- | --- |
| Console | 顯示在命令列上 |
| File | 寫入文字檔案 |
| MailKit | 寄送 SMTP 郵件訊息 |
| Memory | 存放訊息在 ArrayList |

### Rule 功能

Rule 功能可以限制不同等級的訊息輸出到特定的目標 (Target) ，如檔案或網路位址。

部分會用到的 Rule 元素如下：

| 元素  | 說明  |
| --- | --- |
| name | 要記錄的類別名稱，可接受萬用字元 \* 和 ? |
| minlevel | 要記錄的最小等級 |
| maxlevel | 要記錄的最大等級 |
| writeTo | 要寫入的 Target |
| final | (此類別) 最後適用的規則，列在後方的不處理 |
| finalMinLevel | (此類別) 只記錄該等級以上的訊息  
適用於 NLog 5.0 和之後的版本，請參考：[Logging Rules FinalMinLevel · NLog/NLog Wiki · GitHub](https://github.com/NLog/NLog/wiki/Logging-Rules-FinalMinLevel) |

### Layout 功能  

Layout 功能是依照特定標籤和格式顯示 Log，標籤可稱為 Layout Renderer，包含多種實用的除錯內容。

部分會使用到的 Layout 標籤如下：

| 標籤 | 描述  | 連結  |
| --- | --- | --- |
| aspnet-request-url<br> | 顯示需求的 URL，預設顯示 Scheme, Host 和 Path，可另外顯示 Port 和 Query String | [AspNetRequest Url Layout Renderer · NLog/NLog Wiki · GitHub](https://github.com/NLog/NLog/wiki/AspNetRequest-Url-Layout-Renderer)<br> |
| activity<br> | .NET 分散式追蹤的列舉，可以加入 property=TraceId 顯示個別 API 請求的 Trace ID | [GitHub - NLog/NLog.DiagnosticSource](https://github.com/NLog/NLog.DiagnosticSource)<br> |
| level | 訊息等級 | [Level layout renderer · NLog/NLog Wiki · GitHub](https://github.com/NLog/NLog/wiki/Level-Layout-Renderer)<br> |
| longtime | 以 yyyy-MM-dd HH:mm:ss.ffff 格式顯示時間 | [Longdate Layout Renderer · NLog/NLog Wiki · GitHub](https://github.com/NLog/NLog/wiki/LongDate-Layout-Renderer )<br> |
| message | 訊息內容 | [Message Layout Renderer · NLog/NLog Wiki · GitHub](https://github.com/NLog/NLog/wiki/Message-Layout-Renderer )<br> |
| pad<br> | 顯示時內容固定寬度，不足時以空格補充 | [Pad Layout Renderer · NLog/NLog Wiki · GitHub](https://github.com/NLog/NLog/wiki/Pad-Layout-Renderer)<br> |
| replace-newlines<br> | 取代換行字元，預設為空白 | [Replace NewLines Layout Renderer · NLog/NLog Wiki · GitHub](https://github.com/NLog/NLog/wiki/Replace-NewLines-Layout-Renderer)<br> |
| truncate<br> | 內容超過指定長度時，截斷後方內容 | [I want to truncate message layout renderer after 30 characters in database logging.Any suggestion? · Issue #3040 · NLog/NLog · GitHub](https://github.com/NLog/NLog/issues/3040 )<br> |

詳細列表可以參考：[Layout Renderers - Config - NLog](https://nlog-project.org/config/?tab=layout-renderers)

### Log 等級

NLog 的 Log 等級可以分成以下幾種，順序越大表示嚴重性越高：


| 等級 | 順序 | 說明 |
| --- | --- | --- |
| Trace | 0   | 通常用在開發程式的觀察 |
| Debug | 1   | 用於除錯想檢查的部分 |
| Info | 2   | 標示出應用程式的重要事件 |
| Warn | 3   | 警告驗證問題或是可被還原的暫時性錯誤 |
| Error | 4   | 例外發生或功能失效 |
| Fatal | 5   | 最嚴重等級，應用程式將要停止運作 |

### 加入 Trace ID

若想加入每個 API 請求的 Trace ID，以增加除錯的觀察性，可以安裝 `NLog.DiagnosticSource`  套件：[GitHub - NLog/NLog.DiagnosticSource](https://github.com/NLog/NLog.DiagnosticSource) 。這個套件是 [Microsoft Activity Trace](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) (.NET 分散式追蹤) 的 NLog Layout Renderer。

可以透過 NuGet 管理員安裝套件，並在 nlog.config 內加入：

```xml
<extensions>
    <add assembly="NLog.DiagnosticSource"/>
</extensions>
```

即安裝完成。接著在 nlog.config 的 Layout 內增加 `activity:property=TraceId` 屬性，例如：

```xml
<target xsi:type="File" name="web" fileName="${shortdate}.log"
  layout="${time}-[${level}]-[${activity:property=TraceId:truncate=6}]-${replace-newlines:${message}} ${exception:format=tostring}" />
```

### File Target 與歸檔功能

如果將 Log 寫在檔案內，需要讓 Log 可以自動歸檔，然後在一段時間後自動刪除，可以藉由設定 Archive 相關的參數達到這個目標。

請參考以下的 File Target：

```xml
<target name="file" 
xsi:type="File" 
keepFileOpen="true"
layout="${DefaultLayout}"
fileName="${LogRoot}/${machinename}-${shortdate}.txt"
encoding="UTF-8"
archiveFileName="${LogRoot}/${machinename}-${shortdate}.Archive.txt"
maxArchiveDays="3"  
archiveEvery="Day"/> 
```

| 參數  | 說明  |
| --- | --- |
| archiveFileName<br> | 歸檔的檔名，推薦檔案放在同一個目錄下<br> |
| maxArchiveDays<br> | 歸檔最多保留 N 天，超過會刪除<br> |
| archiveEvery<br> | 每天 (或其它條件) 歸檔<br> |

請參考：

- [File target · NLog/NLog Wiki · GitHub](https://github.com/nlog/NLog/wiki/File-target#archival-options )
- [c# - NLog - delete logs older than X days - Stack Overflow](https://stackoverflow.com/questions/32068788/nlog-delete-logs-older-than-x-days)
- [NET C# 使用NLog 紀錄封存處理切割、最多天數、壓縮 · vulcanlee/Blogs2023 · GitHub](https://github.com/vulcanlee/Blogs2023/blob/main/CSharp/Log-Archive-maxFiles-Numbering-Days-Format-Compression.md)

### Memory Target

Memory Target 適用於應用程式內取得寫入的訊息紀錄，因為存取 File Target 時，可能會遇到 NLog 正在占用的情形，可參考：[NLog get log message in c# - Stack Overflow](https://stackoverflow.com/questions/52864862/nlog-get-log-message-in-c-sharp)。