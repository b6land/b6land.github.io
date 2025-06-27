---
layout: post
title: Firefox 利用網頁開發工具保存紀錄 (HAR) 
date: 2025-06-28 02:00:00 +0800
categories: [Programming]
---

HAR 檔案是 HTTP Archive 的簡稱，也可稱為 HTTP 封存檔。HAR 是一種 JSON 格式的檔案，用來記錄網頁與伺服器之間的所有網路請求與回應細節。它是開發者在除錯、分析、或驗證網頁性能、功能、與安全性時非常重要的工具。

### 用 Firefox 儲存 HAR 檔案

Firefox 瀏覽器中，可以在網路標籤旁，右上角的齒輪圖示處，勾選「保留紀錄」。

![保留紀錄](/assets/imgs/2025-06-27/firefox_1.png)

接著再操作要記錄的網路活動，可以看到窗格裡記錄的結果。

![網路活動](/assets/imgs/2025-06-27/firefox_2.png)

然後就可以將其保存為 HAR 檔案。

![保存 HAR 檔案](/assets/imgs/2025-06-27/firefox_3.png)

取得 HAR 檔案以後，可以交給 [Google's HAR Analyzer](https://toolbox.googleapps.com/apps/har_analyzer/) 分析。要注意的是，HAR 檔案包含記錄過程中下載的網頁內容、Cookie 資料和其它上傳的資訊，例如密碼、信用卡號碼等，所以在分享前應該先檢查並遮蔽重要資訊，避免被冒用身分或盜用資料。

### 參考資料

- [在瀏覽器中產生 HAR 檔案](https://help.webex.com/zh-tw/article/WBX9000028670/)
- [Using HAR Files to Troubleshoot Web Pages that are Failing to Fully Load - Cisco Meraki Documentation](https://documentation.meraki.com/General_Administration/Tools_and_Troubleshooting/Using_HAR_Files_to_Troubleshoot_Web_Pages_that_are_Failing_to_Fully_Load)