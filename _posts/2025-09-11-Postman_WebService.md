---
layout: post
title: 用 Postman 呼叫 WebService
date: 2025-09-11 22:00:00 +0800
categories: [ASP.NET]
---  

如何用 Postman 呼叫 WebService，像一般 Restful API 測試呢 ?

### 先來理解什麼是 WebService

Web Services 是一種軟體服務，如同 Restful API 一樣可以提供、交換資料。

傳送 (回傳) 的格式是 XML 格式，傳送期間會序列化成 SOAP 格式，接收時再還原成 XML。

請參考：[\[筆記\] Web Service 概述 ~ m@rcus 學習筆記](https://marcus116.blogspot.com/2017/08/web-service.html)。

### 如何用 Postman 測試 WebService

1. 連到 Web Service 的 asmx 頁面。
2. 選擇要測試的功能。
3. 複製 SOAP 1.2 的請求範例 (不必複製標頭，只要複製 XML 的部分就好)。
4. 切換至 Postman 內，先建立請求，選擇 POST。
5. 選擇 Body > raw 後，複製到 Body 內。
6. 輸入 Web Service 的 asmx 頁面網址。
7. 在 Header 新增一筆參數：
    - Key: `Content-Type` 
    - Value : `application/soap+xml; charset=utf-8`  
8. 接著連線就能取得結果。

詳細的圖文可參考：[\[ C# 開發隨筆 \] 使用 Postman 串接測試 WebService - 小安研究記事 - 點部落](https://dotblogs.com.tw/anmlab/2019/08/30/172607)。

### 如何寫 Web Service

請直接參考 [Web Service與 WCF 系列文章 - MIS2000Lab. - 點部落](https://dotblogs.com.tw/mis2000lab/Series?qq=Web%20Service%E8%88%87%20WCF)。