---
layout: post
title: 使用 PowerShell 呼叫 API
date: 2024-08-30 22:00:00 +0800
categories: [Programming]
--- 

除了使用 Postman 或是程式以外，PowerShell 也可以用來呼叫 API ~

## 基本知識

先來看一些基本知識。

- PowerShell 可以使用 `#` 作為單行註解、`<# ... #>` 作為多行註解。
- PowerShell 可以存成為 ps1 副檔名的腳本檔。
- 預設情形下，執行腳本必須要簽章，需要修改執行策略，才能執行 PowerShell 腳本。如果要限定僅修改目前使用者的執行策略，可加上 `-Scope CurrentUser`  參數：

```powershell
Set-ExecutionPolicy RemoteSigned
```

- [註解型說明 - PowerShell - Microsoft Learn](https://learn.microsoft.com/zh-tw/powershell/scripting/lang-spec/chapter-14?view=powershell-7.2)
- [PowerShell 筆記](https://blog.poychang.net/note-powershell/)  
- [Powershell 學習筆記-黑暗執行緒](https://blog.darkthread.net/blog/powershell-learning-notes/)  

## Invoke-WebRequest 呼叫 API

Linux 底下有 cURL 指令可以用，在 Windows 下則有 PowerShell 的 Invoke-WebRequest 可以使用，以下是範例：

```powershell
$response = Invoke-WebRequest -Uri 'https://jsonplaceholder.typicode.com/posts' -Method POST -Headers $headers -ContentType 'application/json' -Body '{
  "title": "foo",
  "body": "bar",
  "userId": 1
}'
```

將 Invoke-WebRequest 呼叫 API 的結果存至 response 變數，使用 `-Uri` 指定網址、 `-Method` 指定 Rest 的方式、`-Headers` 指定標頭的變數、`-ContentType` 指定內容類型、`-Body` 指定內容。

若 Body 內有單引號時，可以重複輸入單引號，使其不會被誤認為 PowerShell 須處理的符號。例如：

```powershell
 parameters[''query'']; 
```

若回傳內容是 JSON 格式的話，可以用

```powershell
$jsonContent = $response.Content | ConvertFrom-Json
```

轉換 JSON 的內容，ConvertFrom-Json 也會一併處理內容的 UTF-8 文字，所以可以正常顯示中文。

最後使用以下指令顯示 JSON 內容在主控台 (Console) 上。

```powershell
Write-Output $jsonContent
```

![執行過程與結果](/assets/imgs/2024-09-20/PowerShell_WebRequest.png)

### 參考資料

- [How to use the curl command in PowerShell? - Stack Overflow](https://stackoverflow.com/questions/40105458/how-to-use-the-curl-command-in-powershell )  

## 略過憑證檢查

呼叫 Invoke-WebRequest 發生以下錯誤時，表示沒有合適的憑證。自簽署的憑證因為是伺服器自行建立，沒有通過認證，因此客戶端會出現錯誤。

```
Invoke-WebRequest: 基礎連接已關閉: 無法為 SSL/TLS 安全通道建立信任關係。
```

此時可以使用 7.4 版本的 PowerShell，其 Invoke-WebRequest 包含 `-SkipCertificateCheck`  參數，可以用來略過憑證檢查。

### 參考資料

- [POWERSHELL 呼叫WEB API ，無法為 SSL/TLS 安全通道建立信任關係 - iT 邦幫忙](https://ithelp.ithome.com.tw/questions/10214882 "https://ithelp.ithome.com.tw/questions/10214882")  
- 拙作 [自簽署憑證 – Lazy Coding](https://b6land.github.io/Self_Signed_Certificate/ "https://b6land.github.io/Self_Signed_Certificate/")  
