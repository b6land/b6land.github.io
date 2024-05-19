---
layout: post
title: Message Queue 與 Publish - Subscribe 模式
date: 2024-05-19 12:30:00 +0800
categories:  [Messaging]
---

本文介紹 Message Queue 和 Publish - Subscribe 的特色，以及相關的協定。

### Message Queue (訊息佇列) 和 Publish - Subscribe (發布 - 訂閱)

根據 [Message queue - Wikipedia](https://en.wikipedia.org/wiki/Message_queue) 的介紹，是一種非同步的訊息交換模式，其特色是為單一的接收端提供佇列暫時存放訊息，確保能取得發送端的訊息。例如：郵差送信時，你不必立刻收信，信件會被投遞到信箱內，你可以之後再從信箱取得信件。

有多種協定實作這種模式，例如 Microsoft Message Queue (MSMQ)、Azure Service Bus。

根據 [Message Queue vs. Publish-Subscribe - The Iron.io Blog](https://blog.iron.io/message-queue-vs-publish-subscribe/) 的介紹，「Publish - Subscribe」是和「Message Queue」類似的非同步訊息交換模式，最大差異在於「Publish - Subscribe」模式支援將一個訊息發給多個接收端。

此模式下可以發送訊息給不同的主題 (Topic) ，每個主題有多個不同的訂閱者，也可以實作暫存訊息的佇列。有多種協定實作此模式，如 MQTT。

### 關於 Azure Service Bus

- 同時提供「Publish - Subscribe」和「Message Queue」模式。
- 可以透過「無效信件佇列」處理無法傳遞或處理的訊息。
- 負載平衡，以及使用佇列避免流量尖峰的過大負擔。
- 具有 Transaction 的概念，確保傳遞訊息的動作完成以後，能取得完整的結果。

藉由 Azure Service Bus 作為中介，可以從雲端、本地端、Azure 服務傳送和接收訊息。

參考自：

- [訊息服務站 - ServiceBus - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10240878)  
- [Azure 服務匯流排 (企業訊息代理程式) 簡介 - Azure Service Bus - Microsoft Learn](https://learn.microsoft.com/zh-tw/azure/service-bus-messaging/service-bus-messaging-overview)  

### 關於 MQTT

- 因其使用較少資源、網路頻寬的特性，常用於 IoT (Internet of Things，物聯網) 設備間的訊息交換。
- 需要藉由網路協議提供有序、無失真、雙向的連線。
- 是 OASIS 的開放標準。
- 除了 Publisher、Subscriber 以外，需要另外建立 Broker (中介服務) 交換訊息。
- 實作「Publish - Subscribe」模式，也提供佇列，讓訂閱者能取得稍早發布的訊息。

參考自：[MQTT - Wikipedia](https://en.wikipedia.org/wiki/MQTT)