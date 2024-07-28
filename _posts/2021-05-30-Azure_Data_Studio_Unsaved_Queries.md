---
layout: post
title: Azure Data Studio 的未儲存 SQL 查詢功能
date: 2021-05-30 12:00:00 +0800
categories:  [IDE]
--- 

本文章介紹 Azure Data Studio 未儲存的 SQL 查詢功能，以及發生異常時要怎麼處理。

### 未儲存的查詢

以往在 SQL Server Management Studio (SSMS) 內，查詢都需要在關掉程式前存檔。

現在在 Azure Data Studio 內，假如遇到來不及存檔的狀況，仍可以先關閉程式。程式會把目前開啟的查詢語法內容保留起來。

### 發生異常時如何處理

關閉未儲存的查詢 (Unsaved queries)，並結束程式以後，正常狀況下重新啟動後不會再顯示未儲存的查詢。

若再次出現先前關閉的未儲存查詢，可以透過刪除以下路徑內的檔案，來解決問題：

`C:\Users[your username]\AppData\Roaming\azuredatastudio\Backups[random number]\untitled\`

- 請參考 [Manually closed unsaved queries keep opening with Azure Data Studio · Issue #13872 · microsoft/azuredatastudio · GitHub](https://github.com/microsoft/azuredatastudio/issues/13872)
  - 至 2023/04 為止，已經沒有再出現類似問題，如果再次發生，可以至上方連結反映。