---
layout: post
title: jQuery 使用 AJAX 呼叫 API，與跨來源資源共享 (CORS) 簡介
date: 2024-11-11 22:00:00 +0800
categories:  [jQuery]
--- 

如果想要從前端網頁呼叫 API，可以透過 jQuery 撰寫 AJAX 語法達成！

### 使用 AJAX 呼叫 API

範例如下：

```javascript
            const settings = {
                "async": true,
                "crossDomain": true,
                "url": "https://xxx.com/data",
                "type": "GET"
            };

            $.ajax(settings).done(function (response) {
                console.log(response);
            });
```

- `done`: 完成時的處理邏輯，可以替換為 `success` (成功取得資料) 和 `error` (發生錯誤) 時的處理方式。
- `async`: 是否要啟用非同步處理。 _(重要~此描述可設為 `false`，確實等待資料接收完成)_
- `type`: 指定要使用的 HTTP 請求方式，例如 GET, POST, PUT 等，1.9.0 版本之前使用。1.9.0 之後的版本使用 `method` 代替 `type` ([javascript - What is the Diff between type and method in ajax - Stack Overflow](https://stackoverflow.com/questions/43543174/what-is-the-diff-between-type-and-method-in-ajax)。
- `crossDomain`: 強制使用 JSONP 格式，從別的網域取得資料 (只適用 GET)。

### 跨網域的 API 請求

從瀏覽器發送 API 請求時，如果和 API 所在的主機不同網域，要避免 CSRF (Cross Site Request Forgery, 跨站請求偽造) 問題。例如使用者取得 A 網站 cookie 後，瀏覽 B 網站，B 網站取得使用者的 A 網站 Cookie 後，對 A 網站發送偽造請求。

如果請求為 GET 時，會使用 JSONP 處理跨網域的請求。否則就會需要處理 CORS (Cross-Origin Resource Sharing, 跨來源資源共享) 策略。如果沒有使用 JSONP 或 CORS 策略，請求會被拒絕。

處理 AJAX 請求時，如果發現不屬於簡單跨來源請求，會自動發送 Preflight request (預檢請求)，若後端通過預檢請求，瀏覽器才會再接著發送原本的請求。未通過直接發送請求時，會得到 405 Method Not Allowed 的錯誤。

### 參考資料

- 用 jQuery 的 AJAX 呼叫 API：[AJAX : 網頁顯示資料-Jquery篇 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10192450)
- jQuery 的 AJAX 進階用法：[\[jQuery\]\[筆記\] 小心使用 Ajax 防止 Bug 產生 - 分享你的 Coding 新鮮事 - 點部落](https://dotblogs.com.tw/jasonyah/2013/06/02/use-ajax-you-need-to-be-care)
- 詳細瞭解跨來源資源共享：[\[教學\] 深入了解 CORS (跨來源資源共用): 如何正確設定 CORS？ - Shubo 的程式開發筆記](https://www.shubo.io/what-is-cors/)
- 番外 - 如何在 ASP.NET Core 中設定跨來源請求：[Enable Cross-Origin Requests (CORS) in ASP.NET Core - Microsoft Learn](https://learn.microsoft.com/en-us/aspnet/core/security/cors?view=aspnetcore-3.1)  