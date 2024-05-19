---
layout: post
title: Microsoft Message Queue (MSMQ) 發生資源不足問題的解決方式
date: 2024-05-19 15:00:00 +0800
categories:  [Messaging]
---

本文介紹 MSMQ 為什麼發生「沒有足夠的資源執行作業」的問題，與解決的方式。

### 先認識 Microsoft Message Queue

Microsoft Message Queue，可簡稱為 MSMQ，是一種實作訊息佇列模式的訊息交換方式 / 元件。這項技術最早似乎可追朔到 Windows 95 的時代 (約 29 年前)，常用於伺服器 / 程序之間的訊息交換。藉由佇列 (Queue) 的設計，發送端傳出訊息後，接收端可以稍晚才接收。

詳細的工作方式，可閱讀維基百科：[Microsoft Message Queuing - Wikipedia](https://en.wikipedia.org/wiki/Microsoft_Message_Queuing)，以及官方提供的說明文件：[Message Queuing (MSMQ) - Microsoft Learn](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/msmq/ms711472(v=vs.85))。

若想用 .NET 平台實作應用 MSMQ 的協定程式，可以參考簡易的 Sender / Receiver 範例程式：[\[NET\] Message Queuing (MSMQ) : Sender/Receiver 範例 - Mike's開發瘋 - 點部落](https://dotblogs.com.tw/michaelfang/2016/08/27/MSMQ)

### 發生「沒有足夠的資源可執行作業」錯誤時怎麼處理

這個問題可能是因為「私用佇列 (Private Queue) 內的訊息超過大小限制」造成的。

從 MSMQ 的錯誤代碼列舉網頁：[MessageQueueErrorCode 列舉 (System.Messaging) - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/api/system.messaging.messagequeueerrorcode?view=netframework-4.8.1)，可以看到對應的列舉：InsufficientResources, -1072824281, 訊息文字：沒有足夠的資源執行作業。_(網頁內翻譯的文字和系統訊息略有不同，實際系統內訊息：沒有足夠的資源可執行作業。)_

檢查、解決方式如下：

1. 在「電腦管理」> 服務與應用程式 > 訊息佇列，查看目前私用佇列的使用情形。
2. 在「訊息佇列」項目點右鍵，選擇「內容」，並調整或取消佇列大小的限制。
3. 或是透過 Windows 的「服務」工具重新啟動 MSMQ 服務，以清除發送出去的訊息。

這是參考 [MSMQ: What can cause a "Insufficient resources to perform operation" error when receiving from a queue? - Stack Overflow](https://stackoverflow.com/questions/1732515) 文章的做法。

### 其它參考資料

由於 MSMQ 的技術已經有點舊，閱讀以下的網頁，有助於理解相關的設定方式：

- [\[Powershell\]啟用Windows Server MSMQ功能(訊息佇列) - 史丹利好熱 - 點部落](https://dotblogs.com.tw/stanley14/2016/09/26/235912)  
- [\[Powershell\]建立MSMQ(訊息佇列)設定 - 史丹利好熱 - 點部落](https://dotblogs.com.tw/stanley14/2016/09/27/001310)  

另一種解決方式，是重新啟動 MSMQ 服務：

- 、[You must restart the Message Queuing service to clean up message files - Microsoft Support](https://support.microsoft.com/en-us/topic/you-must-restart-the-message-queuing-service-to-clean-up-message-files-4d6a1e97-4428-6756-7584-81a1d1213ec1)