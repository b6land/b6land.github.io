---
layout: post
title: Http Header 加入中文字時，發生「指定值具有無效的 Control 字元。」錯誤
date: 2024-04-14 12:00:00 +0800
categories: [Programming]
---

本問題可參考 [HttpWebRequest.Headers.Add出错 指定的值含有无效的控制字符\_百度知道](https://zhidao.baidu.com/question/560913607.html "https://zhidao.baidu.com/question/560913607.html") ，但說法有誤，除了 127 (例如：板) 以外，0 ~ 31 (例如：布、合) 也會被視為控制字元。

### 什麼是 Http Header

Http Header 是夾帶在 Http Request 和 Response 間的資料。在制定的網路標準中，可以用來表示內容類型、認證方式等等；此外也可以自訂自己想傳輸的內容。

Http Header 的格式為 欄位名稱: 內容，例如：

```plaintext
content-type: application/json
user-agent: insomnia
```

詳細說明可參考 [HTTP headers - HTTP - MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers ) 。

### 「指定值具有無效的 Control 字元。」錯誤

在傳送自訂的 Http Header 內容時，如果輸入的 Value 包含中文，就可能會觸發這個錯誤。

Http Header 定義可以參考：[what characters are allowed in HTTP header values? - Stack Overflow](https://stackoverflow.com/questions/47687379/what-characters-are-allowed-in-http-header-values)，其中列出了控制字元的範圍。由文章可知 Http Header 會以 ASCII 讀取 Header 內的文字，因此會將部分 UTF-8 的中文 (編碼包含控制字元 ASCII 碼的字) 視為控制字元，此時就會引發「指定值具有無效的 Control 字元。」的錯誤。

發生錯誤時，可以使用 [Utf-8 Converter, Utf-8 Encoding and Decoder - CheckSERP](https://checkserp.com/encode/utf8/) 網站解碼傳送內容內的 UTF-8 中文字，檢查低位元是否落在控制字元的範圍。

可用來測試的文字：

- 不會被視為控制字元：二零碼
- 會被視為控制字元：板布合程范