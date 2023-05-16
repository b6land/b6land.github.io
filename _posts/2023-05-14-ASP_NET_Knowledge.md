---
layout: post
title: ASP.NET 常識
date: 2023-05-14 12:00:00 +0800
categories: [ASP.NET]
---

本篇介紹 ASP.NET 中，時常會用到的知識，包含瀏覽網頁時如何存取資料、以及連向其他網頁的差異。

### 網站保存資訊的方式

|存放方式|存放位置|使用者存取|有效期限|用途|
|---|---|---|---|---|
|Application|Server 記憶體中|所有使用者都可以存取|IIS 未關閉、修改設定檔時|設定檔|
|Cookie|使用者電腦|伴隨著每次網頁 Request 送出|通常為 1000 分鐘，可自行設定|使用者偏好設定|
|Session|Server 記憶體中|對應到特定使用者的瀏覽器|直到瀏覽器關閉前、未超過閒置時間|連線相關資訊|
|Cache|Server 記憶體中|所有使用者共用同一變數|自行設定|減少對資料庫存取|
|ViewState|單一頁面|該頁面的使用者|該頁面生命週期|暫存頁面所需資料|

### 重導向的差異

- Response.Redirect: 可跨越不同網站、不保留 Query String 和表單變數、顯示轉址後的網址。
- Server.Transfer: 只能在同一網站中、保留 Query String 和表單變數、顯示原來網址。

### 參考資料

- [[ASP.NET]Response.Redirect與Server.Transfer差別 - gipi的學習筆記-經營管理、數據思考、終身學習 - 點部落](https://dotblogs.com.tw/jimmyyu/2009/11/10/11503)
- [Day30-[ASP.NET]你今天想怎麼保存資訊?Application、Session、Cookie、Cache、ViewState - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10222885)