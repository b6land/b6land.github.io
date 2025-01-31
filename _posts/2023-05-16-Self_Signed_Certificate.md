---
layout: post
title: 自簽署憑證
date: 2023-05-16 12:00:00 +0800
categories: [C#]
---

開發的程式若需連接到使用自簽署憑證的 API，可以用 Postman 檢查，並在 C# 內調整連線方式。

### 關於自簽署憑證

- 自簽署憑證是伺服器自行產生的憑證，未經過正式的第三方單位認證。
- 常應用在需要加密的內部網路環境下。
- [Yuan-Jhen Info Co., LTD. - 什麼是自簽署（self-signed）憑證？](https://twnoc.net/support/Knowledgebase/Article/View/214/17)

### 使用 Postman 測試

- 在 Postman 的設定中，於 General 頁籤裡將 **SSL Certificate Verification** 選項關閉，即可正常連線。

![SSL Certificate Verification](/assets/imgs/2023-05-16/ssl_certificate_verification.png)

- [How to Troubleshoot SSL Certificate & Server Connection Issues - Postman](https://blog.postman.com/self-signed-ssl-certificate-troubleshooting/)
- [Setting up Postman - Postman Learning Center](https://learning.postman.com/docs/getting-started/settings/)

### 在 C# 中如何處理自簽署憑證

- 調整 `ServicePointManager.ServerCertificateValidationCallback`，將 callback 的結果設為 `true`。
  - 需要小心使用，因為這是對系統全域性的修改。建議應手動檢查憑證內容、適用網域是否正確，或在用完後調整回原始值。
  - [c# - Using a self-signed certificate with .NET's HttpWebRequest/Response - Stack Overflow](https://stackoverflow.com/questions/526711/using-a-self-signed-certificate-with-nets-httpwebrequest-response)

- 如何在調整 `ServerCertificateValidationCallback` 屬性時，加入 Host 的判斷:
  - [[faq]解決C#呼叫有ssl憑證問題的網站出現遠端憑證是無效的錯誤問題 @ Alan Tsai 的學習筆記](https://blog.alantsai.net/posts/2017/12/csharp-ssl-remote-validation-error)

- 只用在特定 HttpWebRequest 上。
  ```cs
  var request = (HttpWebRequest)HttpWebRequest.Create(myUri);
  request.ServerCertificateValidationCallback = delegate { return true; };
  ```
  - [c# - Best way to handle self signed certificates - Stack Overflow](https://stackoverflow.com/questions/44506561/best-way-to-handle-self-signed-certificates)

### 其他可能的做法

- 在 Windows 內安裝自簽署憑證: [Installing Self-Signed CA Certificate in Windows - IT Security - Spiceworks](https://community.spiceworks.com/how_to/1839-installing-self-signed-ca-certificate-in-windows)
