---
layout: post
title: 修改 PATH 環境變數
date: 2019-01-28 12:00:00 +0800
categories:  [Windows]
---

這篇文章介紹，如果需要以指令 (如 BAT 檔) 設定 Windows 的 `PATH` 變數，而且長度超過 1024 時，如何處理。

### 修改 PATH 環境變數

`PATH` 環境變數可以讓系統知道要去哪些目錄裡尋找指令。

一般來說，在 Windows 10 裡面，我們會透過「設定 > 系統 > 關於 > 進階電腦設定 > 環境變數」來設定 `PATH` 的內容。

如果有需要透過指令設定 `PATH` 變數，例如安裝程式的批次檔，就可以參考以下設定方式。


1. 使用 `setx /m path "%path%;C:\Folder1"`，不過會有變數的字元長度不能大於 1024 的限制。

2. 直接編輯 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment` 內的 `PATH` 變數。

因為 `PATH` 變數其實是記錄在登錄檔 (Registry) 內，因此可使用 cmd 呼叫 REG.exe，先取出既有的 `PATH` 變數，修改以後再覆寫回原有的登錄值。

如下面範例是附加一個測試目錄至 PATH 變數底下：

```powershell
SET TestPath="%Path%;C:\Test"
REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v Path /t REG_SZ /d %TestPath% /f
```

第一行先設定變數，接著第二行將其加入至登錄檔環境變數，並指定變數的名稱 (/v)、類型 (/t)、內容 (/d) 和強制更新 (/f)。

### 參考資料

- [Set PATH and other environment variables in Windows 10](https://www.opentechguides.com/how-to/article/windows-10/113/windows-10-set-path.html)
- [Reg - Edit Registry - Windows CMD - SS64.com](https://ss64.com/nt/reg.html)

