---
layout: post
title: 解決 Azure Service Bus 的 Message Batch 錯誤
date: 2024-06-27 21:00:00 +0800
categories:  [Messaging]
---

同一個訊息批次 (Message Batch) 物件被 `SendMessagesAsync` 多次呼叫，可能會導致錯誤，以下列出解決方法。

### 錯誤描述

如果 `EventDataBatch.TryAdd` 發生 `System.InvalidOperationException` 錯誤：

```plaintext
System.InvalidOperationException: The message batch is currently being used in communication with the Service Bus service; messages may not be added until the active operation is complete.
```

這段訊息除了表示批次過大的問題外，也可能是表示一個暫時性的問題，例如在該批次傳送完成後再次重新傳送；也可能是多次在同一個訊息批次發布和改變內容，例如發生了競逐狀態 (Race condition)，這通常是無意間造成的。

### 解決方式

試著避免在同一個訊息批次內呼叫多次 `SendMessagesAsync`  方法。

最簡單的方法是根據官方的範例程式碼重寫傳送訊息批次的部分。以下的官方範例內，其重點為：

1. 批次內的訊息大小不能過大。
2. 傳送訊息批次後，要將 sender 和 client 釋放 (Dispose)，確保清空資源。

```csharp
// create a batch 
using ServiceBusMessageBatch messageBatch = await sender.CreateMessageBatchAsync();

for (int i = 1; i <= numOfMessages; i++)
{
    // try adding a message to the batch
    if (!messageBatch.TryAddMessage(new ServiceBusMessage($"Message {i}")))
    {
        // if it is too large for the batch
        throw new Exception($"The message {i} is too large to fit in the batch.");
    }
}

try
{
    // Use the producer client to send the batch of messages to the Service Bus queue
    await sender.SendMessagesAsync(messageBatch);
    Console.WriteLine($"A batch of {numOfMessages} messages has been published to the queue.");
}
finally
{
    // Calling DisposeAsync on client types is required to ensure that network
    // resources and other unmanaged objects are properly cleaned up.
    await sender.DisposeAsync();
    await client.DisposeAsync();
}

Console.WriteLine("Press any key to end the application");
Console.ReadKey();
```

### 參考資料

- 錯誤的解說：[\[QUESTION\] Azure.Messaging.EventHubs.Producer.EventDataBatch.TryAdd Throws InvalidOperationException when EventDataBatch is in use · Issue #18157 · Azure/azure-sdk-for-net · GitHub](https://github.com/Azure/azure-sdk-for-net/issues/18157 ) 
- 錯誤的解決方式：[azureservicebus - .Net5 Azure Function - Azure.Messaging.ServiceBus - Stack Overflow](https://stackoverflow.com/questions/71086574/net5-azure-function-azure-messaging-servicebus)
- 完整的範例程式碼：[快速入門 - 從 .NET 應用程式使用 Azure 服務匯流排 佇列 - Azure Service Bus - Microsoft Learn](https://learn.microsoft.com/zh-tw/azure/service-bus-messaging/service-bus-dotnet-get-started-with-queues?tabs=passwordless)