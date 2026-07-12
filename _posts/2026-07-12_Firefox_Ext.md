---
layout: post
title: 開發 Firefox 擴充套件的基本知識
date: 2026-07-12 11:00:00 +0800
categories: [Other]
---  

這篇文章會介紹初次開發 Firefox 擴充套件時的基本知識。

### 擴充套件的組成

擴充套件一定會有 `Manifest.json`，包含套件的名稱、版本、及需要的權限等資訊，而且提供套件裡其它檔案的路徑。

套件的主要組成通常如下：

| 項目  | 說明  |
| --- | --- |
| Background Scripts | 背景腳本，實作長時間執行的邏輯。 |
| Sidebar, Popup, Options Page | 側邊欄、彈出視窗、選項頁面，供使用者操作。<br> |
| Content Scripts<br> | 運作在網頁中的程式碼。 |

### Manifest.json 注意事項

Firefox 149 現在還不支援 MV3 (Manifest V3) 的部分寫法，因此 background 要寫成：

```json
"background": {
    "scripts": ["src/background/background.js"]
}
/* MV3 寫法，暫不支援
"background": {
    "service_worker": "background.ts"
}
*/
```

如果要上傳到「[附加元件開發者交流中心](https://addons.mozilla.org/zh-tw/developers/) 」給他人使用，需要填寫資料收集權限，可以設定 `required`  和 `optional` ：

```json
"data_collection_permissions": {
  "required": [
    "websiteActivity",
    "websiteContent",
    "browsingActivity",
    "authenticationInfo"
  ]
}

```

以下是權限的簡易翻譯，請留意官方定義有更詳細的說明，建議前往 [Firefox built-in consent for data collection and transmission - Firefox Extension Workshop](https://extensionworkshop.com/documentation/develop/firefox-builtin-data-consent/) 檢閱：


| 資料類型 | Manifest 內的權限 | 定義  |
| --- | --- | --- |
| 個人識別資訊 | personallyIdentifyingInfo | 姓名、地址、電話與其它可識別資訊 |
| 健康資訊 | healthInfo | 醫藥紀錄…等 |
| 金融和付款資訊 | financialAndPaymentInfo | 信用卡號碼、交易紀錄…等 |
| 認證資訊 | authenticationInfo | 帳號、密碼、PIN 碼…等 |
| 個人通訊 | personalCommunications | Email, 聊天訊息、社群發文…等 |
| 位置  | locationInfo | 區域、座標…等 |
| 瀏覽活動 | browsingActivity | 使用者造訪的網站，如URL、網域…等 |
| 網站內容 | websiteContent | 網站可看到的內容，如文字、圖片、影音…等，以及內含的 cookie, header…等 |
| 網站活動 | websiteActivity | 鍵盤、滑鼠的動作、下載與存檔…等 |
| 搜尋項目 | searchTerms | 在網址或搜尋引擎輸入的搜尋項目 |
| 書籤  | bookmarksInfo | 書籤名稱、網站…等 |

### 載入開發中的擴充套件

輸入 `about:debugging`  以後，選擇「這個 Firefox」，在「暫用擴充套件」內可以載入開發中的擴充套件，要選擇擴充套件所在的目錄 (須包含 `manifest.json` )。

![載入開發的擴充套件](/assets/imgs/2026-07-12/firefox_1.png)

### 簽署擴充套件

擴充套件要簽署過，才可以給他人安裝，否則會出現「檔案已損毀」的錯誤。

步驟如下：

1. 在 [附加元件開發者交流中心](https://addons.mozilla.org/zh-tw/developers/)  註冊並登入。
2. 登入後，選擇「上架新的附加元件」。
3. 散佈的方式選擇「您自行處理」。
4. 將擴充套件壓縮成 zip 檔案並上傳 (`manifest.json`  要在最上層)。
5. 驗證完成後就可以簽署附加元件，如有錯誤會顯示。
6. 如果有用過程式混淆、合併的工具等等，需要提交原始碼。
7. 接下來要等待數分鐘，審核通過後就會提供下載連結，可以在擴充套件的設定按鈕裡，按下「從檔案安裝附加元件…」，選擇剛剛經過簽署的 xpi 檔案。

![附加元件開發者交流中心](/assets/imgs/2026-07-12/firefox_2.png)

![選擇簽署的套件檔案](/assets/imgs/2026-07-12/firefox_3.png)

### 參考資料

- 擴充套件開發教學：[你的第一個 WebExtension - Mozilla - MDN](https://developer.mozilla.org/zh-TW/docs/Mozilla/Add-ons/WebExtensions/Your_first_WebExtension)
- 解析擴充套件：[Anatomy of an extension - Mozilla - MDN](https://developer.mozilla.org/zh-TW/docs/Mozilla/Add-ons/WebExtensions/Anatomy_of_a_WebExtension)
- Manifest 的 Background 寫法：[google chrome - Manifest v3 background scripts/service worker on Firefox - Stack Overflow](https://stackoverflow.com/questions/75043889/manifest-v3-background-scripts-service-worker-on-firefox)
- 擴充套件簽署教學：[\[教學\] 如何對開發好的Firefox附加元件進行簽署 - 辛比誌](https://xenby.com/b/185-教學-如何對開發好的firefox附加元件進行簽署)