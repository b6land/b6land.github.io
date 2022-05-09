---
layout: post
title: SQL 解決「緩衝集區裡沒有足夠的可用記憶體」錯誤
date: 2019-09-05 12:00:00 +0800
categories: Database
tags: [Database, SQL Server]
---

本文章介紹解決「緩衝集區裡沒有足夠的可用記憶體」問題的數種方法。

### 錯誤發生

在包含大量資料的資料庫內執行以下查詢語法，可能會得到「Event 802: 緩衝集區裡沒有足夠的可用記憶體。」的錯誤：

```
SELECT TOP 1000 *
FROM [Massive_Database]
ORDER BY Time DESC
```

### 解決方法

以下為參考資料 3 提供的調整語法：

```
調整 instance 最高使用到6.4GB的記憶體。

-- Turn on advanced options
EXEC  sp_configure'Show Advanced Options',1;
GO
RECONFIGURE;
GO

-- Set max server memory
EXEC  sp_configure'max server memory (MB)',6400;
GO
RECONFIGURE;
GO
```

執行上方調整語法後重新執行查詢，可在約 10 秒內取出 100K 的資料，而不會發生緩衝區不足的情形。

這個方法，實際上和「**SQL 伺服器** 上按右鍵 > 伺服器設定 > 最大記憶體」選項相同，對調整語法不熟悉者，可先試著改動該選項的大小。

比起調整記憶體容量，更重要的是：**設計更嚴格條件的查詢語法**，縮小將查詢到的結果範圍，才能減少對儲存空間的存取與記憶體使用；**另外則是減少排序的機會**，以減輕效能的消耗。

除了造成緩衝區錯誤的查詢以外，也可以透過查閱報表，獲得其他查詢指令的效能影響。在 「**SQL 伺服器** 上按右鍵 > 報表 > 標準報表」 內，有數個有用的報表，其中可參考「記憶體耗用量」、「效能 - 依總 CPU 時間」。

### 其它可嘗試作法

根據參考資料 1 的建議，還可檢查以下選項：

1. `sqlserver.exe` 的記憶體變化。
> 在本查詢語法的例子中，沒有大幅度的變化。
2. 使用 Memory Manager，可以從 `DBCC MEMORYSTATUS` 指令找到。
3. `DBCC MEMORYSTATUS` 命令會提供 Microsoft SQL Server 的目前記憶體狀態的快照。
> 選項 2 和 3 需要對 DBCC 有一定程度的瞭解，才能找出和記憶體耗盡有關的報告。
4. 檢查工作負載。

以下 DBCC 指令可用於清除快取，但未必能使設計不良的查詢語法順利執行：

```
DBCC FREESYSTEMCACHE
DBCC FREESESSIONCACHE
DBCC FREEPROCCACHE
```

### 參考資料

1. [MSSQLSERVER_802 - Database Engine 錯誤 - SQL Server - Microsoft Docs](https://docs.microsoft.com/zh-tw/sql/relational-databases/errors-events/mssqlserver-802-database-engine-error?view=sql-server-2017)
2. [如何使用 DBCC MEMORYSTATUS 命令，來監視 SQL Server 2005 上的記憶體使用量](https://support.microsoft.com/zh-tw/help/907877/)
3. [[SQL Server]記憶體配置與效能 - 史丹利好熱 - 點部落](https://dotblogs.com.tw/stanley14/2017/07/22/sqlmemorysetting)