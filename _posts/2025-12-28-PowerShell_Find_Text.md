---
layout: post
title: PowerShell 收集包含特定文字的內容，匯集到單一文字檔
date: 2025-12-28 22:00:00 +0800
categories: [Programming]
--- 

在 Windows 的 Powershell v7 上，可以從來源的所有文字檔裡面，找出包含特定標籤的文字列，並匯集到另一個文字檔，方法如下。

### 語法

```powershell
$files = Get-ChildItem "C:\Logs" -Filter *.txt -Recurse

foreach ($f in $files) {
    Select-String -Path $f.FullName -Pattern "Keyword" |
        ForEach-Object { $_.Line } |
        Add-Content "Result.txt"
}
```

以上語法會查詢 `C:\Logs` 目錄裡的所有 txt 檔案，將包含 Keyword 這個關鍵字的文字列，全部匯集到 Result.txt 內。可以自己代換成需要的目錄、關鍵字和結果檔名。

### 說明

`Get-ChildItem` 列出目錄中的檔案與子目錄，參數如下：

- `-Filter`  ：只抓特定檔名的檔案
- `-Recurse` ：遞迴搜尋所有子資料夾

接著透過 `foreach`  逐一處理 `Get-ChildItem` 找到的檔案。

裡面有 `Select-String`  ，用來搜尋文字或正規表達式，其參數如下：

- `-Path`  ：指定要搜尋之檔案的路徑
- `-Pattern` ：要查詢的文字或正規表達式

找到結果是物件，接著為每個找到的物件，透過 `Add-Content`  將該列文字附加到文字檔內。

### 參考資料

- ChatGPT
- [Get-ChildItem (Microsoft.PowerShell.Management) - PowerShell - Microsoft Learn](https://learn.microsoft.com/zh-tw/powershell/module/microsoft.powershell.management/get-childitem?view=powershell-7.5)
- [Select-String (Microsoft.PowerShell.Utility) - PowerShell - Microsoft Learn](https://learn.microsoft.com/zh-tw/powershell/module/microsoft.powershell.utility/select-string?view=powershell-7.5)