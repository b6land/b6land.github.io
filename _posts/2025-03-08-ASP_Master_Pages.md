---
layout: post
title: ASP 的 Master Pages (主版頁面) 和使用 jQuery
date: 2025-03-08 20:00:00 +0800
categories:  [ASP.NET]
---

ASP 的 Master Pages (主版頁面) 可統一 UI 設計，減少重複編寫 HTML，維護性高。

### 簡介 Master Pages

ASPX 網頁中，可以套用 Master Pages，套用主版後，可以控制哪些部份是需要變動的，哪些部分共用同樣的網頁元件 (類似範本)。

變動的部分可放在 `asp:content` 內，並設定 ContentPlaceHolder 的 ID。

JavaScript, CSS 等等，要加在 head 內 (無論是直接寫在 MasterPage 或是透過 ContentPlaceHolder )。

#### 定義與使用 Master Pages

- Master Pages 檔案的副檔名為 .master。
- `ContentPlaceHolder` 的用途：允許子頁面 (.aspx) 定義變動的內容。
- 子頁面 (.aspx) 透過 `MasterPageFile` 屬性來指定所使用的 Master Page。

#### 在 Master Pages 中使用 JavaScript

- 如果 JavaScript 需要影響整個網站 (如導航欄動畫)，建議寫在 Master Page 的 `<head>` 內。
- 如果 JavaScript 只影響某個子頁面，建議寫在 asp:Content` 內，或使用 `ScriptManager` 動態載入 JavaScript。

### 加入 jQuery
 
若網頁元件放在有設定 Master Pages 的 ASPX 內，jQuery 的選擇器要寫成以下的形式：

```js
[id$=buttonTest]
```

元件的 ID 左右分別要用 \[id$= 和 \] 字元包圍。

這是因為元件名稱前方會加上 Content 的前綴，例如 `ctl00_ContentPlaceHolder1_buttonTest`，選擇器使用上述形式才會找出所有後面是原本 ID 的元件。

(記得善用偵錯工具和 debugger 檢查 !! )

### 參考資料

- 主版頁面的更多使用方式可參考 [\[ASP.NET\] Master Pages 主版頁面 (上) - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10223044)。
- 怎麼在 ASP.NET WebForm 中加入 jQuery：[JavaScript與ASP.NET圓舞曲之三: jQuery初步 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10095401)。 (一系列的文章都值得看!)