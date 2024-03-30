---
layout: post
title: C# NLog 套件寄送信件
date: 2024-03-30 12:00:00 +0800
categories:  [C#]
--- 

想使用 NLog 寄送信件，可以透過設定 NLog.config 內的 MailKit target 達成。輕鬆又愉快。

### 直接使用 MailKit

1\. 透過 NuGet 安裝 NLog 和 NLog.Mail。
2\. 在 nlog.config 內的 extensions 標籤內加入 assembly：

```xml
<nlog>
  <extensions>
    <add assembly="NLog.MailKit"/>
  </extensions>
  ...
```

3\. 在 nlog.config 內的 targets 標籤內加入 Mail target，以下為範例：

```xml
<target xsi:type="Mail" name="MailTarget 名稱" addNewLines="true" subject="郵件測試"
smtpServer="SMTP 伺服器位置" smtpPort="SMTP Port 號碼" smtpUsername="使用者名稱" smtpPassword="使用者密碼" 
smtpAuthentication="認證方式" from="寄件者位址" to="收件者位址"/>
```

4\. 在 rules  標籤內加入規則：

```xml
<rules>
  <logger name="*" minlevel="Error" writeTo="MailTarget 名稱" />
</rules>
```

5\. 可以自行撰寫一個 Error 的 Log 程式並啟動，檢查是否有發出錯誤訊息的郵件。
6\. 若沒有如預期發出郵件，可以查看 nlog-internal.txt 內是否有錯誤訊息。

### 使用背景工作整合 Log 並寄出

雖然上方提到直接使用 NLog 搭配 MailKit 寄送訊息，但使用通用 Logger 紀錄訊息，再使用背景工作整合 Log 並寄出，是比較推薦的方式。

這麼做的好處，是可以將訊息立即記錄到記憶體或較快的儲存裝置中，不會因為網路的延遲影響 Log 的紀錄行為；此外也可以先整理 Log 內容。

在微軟的官方文件中，也說明 `ILogger` 介面沒有提供非同步的紀錄方法。這是因為紀錄訊息應該要能很快執行完。如果需要記錄到資料庫等較慢的儲存媒體，應該先記錄到記憶體的佇列或變數中，再使用背景工作寫入。

### 參考資料

- 如何將 Log 用信件寄出比較適當：[c# - How can I properly send email (with error) from catch ()? - Stack Overflow](https://stackoverflow.com/questions/71855244/how-can-i-properly-send-email-with-error-from-catch)
- C# Log 的實作指南：[C# 中的記錄 - .NET - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/core/extensions/logging?tabs=command-line)
- [Mail target · NLog/NLog Wiki · GitHub](https://github.com/NLog/NLog/wiki/Mail-target)
- Mail target 的設定方式：[mrkt 的程式學習筆記: 使用NLog - Advanced .NET Logging (5)](https://kevintsengtw.blogspot.com/2011/10/nlog-advanced-net-logging-5.html)
  - 搭配 Mail target 使用的 SMTP Client，微軟官方已經因為太舊而建議不使用，因此建議瞭解本文概念後，引用 MailKit 寄送信件。
  - [SmtpClient 類別 (System.Net.Mail) - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/api/system.net.mail.smtpclient?view=net-8.0)