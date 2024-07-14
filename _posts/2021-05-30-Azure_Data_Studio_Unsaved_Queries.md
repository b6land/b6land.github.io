---
layout: post
title: Azure Data Studio 關閉未儲存的 SQL 查詢
date: 2021-05-30 12:00:00 +0800
categories:  [IDE]
--- 

本文章介紹如何關閉 Azure Data Studio 未儲存的 SQL 查詢。

### 問題描述

在 Azure Data Sutdio 將未儲存的查詢 (Unsaved queries) 關閉，並結束程式以後，正常狀況下重新啟動後不會再顯示未儲存的查詢。

若再次出現先前關閉的未儲存查詢，可以透過以下方式處理。

### 解決方式

刪除以下路徑內的檔案，來解決問題：

`C:\Users[your username]\AppData\Roaming\azuredatastudio\Backups[random number]\untitled\`

### 參考資料

[Manually closed unsaved queries keep opening with Azure Data Studio · Issue #13872 · microsoft/azuredatastudio · GitHub](https://github.com/microsoft/azuredatastudio/issues/13872)