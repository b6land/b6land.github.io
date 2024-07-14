---
layout: post
title: POST 送出資料時，Content-Type 的 XML 分別  
date: 2022-07-24 12:00:00 +0800
categories: [Programming]
---

有一次接下的工作，要用 POST 送資料給客戶的伺服器。使用 POST 送出資料時，調整 `Content-Type` 為 `text/xml` 或 `application/xml` 才能讓客戶的伺服器正確接受並回應，為什麼要這樣設定？

### 介紹

POST 將資料送出至伺服器，特點是每次發送至伺服器都會有新的影響（例如購物車連續新增兩個相同商品）。

`Content-Type` 則是 POST 的表頭 (header) ，可表示傳送內容 (body) 的類型。

這兩個屬性都可表示傳送內容是 XML 格式，不過仍有以下區別：

- 如果是 `text/xml` ，表示這份 xml 文件傾向可被使用者閱讀，沒有支援 `text/xml` 的 agent，文件會被當成 text/plain 對待。

- 如果使用 `application/xml`，則表示這份 xml 比較無法被使用者閱讀。

### 參考資料

- [POST - HTTP - MDN](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Methods/POST)
- [菜鳥工程師 肉豬: HTTP 協議的Idempotent Methods](https://matthung0807.blogspot.com/2019/02/http-idempotent-methods.html?m=1)
- [rest - What's the difference between text/xml vs application/xml for webservice response - Stack Overflow](https://stackoverflow.com/questions/4832357/whats-the-difference-between-text-xml-vs-application-xml-for-webservice-respons)