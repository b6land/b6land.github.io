---
layout: post
title: IIS 更新網站、API 的步驟與疑難排解
date: 2024-10-03 08:30:00 +0800
categories: [ASP.NET]
--- 

為放在 IIS 伺服器上的 ASP.NET 的網站、API 更新版本時，使用以下的步驟更新，避免服務發生錯誤。

### 手動更新

1. 從 IIS 先停止應用程式。
2. 更換應用程式的檔案。
    - 可能要避免更換到設定檔，導致測試的設定值覆蓋到生產環境。
3. 在 IIS 啟動應用程式。

### 使用 Visual Studio 更新

1. 打開 Visual Studio，開啟要發布的專案。
2. 從方案總管選擇專案，並點選「發布...」。
3. 接著選擇「網頁伺服器 (IIS)」。
4. 選擇「Web Deploy」。
5. 輸入連線資訊，在「伺服器」中填入名稱、IP 或 URL 位置，例如 `localhost` ；「網站名稱」是 IIS 中的網站名稱；目的地 URL 是發佈完成後要開啟的網址；最後填入存取 IIS 需要的帳號與密碼。其它設定值可參考「[Manage web deployment settings - Visual Studio (Windows) - Microsoft Learn](https://learn.microsoft.com/en-us/visualstudio/deployment/web-deployment-settings?view=vs-2022 "https://learn.microsoft.com/en-us/visualstudio/deployment/web-deployment-settings?view=vs-2022")」
6. 接著可以進行發布。

![Web Deploy 的設定值](/assets/imgs/2024-10-03/web_deploy.png)

欲發布的主機，需安裝 Web Management Service，否則無法發布成功。 此外，如果有防火牆擋住 8172 Port，還是無法使用發佈方式直接更新檔案。

- 其它問題排解，可參考：[針對 Visual Studio 的 Web 部署問題進行疑難排解 - Microsoft Learn](https://learn.microsoft.com/zh-tw/iis/publish/troubleshooting-web-deploy/troubleshooting-web-deploy-problems-with-visual-studio)  

### 發布 API 時發生錯誤 (適用 .net 6.0)

1 若 API 所在的資料夾權限不足時，會遇到 0x80070005 的錯誤，應該設定資料夾啟用 USERS 的權限。

2 若遇到 0x8007000d 錯誤，要安裝對應版本的 Hosting Bundle (裡面已經包含 .Net Runtime，若有需要，仍可自行安裝 Runtime)。安裝完成後，需要在 Windows 命令列中輸入：`iisreset /start`  (需要使用系統管理員權限)。


#### 延伸閱讀

- [c# - HTTP Error 500.31 - Failed to load ASP.NET Core runtime - Stack Overflow](https://stackoverflow.com/questions/65317970/http-error-500-31-failed-to-load-asp-net-core-runtime)
- [living in my life (史蘭諾系統整合服務): \[IIS\]解決 IIS7 無法讀取設定檔案，因為權限不足(Solve IIS7 can not read the settings file because of insufficient permissions)](http://silanors.blogspot.com/2011/04/iisiis7.html)


### 無法取得系統環境變數

如果網站或 API 需要存取系統變數，請檢查以下兩個項目：

1. 如果是剛設定系統變數，需要重新啟動 IIS。
2. 如果是設定在使用者，要留意 IIS 服務是否由其他使用者啟動 ([c# - IIS cannot get my environment variable - Stack Overflow](https://stackoverflow.com/questions/43321731/iis-cannot-get-my-environment-variable))。

重新啟動的方式：在 Windows 命令列中輸入：`iisreset /start`  (需系統管理員權限)。只有在 IIS 管理介面重新啟動，不會重新取得系統環境變數。