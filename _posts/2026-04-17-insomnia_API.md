---
layout: post
title: Insomnia API 匯出 Postman 適用的請求、環境變數
date: 2026-04-17 23:30:00 +0800
categories: [Other]
---  

Insomina 是除了 Postman 以外的另一套測試 API 的工具，可以不註冊會員，在本機端使用大部分功能 (Postman 需要註冊會員)。本篇文章將介紹如何匯出 API 請求，以及設定環境變數。

### 下載與使用

請直接參考官方網站：[https://insomnia.rest/](https://insomnia.rest/)。

### 匯出 Postman 適用的請求

要如何把目前的 API 請求，匯出給在用 Postman 的人使用呢 ？

1\. Insomnia 的左上角，按下 Workspace，選擇 Export。

![按下Export](/assets/imgs/2026-04-17/imsomnia_export_1.png)<br>

2\. 選擇要匯出的資料夾或請求。

![選擇匯出的請求](/assets/imgs/2026-04-17/imsomnia_export_2.png)<br>

3\. 選擇 Insomina JSON 格式匯出即可。

![選擇格式](/assets/imgs/2026-04-17/imsomnia_export_3.png)<br>

4\. 在 Postman 按下 Import，選擇剛剛匯出的 JSON 檔案匯入即可。

![從Postman匯入](/assets/imgs/2026-04-17/imsomnia_export_4.png)<br>

### 設定環境變數

1\. 在視窗左上方，可以找到 Environment 的功能 (如果已經有設定環境變數，會是環境變數的名稱)，可以按下編輯的按鈕。

![按下Environment](/assets/imgs/2026-04-17/imsomnia_env_1.png)

2\. 可以在此頁建立不同的環境，並建立該環境的變數，右方有 Table View 可切換用文字或表格介面編輯變數。

![設定環境](/assets/imgs/2026-04-17/imsomnia_env_2.png)

3\. 可以在 URL 輸入 `{{` 變數名稱 `}}` ，然後 Insonmia 會自動轉為環境變數，可以在下方的 URL Preview 看到實際帶入的網址。

![設定變數](/assets/imgs/2026-04-17/imsomnia_env_3.png)

4\. 應用範例：如果設定了多個環境（如 Dev, Prod），可在左上角下拉選單快速切換，URL 會隨之自動變更。