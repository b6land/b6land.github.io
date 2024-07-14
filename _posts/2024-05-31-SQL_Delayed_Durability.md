---
layout: post
title: SQL Server 交易持久性與延遲交易持久
date: 2024-05-31 21:00:00 +0800
categories: [SQL Server]
--- 

如果需要大量寫入資料庫的時候，可以要求 SQL Server 先回報成功，再將變更結果從緩衝區寫入資料庫，即「延遲交易持久」功能。_(標題看起來很饒舌)_

### 語法和說明

``` sql
WITH ( DELAYED_DURABILITY = { ON | OFF } )
```

- 「交易持久性」表示交易記錄寫入磁碟的時機。
- 不啟用延遲交易持久時，交易完成時一定寫入至磁碟，交易完成後可立即被其它交易看到。
- 啟用延遲交易持久時，先將交易紀錄檔放置於緩衝區中，直到緩衝區滿或清理緩衝區時，才寫入磁碟。交易完成後也可立即被其它交易看到。
  - 特定情況可能會導致資料遺失，例如 SQL Server 當機或關機的情形，有可能來不及將緩衝區的資料寫入到磁碟。

### 參考資料

- 微軟的說明：[控制交易持久性 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/logs/control-transaction-durability?view=sql-server-ver16)
- 對於延遲交易持久的各種實驗：[\[SQL Server\]記憶體緩存資料寫入磁碟(三)延遲持久性Delayed Durability(和魔鬼交易) - 史丹利好熱 - 點部落](https://dotblogs.com.tw/stanley14/2017/02/06/212703)