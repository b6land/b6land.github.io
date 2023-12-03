---
layout: post
title: ASP.NET 的頁面生命週期與 PostBack
date: 2021-07-18 12:00:00 +0800
categories:  [C#,ASP.NET, Interview]
--- 

本篇文章介紹 ASP.NET 的頁面生命週期的各階段行為，以及 PostBack 的簡介、與 CallBack 的差異。

### ASP.NET 的頁面生命週期

目前常用到的 ASP.NET 的生命週期：

**Page_Init**: 在每個控制項初始化後，會觸發本事件。可用來讀取或初始化控制項的屬性。

**Page_Load**: 頁面物件本身、每個控制項載入後，都會觸發本事件。控制項的事件會比頁面的事件晚觸發。

- 第一個頁面資料被回復的地方。
- 絕大部分程式碼會檢查 IsPostBack，去避免不必要的重設狀態。(參考下方章節)
- 可以在這裡建立動態控制項。

**控制項 PostBack**: 控制項的每個事件都會造成 PostBack。

- 可以使用這些事件處理控制項的行為。

**OnPreRender**: 準備被繪製到頁面上。

- 有設定 DataSourceID 的控制項會綁定資料。

- 改寫自: [Page Life Cycle In ASP.NET](https://www.c-sharpcorner.com/UploadFile/8911c4/page-life-cycle-with-examples-in-Asp-Net/)

### 什麼是 PostBack

- 網頁提出 POST 要求，由伺服器處理，並將結果回傳給瀏覽器。
- 讓網頁變成事件驅動 (Event-Driven)，與桌面應用程式 (Winform) 的開發較為相似。
- 開啟網頁時，`Page_Load()` 內的 `IsPostBack` 屬性為 False；網頁控制項發生動作時則為 True。

- 參考資料: [Day29-[ASP.NET][C#]PostBack-為什麼前輩都用IsPostBack當起手式? - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10222506)

### PostBack 和 CallBack 的差異

- PostBack 可能會清除狀態資訊，如變數值。
- PostBack 是把全部的資料都回傳給 Server 處理，而 CallBack 只要回傳有觸發的部分 (ex. 按鈕) 即可。
- CallBack 處理完成後，也只須回傳須更新的資訊，可減輕伺服器的負擔。

**參考資料**

- [PostBack（回傳）與CallBack（回呼）的差別 - MIS2000Lab. - 點部落](https://dotblogs.com.tw/mis2000lab/2008/09/23/postback_callback)

- 什麼是 CallBack: [Client Callbacks in ASP.NET 2.0 - CodeProject](https://www.codeproject.com/Articles/14220/Client-Callbacks-in-ASP-NET-2-0-2)