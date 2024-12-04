---
layout: post
title: JSONPlaceHolder 教學 - 假資料測試 API
date: 2024-12-04 22:00:00 +0800
categories: [Other]
--- 

需要測試 API 時，想要先串接假資料，這時可以使用 [JSONPlaceHolder](https://jsonplaceholder.typicode.com/) 網站的服務。  

### 直接使用瀏覽器測試

先切換到該網站的 [JSONPlaceholder - Guide](https://jsonplaceholder.typicode.com/guide/) 頁面，提供各 API 的範例，可以看到每個標題的指令，以 fetch 指令呈現，以及對應的輸出。

接著按下 F12 鍵，開啟 Chrome 瀏覽器的開發人員工具 (或從「瀏覽器選單 > 更多工具 > 開發人員工具」開啟)，並切換至 Console 頁面。

可以先輸入 allow pasting 後按下 Enter 來允許貼上指令，接著複製 fetch 開頭的這一串指令，按下 Enter 執行，結果如下：

![Chrome 開發人員工具](/assets/imgs/2024-12-04/dev_tools_chrome.png)

可以看到下方取得了 API 回傳的假資料。

使用 Firefox 測試的方法相似，一樣按下 F12 打開「開發人員工具」，輸入 allow pasting 後貼上測試指令，結果如下：

![Firefox 開發人員工具](/assets/imgs/2024-12-04/dev_tools_firefox.png)

### 使用 Postman 測試

如果是 Getting a resource 或類似的取回請求，可以直接在 Postman 建立新分頁，指定連線為 GET 後輸入 URL，接著按下 Send 執行。

URL 在 fetch 的括號內，例如 `fetch('https://jsonplaceholder.typicode.com/posts/1')` 的 URL 為 `https://jsonplaceholder.typicode.com/posts/1`。

執行結果如下：

![Postman 的 GET 請求](/assets/imgs/2024-12-04/postman_get.png)

如果是 Creating a resource 或類似的寫入請求，則需要再另外填入請求方式、請求內容、內容格式才行！

在 Postman 內，先將測試方式從 GET 改成 POST，然後點選下方的 Body，將選單的 none 改成 raw，右方的 Text 改為 JSON。

請求內容在 `body: JSON.stringify` 的括號內，要將其複製出來，並加上雙引號 (數字不用)。修改的結果如下：

```json
{
    "title": "foo",
    "body": "bar",
    "userId": 1
}
```

然後按下 Send 執行，順利的話會回傳結果：

![Postman 的 POST 請求](/assets/imgs/2024-12-04/postman_post.png)

因為是傳回假資料，不會真的新增或修改伺服器上的資料！

### 延伸閱讀

- 其它的 API 列表介紹： [給工程師的12個超好用API、可串接大量有趣資料～！](https://codelove.tw/@tony/post/Ja6JaO)