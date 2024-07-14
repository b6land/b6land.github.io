---
layout: post
title: SQL 變更資料表欄位為 Nullable
date: 2024-01-10 12:00:00 +0800
categories: [SQL Server]
---

如何在不 DROP 資料表的情形下，將資料表中的某一個欄位轉成可為 NULL？

### 使用語法修改

將欄位改為 NULL 的語法如下：

```
ALTER TABLE myTable ALTER COLUMN myColumn {DataType} NULL
```

將其中的 {DataType} 改為需要的資料型態，例如 `varchar(20)` 或 `int`；myTable 和 myColumn 分別改成要修改的資料表和欄位名稱。

改寫自以下問題：[sql server - Can I change a column from NOT NULL to NULL without dropping it? - Stack Overflow](https://stackoverflow.com/questions/7407650/can-i-change-a-column-from-not-null-to-null-without-dropping-it)

### 使用 SSMS 的設計工具

在 SSMS 內，可以使用設計工具變更資料表的欄位 Null 屬性。勾選或取消特定欄位的「允許 Null」屬性後，按右鍵選擇「產生變更指令碼」即可。

![SSMS 資料表設計工具](/assets/imgs/2024-01-10/SSMS_Table_Designer.png) 

不過，當資料表裡已有資料時，變更欄位的 Null 屬性，會出現「不允許儲存變更...」的錯誤訊息：

![不允許儲存變更](/assets/imgs/2024-01-10/changes_is_not_permitted.png)

這是因為從設計工具變更屬性時，會需要重新建立資料表，有可能在重建的過程中，遺失部分資料表內的資料。

如果可以接受資料遺失的風險 (ex. 修改開發環境內的資料表結構)，可以從 SSMS 的設定 > 選項 > 設計，取消「防止儲存需要資料表重建的變更」勾選來關閉此錯誤訊息：

![防止儲存需要資料表重建的變更](/assets/imgs/2024-01-10/SSMS_Options.png)  

請參考微軟的官方說明 [Saving changes is not permitted error message - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/troubleshoot/sql/ssms/error-when-you-save-table) 與 [SQL Server 無法修改資料表定義？！ - Yowko's Notes](https://blog.yowko.com/sql-server-cahnges-not-permitted/)。